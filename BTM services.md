# btm

This is "nn::btm::IBtm".

| Cmd | Name                                           |
| --- | ---------------------------------------------- |
| 0   |                                                |
| 1   |                                                |
| 2   | RegisterSystemEventForConnectedDeviceCondition |
| 3   |                                                |
| 4   |                                                |
| 5   |                                                |
| 6   |                                                |
| 7   |                                                |
| 8   | RegisterSystemEventForRegisteredDeviceInfo     |
| 9   |                                                |
| 10  |                                                |
| 11  |                                                |
| 12  |                                                |
| 13  |                                                |
| 14  | EnableRadio                                    |
| 15  | DisableRadio                                   |
| 16  |                                                |
| 17  |                                                |
| 18  | \[2.0.0+\]                                     |
| 19  | \[4.0.0+\]                                     |
| 20  | \[4.0.0+\]                                     |
| 21  | \[4.0.0+\]                                     |
| 22  | \[5.0.0+\]                                     |
| 23  | \[5.0.0+\]                                     |
| 24  | \[5.0.0+\]                                     |
| 25  | \[5.0.0+\]                                     |
| 26  | \[5.0.0+\]                                     |
| 27  | \[5.0.0+\]                                     |
| 28  | \[5.0.0+\]                                     |
| 29  | \[5.0.0+\]                                     |
| 30  | \[5.0.0+\]                                     |
| 31  | \[5.0.0+\]                                     |
| 32  | \[5.0.0+\]                                     |
| 33  | \[5.0.0+\]                                     |
| 34  | \[5.0.0+\]                                     |
| 35  | \[5.0.0+\]                                     |
| 36  | \[5.0.0+\]                                     |
| 37  | \[5.0.0+\]                                     |
| 38  | \[5.0.0+\]                                     |
| 39  | \[5.0.0+\]                                     |
| 40  | \[5.0.0+\]                                     |
| 41  | \[5.0.0+\]                                     |
| 42  | \[5.0.0+\]                                     |
| 43  | \[5.1.0+\]                                     |
| 44  | \[5.1.0+\]                                     |
| 45  | \[5.1.0+\]                                     |
| 46  | \[5.1.0+\]                                     |
| 47  | \[5.1.0+\]                                     |
| 48  | \[5.1.0+\]                                     |
| 49  | \[5.1.0+\]                                     |
| 50  | \[5.1.0+\]                                     |
| 51  | \[5.1.0+\]                                     |
| 52  | \[5.1.0+\]                                     |
| 53  | \[5.1.0+\]                                     |
| 54  | \[5.1.0+\]                                     |
| 55  | \[5.1.0+\]                                     |
| 56  | \[5.1.0+\]                                     |
| 57  | \[5.1.0+\]                                     |
| 58  | \[5.1.0+\]                                     |
| 59  | \[5.1.0+\]                                     |

# btm:dbg

This is "nn::btm::IBtmDebug".

| Cmd | Name                            |
| --- | ------------------------------- |
| 0   | RegisterSystemEventForDiscovery |
| 1   |                                 |
| 2   |                                 |
| 3   |                                 |
| 4   |                                 |
| 5   |                                 |
| 6   |                                 |
| 7   |                                 |
| 8   |                                 |
| 9   | \[5.0.0+\]                      |
| 10  | \[5.1.0+\]                      |
| 11  | \[5.1.0+\]                      |
| 12  | \[5.1.0+\]                      |

# btm:sys

This is "nn::btm::IBtmSystem".

| Cmd | Name    |
| --- | ------- |
| 0   | GetCore |

## IBtmSystemCore

This is "nn::btm::IBtmSystemCore".

| Cmd | Name                                  |
| --- | ------------------------------------- |
| 0   | StartGamepadPairing                   |
| 1   | CancelGamepadPairing                  |
| 2   | ClearGamepadPairingDatabase           |
| 3   | GetPairedGamepadCount                 |
| 4   | EnableRadio                           |
| 5   | DisableRadio                          |
| 6   | GetRadioOnOff                         |
| 7   | \[2.0.0+\] AcquireRadioEvent          |
| 8   | \[2.0.0+\] AcquireGamepadPairingEvent |
| 9   | \[2.0.0+\] IsGamepadPairingStarted    |

# btm:u

This is "nn::btm::IBtmUser".

| Cmd | Name    |
| --- | ------- |
| 0   | GetCore |

## IBtmUserCore

This is "nn::btm::IBtmUserCore".

| Cmd | Name                            |
| --- | ------------------------------- |
| 0   | AcquireBleScanEvent             |
| 1   | GetBleScanFilterParameter       |
| 2   | GetBleScanFilterParameter2      |
| 3   | StartBleScanForGeneral          |
| 4   | StopBleScanForGeneral           |
| 5   | GetBleScanResultsForGeneral     |
| 6   | StartBleScanForPaired           |
| 7   | StopBleScanForPaired            |
| 8   | StartBleScanForSmartDevice      |
| 9   | StopBleScanForSmartDevice       |
| 10  | GetBleScanResultsForSmartDevice |
| 17  | AcquireBleConnectionEvent       |
| 18  | BleConnect                      |
| 19  | BleDisconnect                   |
| 20  | BleGetConnectionState           |
| 21  | AcquireBlePairingEvent          |
| 22  | BlePairDevice                   |
| 23  | BleUnPairDevice                 |
| 24  | BleUnPairDevice2                |
| 25  | BleGetPairedDevices             |
| 26  | AcquireBleServiceDiscoveryEvent |
| 27  | GetGattServices                 |
| 28  | GetGattService                  |
| 29  | GetGattIncludedServices         |
| 30  | GetBelongingGattService         |
| 31  | GetGattCharacteristics          |
| 32  | GetGattDescriptors              |
| 33  | AcquireBleMtuConfigEvent        |
| 34  | ConfigureBleMtu                 |
| 35  | GetBleMtu                       |
| 36  | RegisterBleGattDataPath         |
| 37  | UnregisterBleGattDataPath       |

[Category:Services](Category:Services "wikilink")
