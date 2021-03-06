This process is launched by [NS](NS%20Services.md "wikilink") when
[PM](Process%20Manager%20services.md "wikilink") signals that there is a
crashing process.

Creport takes a string containing a pid formatted in base10 as input,
and generates an error report. This error report can later be sent to
the cloud server by [Eupld services](Eupld%20services.md "wikilink").

\[2.1.0+\]: An additional input argument string is now used. Only the
first byte is used: `inarg_flag = argv[1][0];` This is compared with '1'
only with the below titleID code, and near the end of nnMain().

## Crash dumping

It uses the [svcDebugActiveProcess](SVC.md "wikilink") to start a
debug-session for the pid. It loops
[svcGetDebugEvent](SVC.md "wikilink") to fetch all debug events.

It has a event buffer of 128 u64's that starts with
"CREP\\x01\\x00\\x00\\x00".

It only cares about type `EXCEPTION` and `ATTACH_PROCESS` events.

For `ATTACH_PROCESS` events it saves the title-id of the crashed process
(this is the first u64 of the type-specific data), and a flag whether to
dump stack.

For `EXCEPTION` events, it adds records to the buffer.

Each exception record starts with 2x u64's: exception type, and
exception address.

Then depending on exception type it stores:

  - `UNDEFINED_INSTRUCTION (0)`: Always (u64) 0.
  - `PREFETCH_ABORT (1)`: Always (u64) 0.
  - `DATA_ABORT (2)`: (u64) Fault register.
  - `UNALIGNED_ACCESS (3)`: (u64) Fault register.
  - `UNDEFINED_SYSCALL (8)`: (u64) Syscall id.
  - `? (9)`: Always (u64) 0.

For all exceptions, it then adds more data from
svcGetDebugThreadParam/svcGetDebugThreadContext. "Usually" (when is this
not the case?), this data includes a size u64 for the register dump
region + the values of all general purpose registers+SP and PC, as well
as 0x80 of stack. This reads the flag from `ATTACH_PROCESS` to determine
whether to read 0x10 bytes using svcReadDebugProcessMemory.

All other events (`USER_BREAK`, etc) don't store any extra data except
type and address.

After it has fetched all events, if it didn't encounter `USER_BREAK` it
constructs an error report:

  - Field10 "ErrorCode": (String) Error-code string formatted with
    ["%04d-%04d"](Error%20codes.md "wikilink").
  - Field115 "ProgramId": (String) Title-id snprintf'ed as "%08llx".
  - Field116 "AbortFlag": (Bool) 0.

It does \*not\* add the event buffer to the report(blacklisted) if
title-id is any of the following(swkbd and all
[web-applets](Internet%20Browser.md "wikilink") except offline-applet):

  - 0100000000001008
  - 010000000000100A
  - 010000000000100B
  - 0100000000001010
  - 0100000000001011

\[2.1.0+\] It also blacklists the creport-sysmodule title-id. Then, if
inarg\_flag(see above) is set to '1', all title-ids are blacklisted
except for the following whitelist:

  - 0100704000B3A000 "Snipperclips" (Game)
  - 01007EF00011E000 "The Legend of Zelda: Breath of the Wild"
  - 01009B500007C000 "ARMS"
  - \[2.3.0+\] 0100C5E003B40000 "ARMS Global Testpunch"
  - 0100D87002EE0000 "Snipperclips - Cut it out, together\!"
  - 0100F8F0000A2000 "Splatoon 2" (EUR)
  - 010000A00218E000 "Splatoon 2 Global Testfire"
  - 01000320000CC000 "1-2 Switch"
  - 0100152000022000 "Mario Kart 8 Deluxe"
  - 01003BC0000A0000 "Splatoon 2" (USA)
  - 01003C700009C000 "Splatoon 2" (JPN)

This is probably because of privacy concerns (software keyboard +
browser could contain passwords and personal info).

The above whitelist handling is probably so that only Nintendo
(published) applications get full exception info reported, since
crash-reports for other applications probably(?) wouldn't be shared with
third-parties.

