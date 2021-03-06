## Setup Process

At the factory, a minimal version of the Switch OS is installed. A
modified version of the [boot2](Boot2.md "wikilink") title
(boot2.manuBoot) is installed that launches an additional
"[Manu](Manu%20Services.md "wikilink")" sysmodule, and the system config
title specifies to launch "Test Application Launcher" instead of
qlaunch.

Test Application Launcher is used to launch a number of tests, "CAL0"
calibration data is written to NAND, and retail firmware is installed.

## Titles

### Overview

Factory firmware contains a stripped down version of the Switch's OS
with unnecessary titles removed, and a number of additional debug titles
installed. The version of the OS installed at the factory is receiving
updates as new switches are manufactured. At least four revisions of
factory firmware are known to have been used in production.

![TestApplicationLauncher running on a
console.](TestApplicationLauncher.jpg
"TestApplicationLauncher running on a console.")

#### Removed Titles

  - The following system data archive titles are present in retail
    firmware, but not installed at the factory: 0100000000000801,
    0100000000000803, 0100000000000804, 0100000000000805,
    0100000000000808, 010000000000080A, 010000000000080B,
    010000000000080C, 010000000000080D, 010000000000081A,
    010000000000081B, 010000000000081E.

<!-- end list -->

  - Every System Applet "10XX" title is not installed.

<!-- end list -->

  - 01008BB00013C000 ("flog") is not
installed.

#### Factory-Only Titles

| Title ID         | Name                                  | Description                                                                                           |
| ---------------- | ------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| 0100000000002000 | BoardFunction                         | Board testing.                                                                                        |
| 0100000000002001 | A3Wireless                            | Wireless testing.                                                                                     |
| 0100000000002002 | C1LcdAndKey                           | LCD/Keyboard testing.                                                                                 |
| 0100000000002003 | C2UsbHpmic                            | USB testing.                                                                                          |
| 0100000000002004 | C3Aging                               | Graphics/Framerate testing.                                                                           |
| 0100000000002005 | C4SixAxis                             | Sixaxis (controller peripheral) testing.                                                              |
| 0100000000002006 | C5Wireless                            | Wireless testing.                                                                                     |
| 0100000000002007 | "FinalCheck"                          |                                                                                                       |
| 0100000000002044 | "HB-TBIntegrationTest"                |                                                                                                       |
| 010000000000204E | A4BoardCalWriti                       | Writes calibration data to NAND.                                                                      |
| 010000000000209C | TestApplication                       | "Test Application Launcher", factory qlaunch replacement. Used to launch other tests.                 |
| 010000000000B14A | [Manu](Manu%20Services.md "wikilink") | Manufacturing sysmodule.                                                                              |
| 1000000000000001 | SystemInitializ                       | Strings internally refer to this as "SystemInitializer". See [here](SystemInitializer.md "wikilink"). |
| 1000000000000004 | CalWriterManu                         | ?                                                                                                     |
| 1000000000000007 | "ApplicationLauncer"                  |                                                                                                       |
|                  |                                       |                                                                                                       |
