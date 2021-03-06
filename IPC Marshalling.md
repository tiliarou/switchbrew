## IPC Command Structure

This is an array of u32's, usually located in [Thread Local
Storage](Thread%20Local%20Storage.md "wikilink").

| Word | Bits  | Description                                                                                                 |
| ---- | ----- | ----------------------------------------------------------------------------------------------------------- |
| 0    | 15-0  | [Type](#Type "wikilink").                                                                                   |
| 0    | 19-16 | Number of buf X descriptors (each: 2 words).                                                                |
| 0    | 23-20 | Number of buf A descriptors (each: 3 words).                                                                |
| 0    | 27-24 | Number of buf B descriptors (each: 3 words).                                                                |
| 0    | 31-28 | Number of buf W desciptors (each: 3 words), not observed                                                    |
| 1    | 9-0   | Size of [raw data](#Raw_data_section "wikilink") in u32s.                                                   |
| 1    | 13-10 | Flags for buf C descriptor.                                                                                 |
| 1    | 30-20 | ?                                                                                                           |
| 1    | 31    | Enable handle descriptor.                                                                                   |
| ...  |       | [Handle descriptor](#Handle_descriptor "wikilink"), if enabled.                                             |
| ...  |       | [Buf X descriptors](#Buffer_descriptor_X_"Pointer" "wikilink"), each one 2 words.                           |
| ...  |       | [Buf A descriptors](#Buffer_descriptor_A/B/W_"Send"/"Receive"/"Exchange" "wikilink"), each one 3 words.     |
| ...  |       | [Buf B descriptors](#Buffer_descriptor_A/B/W_"Send"/"Receive"/"Exchange" "wikilink"), each one 3 words.     |
| ...  |       | [Type W descriptors](#Buffer_descriptor_A/B/W_"Send"/"Receive"/"Exchange" "wikilink"), each one 3 words.    |
| ...  |       | [Raw data section](#Raw_data_section "wikilink") (including padding before and after aligned data section). |
| ...  |       | [Buf C descriptors](#Buffer_descriptor_C_"ReceiveList" "wikilink"), each one 2 words.                       |

First two header u32's and handle descriptor (if enabled) are copied
as-is from one process to the other.

### Type

IPC commands can have different types which influence how the IPC server
processes requests in
"nn::sf::hipc::server::HipcServerSessionManagerBase::ProcessRequest".

| Value | Name                                                                                |
| ----- | ----------------------------------------------------------------------------------- |
| 0     | Invalid                                                                             |
| 1     | [LegacyRequest](#LegacyRequest,_LegacyControl "wikilink")                           |
| 2     | [Close](#Close "wikilink")                                                          |
| 3     | [LegacyControl](#LegacyRequest,_LegacyControl "wikilink")                           |
| 4     | [Request](#Request,_Control "wikilink")                                             |
| 5     | [Control](#Request,_Control "wikilink")                                             |
| 6     | \[5.0.0+\] [RequestWithContext](#RequestWithContext,_ControlWithContext "wikilink") |
| 7     | \[5.0.0+\] [ControlWithContext](#RequestWithContext,_ControlWithContext "wikilink") |

#### Close

When processing a request of this type, the IPC server
    calls:

  - "nn::sf::hipc::server::HipcServerSessionManager::DestroyServerSession"
  - "nn::sf::hipc::CloseServerSessionHandle"

This ensures that the server session is destroyed internally and
properly closed.

#### LegacyRequest, LegacyControl

These types are handled by
    calling:

  - "nn::sf::hipc::detail::HipcMessageBufferAccessor::ParseHeader"
  - "nn::sf::hipc::server::HipcServerSessionManager::ProcessMessage"
  - "nn::sf::hipc::Reply"
  - "nn::sf::hipc::server::HipcServerSessionManager::RegisterServerSessionToWaitBase"

It is speculated that these are part of an older message processing
system where headers were further partitioned.

#### Request, Control

These types are handled by
    calling:

  - "nn::sf::hipc::server::HipcServerSessionManager::ProcessMessage2"
  - "nn::sf::hipc::server::HipcServerSessionManager::RegisterServerSessionToWaitBase"

This represents a more modern message handling system where contents
follow the general marshalling structure.

#### RequestWithContext, ControlWithContext

These are identical to normal Request and Control types, but with the
additional requirement of suppling a token in their [data
payload](#Data_payload "wikilink").

This token is used by "nn::sf::cmif::SetInlineContext" which has the
sole purpose of saving it into the TLS in order for it to be distributed
to any IPC commands that are made while processing the current command.
It's unknown if this token serves any purpose or if it's just a
debug-tool to figure out what IPC command caused a particular chain of
commands.

### Handle descriptor

There can only be one of this descriptor type. It is enabled by bit31 of
the second word.

| Word | Bits | Description               |
| ---- | ---- | ------------------------- |
| 0    | 0    | Send current PID.         |
| 0    | 4-1  | Number of handles to copy |
| 0    | 8-5  | Number of handles to move |
| ...  |      | 8-byte PID if enabled     |
| ...  |      | Handles to copy           |
| ...  |      | Handles to move           |

Sysmodules load the last u64 of rawdata when handling the PID. This is
not written by kernel. For sysmodule handling:

  - In some cases: these commands require a placeholder u64 value passed
    in the input parameters, as mentioned above. In these cases the
    OverwriteClientProcessId method is called to replace the value
    before it is used.
  - In other cases: The rawdata\_u64 is compared with the PID from the
    descriptor. On mismatch and when rawdata\_u64\!=0, error 0x60A is
    returned. The PID value passed to the cmdhandler vtable funcptr is
    the rawdata\_u64.

Handle 0 is allowed, and just means no handle was sent.

### Buffer descriptor X "Pointer"

This one is packed even worse than A, they inserted the bit38-36 of the
address *on top* of the counter field.

Officially, the counter is known as "receive index". This one writes to
the buffer described in the ReceiveList.

| Word | Bits  | Description               |
| ---- | ----- | ------------------------- |
| 0    | 5-0   | Bits 5-0 of counter.      |
| 0    | 8-6   | Bit 38-36 of address.     |
| 0    | 11-9  | Bits 11-9 of counter.     |
| 0    | 15-12 | Bit 35-32 of address.     |
| 0    | 31-16 | Size                      |
| 1    |       | Lower 32-bits of address. |

### Buffer descriptor A/B/W "Send"/"Receive"/"Exchange"

This packing is so unnecessarily complex.

| Word | Bits  | Description                     |
| ---- | ----- | ------------------------------- |
| 0    |       | Lower 32-bits of size.          |
| 1    |       | Lower 32-bits of address.       |
| 2    | 1-0   | Flags. Always set to 0, 1 or 3. |
| 2    | 4-2   | Bit 38-36 of address.           |
| 2    | 27-24 | Bit 35-32 of size.              |
| 2    | 31-28 | Bit 35-32 of address.           |

A reply must not use A/B/W, svcReplyAndReceive will return 0xE801.

[MemoryAttribute](SVC.md "wikilink") IsBorrowed and IsUncached are never
allowed for the source address.

"Send" means buffer is sent from source process into service process.

"Receive" means that data is copied from service process into user
process.

"Exchange" means both "Send" and "Receive".

#### Flags

Determines what [MemoryState](SVC.md "wikilink") to use with the mapped
memory in the sysmodule.

Used to enforce whether or not device mapping is allowed for src and dst
buffers respectively.

  - Flag0: Device mapping \*not\* allowed for src or dst.
  - Flag1: Device mapping allowed for src and dst.
  - Flag3: Device mapping allowed for src but not for dst.

### Buffer descriptor C "ReceiveList"

There's a 4-bit flag in the main header controlling the behavior of C
descriptors.

If it has value 0, the C descriptor functionality is disabled.

If it has value 1, there is an "inlined" C buffer after the raw data.
Received data is copied to ROUND\_UP(cmdbuf+raw\_size+index, 16)

If it has value 2, there is a single C descriptor.

Otherwise it has (flag-2) C descriptors. In this case, index picks which
C descriptor to copy received data to \[instead of picking the offset
into the buffer\].

Data sent with this method must have MemoryState 0x4000000 mask set.

After reply, X descriptors are written to the sender containing the
address, size and index that were copied to.

| Word | Bits  | Description               |
| ---- | ----- | ------------------------- |
| 0    |       | Lower 32-bits of address. |
| 1    | 15-0  | Rest of address.          |
| 1    | 31-16 | Size                      |

### IPC buffers

Buffer descriptor A/B/... map memory into the sysmodule process. For the
mapped memory in the sysmodule the permissions are: desc-A = R--, desc-B
= RW-. The buffer is automatically unmapped while the kernel handles the
cmdreply, the sysmodule doesn't need to specify anything in the cmdreply
to trigger this.

This memory is mapped in the sysmodule to the same vaddr from the
original user-process cmd-request, except with with bits \>=(\~28(?))
changed to a different ASLR'd region.

No user-process-\>sysmodule memcpy is done for outbufs, only
sysmodule-\>user-process.

Buffer descriptors C/X are somewhat different. Rather than mapping new
memory into the server process, C/X descriptors copy data between
existing buffers in different processes. Each X descriptor in a message
has its data copied into a C descriptor on the other side. Each C
descriptor in a message is used to reserve space for the other side's X
descriptors to copy into.

When the kernel processes X descriptors, it must determine where to copy
the data to. If the destination used C descriptors with flags \>= 3,
each X descriptor from the source is matched to a C descriptor in the
destination by the X descriptor's index field. If the destination used a
"single" C descriptor, the data from all the X descriptors is copied
into the same buffer specified by the destination's C descriptor
(causing error 0xce01 if there is not enough space) and the X descriptor
index is ignored. The kernel then modifies the addresses in the X
descriptors to indicate where the data was copied to in the destination.

Before receiving a request, if the IPC server is expecting X
descriptors, it prepares a message with a "single" C descriptor
(flags=2) in its message buffer before calling svcReplyAndReceive so
that X descriptors from the client have a place to copy their data to.
The usage of the flag-2 C descriptor allows the server to receive an
arbitrary number of X descriptors, since they're all packed into the
same buffer. If the server had used flag-3+ C descriptors, it would be
limited in how many X descriptors it could receive since the X
descriptors would have to be matched to distinct C descriptors. The
buffer that the server's C descriptor points to is called the **pointer
buffer**.

When the client sends X descriptors, data is copied into the server's
pointer buffer. When the client sends C descriptors, no data is copied
automatically. The server needs to use X descriptors to copy the data
back to the client's C descriptors (using the index field to match X
descriptors in the response back to the correct C descriptors).

## Raw data section

![An example of an IPC message with a type 0xA buffer in it. Red is
headers/descriptors, yellow is padding, and blue is data/buffer lengths.
Note that the size of the u16 array for type A lengths is padded to fill
up a whole word.](Ipc_msg_buffer_type_a_example.png
"An example of an IPC message with a type 0xA buffer in it. Red is headers/descriptors, yellow is padding, and blue is data/buffer lengths. Note that the size of the u16 array for type A lengths is padded to fill up a whole word.")

| Word | Description                                                                                                                       |
| ---- | --------------------------------------------------------------------------------------------------------------------------------- |
| ...  | Padding to align to 16 bytes.                                                                                                     |
| ...  | If sent to an object domain, a [domain message](#Domain_message "wikilink"), otherwise a [data payload](#Data_payload "wikilink") |
| ...  | Padding                                                                                                                           |
| ...  | Buffer type 0xA lengths (u16 array)                                                                                               |

The total amount of padding within the raw data section is always 0x10
bytes. This means that if no padding is required before the message,
there will be 0x10 bytes of padding after the message (before the buffer
type 0xA lengths).

### Domains

Because the switch has relatively low limits on the total number of
sessions available to the system (Kernel slabheap limits, sysmodule
handle table size limits), HIPC supports a "Domains" feature that allows
multiplexing multiple service sessions through a single handle. Domains
store (effectively) a mapping from u32 object id to a
SharedPointer<IServiceObject> -- When messages are sent to a domain, an
extra header is sent in the raw data section (before anything else) with
information about what object in the domain is being acted on; responses
similarly contain an additional header. Official session code implements
this by just using the dispatch table for the object in the map with the
appropriate ID, instead of the dispatch table the session was
initialized with.

Format for the extra request header for domain
message:

| Word | Bits  | Description                                                                                      |
| ---- | ----- | ------------------------------------------------------------------------------------------------ |
| 0    | 7-0   | Command. 1=send message, 2=close virtual handle                                                  |
| 0    | 8-15  | Input object count                                                                               |
| 0    | 31-16 | Length of [data payload](IPC%20Marshalling#Data%20payload.md##Data_payload "wikilink") in bytes. |
| 1    |       | Object ID (from cmd 0 in [Control](IPC%20Marshalling#Control.md##Control "wikilink")).           |
| 2    |       | Padding                                                                                          |
| 3    |       | \[1.0.0-4.1.0\] Padding \[5.0.0+\] Token for (NewRequest only)                                   |
| 4... |       | [Data payload](#Data_payload "wikilink")                                                         |
| ...  |       | Input object IDs (u32s, not aligned)                                                             |

Format for the extra response header for domain message:

| Word | Bits | Description         |
| ---- | ---- | ------------------- |
| 0    | 31-0 | Output object count |
| 1-3  |      | Padding             |
|      |      |                     |

### Data payload

This is an array of u32's, but individual parameters are generally
stored as
u64's.

| Word | Description                                                                                     |
| ---- | ----------------------------------------------------------------------------------------------- |
| 0    | Magic ("SFCI" for requests, "SFCO" for responses) as u32.                                       |
| 1    | Version as u32. 1 for NewRequest, 0 for Request.                                                |
| 2    | Command id as u64 for requests, [error code](Error%20codes.md "wikilink") as u64 for responses. |
| 3    | \[1.0.0-4.1.0\] Padding \[5.0.0+\] Token (for NewRequest only, non-domain messages).            |
| 4... | Input parameters or return values                                                               |

\[5.0.0+\] Version was incremented from 0 to 1, and a token value was
introduced into raw\_data+12 (regardless of domain or not, in either
case it overlaps with padding).

The input rawdata struct is generated by stable-sorting function
parameters by alignment, from low to high. It is likely this is a
mistake, as it generates structs with suboptimal possible padding --
Nintendo probably meant to sort from high to low (which would give
minimized padding), but couldn't/can't change this without breaking
backwards compatibility.

## Official marshalling code

The official marshalling function takes an array of (buf\_ptr, size)
pairs and a type-field for each such pair.

Bitmask 0x10 seems to indicate null-terminated
strings.

| Type Mask       | Description                                                                 | Direction |
| --------------- | --------------------------------------------------------------------------- | --------- |
| 4 + 1           | Creates a A descriptor with flags=0.                                        | In        |
| 0x40 + 4 + 1    | Creates a A descriptor with flags=1.                                        | In        |
| 0x80 + 4 + 1    | Creates a A descriptor with flags=3.                                        | In        |
| 4 + 2           | Creates a B descriptor with flags=0.                                        | Out       |
| 0x40 + 4 + 2    | Creates a B descriptor with flags=1.                                        | Out       |
| 0x80 + 4 + 2    | Creates a B descriptor with flags=3.                                        | Out       |
| 8 + 1           | Creates an X descriptor                                                     | In        |
| 8 + 2           | Creates a C descriptor, and writes the u16 size to an offset into raw data. | Out       |
| 0x10 + 8 + 2    | Creates a C descriptor                                                      | Out       |
| 0x20 + 1        | Creates both an A and X descriptor                                          | In        |
| 0x20 + 2        | Creates both an B and C descriptor                                          | Out       |
| 0x20 + 2 + 0x40 | Same as 0x20 + 2, except a certain value is set to hard-coded 0x1 instead.  | Out       |

C and X (Pointer and ReceiveList) descriptors are backed by the "pointer
buffer", a buffer in the service process. Its size is a u16, which is
retrieved using the "QueryPointerBufferSize" control message. If the
client code determines all buffers with flag 8 do not fit in the pointer
buffer, it returns error 0x11A0B.

For buffers with flag 0x20 it creates two descriptors (A+X or B+C), but
one descriptor is NULL (zero size and pointer), while the other holds
the expected values. X/C descriptors are used as the non-NULL descriptor
where possible, but if they don't fit in the pointer buffer, A/B
descriptors are used instead. The code defers processing of type 0x20
buffers with sizes that fit in a u16 (and may therefore fit in the
pointer buffer). This ensures all type 8 buffers get pointer-buffer
space before any type 0x20.

(The order in which the deferred type 0x20 buffers are processed is
determined by a convoluted loop.)

## Official IPC Cmd Structure

Official struct that is stored for each IPC command. It contains
precalculated offsets for different portions of the command structure.

All offsets are given is in number of u32 words.

`struct IpcCmdStruct {`  
`  u8  unk0;`  
`  u8  has_handle_descriptor;`  
`  u8  pad0[2];`  
`  u32 cmd0;`  
`  u32 cmd1;`  
`  u32 offset_handle_descriptor;`  
`  u32 pad1;`  
`  u32 offset_handles;          `  
`  u32 pad2;`  
`  u32 offset_x_descriptors;`  
`  u32 offset_a_descriptors;`  
`  u32 offset_b_descriptors;`  
`  u32 offset_w_descriptors; /* this is a guess */`  
`  u32 offset_raw_data;`  
`  u32 offset_c_descriptors;`  
`  u32 unk2;`  
`  u32 unk3;`  
`}`

## Control

When type == 5 you are talking to the IPC manager. These are processed
by the
sysmodule.

| Cmd | Name                         | Arguments              | Output                 |
| --- | ---------------------------- | ---------------------- | ---------------------- |
| 0   | ConvertCurrentObjectToDomain | None                   | u32 CmifDomainObjectId |
| 1   | CopyFromCurrentDomain        | u32 CmifDomainObjectId | u32 NativeHandle       |
| 2   | CloneCurrentObject           | None                   | u32 NativeHandle       |
| 3   | QueryPointerBufferSize       | None                   | u16 size               |
| 4   | CloneCurrentObjectEx         | u32 unknown            | u32 NativeHandle       |
