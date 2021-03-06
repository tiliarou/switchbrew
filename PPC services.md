APM is utilized for setting system performance profiles including clocks
for CPU, GPU, and memory.

# apm

This is
"nn::apm::IManager".

| Cmd | Name               | Notes                                          |
| --- | ------------------ | ---------------------------------------------- |
| 0   | OpenSession        | Returns an [\#ISession](#ISession "wikilink"). |
| 1   | GetPerformanceMode |                                                |

# apm:p

This is "nn::apm::IManagerPrivileged".

| Cmd | Name        | Notes                                          |
| --- | ----------- | ---------------------------------------------- |
| 0   | OpenSession | Returns an [\#ISession](#ISession "wikilink"). |

# apm:sys

This is
"nn::apm::ISystemManager".

| Cmd | Name                            | Notes                                          |
| --- | ------------------------------- | ---------------------------------------------- |
| 0   | RequestPerformanceMode          |                                                |
| 1   | GetPerformanceEvent             | Returns an [\#ISession](#ISession "wikilink"). |
| 2   | GetThrottlingState              |                                                |
| 3   | GetLastThrottlingState          |                                                |
| 4   | ClearLastThrottlingState        |                                                |
| 5   | \[5.0.0+\] LoadAndApplySettings |                                                |

# ISession

This is
"nn::apm::ISession".

| Cmd | Name                        | Notes                                                                   |
| --- | --------------------------- | ----------------------------------------------------------------------- |
| 0   | SetPerformanceConfiguration | Takes u32 PerformanceMode and u32 PerformanceConfiguration.             |
| 1   | GetPerformanceConfiguration | Takes u32 PerformanceMode, returns output u32 PerformanceConfiguration. |

## PerformanceMode

| Value | Name     |
| ----- | -------- |
| 0     | Handheld |
| 1     | Docked   |

## PerformanceConfiguration

| Value      | CPU clock | GPU clock | Memory clock |
| ---------- | --------- | --------- | ------------ |
| 0x00010000 | 1020      | 384       | 1600         |
| 0x00010001 | 1020      | 768       | 1600         |
| 0x00010002 | 1224      | 691.2     | 1600         |
| 0x00020000 | 1020      | 230.4     | 1600         |
| 0x00020001 | 1020      | 307.2     | 1600         |
| 0x00020002 | 1224      | 230.4     | 1600         |
| 0x00020003 | 1020      | 307       | 1331.2       |
| 0x00020004 | 1020      | 384       | 1331.2       |
| 0x00020005 | 1020      | 307.2     | 1065.6       |
| 0x00020006 | 1020      | 384       | 1065.6       |
| 0x92220007 | 1020      | 460.8     | 1600         |
| 0x92220008 | 1020      | 460.8     | 1331.2       |

Clocks are all in MHz.

Configurations 0x10001 and 0x20000 are only available while docked.
Configurations 0x10002 and 0x20002 are only available for SDEV units.

Some of these require apm:p.

# fgm, fgm:0, fgm:9

This is "nn::fgm::sf::ISession".

| Cmd | Name       |
| --- | ---------- |
| 0   | Initialize |

## IRequest

This is "nn::fgm::sf::IRequest".

| Cmd | Name       |
| --- | ---------- |
| 0   | Initialize |
| 1   | Set        |
| 2   | Get        |
| 3   | Cancel     |

# fgm:dbg

This is "nn::fgm::sf::IDebugger".

| Cmd | Name       |
| --- | ---------- |
| 0   | Initialize |
| 1   | Read       |
| 2   | Cancel     |

# Settings

| Key                                          | Size   | Notes                                                                                                           |
| -------------------------------------------- | ------ | --------------------------------------------------------------------------------------------------------------- |
| battery\_draining\_enabled                   | 1      | ?                                                                                                               |
| performance\_mode\_policy                    | strlen | "auto": use boost mode when docked. "normal": never use boost mode. "boost": always use boost mode (SDEV only). |
| sdev\_cpu\_overclock\_enabled                | 1      | SDEV only. Used to allow access to 1224MHz CPU mode.                                                            |
| sdev\_throttling\_additional\_delay\_us      | 4      | SDEV only.                                                                                                      |
| sdev\_throttling\_additional\_delay\_enabled | 1      | SDEV only.                                                                                                      |
| throttling\_for\_smpd\_enabled               | 1      | Not used as of 3.0.                                                                                             |
| throttling\_for\_undock\_enabled             | 1      | Not used as of 3.0.                                                                                             |

[Category:Services](Category:Services "wikilink")
