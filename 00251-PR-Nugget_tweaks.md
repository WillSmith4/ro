# Nugget tweaks

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#251](https://github.com/rooootdev/lara/pull/251)  
> **State:** ⚪ draft  
> **Branch:** `jurre111:main` → `rooootdev:main`  
> **Changes:** +0 / -0 across 0 files  
> **Created:** May 17, 2026  
> **Updated:** May 18, 2026  
> **Closed:** May 18, 2026  

---

## Description

*Opened by [@jurre111](https://github.com/jurre111)* *on May 17, 2026*


NuggetView contains all SB and internal tweaks from nugget inside a dirtyZero like UI


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ✅ [release](https://github.com/rooootdev/lara/actions/runs/26050322163/job/76585228926) — **success** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/26050322163/job/76585019314) — **success** — *GitHub Actions*


---

## Reviews (1)

### 💬 COMMENTED — [@copilot-pull-request-reviewer[bot]](https://github.com/apps/copilot-pull-request-reviewer) · May 17, 2026

## Pull request overview

Adds a new “Nugget” tweaks screen to the app’s Tweaks area, exposing a set of SpringBoard/system/internal plist-based toggles (ported from Nugget) and introducing plist read/write helpers in `laramgr` to support these operations.

**Changes:**
- Added a new `NuggetView` with categorized toggles, plus Apply/Reset actions that write/remove keys in various plist files.
- Exposed `setplistvalue` / `getplistvalue` helper APIs in `laramgr` to read and update plist keys via the existing overwrite mechanism.
- Wired `NuggetView` into the Tweaks navigation list (disabled until sbx/vfs are ready).

### Reviewed changes

Copilot reviewed 3 out of 3 changed files in this pull request and generated 5 comments.

| File | Description |
| ---- | ----------- |
| `lara/views/tweaks/TweaksView.swift` | Adds navigation entry to open the new Nugget tweaks screen. |
| `lara/views/tweaks/NuggetView.swift` | New UI + logic for reading/applying/resetting plist-backed toggles. |
| `lara/classes/laramgr.swift` | Adds plist getter/setter helpers used by `NuggetView`. |






---

💡 <a href="/rooootdev/lara/new/main?filename=.github/instructions/*.instructions.md" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Add Copilot custom instructions</a> for smarter, more guided reviews. <a href="https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Learn how to get started</a>.


---

## Review Comments (5)

*Inline comments on code changes*

### `lara/views/tweaks/NuggetView.swift`

**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · May 17, 2026 (line 6) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -0,0 +1,228 @@
+//
+//  MiscView.swift
+//  lara
+//
+//  Created by jurre111 on 14.04.26.
+//  Mostly copied from/inspired by nugget from leminlimez
```

</details>

File header comment still says "MiscView.swift" even though the file is NuggetView.swift; this is misleading when navigating/searching the project. Update the header to reflect the correct filename/view name.


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · May 17, 2026 (line 60) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -0,0 +1,228 @@
+//
+//  MiscView.swift
+//  lara
+//
+//  Created by jurre111 on 14.04.26.
+//  Mostly copied from/inspired by nugget from leminlimez
+//
+
+import SwiftUI
+
+// ported from nugget, but kinda unnecessary
+enum FileLocation: String, CaseIterable {
+    // Mobile Gestalt
+    // case resolution = "/var/Managed Preferences/mobile/com.apple.iokit.IOMobileGraphicsFamily.plist"
+    // case mga = "/var/containers/Shared/SystemGroup/systemgroup.com.apple.mobilegestaltcache/Library/Caches/com.apple.MobileGestalt.plist"
+
+    // Feature Flags
+    // case featureflags = "/var/preferences/FeatureFlags/Global.plist"
+    
+    // Springboard Options
+    case springboard = "/var/Managed Preferences/mobile/com.apple.springboard.plist"
+    case footnote = "/var/containers/Shared/SystemGroup/systemgroup.com.apple.configurationprofiles/Library/ConfigurationProfiles/SharedDeviceConfiguration.plist"
+    case airdrop = "/var/Managed Preferences/mobile/com.apple.sharingd.plist"
+    case nanoregistry = "/var/mobile/Library/Preferences/com.apple.NanoRegistry.plist"
+    
+    // Internal Options
+    case globalPreferences = "/var/Managed Preferences/mobile/.GlobalPreferences.plist"
+    case appStore = "/var/Managed Preferences/mobile/com.apple.AppStore.plist"
+    case backboardd = "/var/Managed Preferences/mobile/com.apple.backboardd.plist"
+    case coreMotion = "/var/Managed Preferences/mobile/com.apple.CoreMotion.plist"
+    case pasteboard = "/var/Managed Preferences/mobile/com.apple.Pasteboard.plist"
+    case notes = "/var/Managed Preferences/mobile/com.apple.mobilenotes.plist"
+    case uikit = "/var/Managed Preferences/mobile/com.apple.UIKit.plist"
+
+    // Daemons
+    // case disabledDaemons = "/var/db/com.apple.xpc.launchd/disabled.plist"
+    // case screentime = "/var/mobile/Library/Preferences/ScreenTimeAgent.plist"
+
+    // Risky Options
+    // case ota = "/var/Managed Preferences/mobile/com.apple.MobileAsset.plist"
+}
+
+// this only works for keys that are false by default as of now
+struct tweak: Identifiable {
+    var id: String { key }
+    var name: String
+    var info: String?
+    var path: String
+    var key: String
+    var state: (og: Bool, enabled: Bool) = (false, false)
+    var readfailed: Bool = false
+}
+
+struct tweaksection: Identifiable {
+    var id: String { name }
+    var name: String
+    var icon: String
+    var isexpanded: Bool = true
+    var tweaks: [tweak]
+}
```

</details>

New model types are declared as `tweak` / `tweaksection` (lowercase). Swift type names are consistently UpperCamelCase elsewhere in this codebase (e.g., `ZeroTweak`, `ZeroSection`), so this is likely unintentional and makes the API harder to read. Consider renaming to `Tweak` / `TweakSection` (and similarly `isexpanded` -> `isExpanded`).


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · May 17, 2026 (line 88) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -0,0 +1,228 @@
+//
+//  MiscView.swift
+//  lara
+//
+//  Created by jurre111 on 14.04.26.
+//  Mostly copied from/inspired by nugget from leminlimez
+//
+
+import SwiftUI
+
+// ported from nugget, but kinda unnecessary
+enum FileLocation: String, CaseIterable {
+    // Mobile Gestalt
+    // case resolution = "/var/Managed Preferences/mobile/com.apple.iokit.IOMobileGraphicsFamily.plist"
+    // case mga = "/var/containers/Shared/SystemGroup/systemgroup.com.apple.mobilegestaltcache/Library/Caches/com.apple.MobileGestalt.plist"
+
+    // Feature Flags
+    // case featureflags = "/var/preferences/FeatureFlags/Global.plist"
+    
+    // Springboard Options
+    case springboard = "/var/Managed Preferences/mobile/com.apple.springboard.plist"
+    case footnote = "/var/containers/Shared/SystemGroup/systemgroup.com.apple.configurationprofiles/Library/ConfigurationProfiles/SharedDeviceConfiguration.plist"
+    case airdrop = "/var/Managed Preferences/mobile/com.apple.sharingd.plist"
+    case nanoregistry = "/var/mobile/Library/Preferences/com.apple.NanoRegistry.plist"
+    
+    // Internal Options
+    case globalPreferences = "/var/Managed Preferences/mobile/.GlobalPreferences.plist"
+    case appStore = "/var/Managed Preferences/mobile/com.apple.AppStore.plist"
+    case backboardd = "/var/Managed Preferences/mobile/com.apple.backboardd.plist"
+    case coreMotion = "/var/Managed Preferences/mobile/com.apple.CoreMotion.plist"
+    case pasteboard = "/var/Managed Preferences/mobile/com.apple.Pasteboard.plist"
+    case notes = "/var/Managed Preferences/mobile/com.apple.mobilenotes.plist"
+    case uikit = "/var/Managed Preferences/mobile/com.apple.UIKit.plist"
+
+    // Daemons
+    // case disabledDaemons = "/var/db/com.apple.xpc.launchd/disabled.plist"
+    // case screentime = "/var/mobile/Library/Preferences/ScreenTimeAgent.plist"
+
+    // Risky Options
+    // case ota = "/var/Managed Preferences/mobile/com.apple.MobileAsset.plist"
+}
+
+// this only works for keys that are false by default as of now
+struct tweak: Identifiable {
+    var id: String { key }
+    var name: String
+    var info: String?
+    var path: String
+    var key: String
+    var state: (og: Bool, enabled: Bool) = (false, false)
+    var readfailed: Bool = false
+}
+
+struct tweaksection: Identifiable {
+    var id: String { name }
+    var name: String
+    var icon: String
+    var isexpanded: Bool = true
+    var tweaks: [tweak]
+}
+
+
+struct NuggetView: View {
+    @ObservedObject var mgr: laramgr
+    let fm = FileManager.default
+    @State private var tweaks: [tweaksection] = [
+        tweaksection(name: "User Interface", icon: "eye", tweaks: [
+            tweak(name: "Hide Dynamic Island completely", path: FileLocation.springboard.rawValue, key: "SBSuppressDynamicIslandCompletely"),
+            tweak(name: "Show Red/Green Authentication Line on Lock Screen", path: FileLocation.springboard.rawValue, key: "SBExtendedDisplayOverrideSupportForAirPlayAndDontFileRadars"),
+            tweak(name: "Show Build Version in Status Bar", path: FileLocation.globalPreferences.rawValue, key: "UIStatusBarShowBuildVersion"),
+            tweak(name: "Force Right-to-Left Layout", path: FileLocation.globalPreferences.rawValue, key: "NSForceRightToLeftWritingDirection"),
+            tweak(name: "Force Left-to-Right Layout", path: FileLocation.globalPreferences.rawValue, key: "NSForceLeftToRightWritingDirection"),
+            tweak(name: "Keyboard Character Flick", info: "Enables the iPad-style keyboard keyflicks on iPhones.", path: FileLocation.globalPreferences.rawValue, key: "GesturesEnabled"),
+            tweak(name: "Disable Clock Icon Seconds Hand", path: FileLocation.globalPreferences.rawValue, key: "SBDisableClockIconSecondsHand"),
+            tweak(name: "Show Hardware Button Hints in Screenshots", path: FileLocation.globalPreferences.rawValue, key: "SBHardwareButtonHintDropletsAlwaysVisibleInSnapshots"),
+            tweak(name: "Hide Respring Icon", path: FileLocation.backboardd.rawValue, key: "BKHideAppleLogoOnLaunch"),
+            tweak(name: "Disable Breadcrumbs", info: "Removes '< PreviousAppName' glyph in Status Bar when being forwarded to another app.", path: FileLocation.springboard.rawValue, key: "SBNeverBreadcrumb"),
+            tweak(name: "Show Supervision Text on Lock Screen", info: "Shows info about the device supervision status and organization at the bottom of the lock screen.", path: FileLocation.springboard.rawValue, key: "SBShowSupervisionTextOnLockScreen")
+        ]),
+        tweaksection(name: "System", icon: "gear", tweaks: [
+            tweak(name: "Disable AirDrop Time Limit for Everyone Option", path: FileLocation.airdrop.rawValue, key: "OverrideTimeLimitEveryoneMode"),
+            tweak(name: "Disable Lock After Respring", path: FileLocation.springboard.rawValue, key: "SBDontLockAfterCrash"),
+            tweak(name: "Disable screen dimming while charging", path: FileLocation.springboard.rawValue, key: "SBDontDimOrLockOnAC"),
+            tweak(name: "Disable low battery alerts", path: FileLocation.springboard.rawValue, key: "SBHideLowPowerAlerts"),
+            tweak(name: "Hide AC Power on Lock Screen", path: FileLocation.springboard.rawValue, key: "SBHideACPower"),
+            tweak(name: "Show Dynamic Island in Screenshots", path: FileLocation.springboard.rawValue, key: "SBAlwaysShowSystemApertureInSnapshots"),
+            tweak(name: "Enable AirPlay support for Stage Manager", path: FileLocation.springboard.rawValue, key: "SBExtendedDisplayOverrideSupportForAirPlayAndDontFileRadars"),
+            tweak(name: "Show Hidden Icons on Home Screen", info: "Shows hidden apps like Field Test and Continuity on the home screen.", path: FileLocation.globalPreferences.rawValue, key: "SBIconVisibility"),
```

</details>

Two different toggles use the same `key` ("SBExtendedDisplayOverrideSupportForAirPlayAndDontFileRadars") which also means they share the same SwiftUI `id` (`tweak.id` is `key`). This will cause duplicate IDs in `ForEach` (undefined SwiftUI behavior) and `apply()` can end up undoing the user’s selection because one row may set the key while the other removes it depending on ordering. Ensure each row has a unique identifier and avoid having multiple UI controls that write the same plist key (or consolidate them into a single toggle).


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · May 17, 2026 (line 136) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -0,0 +1,228 @@
+//
+//  MiscView.swift
+//  lara
+//
+//  Created by jurre111 on 14.04.26.
+//  Mostly copied from/inspired by nugget from leminlimez
+//
+
+import SwiftUI
+
+// ported from nugget, but kinda unnecessary
+enum FileLocation: String, CaseIterable {
+    // Mobile Gestalt
+    // case resolution = "/var/Managed Preferences/mobile/com.apple.iokit.IOMobileGraphicsFamily.plist"
+    // case mga = "/var/containers/Shared/SystemGroup/systemgroup.com.apple.mobilegestaltcache/Library/Caches/com.apple.MobileGestalt.plist"
+
+    // Feature Flags
+    // case featureflags = "/var/preferences/FeatureFlags/Global.plist"
+    
+    // Springboard Options
+    case springboard = "/var/Managed Preferences/mobile/com.apple.springboard.plist"
+    case footnote = "/var/containers/Shared/SystemGroup/systemgroup.com.apple.configurationprofiles/Library/ConfigurationProfiles/SharedDeviceConfiguration.plist"
+    case airdrop = "/var/Managed Preferences/mobile/com.apple.sharingd.plist"
+    case nanoregistry = "/var/mobile/Library/Preferences/com.apple.NanoRegistry.plist"
+    
+    // Internal Options
+    case globalPreferences = "/var/Managed Preferences/mobile/.GlobalPreferences.plist"
+    case appStore = "/var/Managed Preferences/mobile/com.apple.AppStore.plist"
+    case backboardd = "/var/Managed Preferences/mobile/com.apple.backboardd.plist"
+    case coreMotion = "/var/Managed Preferences/mobile/com.apple.CoreMotion.plist"
+    case pasteboard = "/var/Managed Preferences/mobile/com.apple.Pasteboard.plist"
+    case notes = "/var/Managed Preferences/mobile/com.apple.mobilenotes.plist"
+    case uikit = "/var/Managed Preferences/mobile/com.apple.UIKit.plist"
+
+    // Daemons
+    // case disabledDaemons = "/var/db/com.apple.xpc.launchd/disabled.plist"
+    // case screentime = "/var/mobile/Library/Preferences/ScreenTimeAgent.plist"
+
+    // Risky Options
+    // case ota = "/var/Managed Preferences/mobile/com.apple.MobileAsset.plist"
+}
+
+// this only works for keys that are false by default as of now
+struct tweak: Identifiable {
+    var id: String { key }
+    var name: String
+    var info: String?
+    var path: String
+    var key: String
+    var state: (og: Bool, enabled: Bool) = (false, false)
+    var readfailed: Bool = false
+}
+
+struct tweaksection: Identifiable {
+    var id: String { name }
+    var name: String
+    var icon: String
+    var isexpanded: Bool = true
+    var tweaks: [tweak]
+}
+
+
+struct NuggetView: View {
+    @ObservedObject var mgr: laramgr
+    let fm = FileManager.default
+    @State private var tweaks: [tweaksection] = [
+        tweaksection(name: "User Interface", icon: "eye", tweaks: [
+            tweak(name: "Hide Dynamic Island completely", path: FileLocation.springboard.rawValue, key: "SBSuppressDynamicIslandCompletely"),
+            tweak(name: "Show Red/Green Authentication Line on Lock Screen", path: FileLocation.springboard.rawValue, key: "SBExtendedDisplayOverrideSupportForAirPlayAndDontFileRadars"),
+            tweak(name: "Show Build Version in Status Bar", path: FileLocation.globalPreferences.rawValue, key: "UIStatusBarShowBuildVersion"),
+            tweak(name: "Force Right-to-Left Layout", path: FileLocation.globalPreferences.rawValue, key: "NSForceRightToLeftWritingDirection"),
+            tweak(name: "Force Left-to-Right Layout", path: FileLocation.globalPreferences.rawValue, key: "NSForceLeftToRightWritingDirection"),
+            tweak(name: "Keyboard Character Flick", info: "Enables the iPad-style keyboard keyflicks on iPhones.", path: FileLocation.globalPreferences.rawValue, key: "GesturesEnabled"),
+            tweak(name: "Disable Clock Icon Seconds Hand", path: FileLocation.globalPreferences.rawValue, key: "SBDisableClockIconSecondsHand"),
+            tweak(name: "Show Hardware Button Hints in Screenshots", path: FileLocation.globalPreferences.rawValue, key: "SBHardwareButtonHintDropletsAlwaysVisibleInSnapshots"),
+            tweak(name: "Hide Respring Icon", path: FileLocation.backboardd.rawValue, key: "BKHideAppleLogoOnLaunch"),
+            tweak(name: "Disable Breadcrumbs", info: "Removes '< PreviousAppName' glyph in Status Bar when being forwarded to another app.", path: FileLocation.springboard.rawValue, key: "SBNeverBreadcrumb"),
+            tweak(name: "Show Supervision Text on Lock Screen", info: "Shows info about the device supervision status and organization at the bottom of the lock screen.", path: FileLocation.springboard.rawValue, key: "SBShowSupervisionTextOnLockScreen")
+        ]),
+        tweaksection(name: "System", icon: "gear", tweaks: [
+            tweak(name: "Disable AirDrop Time Limit for Everyone Option", path: FileLocation.airdrop.rawValue, key: "OverrideTimeLimitEveryoneMode"),
+            tweak(name: "Disable Lock After Respring", path: FileLocation.springboard.rawValue, key: "SBDontLockAfterCrash"),
+            tweak(name: "Disable screen dimming while charging", path: FileLocation.springboard.rawValue, key: "SBDontDimOrLockOnAC"),
+            tweak(name: "Disable low battery alerts", path: FileLocation.springboard.rawValue, key: "SBHideLowPowerAlerts"),
+            tweak(name: "Hide AC Power on Lock Screen", path: FileLocation.springboard.rawValue, key: "SBHideACPower"),
+            tweak(name: "Show Dynamic Island in Screenshots", path: FileLocation.springboard.rawValue, key: "SBAlwaysShowSystemApertureInSnapshots"),
+            tweak(name: "Enable AirPlay support for Stage Manager", path: FileLocation.springboard.rawValue, key: "SBExtendedDisplayOverrideSupportForAirPlayAndDontFileRadars"),
+            tweak(name: "Show Hidden Icons on Home Screen", info: "Shows hidden apps like Field Test and Continuity on the home screen.", path: FileLocation.globalPreferences.rawValue, key: "SBIconVisibility"),
+            tweak(name: "Disable Spotlight Searching in Websites", path: FileLocation.globalPreferences.rawValue, key: "SBSearchDisabledDomains"),
+            tweak(name: "Vibrate on Raise-to-Wake", path: FileLocation.coreMotion.rawValue, key: "EnableWakeGestureHaptic"),
+            tweak(name: "Play Sound on Paste", path: FileLocation.pasteboard.rawValue, key: "PlaySoundOnPaste"),
+            tweak(name: "Show Notifications for System Pastes", path: FileLocation.pasteboard.rawValue, key: "AnnounceAllPastes")
+        ]),
+        tweaksection(name: "Debugging", icon: "ladybug.fill", tweaks: [
+            tweak(name: "Metal HUD Debug", path: FileLocation.globalPreferences.rawValue, key: "MetalForceHudEnabled"),
+            tweak(name: "iMessage Debugging", path: FileLocation.globalPreferences.rawValue, key: "iMessageDiagnosticsEnabled"),
+            tweak(name: "Continuity Debugging", path: FileLocation.globalPreferences.rawValue, key: "IDSDiagnosticsEnabled"),
+            tweak(name: "FaceTime Debugging", path: FileLocation.globalPreferences.rawValue, key: "VCDiagnosticsEnabled"),
+            tweak(name: "Show Accessory Developer Settings", path: FileLocation.globalPreferences.rawValue, key: "AccessoryDeveloperEnabled"),
+            tweak(name: "App Store Debug Gesture", path: FileLocation.appStore.rawValue, key: "debugGestureEnabled"),
+            tweak(name: "Notes Debug Mode", path: FileLocation.notes.rawValue, key: "DebugModeEnabled"),
+            tweak(name: "Show Touches With Debug Info", path: FileLocation.backboardd.rawValue, key: "BKDigitizerVisualizeTouches"),
+        ])
+    ]
+
+    var body: some View {
+        NavigationStack {
+            List {
+                Section(header: HeaderLabel(text: "Applying", icon: "checkmark"), footer: Text("After applying, please fully reboot your device to actually see the changes. These tweaks were copied from [Nugget](https://github.com/leminlimez/Nugget) and ported to work with DarkSword.")) {
+                    Button("Apply tweaks") {
+                        apply()
+                    }
+                    Button("Reset to default") {
+                        revert()
+                    }
+                }
+                tweaksui
+            }
+            .navigationTitle("Nugget")
+            .refreshable {
+                load()
+            }
+            .onAppear {
+                load()
+            }
+        }
+    }
+
+    // yes this is copied from dirtyZeroView, the whole view is
+    private var tweaksui: some View {
+        ForEach($tweaks) { $section in
+            Section(header: HeaderDropdown(text: section.name, icon: section.icon, isExpanded: $section.isexpanded, useItemCount: true, itemCount: section.tweaks.count)) {
+                if section.isexpanded {
+                    ForEach($section.tweaks) { $tweak in
+                        if let tweakinfo = tweak.info {
+                            PlainToggle(text: tweak.name, infoType: .info, infoTitle: "Tweak infomation", infoMessage: tweakinfo, isOn: $tweak.state.enabled)
```

</details>

Typo in the info alert title: "Tweak infomation" should be "Tweak information".


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · May 17, 2026 (line 164) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -0,0 +1,228 @@
+//
+//  MiscView.swift
+//  lara
+//
+//  Created by jurre111 on 14.04.26.
+//  Mostly copied from/inspired by nugget from leminlimez
+//
+
+import SwiftUI
+
+// ported from nugget, but kinda unnecessary
+enum FileLocation: String, CaseIterable {
+    // Mobile Gestalt
+    // case resolution = "/var/Managed Preferences/mobile/com.apple.iokit.IOMobileGraphicsFamily.plist"
+    // case mga = "/var/containers/Shared/SystemGroup/systemgroup.com.apple.mobilegestaltcache/Library/Caches/com.apple.MobileGestalt.plist"
+
+    // Feature Flags
+    // case featureflags = "/var/preferences/FeatureFlags/Global.plist"
+    
+    // Springboard Options
+    case springboard = "/var/Managed Preferences/mobile/com.apple.springboard.plist"
+    case footnote = "/var/containers/Shared/SystemGroup/systemgroup.com.apple.configurationprofiles/Library/ConfigurationProfiles/SharedDeviceConfiguration.plist"
+    case airdrop = "/var/Managed Preferences/mobile/com.apple.sharingd.plist"
+    case nanoregistry = "/var/mobile/Library/Preferences/com.apple.NanoRegistry.plist"
+    
+    // Internal Options
+    case globalPreferences = "/var/Managed Preferences/mobile/.GlobalPreferences.plist"
+    case appStore = "/var/Managed Preferences/mobile/com.apple.AppStore.plist"
+    case backboardd = "/var/Managed Preferences/mobile/com.apple.backboardd.plist"
+    case coreMotion = "/var/Managed Preferences/mobile/com.apple.CoreMotion.plist"
+    case pasteboard = "/var/Managed Preferences/mobile/com.apple.Pasteboard.plist"
+    case notes = "/var/Managed Preferences/mobile/com.apple.mobilenotes.plist"
+    case uikit = "/var/Managed Preferences/mobile/com.apple.UIKit.plist"
+
+    // Daemons
+    // case disabledDaemons = "/var/db/com.apple.xpc.launchd/disabled.plist"
+    // case screentime = "/var/mobile/Library/Preferences/ScreenTimeAgent.plist"
+
+    // Risky Options
+    // case ota = "/var/Managed Preferences/mobile/com.apple.MobileAsset.plist"
+}
+
+// this only works for keys that are false by default as of now
+struct tweak: Identifiable {
+    var id: String { key }
+    var name: String
+    var info: String?
+    var path: String
+    var key: String
+    var state: (og: Bool, enabled: Bool) = (false, false)
+    var readfailed: Bool = false
+}
+
+struct tweaksection: Identifiable {
+    var id: String { name }
+    var name: String
+    var icon: String
+    var isexpanded: Bool = true
+    var tweaks: [tweak]
+}
+
+
+struct NuggetView: View {
+    @ObservedObject var mgr: laramgr
+    let fm = FileManager.default
+    @State private var tweaks: [tweaksection] = [
+        tweaksection(name: "User Interface", icon: "eye", tweaks: [
+            tweak(name: "Hide Dynamic Island completely", path: FileLocation.springboard.rawValue, key: "SBSuppressDynamicIslandCompletely"),
+            tweak(name: "Show Red/Green Authentication Line on Lock Screen", path: FileLocation.springboard.rawValue, key: "SBExtendedDisplayOverrideSupportForAirPlayAndDontFileRadars"),
+            tweak(name: "Show Build Version in Status Bar", path: FileLocation.globalPreferences.rawValue, key: "UIStatusBarShowBuildVersion"),
+            tweak(name: "Force Right-to-Left Layout", path: FileLocation.globalPreferences.rawValue, key: "NSForceRightToLeftWritingDirection"),
+            tweak(name: "Force Left-to-Right Layout", path: FileLocation.globalPreferences.rawValue, key: "NSForceLeftToRightWritingDirection"),
+            tweak(name: "Keyboard Character Flick", info: "Enables the iPad-style keyboard keyflicks on iPhones.", path: FileLocation.globalPreferences.rawValue, key: "GesturesEnabled"),
+            tweak(name: "Disable Clock Icon Seconds Hand", path: FileLocation.globalPreferences.rawValue, key: "SBDisableClockIconSecondsHand"),
+            tweak(name: "Show Hardware Button Hints in Screenshots", path: FileLocation.globalPreferences.rawValue, key: "SBHardwareButtonHintDropletsAlwaysVisibleInSnapshots"),
+            tweak(name: "Hide Respring Icon", path: FileLocation.backboardd.rawValue, key: "BKHideAppleLogoOnLaunch"),
+            tweak(name: "Disable Breadcrumbs", info: "Removes '< PreviousAppName' glyph in Status Bar when being forwarded to another app.", path: FileLocation.springboard.rawValue, key: "SBNeverBreadcrumb"),
+            tweak(name: "Show Supervision Text on Lock Screen", info: "Shows info about the device supervision status and organization at the bottom of the lock screen.", path: FileLocation.springboard.rawValue, key: "SBShowSupervisionTextOnLockScreen")
+        ]),
+        tweaksection(name: "System", icon: "gear", tweaks: [
+            tweak(name: "Disable AirDrop Time Limit for Everyone Option", path: FileLocation.airdrop.rawValue, key: "OverrideTimeLimitEveryoneMode"),
+            tweak(name: "Disable Lock After Respring", path: FileLocation.springboard.rawValue, key: "SBDontLockAfterCrash"),
+            tweak(name: "Disable screen dimming while charging", path: FileLocation.springboard.rawValue, key: "SBDontDimOrLockOnAC"),
+            tweak(name: "Disable low battery alerts", path: FileLocation.springboard.rawValue, key: "SBHideLowPowerAlerts"),
+            tweak(name: "Hide AC Power on Lock Screen", path: FileLocation.springboard.rawValue, key: "SBHideACPower"),
+            tweak(name: "Show Dynamic Island in Screenshots", path: FileLocation.springboard.rawValue, key: "SBAlwaysShowSystemApertureInSnapshots"),
+            tweak(name: "Enable AirPlay support for Stage Manager", path: FileLocation.springboard.rawValue, key: "SBExtendedDisplayOverrideSupportForAirPlayAndDontFileRadars"),
+            tweak(name: "Show Hidden Icons on Home Screen", info: "Shows hidden apps like Field Test and Continuity on the home screen.", path: FileLocation.globalPreferences.rawValue, key: "SBIconVisibility"),
+            tweak(name: "Disable Spotlight Searching in Websites", path: FileLocation.globalPreferences.rawValue, key: "SBSearchDisabledDomains"),
+            tweak(name: "Vibrate on Raise-to-Wake", path: FileLocation.coreMotion.rawValue, key: "EnableWakeGestureHaptic"),
+            tweak(name: "Play Sound on Paste", path: FileLocation.pasteboard.rawValue, key: "PlaySoundOnPaste"),
+            tweak(name: "Show Notifications for System Pastes", path: FileLocation.pasteboard.rawValue, key: "AnnounceAllPastes")
+        ]),
+        tweaksection(name: "Debugging", icon: "ladybug.fill", tweaks: [
+            tweak(name: "Metal HUD Debug", path: FileLocation.globalPreferences.rawValue, key: "MetalForceHudEnabled"),
+            tweak(name: "iMessage Debugging", path: FileLocation.globalPreferences.rawValue, key: "iMessageDiagnosticsEnabled"),
+            tweak(name: "Continuity Debugging", path: FileLocation.globalPreferences.rawValue, key: "IDSDiagnosticsEnabled"),
+            tweak(name: "FaceTime Debugging", path: FileLocation.globalPreferences.rawValue, key: "VCDiagnosticsEnabled"),
+            tweak(name: "Show Accessory Developer Settings", path: FileLocation.globalPreferences.rawValue, key: "AccessoryDeveloperEnabled"),
+            tweak(name: "App Store Debug Gesture", path: FileLocation.appStore.rawValue, key: "debugGestureEnabled"),
+            tweak(name: "Notes Debug Mode", path: FileLocation.notes.rawValue, key: "DebugModeEnabled"),
+            tweak(name: "Show Touches With Debug Info", path: FileLocation.backboardd.rawValue, key: "BKDigitizerVisualizeTouches"),
+        ])
+    ]
+
+    var body: some View {
+        NavigationStack {
+            List {
+                Section(header: HeaderLabel(text: "Applying", icon: "checkmark"), footer: Text("After applying, please fully reboot your device to actually see the changes. These tweaks were copied from [Nugget](https://github.com/leminlimez/Nugget) and ported to work with DarkSword.")) {
+                    Button("Apply tweaks") {
+                        apply()
+                    }
+                    Button("Reset to default") {
+                        revert()
+                    }
+                }
+                tweaksui
+            }
+            .navigationTitle("Nugget")
+            .refreshable {
+                load()
+            }
+            .onAppear {
+                load()
+            }
+        }
+    }
+
+    // yes this is copied from dirtyZeroView, the whole view is
+    private var tweaksui: some View {
+        ForEach($tweaks) { $section in
+            Section(header: HeaderDropdown(text: section.name, icon: section.icon, isExpanded: $section.isexpanded, useItemCount: true, itemCount: section.tweaks.count)) {
+                if section.isexpanded {
+                    ForEach($section.tweaks) { $tweak in
+                        if let tweakinfo = tweak.info {
+                            PlainToggle(text: tweak.name, infoType: .info, infoTitle: "Tweak infomation", infoMessage: tweakinfo, isOn: $tweak.state.enabled)
+                                .disabled(tweak.readfailed)
+                        } else {
+                            PlainToggle(text: tweak.name, isOn: $tweak.state.enabled)
+                                .disabled(tweak.readfailed)
+                        }
+                    }
+                }
+            }
+        }
+    }
+
+    private func load() {
+        checkplists()
+        var errormsg = ""
+        for (sectionindex, sectionitem) in tweaks.enumerated() {
+            var section = sectionitem
+            for (tweakindex, tweakitem) in section.tweaks.enumerated() {
+                var tweak = tweakitem
+                tweak.state = (og: false, enabled: false)
+                if fm.fileExists(atPath: tweak.path) {
+                    let result = mgr.getplistvalue(path: tweak.path, key: tweak.key)
+                    if result.ok, let value = result.value as? Bool {
+                        tweak.state.og = value
+                        tweak.state.enabled = value
+                    } else if !(result.message == "key \(tweak.key) not found") {
+                        errormsg += "Failed to read \(tweak.path): \(result.message)\n"
+                        tweak.readfailed = true
+                    }
```

</details>

`load()` never resets `tweak.readfailed` to `false` before retrying reads. Once a read fails, the toggle will stay disabled across refreshes even if the plist becomes readable again. Also, if `getplistvalue` returns `ok == true` but the value isn’t a `Bool`, this falls into the error path and produces an unhelpful "Failed to read ...: success" message. Reset `readfailed` at the start of each iteration and add a dedicated branch for the non-`Bool` successful read case (e.g., report a type mismatch).



---

## Timeline (Verbose)

- May 17, 2026: 👀 @jurre111 requested review from @Copilot
- : 💬 someone reviewed: commented
- May 17, 2026: ⚪ @jurre111 converted to draft
- May 18, 2026: 🔴 Closed by @jurre111
- May 18, 2026: ⬆️ @jurre111 force-pushed


---

## Comments (1)

### Comment #1 — [@jurre111](https://github.com/jurre111) · May 17, 2026

`ID: 4468515240`


i forgot about the rewrite so ill mark it as "Ready for review" if its done


---

*Exported on 10/14/2026, 3:07:26 PM from [https://github.com/rooootdev/lara/pull/251](https://github.com/rooootdev/lara/pull/251)*
