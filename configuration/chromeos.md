# ChromeOS

## Security Policies - Recommended Values

### General Security Policies

#### Passwords and Authentication

* `PasswordManagerEnabled`: true (Enable the password manager)
* `PasswordMinimumLength`: 12 or higher (Set a strong minimum password length)
* `PasswordRequireLetters`: true (Require at least one letter in passwords)
* `PasswordRequireNumbers`: true (Require at least one number in passwords)
* `PasswordRequireSymbols`: true (Require at least one symbol in passwords)
* `PasswordRequireUpperCase`: true (Require at least one upper-case letter in passwords)
* `PasswordRequireLowerCase`: true (Require at least one lower-case letter in passwords)
* `AuthenticatorEnabled`: true (Enable two-factor authentication)

#### Updates

* `DeviceAutoUpdateDisabled`: false (Allow automatic updates for ChromeOS)

_**Content and Privacy**_

* `DeveloperToolsAvailability`: 2 (Disable developer tools for non-admin users)
* `IncognitoModeAvailability`: 1 (Disallow incognito mode)
* `SavingBrowserHistoryDisabled`: false (Enable browser history for monitoring and auditing purposes)
* `ThirdPartyCookiesBlocked`: true (Block third-party cookies for enhanced privacy)

#### Extensions and Plugins

* `DefaultBlockAllMixedContent`: true (Block mixed content)
* `ExtensionInstallBlocklist`: Provide a list of unwanted extensions to block
* `ExtensionInstallAllowlist`: Provide a list of allowed extensions
* `BlockExternalExtensions`: true (Block external extensions)
* `AllowOutdatedPlugins`: false (Disallow outdated plugins)

#### Network Security Policies

* `DeviceWiFiFastTransitionEnabled`: false (Disable Fast Transition roaming)
* `DeviceWiFiRoamingAllowed`: false (Disable Wi-Fi roaming)
* `ProxySettings`: Configure appropriate proxy settings for your network
* `DeviceOpenNetworkConfiguration`: Configure your network according to your organization's security policies

#### Device Management Policies

* `DeviceEnrollment`: Configure device enrollment settings as per your organization's requirements
* `DevicePowerManagementDisabled`: false (Enable power management)
* `DeviceScreenLock`: Set the screen lock settings for your organization
* `DevicePolicyRefreshRate`: Set an appropriate policy refresh rate for your organization

### Precise Policies

#### Remote Desktop

All Policies : `not configured or disabled`

#### Google Assistant

All Policies : `False`

#### Remote Attestation

* AttestationEnabled: `true`
* AttestationServerURL: Set a secure URL to your organization's remote attestation server.
* AttestationCACertificate: Set the CA certificate that matches the attestation server's SSL/TLS certificate.
* AttestationEnrollmentId: Set a unique enrollment ID for each device, following a secure and consistent pattern.
* AttestationEnrollmentKey: Set a unique and securely generated private key for each device.
* AttestationForContentProtectionEnabled: `true`

#### HTTP Authentication

* AuthServerWhitelist: `""` (empty)
* AuthNegotiateDelegateWhitelist: `""` (empty)
* AuthSchemes: `"basic,digest,ntlm,negotiate"`
* AuthCacheSize: `10`
* AuthNegotiateDelegateByKdcPolicy: `false`
* NtlmV2Enabled: `true`
* AllowCrossOriginAuthPrompt: `false`
* BasicAuthOverHttpEnabled: `false`

#### Linux Container

