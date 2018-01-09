**DRAFT DRAFT DRAFT DRAFT DRAFT DRAFT DRAFT DRAFT DRAFT DRAFT DRAFT
DRAFT DRAFT DRAFT**

### Entrypoint

Entrypoint is at binary\_ptr+0. At this offset, there is a branch
instruction that jumps past the NRO0 header. This is for (.text) file
format polyglot compatibility with NSO0.

Every application should clean itself up before returning to loader.

This includes tasks like:

  - Not leaking handles
  - Resetting MemoryState
  - No leftover threads running in the background

### Entrypoint Arguments

**NSO0:** the system executes binary\_ptr+0 with X0=NULL,
X1=main\_thread\_handle.

**NRO0:** the homebrew loader puts X1=0xFFFFFFFFFFFFFFFF
\[INVALID\_HANDLE\] and X0=loader\_config\_ptr.

### Loader Config

Loader config allows overriding functionality to workaround limitations
in a limited homebrew environment.

It is an array of tuples:

`struct LoaderConfig {`  
`  LoaderConfigEntry Entries[];`  
`};`

Each entry is key-value pair:

`struct LoaderConfigEntry {`  
`  u32 Key;`  
`  u32 Flags;`  
`  u64 Value[2];`  
`};`

`enum LoaderConfigFlags {`  
`  IsMandatory = BIT(0),`  
`};`

### Loader Config Keys

A loader key can be mandatory or not mandatory.

If an (at the time) mandatory unknown key is encountered, the program
should jump to [\#LoaderReturnAddr](#LoaderReturnAddr "wikilink") with
`result_code=346 | ((100 + key) << 9);`.

If a key that is mandatory is not found (for example with an outdated
loader), use `result_code=346 | ((200 + key) << 9);`.

  - 0: [\#EndOfList](#EndOfList "wikilink") \[MANDATORY\]

<!-- end list -->

  - 1: [\#MainThreadHandle](#MainThreadHandle "wikilink") \[MANDATORY\]

<!-- end list -->

  - 2: [\#LoaderReturnAddr](#LoaderReturnAddr "wikilink") \[MANDATORY\]

<!-- end list -->

  - 3: [\#OverrideHeap](#OverrideHeap "wikilink")

<!-- end list -->

  - 4: [\#OverrideService](#OverrideService "wikilink")

<!-- end list -->

  - 5: [\#Argv](#Argv "wikilink")

<!-- end list -->

  - 6: [\#SyscallAvailableHint](#SyscallAvailableHint "wikilink")

<!-- end list -->

  - 7: [\#AppletType](#AppletType "wikilink") \[MANDATORY\]

#### EndOfList

EndOfList is the final entry in the LoaderConfig.

  - **Key:** 0
  - **IsMandatory:** True
  - **Value\[0\]:** Ignored.
  - **Value\[1\]:** Ignored.

#### MainThreadHandle

This is the handle to the thread that is executing the entrypoint.
Required for mutex to function.

  - **Key:** 1
  - **IsMandatory:** True
  - **Value\[0\]:** Handle to the main thread.
  - **Value\[1\]:** Ignored.

#### LoaderReturnAddr

When the homebrew has finished executing, it shall jump to this address
to return to the homebrew menu.

  - **Key:** 2
  - **IsMandatory:** True
  - **Value\[0\]:** Function pointer with type `void __noreturn (*)(int
    result_code);`
  - **Value\[1\]:** Ignored.
  - **DefaultBehavior:** Exits process using svcExitProcess.

#### OverrideHeap

If the NRO loader has reserved some space in the heap for itself, the
application must not manipulate the heap.

  - **Key:** 3
  - **IsMandatory:** Loader-defined.
  - **Value\[0\]:** Base address of heap. Must be MemoryType 4, 5, or 9
    with all reference counts being zero.
  - **Value\[1\]:** Size of heap.
  - **DefaultBehavior:** Allocates heap using svcSetHeapSize instead.

#### OverrideService

The NRO loader should be able to steal handles from more priliveged
processes. In this case, the homebrew should use this handle instead of
the normal one.

Homebrew should allow up to 32 service overrides.

Note: For services that authenticate with pid, the app should not
attempt re-authentication with an overridden handle.

Note: There needs be special handling for the file system service. If
the original fsp-srv handle doesn't allow MountSdcard, it shall fallback
to the stolen one, etc etc.

  - **Key:** 4
  - **IsMandatory:** Loader-defined.
  - **Value\[0\]:** Name of service, same format as for sm.
  - **Value\[1\]:** Service handle.
  - **DefaultBehavior:** Fetches service from "sm:" named port.

#### Argv

The NRO loader should be able to send argv.

  - **Key:** 5
  - **IsMandatory:** False
  - **Value\[0\]:** Argc.
  - **Value\[1\]:** Argv string pointer.
  - **DefaultBehavior:** Setting (argc == 1, argv\[0\] == "unknown",
    argv\[1\] == NULL), or argv parsed in NSO0 fashion.

#### SyscallAvailableHint

Homebrew doesn't know if any privileged syscalls are available.

This entry allows loader to give hints about having access to rare
syscalls (such as JIT).

  - **Key:** 6
  - **IsMandatory:** False
  - **Value\[0\]:** Each byte is a rare syscall number that the process
    has access to. `0xFF` means byte should be ignored.
  - **Value\[1\]:** Same as above.
  - **DefaultBehavior:** If NSO0, assume kernelhax thus all rare
    syscalls are allowed. If NRO0, assume all rare syscalls are
    inaccessible.

#### AppletType

Specifies the [AM](AM%20services.md "wikilink") AppletType, used for
selecting which Open\*Proxy command to use.

  - **Key:** 7
  - **IsMandatory:** True
  - **Value\[0\]:** AppletType
  - **Value\[1\]:** Ignored.

`enum LoaderConfigAppletType {`  
`  LoaderConfigAppletType_Application = 0,`  
`  LoaderConfigAppletType_SystemApplet = 1,`  
`  LoaderConfigAppletType_LibraryApplet = 2,`  
`  LoaderConfigAppletType_OverlayApplet = 3,`  
`  LoaderConfigAppletType_SystemApplication = 4,`  
`};`