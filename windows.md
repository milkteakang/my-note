
## Chris Titus Tech - Windows Utility 추천!

- GUI로 정리 가능. Bloatware 제거, 서비스 비활성화, 최적화까지 한번에 가능.
- GitHub: https://github.com/ChrisTitusTech/winutil
- PowerShell에 아래 명령어 입력
```powershell
irm https://christitus.com/win | iex
```

## O&O AppBuster 사용 (GUI 방식, 쉬움)

- 기본 앱 한 번에 제거 가능
- 원클릭 제거 / 복원도 가능해서 실수해도 안심
- https://www.oo-software.com/en/ooappbuster

## 광고 & 텔레메트리 차단

- O&O ShutUp10++ (무료툴)
- 광고, 피드백, 자동업데이트, 위치추적 등 완전 비활성화 가능
- 추천 옵션: "모든 권장 설정 적용" 클릭
- https://www.oo-software.com/en/download/current/ooshutup10

## 자동 업데이트 비활성화 (선택사항)

- gpedit.msc →
컴퓨터 구성 → 관리 템플릿 → Windows 업데이트 →
- "자동 업데이트 구성" → "사용" → "2 - 알림만 하고 자동 설치 안 함"

## 시작 프로그램 & 서비스 정리

- Ctrl + Shift + Esc → 작업 관리자 → [시작프로그램] 탭에서 비활성화
- services.msc 입력 → 다음 서비스들을 비활성화 또는 수동으로 변경 (주의해서!)

| 서비스 이름 | 설명 | 설정 권장값 |
|-------------|------|-------------|
|Connected User Experiences | 사용 데이터 전송 관련 | 사용 안 함|
|Diagnostics Tracking | 진단 데이터 수집 | 사용 안 함|
|Windows Search | 검색 인덱싱 (잘 안 쓰면 꺼도 됨) | 사용 안 함|
|Print Spooler | 프린터 안 쓰면 | 사용 안 함|
|Remote Registry | 보안상 꺼두는 게 좋음 | 사용 안 함|

## Windows Defender 비활성화 (선택 사항)

- 가벼운 시스템을 원한다면 Defender 비활성화도 고려할 수 있음. (단, 보안에 유의)
```powershell
Set-MpPreference -DisableRealtimeMonitoring $true
```

## 윈도우 업데이트 비활성화

- 업데이트가 필요 없으면 비활성화할 수도 있어. (보안 업데이트는 비활성화하지 않는 게 좋음)
```powershell
sc config wuauserv start= disabled
```

## PowerShell 명령어 분석

### 앱 제거 (Get-AppxPackage)

- Microsoft.549981C3F5F10: Microsoft Store
- Microsoft.ZuneMusic: Groove Music (이제 음악 앱)
- Microsoft.ZuneVideo: Films & TV (이제 비디오 앱)
- Microsoft.MicrosoftSolitaireCollection: 솔리테어 게임
- Microsoft.WindowsFeedbackHub: 피드백 허브
- Disney.37853FC22B2CE: Disney+ 앱
- Xbox 관련 앱들: Xbox 게임 관련 앱들 (게임 저장, 오버레이, Xbox Game Bar 등)
- YourPhone: 모바일 연결 앱 (전화, 문자 등)

### 서비스 및 스케줄러 삭제 (sc delete, schtasks)

- XblAuthManager / XblGameSave / XboxNetApiSvc / XboxGipSvc: Xbox와 관련된 서비스들을 완전히 삭제.
- reg delete 명령어: Xbox 관련 레지스트리 항목 삭제
- schtasks 명령어: Xbox 관련 예약 작업 비활성화.

### GameDVR 설정 비활성화

- reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\GameDVR": 게임 녹화 (GameDVR) 기능을 비활성화하는 레지스트리 수정.
```powershell
Get-AppxPackage -allusers *xbox* | Remove-AppxPackage
Get-AppxPackage -allusers *solitaire* | Remove-AppxPackage
Get-AppxPackage -allusers *zune* | Remove-AppxPackage
Get-AppxPackage -allusers *skype* | Remove-AppxPackage
Get-AppxPackage -allusers *people* | Remove-AppxPackage

Get-appxpackage -allusers *Microsoft.549981C3F5F10* | Remove-AppxPackage
Get-AppxPackage -allusers Microsoft.ZuneMusic | Remove-AppxPackage
Get-AppxPackage -allusers Microsoft.ZuneVideo | Remove-AppxPackage
Get-AppxPackage -allusers *Microsoft.MicrosoftSolitaireCollection* | Remove-AppxPackage
Get-AppxPackage -allusers Microsoft.WindowsFeedbackHub | Remove-AppxPackage
Get-AppxPackage -allUsers Disney.37853FC22B2CE | Remove-AppxPackage
Get-AppxPackage -allUsers *xboxgaming* | Remove-AppxPackage
Get-AppxPackage -allUsers *XboxIdent* | Remove-AppxPackage
Get-AppxPackage -allUsers *XboxSpeechToTextOverlay* | Remove-AppxPackage
Get-AppxPackage -allUsers *XboxGameOverlay* | Remove-AppxPackage
Get-AppxPackage -allUsers *XboxGameCallableUI* | Remove-AppxPackage
Get-AppxPackage -allUsers *GamingServices* | Remove-AppxPackage
Get-AppxPackage -allUsers *YourPhone* | Remove-AppxPackage


sc delete XblAuthManager
sc delete XblGameSave
sc delete XboxNetApiSvc
sc delete XboxGipSvc reg delete "HKLM\SYSTEM\CurrentControlSet\Services\xbgm" /f
schtasks /Change /TN "Microsoft\XblGameSave\XblGameSaveTask" /disable
schtasks /Change /TN "Microsoft\XblGameSave\XblGameSaveTaskLogon" /disable
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\GameDVR" /v AllowGameDVR /t REG_DWORD /d 0 /f
```