* AllowRunningInsecureContent: `false`
* DefaultCookiesSetting: `2` (Block third-party cookies)
* DefaultGeolocationSetting: `2` (Block)
* DefaultImagesSetting: `1` (Allow)
* DefaultJavaScriptSetting: `1` (Allow)
* DefaultPluginsSetting: `1` (Allow)
* DefaultPopupsSetting: `2` (Block)
* DeveloperToolsAvailability: `1` (Disallow)
* ExtensionInstallBlocklist: `['*']` (Block all extensions)
* ForceEphemeralProfiles: `true`
* GuestModeEnabled: `false`
* IncognitoModeAvailability: `1` (Disallow)
* MaxConnectionsPerProxy: (Choose a reasonable limit based on your network requirements)
* PasswordManagerEnabled: `true`
* SafeBrowsingEnabled: `true`
* SameSiteByDefaultCookies: `true`
* TranslateEnabled: `false` (Disable if translation services are not needed)
* URLBlocklist: `[list of URLs to block]` (Customize according to your organization's requirements)
* URLWhitelist: `[list of URLs to allow]` (Customize according to your organization's requirements)

#### MISC

* AccountManagerEnabled: `false`
* ArcEnabled: `false`
* WakeOnWifiEnabled: `false`
* AssistantDisabled: `true`
* FastPairEnabled: `false`
* BrowserSwitcherEnabled: `false`
* ImportEnterpriseRoots: `true`
* BrowserNetworkTimeEnabled: `true`
* DeviceAutoUpdateTimeRestrictions: `{ "allowed_auto_update_days": [] }`
* CrosHealthdTelemetry: `{ "type": "disabled" }`
* CryptAuthDeviceSyncAllowed: `true`
* DeviceQuirksDownloadEnabled: `false`
* DeviceStateReportDevice: `true`
* DeviceStateReportSession: `true`
* DeviceStateReportUser: `true`
* DataLeakPreventionRulesList: `[]` (Empty list, meaning no rules are allowed)
* EasyUnlockAllowed: `false`
* RuntimeBlockedHosts: `{ "values": ["*"] }`
* FamilyLinkDisabled: `false`
* FeedbackAllowed: `true`
* FineGrainedTimeZoneResolveEnabled: `true`
* GCMChannelStatus: `{ "gcm_channel_status": false }`
* DefaultGeolocationSetting: `2`
* KioskEnabled: `false`
* LoginScreenIsolateOrigins: `{ "origins": [] }`
* MediaRouterEnabled: false
* DeviceNativePrintersBlacklist: `{ "blacklist": [] }`
* NetworkPredictionOptions: `2`
* NtpEnabled: `true`
* OAuth2ClientAppBlocklist: `{ "blocklist": [] }`
* KeyPermissions: `{ "policy": [] }`
* AllowOutdatedPlugins: false
* AllowedCloudPrinters: `{ "allowed_printers": [] }`
* QuickUnlockModeWhitelist: `[]`
* DeviceReportingEnabled: true
* SafeBrowsingEnabled: `true`
* SamePartitionDomainRelaxingEnabled: `false`
* SignInAllowed: `true`
* SignInToSecondaryAccountsAllowed: `false`
* SyncDisabled: `false`
* SystemTimezoneAutomaticDetection: `3`
* TetherAllowed: `false`
* TimeZoneResolverEnabled: `true`
* TimeZoneResolverEnabled: `true`
* WebUsbAllowDevicesForUrls: `[]`
* WifiRoamingEnabled: `true`

#### Extensions

* ExtensionInstallBlacklist : `["*"]`
* ExtensionInstallWhitelist : `["extension_id1", "extension_id2", ...]`
* ExtensionInstallSources : `["https://clients2.google.com/service/update2/crx"]`
* ExtensionAllowedTypes : `["extension", "theme"]`
* DefaultExtensionsSetting : 3
* ExtensionSettings :

```json
"extension_id1": {
    "installation_mode": "blocked",
    "runtime_blocked_hosts": ["*"],
    "runtime_allowed_hosts": ["https://*.example.com"]
  },
  "extension_id2": {
    "installation_mode": "allowed",
    "runtime_blocked_hosts": ["*"],
    "runtime_allowed_hosts": ["https://*.example.com"]
  }
```

* ExtensionUpdate : `1`
* DeviceAutoUpdateSettings :

```json
{
  "RestrictParameter": "restrict",
  "TargetVersionPrefix": "92."
}
```

* SitePerProcess : `true`
* InsecureContentAllowedForUrls : `[]`
* InsecureContentBlockedForUrls : `["*"]`
* DeveloperToolsDisabled : `true`
* DeveloperToolsAvailability : `1`

#### Power Management

* ACIdleAction : `0`
* ACIdleDelay : `1800`
* BatteryIdleAction : `2`
* BatteryIdleDelay : `900`
* LidCloseAction : `2`
* PresentationIdleAction : `1`
* PresentationIdleDelay : `300`
* UserActivityScreenDimDelay : `120`
* UserActivityScreenDimScaled : `true`
* UserActivityScreenOffDelay : `600`
* UserActivityScreenOffScaled : `true`
* WakeOnLanEnabled : `false`

#### Creation of reports on users and devices

> General recommendations, as some policies require customization based on the organization's requirements. Generally, all the policies should be set to : true.

* DeviceStateReportDevice: `true`
* DeviceStateReportSession: `true`
* DeviceStateReportUser: `true`
* DeviceMetricsReportingEnabled: `false`
* ReportDeviceVersionInfo: `true`
* ReportDeviceActivityTimes: `true`
* ReportDeviceBootMode: `true`
* ReportDeviceNetworkInterfaces: `true`
* ReportDeviceUsers: `true`
* ReportDeviceHardwareStatus: `true`
* ReportDeviceSecurityStatus: `true`
* ReportDeviceSessionStatus: `true`
* ReportDevicePerformanceData: `true`
* HeartbeatEnabled: `true`

#### Start and Stop

* DeviceLoginScreenPowerManagement : `{ "AC": { "idle_action": "DoNothing", "delay": "1800000", "idle_warning_delay": "60000" }, "Battery": { "idle_action": "DoNothing", "delay": "1800000", "idle_warning_delay": "60000" } }`
* DeviceRebootOnShutdown : `true`
* UptimeLimit : `43200` (12 hours)

#### Quick Unlock

* PinUnlockAutosubmitEnabled: `false`
* PinUnlockMaximumLength: `16`
* PinUnlockMinimumLength: `6`
* PinUnlockWeakPinsAllowed: `false`
* QuickUnlockModeAllowlist: `[]`
* QuickUnlockTimeout: `0`

#### Password Manager

* PasswordDismissCompromisedAlertEnabled: `true`
* PasswordLeakDetectionEnabled: `true`
* PasswordManagerEnabled: `false`

#### Google Drive

* DriveDisabled: `true`
* DriveDisabledOverCellular: `true`

#### Printing

* UserNativePrintDialog: `true`
* UserDestinationSearchEnabled: `false`
* UserDestinationSearchManaged: `true`
* UserManualDuplexMode: `true`
* UserScreenshotsDisabled: `true`

#### Kerberos

* KerberosEnabled: `true`
* KerberosKeytabFiles: `/etc/krb5.keytab`
* KerberosRealm: `EXAMPLE.COM`
* KerberosServers: `kdc.example.com`
* KerberosUserPrincipalSuffix: `@example.com`

#### Legacy Browser Support

* AlternativeBrowserParameters: `--disable-logging --disable-plugins`
* AlternativeBrowserPath: `/usr/bin/firefox`
* BrowserSwitcherChromeParameters: `--disable-logging --disable-plugins`
* BrowserSwitcherChromePath: `/usr/bin/chromium-browser`
* BrowserSwitcherDelay: `5`
* BrowserSwitcherEnabled: `true`
* BrowserSwitcherExternalGreylistUrl: `""`
* BrowserSwitcherExternalSitelistUrl: `""`
* BrowserSwitcherKeepLastChromeTab: `true`
* BrowserSwitcherParsingMode: `URL`
* BrowserSwitcherUrlGreylist: `""`
* BrowserSwitcherUrlList: `""`
* BrowserSwitcherUseIeSitelist: `false`

#### Native Messaging

* NativeMessagingAllowlist: `[]`
* NativeMessagingBlocklist: `[]`
* NativeMessagingUserLevelHosts: `[]`

#### Android Settings

* AppRecommendationZeroStateEnabled: `false`
* ArcAppInstallEventLoggingEnabled: `false`
* ArcAppToWebAppSharingEnabled: `false`
* ArcBackupRestoreEnabled: `false`
* ArcBackupRestoreServiceEnabled: `false`
* ArcCertificatesSyncMode: `disabled`
* ArcEnabled: `false`
* ArcGoogleLocationServicesEnabled: `false`
* ArcLocationServiceEnabled: `false`
* ArcPolicy: `enabled` (depends on the organization)
* DeviceArcDataSnapshotHours: `0`
* UnaffiliatedArcAllowed: `false`

#### Connection Settings

* DeviceAllowNewUsers: `false`
* DeviceAutofillSAMLUsername: `false`
* DeviceEphemeralUsersEnabled: `false`
* DeviceFamilyLinkAccountsAllowed: `false`
* DeviceGuestModeEnabled: `false`
* DeviceLoginScreenAutoSelectCertificateForUrls: `false`
* DeviceLoginScreenDomainAutoComplete: `false`
* DeviceLoginScreenExtensions: `false`
* DeviceLoginScreenInputMethods: `false`
* DeviceLoginScreenIsolateOrigins: `true`
* DeviceLoginScreenLocales: `en-US`
* DeviceLoginScreenPromptOnMultipleMatchingCertificates: `false`
* DeviceLoginScreenSitePerProcess: `true`
* DeviceLoginScreenSystemInfoEnforced: `true`
* DeviceRunAutomaticCleanupOnLogin: `true`
* DeviceSecondFactorAuthentication:
* DeviceShowNumericKeyboardForPassword: `true`
* DeviceShowUserNamesOnSignin: `false`
* DeviceStartUpFlags: `--disable-logging --disable-login-animations --disable-background-timer-throttling`
* DeviceTransferSAMLCookies: `false`
* DeviceUserAllowlist:
* DeviceWallpaperImage:
* LoginAuthenticationBehavior: `1`
* LoginVideoCaptureAllowedUrls:
* RecoveryFactorBehavior: `false`

#### Certificate management settings

* RequiredClientCertificateForDevice: `true`
* RequiredClientCertificateForUser: `true`

#### Kiosk settings

* AllowKioskAppControlChromeVersion: `false`
* DeviceLocalAccountAutoLoginBailoutEnabled: `false`
* DeviceLocalAccountAutoLoginDelay: `5`
* DeviceLocalAccountAutoLoginId: `""`
* DeviceLocalAccountPromptForNetworkWhenOffline: `false`
* DeviceLocalAccounts: `""`

#### Privacy Screen Settings

* DeviceLoginScreenPrivacyScreenEnabled: `true`
* PrivacyScreenEnabled: `true`

#### Network File Sharing feature settings

* NTLMShareAuthenticationEnabled: `false`
* NetBiosShareDiscoveryEnabled: `false`
* NetworkFileSharesAllowed: `false`
* NetworkFileSharesPreconfiguredShares: `<empty>`

## Official ChromeOS Policy Documentation

For a comprehensive list of ChromeOS policies and their descriptions, please refer to the official ChromeOS policy documentation:&#x20;

{% embed url="https://support.google.com/chrome/a/answer/9102677" %}

Remember to consult your organization's security policies and local regulations to determine the most appropriate settings for your specific context.
