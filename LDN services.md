LDN handles all local network communication.

# ldn:m

This is "nn::ldn::detail::IMonitorServiceCreator".

| Cmd | Name                                                       |
| --- | ---------------------------------------------------------- |
| 0   | [\#CreateMonitorService](#CreateMonitorService "wikilink") |
|     |                                                            |

## CreateMonitorService

Returns an [\#IMonitorService](#IMonitorService "wikilink").

## IMonitorService

This is "nn::ldn::detail::IMonitorService".

| Cmd | Name                           |
| --- | ------------------------------ |
| 0   | GetStateForMonitor             |
| 1   | GetNetworkInfoForMonitor       |
| 2   | GetIpv4AddressForMonitor       |
| 3   | GetDisconnectReasonForMonitor  |
| 4   | GetSecurityParameterForMonitor |
| 5   | GetNetworkConfigForMonitor     |
| 100 | InitializeMonitor              |
| 101 | FinalizeMonitor                |
|     |                                |

# ldn:s

This is
"nn::ldn::detail::ISystemServiceCreator".

| Cmd | Name                                                                                         |
| --- | -------------------------------------------------------------------------------------------- |
| 0   | [\#CreateSystemLocalCommunicationService](#CreateSystemLocalCommunicationService "wikilink") |
|     |                                                                                              |

## CreateSystemLocalCommunicationService

Returns an
[\#ISystemLocalCommunicationService](#ISystemLocalCommunicationService "wikilink").

## ISystemLocalCommunicationService

This is "nn::ldn::detail::ISystemLocalCommunicationService".

| Cmd | Name                       |
| --- | -------------------------- |
| 0   | GetState                   |
| 1   | GetNetworkInfo             |
| 2   | GetIpv4Address             |
| 3   | GetDisconnectReason        |
| 4   | GetSecurityParameter       |
| 5   | GetNetworkConfig           |
| 100 | AttachStateChangeEvent     |
| 101 | GetNetworkInfoLatestUpdate |
| 102 | Scan                       |
| 103 | ScanPrivate                |
| 200 | OpenAccessPoint            |
| 201 | CloseAccessPoint           |
| 202 | CreateNetwork              |
| 203 | CreateNetworkPrivate       |
| 204 | DestroyNetwork             |
| 205 | Reject                     |
| 206 | SetAdvertiseData           |
| 207 | SetStationAcceptPolicy     |
| 208 | AddAcceptFilterEntry       |
| 209 | ClearAcceptFilter          |
| 300 | OpenStation                |
| 301 | CloseStation               |
| 302 | Connect                    |
| 303 | ConnectPrivate             |
| 304 | Disconnect                 |
| 400 | InitializeSystem           |
| 401 | FinalizeSystem             |
|     |                            |

# ldn:u

This is
"nn::ldn::detail::IUserServiceCreator".

| Cmd | Name                                                                                     |
| --- | ---------------------------------------------------------------------------------------- |
| 0   | [\#CreateUserLocalCommunicationService](#CreateUserLocalCommunicationService "wikilink") |
|     |                                                                                          |

## CreateUserLocalCommunicationService

Returns an
[\#IUserLocalCommunicationService](#IUserLocalCommunicationService "wikilink").

## IUserLocalCommunicationService

This is "nn::ldn::detail::IUserLocalCommunicationService".

| Cmd | Name                       |
| --- | -------------------------- |
| 0   | GetState                   |
| 1   | GetNetworkInfo             |
| 2   | GetIpv4Address             |
| 3   | GetDisconnectReason        |
| 4   | GetSecurityParameter       |
| 5   | GetNetworkConfig           |
| 100 | AttachStateChangeEvent     |
| 101 | GetNetworkInfoLatestUpdate |
| 102 | Scan                       |
| 103 | ScanPrivate                |
| 200 | OpenAccessPoint            |
| 201 | CloseAccessPoint           |
| 202 | CreateNetwork              |
| 203 | CreateNetworkPrivate       |
| 204 | DestroyNetwork             |
| 205 | Reject                     |
| 206 | SetAdvertiseData           |
| 207 | SetStationAcceptPolicy     |
| 208 | AddAcceptFilterEntry       |
| 209 | ClearAcceptFilter          |
| 300 | OpenStation                |
| 301 | CloseStation               |
| 302 | Connect                    |
| 303 | ConnectPrivate             |
| 304 | Disconnect                 |
| 400 | Initialize                 |
| 401 | Finalize                   |
|     |                            |

[Category:Services](Category:Services "wikilink")