```powershell
Get-AppxPackage | Select Name, PackageFamilyName

Microsoft.UI.Xaml.CBS                           Microsoft.UI.Xaml.CBS_8wekyb3d8bbwe
1527c705-839a-4832-9118-54d4Bd6a0c89            1527c705-839a-4832-9118-54d4Bd6a0c89_cw5n1h2txyewy
c5e2524a-ea46-4f67-841f-6a9465d9d515            c5e2524a-ea46-4f67-841f-6a9465d9d515_cw5n1h2txyewy
E2A4F912-2574-4A75-9BB0-0D023378592B            E2A4F912-2574-4A75-9BB0-0D023378592B_cw5n1h2txyewy
F46D4000-FD22-4DB4-AC8E-4E1DDDE828FE            F46D4000-FD22-4DB4-AC8E-4E1DDDE828FE_cw5n1h2txyewy
Microsoft.AccountsControl                       Microsoft.AccountsControl_cw5n1h2txyewy
Microsoft.AsyncTextService                      Microsoft.AsyncTextService_8wekyb3d8bbwe
Microsoft.BioEnrollment                         Microsoft.BioEnrollment_cw5n1h2txyewy
Microsoft.CredDialogHost                        Microsoft.CredDialogHost_cw5n1h2txyewy
Microsoft.MicrosoftEdgeDevToolsClient           Microsoft.MicrosoftEdgeDevToolsClient_8wekyb3d8bbwe
Microsoft.Win32WebViewHost                      Microsoft.Win32WebViewHost_cw5n1h2txyewy
Microsoft.Windows.Apprep.ChxApp                 Microsoft.Windows.Apprep.ChxApp_cw5n1h2txyewy
Microsoft.Windows.AssignedAccessLockApp         Microsoft.Windows.AssignedAccessLockApp_cw5n1h2txyewy
Microsoft.Windows.CapturePicker                 Microsoft.Windows.CapturePicker_cw5n1h2txyewy
Microsoft.Windows.CloudExperienceHost           Microsoft.Windows.CloudExperienceHost_cw5n1h2txyewy
Microsoft.Windows.ContentDeliveryManager        Microsoft.Windows.ContentDeliveryManager_cw5n1h2txyewy
Microsoft.Windows.OOBENetworkCaptivePortal      Microsoft.Windows.OOBENetworkCaptivePortal_cw5n1h2txyewy
Microsoft.Windows.OOBENetworkConnectionFlow     Microsoft.Windows.OOBENetworkConnectionFlow_cw5n1h2txyewy
Microsoft.Windows.ParentalControls              Microsoft.Windows.ParentalControls_cw5n1h2txyewy
Microsoft.Windows.PeopleExperienceHost          Microsoft.Windows.PeopleExperienceHost_cw5n1h2txyewy
Microsoft.Windows.PinningConfirmationDialog     Microsoft.Windows.PinningConfirmationDialog_cw5n1h2txyewy
Microsoft.Windows.PrintQueueActionCenter        Microsoft.Windows.PrintQueueActionCenter_cw5n1h2txyewy
Microsoft.Windows.SecureAssessmentBrowser       Microsoft.Windows.SecureAssessmentBrowser_cw5n1h2txyewy
Microsoft.Windows.XGpuEjectDialog               Microsoft.Windows.XGpuEjectDialog_cw5n1h2txyewy
Microsoft.XboxGameCallableUI                    Microsoft.XboxGameCallableUI_cw5n1h2txyewy
MicrosoftWindows.Client.FileExp                 MicrosoftWindows.Client.FileExp_cw5n1h2txyewy
MicrosoftWindows.Client.OOBE                    MicrosoftWindows.Client.OOBE_cw5n1h2txyewy
MicrosoftWindows.UndockedDevKit                 MicrosoftWindows.UndockedDevKit_cw5n1h2txyewy
Windows.CBSPreview                              Windows.CBSPreview_cw5n1h2txyewy
windows.immersivecontrolpanel                   windows.immersivecontrolpanel_cw5n1h2txyewy
Windows.PrintDialog                             Windows.PrintDialog_cw5n1h2txyewy
Microsoft.NET.Native.Framework.2.2              Microsoft.NET.Native.Framework.2.2_8wekyb3d8bbwe
Microsoft.NET.Native.Runtime.2.2                Microsoft.NET.Native.Runtime.2.2_8wekyb3d8bbwe
Microsoft.VCLibs.140.00                         Microsoft.VCLibs.140.00_8wekyb3d8bbwe
Microsoft.VCLibs.140.00                         Microsoft.VCLibs.140.00_8wekyb3d8bbwe
Microsoft.NET.Native.Runtime.2.2                Microsoft.NET.Native.Runtime.2.2_8wekyb3d8bbwe
Microsoft.NET.Native.Framework.2.2              Microsoft.NET.Native.Framework.2.2_8wekyb3d8bbwe
Microsoft.Services.Store.Engagement             Microsoft.Services.Store.Engagement_8wekyb3d8bbwe
Microsoft.Services.Store.Engagement             Microsoft.Services.Store.Engagement_8wekyb3d8bbwe
Microsoft.VCLibs.140.00.UWPDesktop              Microsoft.VCLibs.140.00.UWPDesktop_8wekyb3d8bbwe
Microsoft.VCLibs.140.00.UWPDesktop              Microsoft.VCLibs.140.00.UWPDesktop_8wekyb3d8bbwe
Microsoft.ECApp                                 Microsoft.ECApp_8wekyb3d8bbwe
Microsoft.Windows.NarratorQuickStart            Microsoft.Windows.NarratorQuickStart_8wekyb3d8bbwe
MSTeams                                         MSTeams_8wekyb3d8bbwe
MicrosoftCorporationII.QuickAssist              MicrosoftCorporationII.QuickAssist_8wekyb3d8bbwe
Microsoft.Windows.DevHome                       Microsoft.Windows.DevHome_8wekyb3d8bbwe
Microsoft.MPEG2VideoExtension                   Microsoft.MPEG2VideoExtension_8wekyb3d8bbwe
Microsoft.OutlookForWindows                     Microsoft.OutlookForWindows_8wekyb3d8bbwe
Microsoft.WindowsAppRuntime.1.5                 Microsoft.WindowsAppRuntime.1.5_8wekyb3d8bbwe
Microsoft.WindowsAppRuntime.1.5                 Microsoft.WindowsAppRuntime.1.5_8wekyb3d8bbwe
Microsoft.MicrosoftEdge.Stable                  Microsoft.MicrosoftEdge.Stable_8wekyb3d8bbwe
Microsoft.SecHealthUI                           Microsoft.SecHealthUI_8wekyb3d8bbwe
AdvancedMicroDevicesInc-2.AMDRadeonSoftware     AdvancedMicroDevicesInc-2.AMDRadeonSoftware_0a9344xs7nr4m
Microsoft.Windows.AugLoop.CBS                   Microsoft.Windows.AugLoop.CBS_8wekyb3d8bbwe
Microsoft.WebpImageExtension                    Microsoft.WebpImageExtension_8wekyb3d8bbwe
Microsoft.WebMediaExtensions                    Microsoft.WebMediaExtensions_8wekyb3d8bbwe
Microsoft.MicrosoftStickyNotes                  Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe
Microsoft.RawImageExtension                     Microsoft.RawImageExtension_8wekyb3d8bbwe
Microsoft.HEIFImageExtension                    Microsoft.HEIFImageExtension_8wekyb3d8bbwe
Microsoft.VP9VideoExtensions                    Microsoft.VP9VideoExtensions_8wekyb3d8bbwe
Microsoft.UI.Xaml.2.7                           Microsoft.UI.Xaml.2.7_8wekyb3d8bbwe
Microsoft.UI.Xaml.2.7                           Microsoft.UI.Xaml.2.7_8wekyb3d8bbwe
Microsoft.WindowsAppRuntime.1.4                 Microsoft.WindowsAppRuntime.1.4_8wekyb3d8bbwe
Microsoft.WindowsAppRuntime.1.4                 Microsoft.WindowsAppRuntime.1.4_8wekyb3d8bbwe
Microsoft.Todos                                 Microsoft.Todos_8wekyb3d8bbwe
Microsoft.WindowsAppRuntime.1.6                 Microsoft.WindowsAppRuntime.1.6_8wekyb3d8bbwe
Microsoft.ApplicationCompatibilityEnhancements  Microsoft.ApplicationCompatibilityEnhancements_8wekyb3d8bbwe
Microsoft.AV1VideoExtension                     Microsoft.AV1VideoExtension_8wekyb3d8bbwe
MicrosoftCorporationII.WindowsSubsystemForLinux MicrosoftCorporationII.WindowsSubsystemForLinux_8wekyb3d8bbwe
CanonicalGroupLimited.Ubuntu                    CanonicalGroupLimited.Ubuntu_79rhkp1fndgsc
Microsoft.Winget.Source                         Microsoft.Winget.Source_8wekyb3d8bbwe
Microsoft.StorePurchaseApp                      Microsoft.StorePurchaseApp_8wekyb3d8bbwe
Microsoft.LanguageExperiencePackko-KR           Microsoft.LanguageExperiencePackko-KR_8wekyb3d8bbwe
Microsoft.UI.Xaml.2.8                           Microsoft.UI.Xaml.2.8_8wekyb3d8bbwe
Microsoft.UI.Xaml.2.8                           Microsoft.UI.Xaml.2.8_8wekyb3d8bbwe
Microsoft.WindowsAppRuntime.1.6                 Microsoft.WindowsAppRuntime.1.6_8wekyb3d8bbwe
Microsoft.BingSearch                            Microsoft.BingSearch_8wekyb3d8bbwe
MicrosoftWindows.Client.Photon                  MicrosoftWindows.Client.Photon_cw5n1h2txyewy
Microsoft.WindowsCamera                         Microsoft.WindowsCamera_8wekyb3d8bbwe
Microsoft.WindowsAlarms                         Microsoft.WindowsAlarms_8wekyb3d8bbwe
Microsoft.DesktopAppInstaller                   Microsoft.DesktopAppInstaller_8wekyb3d8bbwe
Microsoft.AVCEncoderVideoExtension              Microsoft.AVCEncoderVideoExtension_8wekyb3d8bbwe
E046963F.LenovoCompanion                        E046963F.LenovoCompanion_k1h2ywk1493x8
Microsoft.WindowsAppRuntime.1.6                 Microsoft.WindowsAppRuntime.1.6_8wekyb3d8bbwe
Microsoft.HEVCVideoExtension                    Microsoft.HEVCVideoExtension_8wekyb3d8bbwe
DolbyLaboratories.DolbyAccess                   DolbyLaboratories.DolbyAccess_rz1tebttyb220
Microsoft.WindowsSoundRecorder                  Microsoft.WindowsSoundRecorder_8wekyb3d8bbwe
Microsoft.YourPhone                             Microsoft.YourPhone_8wekyb3d8bbwe
Microsoft.WindowsTerminal                       Microsoft.WindowsTerminal_8wekyb3d8bbwe
Microsoft.WindowsCalculator                     Microsoft.WindowsCalculator_8wekyb3d8bbwe
Microsoft.WidgetsPlatformRuntime                Microsoft.WidgetsPlatformRuntime_8wekyb3d8bbwe
Microsoft.ScreenSketch                          Microsoft.ScreenSketch_8wekyb3d8bbwe
MicrosoftWindows.CrossDevice                    MicrosoftWindows.CrossDevice_cw5n1h2txyewy
Microsoft.WindowsNotepad                        Microsoft.WindowsNotepad_8wekyb3d8bbwe
Microsoft.Paint                                 Microsoft.Paint_8wekyb3d8bbwe
Microsoft.Windows.Photos                        Microsoft.Windows.Photos_8wekyb3d8bbwe
MicrosoftWindows.Client.WebExperience           MicrosoftWindows.Client.WebExperience_cw5n1h2txyewy
Microsoft.WindowsStore                          Microsoft.WindowsStore_8wekyb3d8bbwe
Microsoft.MicrosoftOfficeHub                    Microsoft.MicrosoftOfficeHub_8wekyb3d8bbwe
Microsoft.OneDriveSync                          Microsoft.OneDriveSync_8wekyb3d8bbwe
Microsoft.Copilot                               Microsoft.Copilot_8wekyb3d8bbwe
Clipchamp.Clipchamp                             Clipchamp.Clipchamp_yxz26nhyzhsrt
Microsoft.AAD.BrokerPlugin                      Microsoft.AAD.BrokerPlugin_cw5n1h2txyewy
Microsoft.LockApp                               Microsoft.LockApp_cw5n1h2txyewy
Microsoft.Windows.ShellExperienceHost           Microsoft.Windows.ShellExperienceHost_cw5n1h2txyewy
Microsoft.Windows.StartMenuExperienceHost       Microsoft.Windows.StartMenuExperienceHost_cw5n1h2txyewy
Microsoft.WindowsAppRuntime.CBS.1.6             Microsoft.WindowsAppRuntime.CBS.1.6_8wekyb3d8bbwe
Microsoft.WindowsAppRuntime.CBS                 Microsoft.WindowsAppRuntime.CBS_8wekyb3d8bbwe
MicrosoftWindows.55182690.Taskbar               MicrosoftWindows.55182690.Taskbar_cw5n1h2txyewy
MicrosoftWindows.Client.CBS                     MicrosoftWindows.Client.CBS_cw5n1h2txyewy
MicrosoftWindows.Client.Core                    MicrosoftWindows.Client.Core_cw5n1h2txyewy
MicrosoftWindows.LKG.AccountsService            MicrosoftWindows.LKG.AccountsService_cw5n1h2txyewy
MicrosoftWindows.LKG.DesktopSpotlight           MicrosoftWindows.LKG.DesktopSpotlight_cw5n1h2txyewy
MicrosoftWindows.LKG.IrisService                MicrosoftWindows.LKG.IrisService_cw5n1h2txyewy
MicrosoftWindows.LKG.RulesEngine                MicrosoftWindows.LKG.RulesEngine_cw5n1h2txyewy
MicrosoftWindows.LKG.SpeechRuntime              MicrosoftWindows.LKG.SpeechRuntime_cw5n1h2txyewy
MicrosoftWindows.LKG.TwinSxS                    MicrosoftWindows.LKG.TwinSxS_cw5n1h2txyewy
Microsoft.PowerAutomateDesktop                  Microsoft.PowerAutomateDesktop_8wekyb3d8bbwe
Microsoft.WindowsAppRuntime.1.6                 Microsoft.WindowsAppRuntime.1.6_8wekyb3d8bbwe
Microsoft.WindowsAppRuntime.1.6                 Microsoft.WindowsAppRuntime.1.6_8wekyb3d8bbwe
```