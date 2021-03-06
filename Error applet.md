The error applet is used to display an error result by several titles
and applets. (AppletId 0xE)

Depending on the type, the applet will display the error in several
ways.

It takes two input storages: common arguments and a custom storage.

## Error applet types

### Common

For all of the following structures, the first two bytes are common and
identify mode.

| Offset | Size | Typical Value | Notes                             |
| ------ | ---- | ------------- | --------------------------------- |
| 0x0    | 1    | 0             | Error Applet Mode                 |
| 0x1    | 1    | 1             | Whether or not to use 'jump' mode |

In SDK, the jump mode byte is always one, except for
nn::err::ShowErrorWithoutJump. HW-Testing has shown this to have no
visual impact on error applet.

### Error (common one)

Takes a CommonArgs storage with version 0. Uses a mode byte of 0 in the
above struct.

#### Custom storage

Unknown exact size, using size 20 seems to work fine.

The type (byte 0 of this storage) is
0.

| Offset | Size    | Typical Value | Notes                                                 |
| ------ | ------- | ------------- | ----------------------------------------------------- |
| 0x10   | 4 (u32) | 0 (2000-0000) | Result code, same Result type used everywhere in HOS. |

The error will display error code 2000-0000 if the Result is not set.
The text is the default one.

### SystemError

Takes a CommonArgs storage with version 0. Uses a mode byte of 1 in the
above struct.

#### Custom storage

SDK uses size 4120 for this storage.

The type (byte 0 of this storage) is
1.

| Offset | Size    | Typical Value | Notes                                                                       |
| ------ | ------- | ------------- | --------------------------------------------------------------------------- |
| 0x8    | 8 (u64) | 0 (0000-0000) | ErrorCode (not Result like in normal errors) of the error to use.           |
| 0x18   | 0x800   | \-            | String of the text to be shown as a short description of the error.         |
| 0x818  | 0x800   | \-            | String of the text to be shown as a more detailed description of the error. |

If the first text is not specified but the second one is, the applet
will directly load the "Details" display with the second error text.

If both texts are set, the applet will load the dialog with "Close" and
"Details" options, and will load the display mentiones above after
selecting "Details".

If no ErrorCode is supplied, the applet will use 0000-0000.

### ApplicationError

Allows an application to show a custom-defined error text. Uses a mode
byte of 2 in the above struct.

The SDK uses size 4116 (0x1014) for this
storage.

| Offset | Size    | Typical Value | Notes                                                                                      |
| ------ | ------- | ------------- | ------------------------------------------------------------------------------------------ |
| 0x8    | 4 (u32) | 0 (0000-0000) | ErrorCode of the error to use.                                                             |
| 0xC    | 8 (u64) | 'en-US\\0'    | The Language of the following text. This uses the same language names as settings service. |
| 0x14   | 0x800   | \-            | String of the text to be shown as a short description of the error.                        |
| 0x814  | 0x800   | \-            | String of the text to be shown as a more detailed description of the error.                |

On hardware, the short description will be displayed initially in a
dialog box with the error code. There is a details button that when
clicked will display the detailed description.

### ShowErrorRecord (ShowError with Timestamp)

Uses common arguments version 0 and a mode byte of 5.

Performs the same function as ShowError, but also displays the date and
time of
error.

| Offset | Size    | Typical Value               | Notes                                                                                                  |
| ------ | ------- | --------------------------- | ------------------------------------------------------------------------------------------------------ |
| 0x8    | 8 (u64) | 0 (0000-0000)               | Result code to use. This is formatted with the first u32 = description and second u32 = module + 2000. |
| 0x10   | 8 (u64) | 0 (Jan 1, 1970 00:00:00 AM) | POSIX time of the error to be displayed (as seconds since epoch).                                      |

Unlike ShowError, this is fullscreen and not a dialog.

[Category:Library Applets](Category:Library_Applets "wikilink")
