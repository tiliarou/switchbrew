# erpt:c

This is "nn::erpt::sf::IContext".

| Cmd | Name                                                |
| --- | --------------------------------------------------- |
| 0   | SubmitContext                                       |
| 1   | CreateReport                                        |
| 2   | \[3.0.0+\] SetInitialLaunchSettingsCompletionTime   |
| 3   | \[3.0.0+\] ClearInitialLaunchSettingsCompletionTime |
| 4   | \[3.0.0+\] UpdatePowerOnTime                        |
| 5   | \[3.0.0+\] UpdateAwakeTime                          |
| 6   | \[5.0.0+\] SubmitMultipleCategoryContext            |
| 7   | \[6.0.0+\] UpdateApplicationLaunchTime              |
| 8   | \[6.0.0+\] ClearApplicationLaunchTime               |

# erpt:r

This is "nn::erpt::sf::ISession".

| Cmd | Name        |
| --- | ----------- |
| 0   | OpenReport  |
| 1   | OpenManager |

## IReport

This is "nn::erpt::sf::IReport".

| Cmd | Name     |
| --- | -------- |
| 0   | Open     |
| 1   | Read     |
| 2   | SetFlags |
| 3   | GetFlags |
| 4   | Close    |
| 5   | GetSize  |

## IManager

This is "nn::erpt::sf::IManager".

| Cmd | Name                                 |
| --- | ------------------------------------ |
| 0   | GetReportList                        |
| 1   | GetEvent                             |
| 2   | \[4.0.0+\] CleanupReports            |
| 3   | \[5.0.0+\] DeleteReport              |
| 4   | \[5.0.0+\] GetStorageUsageStatistics |

# Reports

## Fields

