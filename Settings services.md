# set

This is
"nn::settings::ISettingsServer".

| Cmd | Name                                      | Notes                                                                                                                  |
| --- | ----------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| 0   | GetLanguageCode                           | No input, returns an output [\#LanguageCode](#LanguageCode "wikilink"). This is the current system language.           |
| 1   | GetAvailableLanguageCodes                 | Takes a type-0xA buffer containing the [\#LanguageCode](#LanguageCode "wikilink") output array, returns an output s32. |
| 2   | \[4.0.0+\] MakeLanguageCode               | Takes an input [\#Language](#Language "wikilink"), returns an output [\#LanguageCode](#LanguageCode "wikilink").       |
| 3   | GetAvailableLanguageCodeCount             | No input, returns an output s32.                                                                                       |
| 4   | GetRegionCode                             | No input, returns a [\#RegionCode](#RegionCode "wikilink").                                                            |
| 5   | \[4.0.0+\] GetAvailableLanguageCodes2     | Takes a type-0x6 buffer containing the [\#LanguageCode](#LanguageCode "wikilink") output array, returns an output s32. |
| 6   | \[4.0.0+\] GetAvailableLanguageCodeCount2 | No input, returns an output s32.                                                                                       |
| 7   | \[4.0.0+\] GetKeyCodeMap                  |                                                                                                                        |
| 8   | \[5.0.0+\] GetQuestFlag                   | Identical to "set:sys" [GetQuestFlag](#set:sys "wikilink").                                                            |

\[4.0.0+\] Official user-processes now use
GetAvailableLanguageCodes2/GetAvailableLanguageCodeCount2 instead of
{original commands}.

In official user-processes in the
[\#Language](#Language "wikilink")-\>[\#LanguageCode](#LanguageCode "wikilink")
conversion function (MakeLanguageCode):

  - During one-time init, GetAvailableLanguageCodes is used to
    initialize the LanguageCodes array cache, with max\_entries=0xF
    (buffer size in u64s). \[4.0.0+\] GetAvailableLanguageCodes2 is now
    used with max\_entries 0x40.
  - \[4.0.0+\] When the input [\#Language](#Language "wikilink") is
    larger than the cached total\_entries from the above command output,
    or [\#Language](#Language "wikilink") is negative, command
    MakeLanguageCode is used instead of the array.

## GetKeyCodeMap

Takes a type-0x16 output buffer containing KeyCodeMap, official sw uses
fixed size 0x1000. This is probably related to HID keyboard.

## Language

"nn::settings::Language" (s32) is basically array indices in the output
array from GetAvailableLanguageCodes.

## LanguageCode

This is "nn::settings::LanguageCode".

This is an u64, which is a NUL-terminated
string.

| Array-index / [\#Language](#Language "wikilink") | [\#LanguageCode](#LanguageCode "wikilink") | Icon [language](NCA%20Content%20FS#FS-type3.md##FS-type3 "wikilink") filename |
| ------------------------------------------------ | ------------------------------------------ | ----------------------------------------------------------------------------- |
| 0                                                | ja                                         | "Japanese"                                                                    |
| 1                                                | en-US                                      | "AmericanEnglish"                                                             |
| 2                                                | fr                                         | "French"                                                                      |
| 3                                                | de                                         | "German"                                                                      |
| 4                                                | it                                         | "Italian"                                                                     |
| 5                                                | es                                         | "Spanish"                                                                     |
| 6                                                | zh-CN                                      | "Chinese"                                                                     |
| 7                                                | ko                                         | "Korean"                                                                      |
| 8                                                | nl                                         | "Dutch"                                                                       |
| 9                                                | pt                                         | "Portuguese"                                                                  |
| 10                                               | ru                                         | "Russian"                                                                     |
| 11                                               | zh-TW                                      | "Taiwanese"                                                                   |
| 12                                               | en-GB                                      | "BritishEnglish"                                                              |
| 13                                               | fr-CA                                      | "CanadianFrench"                                                              |
| 14                                               | es-419                                     | "LatinAmericanSpanish"                                                        |
| \[4.0.0+\] 15                                    | zh-Hans                                    | "SimplifiedChinese"                                                           |
| \[4.0.0+\] 16                                    | zh-Hant                                    | "TraditionalChinese"                                                          |

## RegionCode

A region code is a signed 32-bit value representing a particular region.
Currently the available regions defined by the system are as follows:

| Value           | Region                       |
| --------------- | ---------------------------- |
| 0               | Japan                        |
| 1               | USA                          |
| 2               | Europe                       |
| 3               | Australia                    |
| 4               | China                        |
| 5               | Korea                        |
| 6               | Taiwan                       |
| Any other value | Considered an unknown region |
|                 |                              |

# set:fd

This is "nn::settings::IFirmwareDebugSettingsServer".

\[4.0.0+\] Only exposed if in [debug
mode](SPL%20services#IsDebugMode.md##IsDebugMode "wikilink").

| Cmd | Name                           | Notes                                                                          |
| --- | ------------------------------ | ------------------------------------------------------------------------------ |
| 2   | SetSettingsItemValue           |                                                                                |
| 3   | ResetSettingsItemValue         |                                                                                |
| 4   | CreateSettingsItemKeyIterator  | Returns an [\#ISettingsItemKeyIterator](#ISettingsItemKeyIterator "wikilink"). |
| 10  | \[4.0.0+\] ReadSettings        |                                                                                |
| 11  | \[4.0.0+\] ResetSettings       |                                                                                |
| 20  | \[4.0.0+\] SetWebInspectorFlag |                                                                                |
| 21  | \[4.0.0+\] SetAllowedSslHosts  |                                                                                |
| 22  | \[4.0.0+\] SetHostFsMountPoint |                                                                                |

## ISettingsItemKeyIterator

| Cmd | Name       |
| --- | ---------- |
| 0   | GoNext     |
| 1   | GetKeySize |
| 2   | GetKey     |

# set:cal

This is
"nn::settings::IFactorySettingsServer".

| Cmd | Name                                                                          |
| --- | ----------------------------------------------------------------------------- |
| 0   | GetBluetoothBdAddress                                                         |
| 1   | GetConfigurationId1                                                           |
| 2   | GetAccelerometerOffset                                                        |
| 3   | GetAccelerometerScale                                                         |
| 4   | GetGyroscopeOffset                                                            |
| 5   | GetGyroscopeScale                                                             |
| 6   | GetWirelessLanMacAddress                                                      |
| 7   | GetWirelessLanCountryCodeCount                                                |
| 8   | GetWirelessLanCountryCodes                                                    |
| 9   | GetSerialNumber                                                               |
| 10  | SetInitialSystemAppletProgramId                                               |
| 11  | SetOverlayDispProgramId                                                       |
| 12  | GetBatteryLot                                                                 |
| 14  | [\#GetEciDeviceCertificate](#GetEciDeviceCertificate "wikilink")              |
| 15  | [\#GetEticketDeviceCertificate](#GetEticketDeviceCertificate "wikilink")      |
| 16  | [\#GetSslKey](#GetSslKey "wikilink")                                          |
| 17  | [\#GetSslCertificate](#GetSslCertificate "wikilink")                          |
| 18  | [\#GetGameCardKey](#GetGameCardKey "wikilink")                                |
| 19  | [\#GetGameCardCertificate](#GetGameCardCertificate "wikilink")                |
| 20  | [\#GetEciDeviceKey](#GetEciDeviceKey "wikilink")                              |
| 21  | [\#GetEticketDeviceKey](#GetEticketDeviceKey "wikilink")                      |
| 22  | GetSpeakerParameter                                                           |
| 23  | \[4.0.0+\] GetLcdVendorId                                                     |
| 24  | \[5.0.0+\] [\#GetEciDeviceCertificate2](#GetEciDeviceCertificate2 "wikilink") |
| 25  | \[5.0.0+\] [\#GetEciDeviceKey2](#GetEciDeviceKey2 "wikilink")                 |
| 26  | \[5.0.0+\] GetAmiiboKey                                                       |
| 27  | \[5.0.0+\] GetAmiiboEcqvCertificate                                           |
| 28  | \[5.0.0+\] GetAmiiboEcdsaCertificate                                          |
| 29  | \[5.0.0+\] GetAmiiboEcqvBlsKey                                                |
| 30  | \[5.0.0+\] GetAmiiboEcqvBlsCertificate                                        |
| 31  | \[5.0.0+\] GetAmiiboEcqvBlsRootCertificate                                    |
| 32  | \[5.0.0+\] GetUsbTypeCPowerSourceCircuitVersion                               |
| 33  | \[6.0.0+\] GetBatteryVersion                                                  |

Used for accessing data calibrated at the factory.

## GetEciDeviceCertificate

Takes a type-0x16 output buffer with fixed size 0x180.

Returns the device certificate (ECC signed). This is identical to 3DS
DeviceCert/CTCert besides the strings. NIM loads the DeviceId from this.

## GetEticketDeviceCertificate

Takes a type-0x16 output buffer with fixed size 0x240.

Returns the ETicket certificate (RSA signed).

## GetSslKey

Takes a type-0x16 output buffer with fixed size 0x134.

Returns the extended SSL key (0x130 bytes) from
[CAL0](Calibration#CAL0.md##CAL0 "wikilink"). If the extended key is not
programmed then it falls back to the normal SSL key (0x110 bytes).

Used by SSL-sysmodule, see [here](SSL%20services.md "wikilink").

## GetSslCertificate

Takes a type-0x16 output buffer with fixed size 0x804.

Returns a
[container](Settings%20services#setcal%20Container%20Structure.md##setcal_Container_Structure "wikilink")
with the plaintext SSL certificate.

Used by SSL-sysmodule, see [here](SSL%20services.md "wikilink").

## GetGameCardKey

Takes a type-0x16 output buffer with fixed size 0x134.

Returns the extended GameCard key (0x130 bytes) from
[CAL0](Calibration#CAL0.md##CAL0 "wikilink"). If the extended key is not
programmed then it falls back to the normal GameCard key (0x110 bytes).

## GetGameCardCertificate

Takes a type-0x16 output buffer with fixed size 0x404.

Returns a
[container](Settings%20services#setcal%20Container%20Structure.md##setcal_Container_Structure "wikilink")
with the GameCard certificate.

## GetEciDeviceKey

Returns the extended device ECC-B233 key (0x50 bytes) from
[CAL0](Calibration#CAL0.md##CAL0 "wikilink"). If the extended key is not
programmed then it falls back to the normal device ECC-B233 key (0x30
bytes).

## GetEticketDeviceKey

Takes a type-0x16 output buffer with fixed size 0x244.

Returns the extended ETicket RSA-2048 key (0x240 bytes) from
[CAL0](Calibration#CAL0.md##CAL0 "wikilink"). If the extended key is not
programmed then it falls back to the normal ETicket RSA-2048 key (0x220
bytes).

## GetEciDeviceCertificate2

Same as
[\#GetEciDeviceCertificate](#GetEciDeviceCertificate "wikilink"), but
returns a RSA-2048 variant of the device certificate.

## GetEciDeviceKey2

Same as [\#GetEciDeviceKey](#GetEciDeviceKey "wikilink"), but returns a
RSA-2048 variant of the device
key.

## setcal Container Structure

| Offset | Size         | Name                                           |
| ------ | ------------ | ---------------------------------------------- |
| 0x0    | 0x4          | Size (same size used for decryption if needed) |
| 0x4    | {above size} | Actual data starts here.                       |

This container is used for returning data with variable sizes.

# set:sys

This is
"nn::settings::ISystemSettingsServer".

| Cmd | Name                                                                                              |
| --- | ------------------------------------------------------------------------------------------------- |
| 0   | SetLanguageCode                                                                                   |
| 1   | SetNetworkSettings                                                                                |
| 2   | GetNetworkSettings                                                                                |
| 3   | [\#GetFirmwareVersion](#GetFirmwareVersion "wikilink")                                            |
| 4   | \[3.0.0+\] GetFirmwareVersion2                                                                    |
| 5   | \[5.0.0+\] GetFirmwareVersionDigest                                                               |
| 7   | GetLockScreenFlag                                                                                 |
| 8   | SetLockScreenFlag                                                                                 |
| 9   | GetBacklightSettings                                                                              |
| 10  | SetBacklightSettings                                                                              |
| 11  | SetBluetoothDevicesSettings                                                                       |
| 12  | GetBluetoothDevicesSettings                                                                       |
| 13  | GetExternalSteadyClockSourceId                                                                    |
| 14  | SetExternalSteadyClockSourceId                                                                    |
| 15  | GetUserSystemClockContext                                                                         |
| 16  | SetUserSystemClockContext                                                                         |
| 17  | GetAccountSettings                                                                                |
| 18  | SetAccountSettings                                                                                |
| 19  | GetAudioVolume                                                                                    |
| 20  | SetAudioVolume                                                                                    |
| 21  | GetEulaVersions                                                                                   |
| 22  | SetEulaVersions                                                                                   |
| 23  | [\#GetColorSetId](#GetColorSetId "wikilink")                                                      |
| 24  | [\#SetColorSetId](#SetColorSetId "wikilink")                                                      |
| 25  | GetConsoleInformationUploadFlag                                                                   |
| 26  | SetConsoleInformationUploadFlag                                                                   |
| 27  | GetAutomaticApplicationDownloadFlag                                                               |
| 28  | SetAutomaticApplicationDownloadFlag                                                               |
| 29  | GetNotificationSettings                                                                           |
| 30  | SetNotificationSettings                                                                           |
| 31  | GetAccountNotificationSettings                                                                    |
| 32  | SetAccountNotificationSettings                                                                    |
| 35  | GetVibrationMasterVolume                                                                          |
| 36  | SetVibrationMasterVolume                                                                          |
| 37  | GetSettingsItemValueSize                                                                          |
| 38  | [\#GetSettingsItemValue](#GetSettingsItemValue "wikilink")                                        |
| 39  | GetTvSettings                                                                                     |
| 40  | SetTvSettings                                                                                     |
| 41  | GetEdid                                                                                           |
| 42  | SetEdid                                                                                           |
| 43  | GetAudioOutputMode                                                                                |
| 44  | SetAudioOutputMode                                                                                |
| 45  | IsForceMuteOnHeadphoneRemoved                                                                     |
| 46  | SetForceMuteOnHeadphoneRemoved                                                                    |
| 47  | [\#GetQuestFlag](#GetQuestFlag "wikilink")                                                        |
| 48  | SetQuestFlag                                                                                      |
| 49  | GetDataDeletionSettings                                                                           |
| 50  | SetDataDeletionSettings                                                                           |
| 51  | GetInitialSystemAppletProgramId                                                                   |
| 52  | GetOverlayDispProgramId                                                                           |
| 53  | GetDeviceTimeZoneLocationName                                                                     |
| 54  | SetDeviceTimeZoneLocationName                                                                     |
| 55  | GetWirelessCertificationFileSize                                                                  |
| 56  | [GetWirelessCertificationFile](Flash%20Filesystem#PRODINFOF.md##PRODINFOF "wikilink")             |
| 57  | SetRegionCode                                                                                     |
| 58  | GetNetworkSystemClockContext                                                                      |
| 59  | SetNetworkSystemClockContext                                                                      |
| 60  | IsUserSystemClockAutomaticCorrectionEnabled                                                       |
| 61  | SetUserSystemClockAutomaticCorrectionEnabled                                                      |
| 62  | [\#GetDebugModeFlag](#GetDebugModeFlag "wikilink")                                                |
| 63  | GetPrimaryAlbumStorage                                                                            |
| 64  | SetPrimaryAlbumStorage                                                                            |
| 65  | GetUsb30EnableFlag                                                                                |
| 66  | SetUsb30EnableFlag                                                                                |
| 67  | GetBatteryLot                                                                                     |
| 68  | [\#GetSerialNumber](#GetSerialNumber "wikilink")                                                  |
| 69  | GetNfcEnableFlag                                                                                  |
| 70  | SetNfcEnableFlag                                                                                  |
| 71  | GetSleepSettings                                                                                  |
| 72  | SetSleepSettings                                                                                  |
| 73  | GetWirelessLanEnableFlag                                                                          |
| 74  | SetWirelessLanEnableFlag                                                                          |
| 75  | GetInitialLaunchSettings                                                                          |
| 76  | SetInitialLaunchSettings                                                                          |
| 77  | GetDeviceNickName                                                                                 |
| 78  | SetDeviceNickName                                                                                 |
| 79  | GetProductModel                                                                                   |
| 80  | GetLdnChannel                                                                                     |
| 81  | SetLdnChannel                                                                                     |
| 82  | AcquireTelemetryDirtyFlagEventHandle                                                              |
| 83  | GetTelemetryDirtyFlags                                                                            |
| 84  | GetPtmBatteryLot                                                                                  |
| 85  | SetPtmBatteryLot                                                                                  |
| 86  | GetPtmFuelGaugeParameter                                                                          |
| 87  | SetPtmFuelGaugeParameter                                                                          |
| 88  | GetBluetoothEnableFlag                                                                            |
| 89  | SetBluetoothEnableFlag                                                                            |
| 90  | GetMiiAuthorId                                                                                    |
| 91  | SetShutdownRtcValue                                                                               |
| 92  | GetShutdownRtcValue                                                                               |
| 93  | AcquireFatalDirtyFlagEventHandle                                                                  |
| 94  | GetFatalDirtyFlags                                                                                |
| 95  | \[2.0.0+\] GetAutoUpdateEnableFlag                                                                |
| 96  | \[2.0.0+\] SetAutoUpdateEnableFlag                                                                |
| 97  | \[2.0.0+\] GetNxControllerSettings                                                                |
| 98  | \[2.0.0+\] SetNxControllerSettings                                                                |
| 99  | \[2.0.0+\] GetBatteryPercentageFlag                                                               |
| 100 | \[2.0.0+\] SetBatteryPercentageFlag                                                               |
| 101 | \[2.0.0+\] GetExternalRtcResetFlag                                                                |
| 102 | \[2.0.0+\] SetExternalRtcResetFlag                                                                |
| 103 | \[3.0.0+\] GetUsbFullKeyEnableFlag                                                                |
| 104 | \[3.0.0+\] SetUsbFullKeyEnableFlag                                                                |
| 105 | \[3.0.0+\] SetExternalSteadyClockInternalOffset                                                   |
| 106 | \[3.0.0+\] GetExternalSteadyClockInternalOffset                                                   |
| 107 | \[3.0.0+\] GetBacklightSettingsEx                                                                 |
| 108 | \[3.0.0+\] SetBacklightSettingsEx                                                                 |
| 109 | \[3.0.0+\] GetHeadphoneVolumeWarningCount                                                         |
| 110 | \[3.0.0+\] SetHeadphoneVolumeWarningCount                                                         |
| 111 | \[3.0.0+\] GetBluetoothAfhEnableFlag                                                              |
| 112 | \[3.0.0+\] SetBluetoothAfhEnableFlag                                                              |
| 113 | \[3.0.0+\] GetBluetoothBoostEnableFlag                                                            |
| 114 | \[3.0.0+\] SetBluetoothBoostEnableFlag                                                            |
| 115 | \[3.0.0+\] GetInRepairProcessEnableFlag                                                           |
| 116 | \[3.0.0+\] SetInRepairProcessEnableFlag                                                           |
| 117 | \[3.0.0+\] GetHeadphoneVolumeUpdateFlag                                                           |
| 118 | \[3.0.0+\] SetHeadphoneVolumeUpdateFlag                                                           |
| 119 | \[3.0.0+\] NeedsToUpdateHeadphoneVolume                                                           |
| 120 | \[3.0.0+\] GetPushNotificationActivityModeOnSleep                                                 |
| 121 | \[3.0.0+\] SetPushNotificationActivityModeOnSleep                                                 |
| 122 | \[4.0.0+\] [\#GetServiceDiscoveryControlSettings](#GetServiceDiscoveryControlSettings "wikilink") |
| 123 | \[4.0.0+\] SetServiceDiscoveryControlSettings                                                     |
| 124 | \[4.0.0+\] GetErrorReportSharePermission                                                          |
| 125 | \[4.0.0+\] SetErrorReportSharePermission                                                          |
| 126 | \[4.0.0+\] GetAppletLaunchFlags                                                                   |
| 127 | \[4.0.0+\] SetAppletLaunchFlags                                                                   |
| 128 | \[4.0.0+\] GetConsoleSixAxisSensorAccelerationBias                                                |
| 129 | \[4.0.0+\] SetConsoleSixAxisSensorAccelerationBias                                                |
| 130 | \[4.0.0+\] GetConsoleSixAxisSensorAngularVelocityBias                                             |
| 131 | \[4.0.0+\] SetConsoleSixAxisSensorAngularVelocityBias                                             |
| 132 | \[4.0.0+\] GetConsoleSixAxisSensorAccelerationGain                                                |
| 133 | \[4.0.0+\] SetConsoleSixAxisSensorAccelerationGain                                                |
| 134 | \[4.0.0+\] GetConsoleSixAxisSensorAngularVelocityGain                                             |
| 135 | \[4.0.0+\] SetConsoleSixAxisSensorAngularVelocityGain                                             |
| 136 | \[4.0.0+\] GetKeyboardLayout                                                                      |
| 137 | \[4.0.0+\] SetKeyboardLayout                                                                      |
| 138 | \[4.0.0+\] GetWebInspectorFlag                                                                    |
| 139 | \[4.0.0+\] [\#GetAllowedSslHosts](#GetAllowedSslHosts "wikilink")                                 |
| 140 | \[4.0.0+\] GetHostFsMountPoint                                                                    |
| 141 | \[5.0.0+\] GetRequiresRunRepairTimeReviser                                                        |
| 142 | \[5.0.0+\] SetRequiresRunRepairTimeReviser                                                        |
| 143 | \[5.0.0+\] SetBlePairingSettings                                                                  |
| 144 | \[5.0.0+\] GetBlePairingSettings                                                                  |
| 145 | \[5.0.0+\] GetConsoleSixAxisSensorAngularVelocityTimeBias                                         |
| 146 | \[5.0.0+\] SetConsoleSixAxisSensorAngularVelocityTimeBias                                         |
| 147 | \[5.0.0+\] GetConsoleSixAxisSensorAngularAcceleration                                             |
| 148 | \[5.0.0+\] SetConsoleSixAxisSensorAngularAcceleration                                             |
| 149 | \[5.0.0+\] GetRebootlessSystemUpdateVersion                                                       |

Official user-processes get a new service session handle each time a
set:sys cmd is used, with the session being closed aftewards.

## GetFirmwareVersion

Takes a type-0x1A output buffer. User-processes use hard-coded size
0x100.

If needed, reads the content of the
[System\_Version\_Title](System%20Version%20Title.md "wikilink") "/file"
into state. This is only done once.

Then the above 0x100-byte data is copied to the output buffer.

## GetColorSetId

No input, returns an output s32.

This is the current Theme set by System Settings.

  - 0: "Basic White"
  - 1: "Basic Black"

## SetColorSetId

Takes an input s32, no output.

## GetSettingsItemValue

Takes two type-0x19 input buffers and a type-0x6 output buffer. Returns
an output u64 for the actual size written to the outbuf.

The outbuf\_size is compared with the config\_size. When config\_size is
larger than outbuf\_size, outbuf\_size is used for the memcpy, otherwise
config\_size is used. Afterwards the size used for the memcpy is written
to output(see above).

If loading from main config fails, it will also attempt to load config
from various state if the input strings match hard-coded strings.

## GetDebugModeFlag

Returns an output u8.

Loads the 1-byte config for \<"settings\_debug",
"is\_debug\_mode\_enabled"\>. If that fails, value 0x1 is written to
output. This uses the same func as ReadSetting internally.

Returned retval is always 0.

## GetSerialNumber

Returns the 0x18-byte SerialNumber string.

## GetServiceDiscoveryControlSettings

Returns 0x01 if [safemode](Safemode.md "wikilink") needs to be launched.

## GetAllowedSslHosts

Takes a type-0x6 output buffer, returns an output s32. This buffer
contains an array of 0x8-byte "nn::settings::system::AllowedSslHost"
entries.

## GetQuestFlag

Gets a flag determining whether the console is a kiosk unit (codenamed
"Quest"). Used by qlaunch to determine whether to launch Retail
Interactive Display Menu.

# System Config

There's a common config title (\*818), and a config title for each
[HardwareType](SPL%20services.md "wikilink").

\[5.0.0+\] New config fields were added to the HardwareType-specific
config:

  - "systeminitializer\!eks\_enabled" 1 for non-Mariko, 0 otherwise.
  - "systeminitializer\!bct\_eks\_offset" Offset within the
    [BCT](BCT.md "wikilink") where the
    [keyblob](Flash%20Filesystem.md "wikilink")/"EKS" is stored.
  - "systeminitializer\!bct\_version\_offset" Offset within the
    [BCT](BCT.md "wikilink") where the keyblob version is stored
    (bootloader0\_info.version).
  - "systeminitializer\!boot\_image\_update\_type" 0 for non-Mariko, 1
    otherwise.

"bct\_eks\_offset" and "bct\_version\_offset" are only present in
non-Mariko config, since (?) Mariko "eks\_enabled" is 0. This presumably
means the [keyblob](Flash%20Filesystem.md "wikilink")/"EKS" is not
embedded in [BCT](BCT.md "wikilink") with Mariko?

[Category:Services](Category:Services "wikilink")
