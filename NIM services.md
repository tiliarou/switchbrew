# nim

This is
"nn::nim::detail::INetworkInstallManager".

| Cmd | Name                                                             |
| --- | ---------------------------------------------------------------- |
| 0   | CreateSystemUpdateTask                                           |
| 1   | [\#DestroySystemUpdateTask](#DestroySystemUpdateTask "wikilink") |
| 2   | [\#ListSystemUpdateTask](#ListSystemUpdateTask "wikilink")       |
| 3   | RequestSystemUpdateTaskRun                                       |
| 4   | GetSystemUpdateTaskInfo                                          |
| 5   | CommitSystemUpdateTask                                           |
| 6   | CreateNetworkInstallTask                                         |
| 7   | DestroyNetworkInstallTask                                        |
| 8   | ListNetworkInstallTask                                           |
| 9   | RequestNetworkInstallTaskRun                                     |
| 10  | GetNetworkInstallTaskInfo                                        |
| 11  | CommitNetworkInstallTask                                         |
| 12  | RequestLatestSystemUpdateMeta                                    |
| 14  | ListApplicationNetworkInstallTask                                |
| 15  | ListNetworkInstallTaskContentMeta                                |
| 16  | RequestLatestVersion                                             |
| 17  | SetNetworkInstallTaskAttribute                                   |
| 18  | AddNetworkInstallTaskContentMeta                                 |
| 19  | GetDownloadedSystemDataPath                                      |
| 20  | CalculateNetworkInstallTaskRequiredSize                          |
| 21  | IsExFatDriverIncluded                                            |
| 22  | GetBackgroundDownloadStressTaskInfo                              |
| 23  | \[2.0.0+\] RequestDeviceAuthenticationToken                      |
| 24  | \[2.0.0+\] RequestGameCardRegistrationStatus                     |
| 25  | \[2.0.0+\] RequestRegisterGameCard                               |
| 26  | \[2.0.0+\] RequestRegisterNotificationToken                      |
| 27  | \[2.0.0+\] RequestDownloadTaskList                               |
| 28  | \[2.0.0+\] RequestApplicationControl                             |
| 29  | \[2.0.0+\] RequestLatestApplicationControl                       |
| 30  | \[2.0.0+\] RequestVersionList                                    |
| 31  | \[2.0.0+\] CreateApplyDeltaTask                                  |
| 32  | \[2.0.0+\] DestroyApplyDeltaTask                                 |
| 33  | \[2.0.0+\] ListApplicationApplyDeltaTask                         |
| 34  | \[2.0.0+\] RequestApplyDeltaTaskRun                              |
| 35  | \[2.0.0+\] GetApplyDeltaTaskInfo                                 |
| 36  | \[2.0.0+\] ListApplyDeltaTask                                    |
| 37  | \[2.0.0+\] CommitApplyDeltaTask                                  |
| 38  | \[2.0.0+\] CalculateApplyDeltaTaskRequiredSize                   |
| 39  | \[2.0.0+\] PrepareShutdown                                       |
| 40  | \[2.0.0+\] ListApplyDeltaTask2                                   |
| 41  | \[2.0.0+\] ClearNotEnoughSpaceStateOfApplyDeltaTask              |
| 42  | \[3.0.0+\]                                                       |
| 43  | \[3.0.0+\]                                                       |
| 44  | \[3.0.0+\]                                                       |
| 45  | \[3.0.0+\]                                                       |
| 46  | \[3.0.0+\]                                                       |

## DestroySystemUpdateTask

Takes a 0x10-byte input struct, from the output of ListSystemUpdateTask.

## ListSystemUpdateTask

Takes a type-0x6 output buffer, for an array of 0x10-byte entries.
Returns an u32 for total output entries.

Doesn't return anything on a v2.1 system where sysupdate domains are
blocked.

# nim:shp

This is "nn::nim::detail::IShopServiceManager".

| Cmd | Name                                                    |
| --- | ------------------------------------------------------- |
| 0   | RequestDeviceAuthenticationToken                        |
| 1   | RequestCachedDeviceAuthenticationToken                  |
| 100 | RequestRegisterDeviceAccount                            |
| 101 | RequestUnregisterDeviceAccount                          |
| 102 | RequestDeviceAccountStatus                              |
| 103 | GetDeviceAccountInfo                                    |
| 104 | RequestDeviceRegistrationInfo                           |
| 105 | RequestTransferDeviceAccount                            |
| 106 | RequestSyncRegistration                                 |
| 107 | IsOwnDeviceId                                           |
| 200 | RequestRegisterNotificationToken                        |
| 300 | RequestUnlinkDevice                                     |
| 301 | RequestUnlinkDeviceIntegrated                           |
| 302 | RequestLinkDevice                                       |
| 303 | HasDeviceLink                                           |
| 304 | RequestUnlinkDeviceAll                                  |
| 305 | RequestCreateVirtualAccount                             |
| 306 | RequestDeviceLinkStatus                                 |
| 400 | GetAccountByVirtualAccount                              |
| 500 | RequestSyncTicket                                       |
| 501 | RequestDownloadTicket                                   |
| 502 | \[4.0.0+\] RequestDownloadTicketForPrepurchasedContents |

# nim:eca

This is "nn::nim::detail::IShopServiceAccessServerInterface".

| Cmd | Name                     |
| --- | ------------------------ |
| 0   | CreateServerInterface    |
| 1   | RefreshDebugAvailability |
| 2   | ClearDebugResponse       |
| 3   | RegisterDebugResponse    |

# ntc

This is "nn::ntc::detail::<service::IStaticService>".

| Cmd | Name                                      |
| --- | ----------------------------------------- |
| 0   | OpenEnsureNetworkClockAvailabilityService |
| 100 | SuspendAutonomicTimeCorrection            |
| 101 | ResumeAutonomicTimeCorrection             |

Network-time-sync uses the "\<...\>/time" HTTPS URL listed in the below
Network section. This just returns an UTC Unix time string.

## IEnsureNetworkClockAvailabilityService

This is
"nn::ntc::detail::<service::IEnsureNetworkClockAvailabilityService>".

| Cmd | Name                       |
| --- | -------------------------- |
| 0   | StartTask                  |
| 1   | GetFinishNotificationEvent |
| 2   | GetResult                  |
| 3   | Cancel                     |
| 4   | IsProcessing               |
| 5   | GetServerTime              |

[Category:Services](Category:Services "wikilink")