For all other title-ids, it generates a random AES-128 key and CTR using
`csrng`.

It encrypts the entire event buffer with this AES key and CTR. Then it
encrypts the key-iv-pair using RSA-PSS with a hardcoded pubkey and
exponent `0x10001`.

These are added to the error report:

  - Field206 "EncryptionKey": (Raw) RSA-encrypted AES-key.
  - Field207 "EncryptedExceptionInfo": (Raw) Encrypted crash-info.

Thus you need the private key to decrypt the crash dump.

Finally, the error report is sent to "erpt:c" cmd1.

It uses "ns:dev" to terminate the pid. If certain checks pass, then it
throws the following fatal-err depending on exception type:

  - 0 -\> 0xA8
  - 1 -\> 0x2A8
  - 2 -\> 0x4A8,
  - 3 -\> 0x6A8,
  - 6 -\> No fatal-err
  - 8 -\> 0x10A8
  - Default: 0x4A2

## [2.1.0](2.1.0.md "wikilink")

Exactly the following code was changed:

  - nnMain(): Updated.
  - L\_1ee0(prev ver L\_1c90): Updated.
  - L\_ad34: New func, svcQueryDebugProcessMemory.
  - L\_e290: New func. "L\_12cf0(<inparams>); return inx0;"
  - L\_12cf0: New func. Only called by L\_e290.

nnMain:

  - Two input arguments are now used+required, see above.
  - During init near the start of this func, u64 val0 is now written to
    x24+32.
  - A lot of new code was added.
  - TID handling block was updated, see above.
  - The check for <is_blacklisted> was changed from
    "if(val\<=0)<branch>" to "if(val\<1)<branch>".
  - The code block at the very end of this func was updated.
      - Prev code: `if(`<loadedval>` != 1)return;` New code:
        `if(`<loadedval>`
        != 1)`<jump over the two following func calls which are the same as prev ver>
      - Following the two funcs mentioned above, prev code:
        `if(`<loadedval>` == 1 && (u8 *(ptr+1) & 1) == 0)`<call func>`;
        return;` New code: `if(inarg_flag != '1' && (u8 *(ptr+1) & 1)
        == 0 && ((u8 *(ptr+0) ^ 0x1) & 0x1) == 0)`<call func>`; return;`
      - <call func> here is throw\_fatalerr(ptr+4). The above second
        block basically changed the conditions required for throwing
        fatal-error. For example, fatal-error is no longer thrown when
        applications crash.

## [2.3.0](2.3.0.md "wikilink")

Exactly the following code was changed:

Only change was adding a titleID to the above whitelist.

## [5.0.0](5.0.0.md "wikilink")

Many changes were made to add more detail to reports. In particular:

  - The second input flag is no longer actually used, instead whether
    the process is an application is parsed from the ATTACH\_PROCESS
    debug info.
  - Support was added for reading a custom user error code from process
    memory in the UserBdsak case.
  - Support was added for reading a custom user "Dying Message" of up to
    0x1000 bytes from process memory if the crashes process was an
    application.
  - All reports now have additional info in their crash reports:
      - A list of up to 0x60 threads is retrieved via svcGetThreadList,
        and each thread has a full register dump + stacktrace added to
        the report.
      - The crashing thread's PC and LR are used to try to locate the
        base executable region that caused the crash -- if found, it and
        up to 15 code regions with higher virtual addresses have their
        start and end addresses saved, and their executable name and GNU
        build IDs read out of .rodata and added to the report. This
        fixes the problem of crash reports in previous versions not
        including information on ASLR.

## [6.1.0](6.1.0.md "wikilink")

Support was improved for detecting code regions. In particular:

  - The number of processable code regions was increased from 16 to 96.
  - Instead of processing the crashing thread's PC or LR, now both are
    processed, and additionally every address in the thread's stacktrace
    are processed.
      - If the crashed module is an application, this is further done
        for all threads.