| Name                                                           | Format  | Description                                                           |
| -------------------------------------------------------------- | ------- | --------------------------------------------------------------------- |
| TestU64                                                        |         |                                                                       |
| TestU32                                                        |         |                                                                       |
| TestI64                                                        |         |                                                                       |
| TestI32                                                        |         |                                                                       |
| TestString                                                     |         |                                                                       |
| TestU8Array                                                    |         |                                                                       |
| TestU32Array                                                   |         |                                                                       |
| TestU64Array                                                   |         |                                                                       |
| TestI32Array                                                   |         |                                                                       |
| TestI64Array                                                   |         |                                                                       |
| ErrorCode                                                      |         |                                                                       |
| ErrorDescription                                               |         |                                                                       |
| OccurrenceTimestamp                                            | Integer | UTC unix timestamp from [user-time](PCV%20services.md "wikilink").    |
| ReportIdentifier                                               |         |                                                                       |
| ConnectionStatus                                               |         |                                                                       |
| AccessPointSSID                                                |         |                                                                       |
| AccessPointSecurityType                                        |         |                                                                       |
| RadioStrength                                                  |         |                                                                       |
| NXMacAddress                                                   |         |                                                                       |
| IPAddressAcquisitionMethod                                     |         |                                                                       |
| CurrentIPAddress                                               |         |                                                                       |
| SubnetMask                                                     |         |                                                                       |
| GatewayIPAddress                                               |         |                                                                       |
| DNSType                                                        |         |                                                                       |
| PriorityDNSIPAddress                                           |         |                                                                       |
| AlternateDNSIPAddress                                          |         |                                                                       |
| UseProxyFlag                                                   |         |                                                                       |
| ProxyIPAddress                                                 |         |                                                                       |
| ProxyPort                                                      |         |                                                                       |
| ProxyAutoAuthenticateFlag                                      |         |                                                                       |
| MTU                                                            |         |                                                                       |
| ConnectAutomaticallyFlag                                       |         |                                                                       |
| UseStealthNetworkFlag                                          |         |                                                                       |
| LimitHighCapacityFlag                                          |         |                                                                       |
| NATType                                                        |         |                                                                       |
| WirelessAPMacAddress                                           |         |                                                                       |
| GlobalIPAddress                                                |         |                                                                       |
| EnableWirelessInterfaceFlag                                    |         |                                                                       |
| EnableWifiFlag                                                 |         |                                                                       |
| EnableBluetoothFlag                                            |         |                                                                       |
| EnableNFCFlag                                                  |         |                                                                       |
| NintendoZoneSSIDListVersion                                    |         |                                                                       |
| LANAdapterMacAddress                                           |         |                                                                       |
| ApplicationID                                                  |         |                                                                       |
| ApplicationTitle                                               |         |                                                                       |
| ApplicationVersion                                             |         |                                                                       |
| ApplicationStorageLocation                                     |         |                                                                       |
| DownloadContentType                                            |         |                                                                       |
| InstallContentType                                             |         |                                                                       |
| ConsoleStartingUpFlag                                          |         |                                                                       |
| SystemStartingUpFlag                                           |         |                                                                       |
| ConsoleFirstInitFlag                                           |         |                                                                       |
| HomeMenuScreenDisplayedFlag                                    |         |                                                                       |
| DataManagementScreenDisplayedFlag                              |         |                                                                       |
| ConnectionTestingFlag                                          |         |                                                                       |
| ApplicationRunningFlag                                         |         |                                                                       |
| DataCorruptionDetectedFlag                                     |         |                                                                       |
| ProductModel                                                   |         |                                                                       |
| CurrentLanguage                                                |         |                                                                       |
| UseNetworkTimeProtocolFlag                                     |         |                                                                       |
| TimeZone                                                       |         |                                                                       |
| ControllerFirmware                                             |         |                                                                       |
| VideoOutputSetting                                             |         |                                                                       |
| NANDFreeSpace                                                  |         |                                                                       |
| SDCardFreeSpace                                                |         |                                                                       |
| SerialNumber                                                   |         |                                                                       |
| OsVersion                                                      |         |                                                                       |
| ScreenBrightnessAutoAdjustFlag                                 |         |                                                                       |
| HdmiAudioOutputMode                                            |         |                                                                       |
| SpeakerAudioOutputMode                                         |         |                                                                       |
| HeadphoneAudioOutputMode                                       |         |                                                                       |
| MuteOnHeadsetUnpluggedFlag                                     |         |                                                                       |
| NumUserRegistered                                              |         |                                                                       |
| StorageAutoOrganizeFlag                                        |         |                                                                       |
| ControllerVibrationVolume                                      |         |                                                                       |
| LockScreenFlag                                                 |         |                                                                       |
| InternalBatteryLotNumber                                       |         |                                                                       |
| LeftControllerSerialNumber                                     |         |                                                                       |
| RightControllerSerialNumber                                    |         |                                                                       |
| NotifyInGameDownloadCompletionFlag                             |         |                                                                       |
| NotificationSoundFlag                                          |         |                                                                       |
| TVResolutionSetting                                            |         |                                                                       |
| RGBRangeSetting                                                |         |                                                                       |
| ReduceScreenBurnFlag                                           |         |                                                                       |
| TVAllowsCecFlag                                                |         |                                                                       |
| HandheldModeTimeToScreenSleep                                  |         |                                                                       |
| ConsoleModeTimeToScreenSleep                                   |         |                                                                       |
| StopAutoSleepDuringContentPlayFlag                             |         |                                                                       |
| LastConnectionTestDownloadSpeed                                |         |                                                                       |
| LastConnectionTestUploadSpeed                                  |         |                                                                       |
| \[5.0.0+\] DEPRECATED\_ServerFQDN (\[1.0.0-4.1.0\] ServerFQDN) |         |                                                                       |
| HTTPRequestContents                                            |         |                                                                       |
| HTTPRequestResponseContents                                    |         |                                                                       |
| EdgeServerIPAddress                                            |         |                                                                       |
| CDNContentPath                                                 |         |                                                                       |
| FileAccessPath                                                 |         |                                                                       |
| GameCardCID                                                    |         |                                                                       |
| NANDCID                                                        |         |                                                                       |
| MicroSDCID                                                     |         |                                                                       |
| NANDSpeedMode                                                  |         |                                                                       |
| MicroSDSpeedMode                                               |         |                                                                       |
| GameCardSpeedMode                                              |         |                                                                       |
| UserAccountInternalID                                          |         |                                                                       |
| NetworkServiceAccountInternalID                                |         |                                                                       |
| NintendoAccountInternalID                                      |         |                                                                       |
| USB3AvailableFlag                                              |         |                                                                       |
| CallStack                                                      |         |                                                                       |
| SystemStartupLog                                               |         |                                                                       |
| RegionSetting                                                  |         |                                                                       |
| NintendoZoneConnectedFlag                                      |         |                                                                       |
| ForcedSleepHighTemperatureReading                              |         |                                                                       |
| ForcedSleepFanSpeedReading                                     |         |                                                                       |
| ForcedSleepHWInfo                                              |         |                                                                       |
| AbnormalPowerStateInfo                                         |         |                                                                       |
| ScreenBrightnessLevel                                          |         |                                                                       |
| ProgramId                                                      |         |                                                                       |
| AbortFlag                                                      |         |                                                                       |
| ReportVisibilityFlag                                           |         |                                                                       |
| FatalFlag                                                      |         |                                                                       |
| OccurrenceTimestampNet                                         | Integer | UTC unix timestamp from [network-time](PCV%20services.md "wikilink"). |
| ResultBacktrace                                                |         |                                                                       |
| GeneralRegisterAarch32                                         |         |                                                                       |
| StackBacktrace32                                               |         |                                                                       |
| ExceptionInfoAarch32                                           |         |                                                                       |
| GeneralRegisterAarch64                                         |         |                                                                       |
| ExceptionInfoAarch64                                           |         |                                                                       |
| StackBacktrace64                                               |         |                                                                       |
| RegisterSetFlag32                                              |         |                                                                       |
| RegisterSetFlag64                                              |         |                                                                       |
| ProgramMappedAddr32                                            |         |                                                                       |
| ProgramMappedAddr64                                            |         |                                                                       |
| AbortType                                                      |         |                                                                       |
| PrivateOsVersion                                               |         |                                                                       |
| CurrentSystemPowerState                                        |         |                                                                       |
| PreviousSystemPowerState                                       |         |                                                                       |
| DestinationSystemPowerState                                    |         |                                                                       |
| PscTransitionCurrentState                                      |         |                                                                       |
| PscTransitionPreviousState                                     |         |                                                                       |
| PscInitializedList                                             |         |                                                                       |
| PscCurrentPmStateList                                          |         |                                                                       |
| PscNextPmStateList                                             |         |                                                                       |
| PerformanceMode                                                |         |                                                                       |
| PerformanceConfiguration                                       |         |                                                                       |
| Throttled                                                      |         |                                                                       |
| ThrottlingDuration                                             |         |                                                                       |
| ThrottlingTimestamp                                            | Integer | UTC unix timestamp                                                    |
| GameCardCrcErrorCount                                          |         |                                                                       |
| GameCardAsicCrcErrorCount                                      |         |                                                                       |
| GameCardRefreshCount                                           |         |                                                                       |
| GameCardReadRetryCount                                         |         |                                                                       |
| EdidBlock                                                      |         |                                                                       |
| EdidExtensionBlock                                             |         |                                                                       |
| CreateProcessFailureFlag                                       |         |                                                                       |
| TemperaturePcb                                                 |         |                                                                       |
| TemperatureSoc                                                 |         |                                                                       |
| CurrentFanDuty                                                 |         |                                                                       |
| LastDvfsThresholdTripped                                       |         |                                                                       |
| CradlePdcHFwVersion                                            |         |                                                                       |
| CradlePdcAFwVersion                                            |         |                                                                       |
| CradleMcuFwVersion                                             |         |                                                                       |
| CradleDp2HdmiFwVersion                                         |         |                                                                       |
| RunningApplicationId                                           |         |                                                                       |
| RunningApplicationTitle                                        |         |                                                                       |
| RunningApplicationVersion                                      |         |                                                                       |
| RunningApplicationStorageLocation                              |         |                                                                       |
| RunningAppletList                                              |         |                                                                       |
| FocusedAppletHistory                                           |         |                                                                       |
| CompositorState                                                |         |                                                                       |
| CompositorLayerState                                           |         |                                                                       |
| CompositorDisplayState                                         |         |                                                                       |
| CompositorHWCState                                             |         |                                                                       |
| InputCurrentLimit                                              |         |                                                                       |
| BoostModeCurrentLimit                                          |         |                                                                       |
| FastChargeCurrentLimit                                         |         |                                                                       |
| ChargeVoltageLimit                                             |         |                                                                       |
| ChargeConfiguration                                            |         |                                                                       |
| HizMode                                                        |         |                                                                       |
| ChargeEnabled                                                  |         |                                                                       |
| PowerSupplyPath                                                |         |                                                                       |
| BatteryTemperature                                             |         |                                                                       |
| BatteryChargePercent                                           |         |                                                                       |
| BatteryChargeVoltage                                           |         |                                                                       |
| BatteryAge                                                     |         |                                                                       |
| PowerRole                                                      |         |                                                                       |
| PowerSupplyType                                                |         |                                                                       |
| PowerSupplyVoltage                                             |         |                                                                       |
| PowerSupplyCurrent                                             |         |                                                                       |
| FastBatteryChargingEnabled                                     |         |                                                                       |
| ControllerPowerSupplyAcquired                                  |         |                                                                       |
| OtgRequested                                                   |         |                                                                       |
| NANDPreEolInfo                                                 |         |                                                                       |
| NANDDeviceLifeTimeEstTypA                                      |         |                                                                       |
| NANDDeviceLifeTimeEstTypB                                      |         |                                                                       |
| NANDPatrolCount                                                |         |                                                                       |
| NANDNumActivationFailures                                      |         |                                                                       |
| NANDNumActivationErrorCorrections                              |         |                                                                       |
| NANDNumReadWriteFailures                                       |         |                                                                       |
| NANDNumReadWriteErrorCorrections                               |         |                                                                       |
| NANDErrorLog                                                   |         |                                                                       |
| SdCardUserAreaSize                                             |         |                                                                       |
| SdCardProtectedAreaSize                                        |         |                                                                       |
| SdCardNumActivationFailures                                    |         |                                                                       |
| SdCardNumActivationErrorCorrections                            |         |                                                                       |
| SdCardNumReadWriteFailures                                     |         |                                                                       |
| SdCardNumReadWriteErrorCorrections                             |         |                                                                       |
| SdCardErrorLog                                                 |         |                                                                       |
| EncryptionKey                                                  |         |                                                                       |
| EncryptedExceptionInfo                                         |         |                                                                       |
| GameCardTimeoutRetryErrorCount                                 |         |                                                                       |
| FsRemountForDataCorruptCount                                   |         |                                                                       |
| FsRemountForDataCorruptRetryOutCount                           |         |                                                                       |
| \[3.0.0+\] GameCardInsertionCount                              |         |                                                                       |
| \[3.0.0+\] GameCardRemovalCount                                |         |                                                                       |
| \[3.0.0+\] GameCardAsicInitializeCount                         |         |                                                                       |
| \[3.0.0+\] TestU16                                             |         |                                                                       |
| \[3.0.0+\] TestU8                                              |         |                                                                       |
| \[3.0.0+\] TestI16                                             |         |                                                                       |
| \[3.0.0+\] TestI8                                              |         |                                                                       |
| \[3.0.0+\] SystemAppletScene                                   |         |                                                                       |
| \[3.0.0+\] CodecType                                           |         |                                                                       |
| \[3.0.0+\] DecodeBuffers                                       |         |                                                                       |
| \[3.0.0+\] FrameWidth                                          |         |                                                                       |
| \[3.0.0+\] FrameHeight                                         |         |                                                                       |
| \[3.0.0+\] ColorPrimaries                                      |         |                                                                       |
| \[3.0.0+\] TransferCharacteristics                             |         |                                                                       |
| \[3.0.0+\] MatrixCoefficients                                  |         |                                                                       |
| \[3.0.0+\] DisplayWidth                                        |         |                                                                       |
| \[3.0.0+\] DisplayHeight                                       |         |                                                                       |
| \[3.0.0+\] DARWidth                                            |         |                                                                       |
| \[3.0.0+\] DARHeight                                           |         |                                                                       |
| \[3.0.0+\] ColorFormat                                         |         |                                                                       |
| \[3.0.0+\] ColorSpace                                          |         |                                                                       |
| \[3.0.0+\] SurfaceLayout                                       |         |                                                                       |
| \[3.0.0+\] BitStream                                           |         |                                                                       |
| \[3.0.0+\] VideoDecState                                       |         |                                                                       |
| \[3.0.0+\] GpuErrorChannelId                                   |         |                                                                       |
| \[3.0.0+\] GpuErrorAruId                                       |         |                                                                       |
| \[3.0.0+\] GpuErrorType                                        |         |                                                                       |
| \[3.0.0+\] GpuErrorFaultInfo                                   |         |                                                                       |
| \[3.0.0+\] GpuErrorWriteAccess                                 |         |                                                                       |
| \[3.0.0+\] GpuErrorFaultAddress                                |         |                                                                       |
| \[3.0.0+\] GpuErrorFaultUnit                                   |         |                                                                       |
| \[3.0.0+\] GpuErrorFaultType                                   |         |                                                                       |
| \[3.0.0+\] GpuErrorHwContextPointer                            |         |                                                                       |
| \[3.0.0+\] GpuErrorContextStatus                               |         |                                                                       |
| \[3.0.0+\] GpuErrorPbdmaIntr                                   |         |                                                                       |
| \[3.0.0+\] GpuErrorPbdmaErrorType                              |         |                                                                       |
| \[3.0.0+\] GpuErrorPbdmaHeaderShadow                           |         |                                                                       |
| \[3.0.0+\] GpuErrorPbdmaHeader                                 |         |                                                                       |
| \[3.0.0+\] GpuErrorPbdmaGpShadow0                              |         |                                                                       |
| \[3.0.0+\] GpuErrorPbdmaGpShadow1                              |         |                                                                       |
| \[3.0.0+\] AccessPointChannel                                  |         |                                                                       |
| \[3.0.0+\] ThreadName                                          |         |                                                                       |
| \[3.0.0+\] AdspExceptionRegisters                              |         |                                                                       |
| \[3.0.0+\] AdspExceptionSpsr                                   |         |                                                                       |
| \[3.0.0+\] AdspExceptionProgramCounter                         |         |                                                                       |
| \[3.0.0+\] AdspExceptionLinkRegister                           |         |                                                                       |
| \[3.0.0+\] AdspExceptionStackPointer                           |         |                                                                       |
| \[3.0.0+\] AdspExceptionArmModeRegisters                       |         |                                                                       |
| \[3.0.0+\] AdspExceptionStackAddress                           |         |                                                                       |
| \[3.0.0+\] AdspExceptionStackDump                              |         |                                                                       |
| \[3.0.0+\] AdspExceptionReason                                 |         |                                                                       |
| \[3.0.0+\] OscillatorClock                                     |         |                                                                       |
| \[3.0.0+\] CpuDvfsTableClocks                                  |         |                                                                       |
| \[3.0.0+\] CpuDvfsTableVoltages                                |         |                                                                       |
| \[3.0.0+\] GpuDvfsTableClocks                                  |         |                                                                       |
| \[3.0.0+\] GpuDvfsTableVoltages                                |         |                                                                       |
| \[3.0.0+\] EmcDvfsTableClocks                                  |         |                                                                       |
| \[3.0.0+\] EmcDvfsTableVoltages                                |         |                                                                       |
| \[3.0.0+\] ModuleClockFrequencies                              |         |                                                                       |
| \[3.0.0+\] ModuleClockEnableFlags                              |         |                                                                       |
| \[3.0.0+\] ModulePowerEnableFlags                              |         |                                                                       |
| \[3.0.0+\] ModuleResetAssertFlags                              |         |                                                                       |
| \[3.0.0+\] ModuleMinimumVoltageClockRates                      |         |                                                                       |
| \[3.0.0+\] PowerDomainEnableFlags                              |         |                                                                       |
| \[3.0.0+\] PowerDomainVoltages                                 |         |                                                                       |
| \[3.0.0+\] AccessPointRssi                                     |         |                                                                       |
| \[3.0.0+\] FuseInfo                                            |         |                                                                       |
| \[3.0.0+\] VideoLog                                            |         |                                                                       |
| \[3.0.0+\] GameCardDeviceId                                    |         |                                                                       |
| \[3.0.0+\] GameCardAsicReinitializeCount                       |         |                                                                       |
| \[3.0.0+\] GameCardAsicReinitializeFailureCount                |         |                                                                       |
| \[3.0.0+\] GameCardAsicReinitializeFailureDetail               |         |                                                                       |
| \[3.0.0+\] GameCardRefreshSuccessCount                         |         |                                                                       |
| \[3.0.0+\] GameCardAwakenCount                                 |         |                                                                       |
| \[3.0.0+\] GameCardAwakenFailureCount                          |         |                                                                       |
| \[3.0.0+\] GameCardReadCountFromInsert                         |         |                                                                       |
| \[3.0.0+\] GameCardReadCountFromAwaken                         |         |                                                                       |
| \[3.0.0+\] GameCardLastReadErrorPageAddress                    |         |                                                                       |
| \[3.0.0+\] GameCardLastReadErrorPageCount                      |         |                                                                       |
| \[3.0.0+\] AppletManagerContextTrace                           |         |                                                                       |
| \[3.0.0+\] NvDispIsRegistered                                  |         |                                                                       |
| \[3.0.0+\] NvDispIsSuspend                                     |         |                                                                       |
| \[3.0.0+\] NvDispDC0SurfaceNum                                 |         |                                                                       |
| \[3.0.0+\] NvDispDC1SurfaceNum                                 |         |                                                                       |
| \[3.0.0+\] NvDispWindowSrcRectX                                |         |                                                                       |
| \[3.0.0+\] NvDispWindowSrcRectY                                |         |                                                                       |
| \[3.0.0+\] NvDispWindowSrcRectWidth                            |         |                                                                       |
| \[3.0.0+\] NvDispWindowSrcRectHeight                           |         |                                                                       |
| \[3.0.0+\] NvDispWindowDstRectX                                |         |                                                                       |
| \[3.0.0+\] NvDispWindowDstRectY                                |         |                                                                       |
| \[3.0.0+\] NvDispWindowDstRectWidth                            |         |                                                                       |
| \[3.0.0+\] NvDispWindowDstRectHeight                           |         |                                                                       |
| \[3.0.0+\] NvDispWindowIndex                                   |         |                                                                       |
| \[3.0.0+\] NvDispWindowBlendOperation                          |         |                                                                       |
| \[3.0.0+\] NvDispWindowAlphaOperation                          |         |                                                                       |
| \[3.0.0+\] NvDispWindowDepth                                   |         |                                                                       |
| \[3.0.0+\] NvDispWindowAlpha                                   |         |                                                                       |
| \[3.0.0+\] NvDispWindowHFilter                                 |         |                                                                       |
| \[3.0.0+\] NvDispWindowVFilter                                 |         |                                                                       |
| \[3.0.0+\] NvDispWindowOptions                                 |         |                                                                       |
| \[3.0.0+\] NvDispWindowSyncPointId                             |         |                                                                       |
| \[3.0.0+\] NvDispDPSorPower                                    |         |                                                                       |
| \[3.0.0+\] NvDispDPClkType                                     |         |                                                                       |
| \[3.0.0+\] NvDispDPEnable                                      |         |                                                                       |
| \[3.0.0+\] NvDispDPState                                       |         |                                                                       |
| \[3.0.0+\] NvDispDPEdid                                        |         |                                                                       |
| \[3.0.0+\] NvDispDPEdidSize                                    |         |                                                                       |
| \[3.0.0+\] NvDispDPEdidExtSize                                 |         |                                                                       |
| \[3.0.0+\] NvDispDPFakeMode                                    |         |                                                                       |
| \[3.0.0+\] NvDispDPModeNumber                                  |         |                                                                       |
| \[3.0.0+\] NvDispDPPlugInOut                                   |         |                                                                       |
| \[3.0.0+\] NvDispDPAuxIntHandler                               |         |                                                                       |
| \[3.0.0+\] NvDispDPForceMaxLinkBW                              |         |                                                                       |
| \[3.0.0+\] NvDispDPIsConnected                                 |         |                                                                       |
| \[3.0.0+\] NvDispDPLinkValid                                   |         |                                                                       |
| \[3.0.0+\] NvDispDPLinkMaxBW                                   |         |                                                                       |
| \[3.0.0+\] NvDispDPLinkMaxLaneCount                            |         |                                                                       |
| \[3.0.0+\] NvDispDPLinkDownSpread                              |         |                                                                       |
| \[3.0.0+\] NvDispDPLinkSupportEnhancedFraming                  |         |                                                                       |
| \[3.0.0+\] NvDispDPLinkBpp                                     |         |                                                                       |
| \[3.0.0+\] NvDispDPLinkScaramberCap                            |         |                                                                       |
| \[3.0.0+\] NvDispDPLinkBW                                      |         |                                                                       |
| \[3.0.0+\] NvDispDPLinkLaneCount                               |         |                                                                       |
| \[3.0.0+\] NvDispDPLinkEnhancedFraming                         |         |                                                                       |
| \[3.0.0+\] NvDispDPLinkScrambleEnable                          |         |                                                                       |
| \[3.0.0+\] NvDispDPLinkActivePolarity                          |         |                                                                       |
| \[3.0.0+\] NvDispDPLinkActiveCount                             |         |                                                                       |
| \[3.0.0+\] NvDispDPLinkTUSize                                  |         |                                                                       |
| \[3.0.0+\] NvDispDPLinkActiveFrac                              |         |                                                                       |
| \[3.0.0+\] NvDispDPLinkWatermark                               |         |                                                                       |
| \[3.0.0+\] NvDispDPLinkHBlank                                  |         |                                                                       |
| \[3.0.0+\] NvDispDPLinkVBlank                                  |         |                                                                       |
| \[3.0.0+\] NvDispDPLinkOnlyEnhancedFraming                     |         |                                                                       |
| \[3.0.0+\] NvDispDPLinkOnlyEdpCap                              |         |                                                                       |
| \[3.0.0+\] NvDispDPLinkSupportFastLT                           |         |                                                                       |
| \[3.0.0+\] NvDispDPLinkLTDataValid                             |         |                                                                       |
| \[3.0.0+\] NvDispDPLinkTsp3Support                             |         |                                                                       |
| \[3.0.0+\] NvDispDPLinkAuxInterval                             |         |                                                                       |
| \[3.0.0+\] NvDispHdcpCreated                                   |         |                                                                       |
| \[3.0.0+\] NvDispHdcpUserRequest                               |         |                                                                       |
| \[3.0.0+\] NvDispHdcpPlugged                                   |         |                                                                       |
| \[3.0.0+\] NvDispHdcpState                                     |         |                                                                       |
| \[3.0.0+\] NvDispHdcpFailCount                                 |         |                                                                       |
| \[3.0.0+\] NvDispHdcpHdcp22                                    |         |                                                                       |
| \[3.0.0+\] NvDispHdcpMaxRetry                                  |         |                                                                       |
| \[3.0.0+\] NvDispHdcpHpd                                       |         |                                                                       |
| \[3.0.0+\] NvDispHdcpRepeater                                  |         |                                                                       |
| \[3.0.0+\] NvDispCecRxBuf                                      |         |                                                                       |
| \[3.0.0+\] NvDispCecRxLength                                   |         |                                                                       |
| \[3.0.0+\] NvDispCecTxBuf                                      |         |                                                                       |
| \[3.0.0+\] NvDispCecTxLength                                   |         |                                                                       |
| \[3.0.0+\] NvDispCecTxRet                                      |         |                                                                       |
| \[3.0.0+\] NvDispCecState                                      |         |                                                                       |
| \[3.0.0+\] NvDispCecTxInfo                                     |         |                                                                       |
| \[3.0.0+\] NvDispCecRxInfo                                     |         |                                                                       |
| \[3.0.0+\] NvDispDCIndex                                       |         |                                                                       |
| \[3.0.0+\] NvDispDCInitialize                                  |         |                                                                       |
| \[3.0.0+\] NvDispDCClock                                       |         |                                                                       |
| \[3.0.0+\] NvDispDCFrequency                                   |         |                                                                       |
| \[3.0.0+\] NvDispDCFailed                                      |         |                                                                       |
| \[3.0.0+\] NvDispDCModeWidth                                   |         |                                                                       |
| \[3.0.0+\] NvDispDCModeHeight                                  |         |                                                                       |
| \[3.0.0+\] NvDispDCModeBpp                                     |         |                                                                       |
| \[3.0.0+\] NvDispDCPanelFrequency                              |         |                                                                       |
| \[3.0.0+\] NvDispDCWinDirty                                    |         |                                                                       |
| \[3.0.0+\] NvDispDCWinEnable                                   |         |                                                                       |
| \[3.0.0+\] NvDispDCVrr                                         |         |                                                                       |
| \[3.0.0+\] NvDispDCPanelInitialize                             |         |                                                                       |
| \[3.0.0+\] NvDispDsiDataFormat                                 |         |                                                                       |
| \[3.0.0+\] NvDispDsiVideoMode                                  |         |                                                                       |
| \[3.0.0+\] NvDispDsiRefreshRate                                |         |                                                                       |
| \[3.0.0+\] NvDispDsiLpCmdModeFrequency                         |         |                                                                       |
| \[3.0.0+\] NvDispDsiHsCmdModeFrequency                         |         |                                                                       |
| \[3.0.0+\] NvDispDsiPanelResetTimeout                          |         |                                                                       |
| \[3.0.0+\] NvDispDsiPhyFrequency                               |         |                                                                       |
| \[3.0.0+\] NvDispDsiFrequency                                  |         |                                                                       |
| \[3.0.0+\] NvDispDsiInstance                                   |         |                                                                       |
| \[3.0.0+\] NvDispDcDsiHostCtrlEnable                           |         |                                                                       |
| \[3.0.0+\] NvDispDcDsiInit                                     |         |                                                                       |
| \[3.0.0+\] NvDispDcDsiEnable                                   |         |                                                                       |
| \[3.0.0+\] NvDispDcDsiHsMode                                   |         |                                                                       |
| \[3.0.0+\] NvDispDcDsiVendorId                                 |         |                                                                       |
| \[3.0.0+\] NvDispDcDsiLcdVendorNum                             |         |                                                                       |
| \[3.0.0+\] NvDispDcDsiHsClockControl                           |         |                                                                       |
| \[3.0.0+\] NvDispDcDsiEnableHsClockInLpMode                    |         |                                                                       |
| \[3.0.0+\] NvDispDcDsiTeFrameUpdate                            |         |                                                                       |
| \[3.0.0+\] NvDispDcDsiGangedType                               |         |                                                                       |
| \[3.0.0+\] NvDispDcDsiHbpInPktSeq                              |         |                                                                       |
| \[3.0.0+\] NvDispErrID                                         |         |                                                                       |
| \[3.0.0+\] NvDispErrData0                                      |         |                                                                       |
| \[3.0.0+\] NvDispErrData1                                      |         |                                                                       |
| \[3.0.0+\] SdCardMountStatus                                   |         |                                                                       |
| \[3.0.0+\] SdCardMountUnexpectedResult                         |         |                                                                       |
| \[3.0.0+\] NANDTotalSize                                       |         |                                                                       |
| \[3.0.0+\] SdCardTotalSize                                     |         |                                                                       |
| \[3.0.0+\] ElapsedTimeSinceInitialLaunch                       |         |                                                                       |
| \[3.0.0+\] ElapsedTimeSincePowerOn                             |         |                                                                       |
| \[3.0.0+\] ElapsedTimeSinceLastAwake                           |         |                                                                       |
| \[3.0.0+\] OccurrenceTick                                      |         |                                                                       |
| \[3.0.0+\] RetailInteractiveDisplayFlag                        |         |                                                                       |
| \[3.0.0+\] FatFsError                                          |         |                                                                       |
| \[3.0.0+\] FatFsExtraError                                     |         |                                                                       |
| \[3.0.0+\] FatFsErrorDrive                                     |         |                                                                       |
| \[3.0.0+\] FatFsErrorName                                      |         |                                                                       |
| \[3.0.0+\] MonitorManufactureCode                              |         |                                                                       |
| \[3.0.0+\] MonitorProductCode                                  |         |                                                                       |
| \[3.0.0+\] MonitorSerialNumber                                 |         |                                                                       |
| \[3.0.0+\] MonitorManufactureYear                              |         |                                                                       |
| \[3.0.0+\] PhysicalAddress                                     |         |                                                                       |
| \[3.0.0+\] Is4k60Hz                                            |         |                                                                       |
| \[3.0.0+\] Is4k30Hz                                            |         |                                                                       |
| \[3.0.0+\] Is1080P60Hz                                         |         |                                                                       |
| \[3.0.0+\] Is720P60Hz                                          |         |                                                                       |
| \[3.0.0+\] PcmChannelMax                                       |         |                                                                       |
| \[3.0.0+\] CrashReportHash                                     |         |                                                                       |
| \[3.0.0+\] ErrorReportSharePermission                          |         |                                                                       |
| \[3.0.0+\] VideoCodecTypeEnum                                  |         |                                                                       |
| \[3.0.0+\] VideoBitRate                                        |         |                                                                       |
| \[3.0.0+\] VideoFrameRate                                      |         |                                                                       |
| \[3.0.0+\] VideoWidth                                          |         |                                                                       |
| \[3.0.0+\] VideoHeight                                         |         |                                                                       |
| \[3.0.0+\] AudioCodecTypeEnum                                  |         |                                                                       |
| \[3.0.0+\] AudioSampleRate                                     |         |                                                                       |
| \[3.0.0+\] AudioChannelCount                                   |         |                                                                       |
| \[3.0.0+\] AudioBitRate                                        |         |                                                                       |
| \[3.0.0+\] MultimediaContainerType                             |         |                                                                       |
| \[3.0.0+\] MultimediaProfileType                               |         |                                                                       |
| \[3.0.0+\] MultimediaLevelType                                 |         |                                                                       |
| \[3.0.0+\] MultimediaCacheSizeEnum                             |         |                                                                       |
| \[3.0.0+\] MultimediaErrorStatusEnum                           |         |                                                                       |
| \[3.0.0+\] MultimediaErrorLog                                  |         |                                                                       |
| \[3.0.0+\] ServerFqdn                                          |         |                                                                       |
| \[3.0.0+\] ServerIpAddress                                     |         |                                                                       |
| \[3.0.0+\] TestStringEncrypt                                   |         |                                                                       |
| \[3.0.0+\] TestU8ArrayEncrypt                                  |         |                                                                       |
| \[3.0.0+\] TestU32ArrayEncrypt                                 |         |                                                                       |
| \[3.0.0+\] TestU64ArrayEncrypt                                 |         |                                                                       |
| \[3.0.0+\] TestI32ArrayEncrypt                                 |         |                                                                       |
| \[3.0.0+\] TestI64ArrayEncrypt                                 |         |                                                                       |
| \[3.0.0+\] CipherKey                                           |         |                                                                       |
| \[3.0.0+\] FileSystemPath                                      |         |                                                                       |
| \[3.0.0+\] WebMediaPlayerOpenUrl                               |         |                                                                       |
| \[3.0.0+\] WebMediaPlayerLastSocketErrors                      |         |                                                                       |
| \[3.0.0+\] UnknownControllerCount                              |         |                                                                       |
| \[3.0.0+\] AttachedControllerCount                             |         |                                                                       |
| \[3.0.0+\] BluetoothControllerCount                            |         |                                                                       |
| \[3.0.0+\] UsbControllerCount                                  |         |                                                                       |
| \[3.0.0+\] ControllerTypeList                                  |         |                                                                       |
| \[3.0.0+\] ControllerInterfaceList                             |         |                                                                       |
| \[3.0.0+\] ControllerStyleList                                 |         |                                                                       |
| \[3.0.0+\] FsPooledBufferPeakFreeSize                          |         |                                                                       |
| \[3.0.0+\] FsPooledBufferRetriedCount                          |         |                                                                       |
| \[3.0.0+\] FsPooledBufferReduceAllocationCount                 |         |                                                                       |
| \[3.0.0+\] FsBufferManagerPeakFreeSize                         |         |                                                                       |
| \[3.0.0+\] FsBufferManagerRetriedCount                         |         |                                                                       |
| \[3.0.0+\] FsExpHeapPeakFreeSize                               |         |                                                                       |
| \[3.0.0+\] FsBufferPoolPeakFreeSize                            |         |                                                                       |
| \[3.0.0+\] FsPatrolReadAllocateBufferSuccessCount              |         |                                                                       |
| \[3.0.0+\] FsPatrolReadAllocateBufferFailureCount              |         |                                                                       |
| \[5.0.0+\] SteadyClockInternalOffset                           |         |                                                                       |
| \[5.0.0+\] SteadyClockCurrentTimePointValue                    |         |                                                                       |
| \[5.0.0+\] UserClockContextOffset                              |         |                                                                       |
| \[5.0.0+\] UserClockContextTimeStampValue                      |         |                                                                       |
| \[5.0.0+\] NetworkClockContextOffset                           |         |                                                                       |
| \[5.0.0+\] NetworkClockContextTimeStampValue                   |         |                                                                       |
| \[5.0.0+\] SystemAbortFlag                                     |         |                                                                       |
| \[5.0.0+\] ApplicationAbortFlag                                |         |                                                                       |
| \[5.0.0+\] NifmErrorCode                                       |         |                                                                       |
| \[5.0.0+\] LcsApplicationId                                    |         |                                                                       |
| \[5.0.0+\] LcsContentMetaKeyIdList                             |         |                                                                       |
| \[5.0.0+\] LcsContentMetaKeyVersionList                        |         |                                                                       |
| \[5.0.0+\] LcsContentMetaKeyTypeList                           |         |                                                                       |
| \[5.0.0+\] LcsContentMetaKeyInstallTypeList                    |         |                                                                       |
| \[5.0.0+\] LcsSenderFlag                                       |         |                                                                       |
| \[5.0.0+\] LcsApplicationRequestFlag                           |         |                                                                       |
| \[5.0.0+\] LcsHasExFatDriverFlag                               |         |                                                                       |
| \[5.0.0+\] LcsIpAddress                                        |         |                                                                       |
| \[5.0.0+\] AcpStartupUserAccount                               |         |                                                                       |
| \[5.0.0+\] AcpAocRegistrationType                              |         |                                                                       |
| \[5.0.0+\] AcpAttributeFlag                                    |         |                                                                       |
| \[5.0.0+\] AcpSupportedLanguageFlag                            |         |                                                                       |
| \[5.0.0+\] AcpParentalControlFlag                              |         |                                                                       |
| \[5.0.0+\] AcpScreenShot                                       |         |                                                                       |
| \[5.0.0+\] AcpVideoCapture                                     |         |                                                                       |
| \[5.0.0+\] AcpDataLossConfirmation                             |         |                                                                       |
| \[5.0.0+\] AcpPlayLogPolicy                                    |         |                                                                       |
| \[5.0.0+\] AcpPresenceGroupId                                  |         |                                                                       |
| \[5.0.0+\] AcpRatingAge                                        |         |                                                                       |
| \[5.0.0+\] AcpAocBaseId                                        |         |                                                                       |
| \[5.0.0+\] AcpSaveDataOwnerId                                  |         |                                                                       |
| \[5.0.0+\] AcpUserAccountSaveDataSize                          |         |                                                                       |
| \[5.0.0+\] AcpUserAccountSaveDataJournalSize                   |         |                                                                       |
| \[5.0.0+\] AcpDeviceSaveDataSize                               |         |                                                                       |
| \[5.0.0+\] AcpDeviceSaveDataJournalSize                        |         |                                                                       |
| \[5.0.0+\] AcpBcatDeliveryCacheStorageSize                     |         |                                                                       |
| \[5.0.0+\] AcpApplicationErrorCodeCategory                     |         |                                                                       |
| \[5.0.0+\] AcpLocalCommunicationId                             |         |                                                                       |
| \[5.0.0+\] AcpLogoType                                         |         |                                                                       |
| \[5.0.0+\] AcpLogoHandling                                     |         |                                                                       |
| \[5.0.0+\] AcpRuntimeAocInstall                                |         |                                                                       |
| \[5.0.0+\] AcpCrashReport                                      |         |                                                                       |
| \[5.0.0+\] AcpHdcp                                             |         |                                                                       |
| \[5.0.0+\] AcpSeedForPseudoDeviceId                            |         |                                                                       |
| \[5.0.0+\] AcpBcatPassphrase                                   |         |                                                                       |
| \[5.0.0+\] AcpUserAccountSaveDataSizeMax                       |         |                                                                       |
| \[5.0.0+\] AcpUserAccountSaveDataJournalSizeMax                |         |                                                                       |
| \[5.0.0+\] AcpDeviceSaveDataSizeMax                            |         |                                                                       |
| \[5.0.0+\] AcpDeviceSaveDataJournalSizeMax                     |         |                                                                       |
| \[5.0.0+\] AcpTemporaryStorageSize                             |         |                                                                       |
| \[5.0.0+\] AcpCacheStorageSize                                 |         |                                                                       |
| \[5.0.0+\] AcpCacheStorageJournalSize                          |         |                                                                       |
| \[5.0.0+\] AcpCacheStorageDataAndJournalSizeMax                |         |                                                                       |
| \[5.0.0+\] AcpCacheStorageIndexMax                             |         |                                                                       |
| \[5.0.0+\] AcpPlayLogQueryableApplicationId                    |         |                                                                       |
| \[5.0.0+\] AcpPlayLogQueryCapability                           |         |                                                                       |
| \[5.0.0+\] AcpRepairFlag                                       |         |                                                                       |
| \[5.0.0+\] RunningApplicationPatchStorageLocation              |         |                                                                       |
| \[5.0.0+\] RunningApplicationVersionNumber                     |         |                                                                       |
| \[5.0.0+\] FsRecoveredByInvalidateCacheCount                   |         |                                                                       |
| \[5.0.0+\] FsSaveDataIndexCount                                |         |                                                                       |
| \[5.0.0+\] FsBufferManagerPeakTotalAllocatableSize             |         |                                                                       |
| \[5.0.0+\] MonitorCurrentWidth                                 |         |                                                                       |
| \[5.0.0+\] MonitorCurrentHeight                                |         |                                                                       |
| \[5.0.0+\] MonitorCurrentRefreshRate                           |         |                                                                       |
| \[5.0.0+\] RebootlessSystemUpdateVersion                       |         |                                                                       |
| \[5.0.0+\] EncryptedExceptionInfo1                             |         |                                                                       |
| \[5.0.0+\] EncryptedExceptionInfo2                             |         |                                                                       |
| \[5.0.0+\] EncryptedExceptionInfo3                             |         |                                                                       |
| \[5.0.0+\] EncryptedDyingMessage                               |         |                                                                       |
| \[5.0.0+\] DramId                                              |         |                                                                       |
| \[6.0.0+\] NifmConnectionTestRedirectUrl                       |         |                                                                       |
| \[6.0.0+\] AcpRequiredNetworkServiceLicenseOnLaunchFlag        |         |                                                                       |
| \[6.0.0+\] PciePort0Flags                                      |         |                                                                       |
| \[6.0.0+\] PciePort0Speed                                      |         |                                                                       |
| \[6.0.0+\] PciePort0ResetTimeInUs                              |         |                                                                       |
| \[6.0.0+\] PciePort0IrqCount                                   |         |                                                                       |
| \[6.0.0+\] PciePort0Statistics                                 |         |                                                                       |
| \[6.0.0+\] PciePort1Flags                                      |         |                                                                       |
| \[6.0.0+\] PciePort1Speed                                      |         |                                                                       |
| \[6.0.0+\] PciePort1ResetTimeInUs                              |         |                                                                       |
| \[6.0.0+\] PciePort1IrqCount                                   |         |                                                                       |
| \[6.0.0+\] PciePort1Statistics                                 |         |                                                                       |
| \[6.0.0+\] PcieFunction0VendorId                               |         |                                                                       |
| \[6.0.0+\] PcieFunction0DeviceId                               |         |                                                                       |
| \[6.0.0+\] PcieFunction0PmState                                |         |                                                                       |
| \[6.0.0+\] PcieFunction0IsAcquired                             |         |                                                                       |
| \[6.0.0+\] PcieFunction1VendorId                               |         |                                                                       |
| \[6.0.0+\] PcieFunction1DeviceId                               |         |                                                                       |
| \[6.0.0+\] PcieFunction1PmState                                |         |                                                                       |
| \[6.0.0+\] PcieFunction1IsAcquired                             |         |                                                                       |
| \[6.0.0+\] PcieGlobalRootComplexStatistics                     |         |                                                                       |
| \[6.0.0+\] PciePllResistorCalibrationValue                     |         |                                                                       |
| \[6.0.0+\] CertificateRequestedHostName                        |         |                                                                       |
| \[6.0.0+\] CertificateCommonName                               |         |                                                                       |
| \[6.0.0+\] CertificateSANCount                                 |         |                                                                       |
| \[6.0.0+\] CertificateSANs                                     |         |                                                                       |
| \[6.0.0+\] FsBufferPoolMaxAllocateSize                         |         |                                                                       |
| \[6.0.0+\] CertificateIssuerName                               |         |                                                                       |
| \[6.0.0+\] ApplicationAliveTime                                |         |                                                                       |
| \[6.0.0+\] ApplicationInFocusTime                              |         |                                                                       |
| \[6.0.0+\] ApplicationOutOfFocusTime                           |         |                                                                       |
| \[6.0.0+\] ApplicationBackgroundFocusTime                      |         |                                                                       |
| \[6.0.0+\] AcpUserAccountSwitchLock                            |         |                                                                       |
| \[6.0.0+\] USB3HostAvailableFlag                               |         |                                                                       |
| \[6.0.0+\] USB3DeviceAvailableFlag                             |         |                                                                       |

[Category:Services](Category:Services "wikilink")
