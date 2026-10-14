# fix(source.json): correct minOSVersion, broken headerURL, and stale feature list

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#177](https://github.com/rooootdev/lara/pull/177)  
> **State:** 🔴 closed  
> **Branch:** `tyhallcsu:fix/source-json-metadata` → `rooootdev:main`  
> **Changes:** +4 / -4 across 1 files  
> **Created:** Apr 23, 2026  
> **Updated:** Apr 23, 2026  
> **Closed:** Apr 23, 2026  

---

## Description

*Opened by [@tyhallcsu](https://github.com/tyhallcsu)* *on Apr 23, 2026*


## Summary

Four metadata fixes in `source.json` (the AltStore source file). No code changes.

### 1. `minOSVersion`: `15.0` → `17.0`
`offsets_init()` in `lara/kexploit/offsets.m` calls `exit(EXIT_FAILURE)` for any iOS version below 17.0. AltStore currently offers the IPA to iOS 15 and 16 users, who get a silent crash on first launch with no explanation. Setting `minOSVersion` to `17.0` prevents the install from being offered on unsupported versions.

### 2. `headerURL`: `icon.png` → `lara.png`
`icon.png` does not exist in the repo. The AltStore source listing header image was returning 404. `lara.png` (the logo in the README) is the file that exists.

### 3. `localizedDescription` feature list sync with README
- **JIT** moved from "coming soon" to "implemented" (matches README)
- Removed `"doesnt work on ipad m1-m4?"` (no longer in README known-issues)
- Added two known issues that were present in README but missing here: RemoteCall not supported on A16+/M-series yet; apps don't detect JIT is enabled

### 4. `date` format: `"2026-04-5"` → `"2026-04-05"` (ISO-8601 zero-padding)

## Verification
- `curl -s https://raw.githubusercontent.com/rooootdev/lara/refs/heads/main/lara.png | head -4` — file exists
- `icon.png` does not exist at that path in the repo
- `offsets.m:253–256` — `exit(EXIT_FAILURE)` on iOS < 17.0 confirms the minimum


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (1)

- [`8904a18`](https://github.com/rooootdev/lara/commit/8904a18a0501028c70c016224c83d855dc3df1e4) fix(source.json): correct minOSVersion, broken headerURL, stale feature list — *tyhallcsu* · Apr 23, 2026


---

## Changed Files (1)

- 🟡 `source.json` (modified) — +4 / -4
  <details><summary>Diff</summary>

  ```diff
  @@ -3,7 +3,7 @@
   "subtitle": "WIP darksword kexploit implement",
   "description": "",
   "iconURL": "https://avatars.githubusercontent.com/u/103732419?v=4",
-  "headerURL": "https://raw.githubusercontent.com/rooootdev/lara/refs/heads/main/icon.png",
+  "headerURL": "https://raw.githubusercontent.com/rooootdev/lara/refs/heads/main/lara.png",
   "website": "https://github.com/rooootdev/lara",
   "tintColor": "#4185A9",
   "featuredApps": [
@@ -15,17 +15,17 @@
       "bundleIdentifier": "com.roooot.lara",
       "developerName": "rooootdev",
       "subtitle": "iOS 18.7.1- & 26.0.1-",
-      "localizedDescription": "lara will at its absolute best only ever support versions up to iOS 26.0.1/iOS 18.7.1. the exploit was patched after those versions.\n\nCurrently tested on iOS 17.2.1 - 26.0.1. If you run lara on your device, and it ends up working, please contact me on discord (@roooot.dev) and tell me:\n\n1. your device\n2. your iOS version\n3. what you tested in lara (eg. Run Exploit, Init KFS, etc.)\n\nIf lara doesnt work on your device, and you want to help the project, please also provide your logs and iOS version.\n\nFeatures:\n\nimplemented:\n• Font Overwrite\n• Custom Overwrite\n• Card Overwrite\n• File Manager (Full Disk r/w)\n• MobileGestalt Editor\n• 3 App Bypass\n• DirtyZero 2\n• 5 App Dock\n• Status Bar Tweaks\n• Hide labels\n• Upside Down\n• Floating Dock (Broken)\n• Grid App Switcher\n• Performance\n\ncoming soon:\n• JIT\n• App Decrypt\n\nknown issues:\n• wont work on M5, A19 and A19 Pro due to MTE\n• on iOS 17.x, the kernel may panic when lara is closed from the app switcher.\n• downloading OTA updates does not work.\n• dirtyzero does not work.\n• ui is buggy on 17.x\n• doesnt work on ipad m1-m4?\n• .aea ota updates do not work.\n\ntips:\ndeleting and redownloading kernelcache is known to fix many issues. do this before asking me for support. closing and reopening the app can fix font change issues. respringing is needed to apply springboard changes such as font changes.\n\ncredits:\n• opa334 for the kernel exploit poc, ChOma and XPF\n• AppInstaller iOS for help with offsets\n• AlfieCG for libgrabkernel2",
+      "localizedDescription": "lara will at its absolute best only ever support versions up to iOS 26.0.1/iOS 18.7.1. the exploit was patched after those versions.\n\nCurrently tested on iOS 17.2.1 - 26.0.1. If you run lara on your device, and it ends up working, please contact me on discord (@roooot.dev) and tell me:\n\n1. your device\n2. your iOS version\n3. what you tested in lara (eg. Run Exploit, Init KFS, etc.)\n\nIf lara doesnt work on your device, and you want to help the project, please also provide your logs and iOS version.\n\nFeatures:\n\nimplemented:\n• Font Overwrite\n• Custom Overwrite\n• Card Overwrite\n• File Manager (Full Disk r/w)\n• MobileGestalt Editor\n• 3 App Bypass\n• DirtyZero 2\n• 5 App Dock\n• Status Bar Tweaks\n• Hide labels\n• Upside Down\n• Floating Dock (Broken)\n• Grid App Switcher\n• Performance\n• JIT\n\ncoming soon:\n• App Decrypt\n\nknown issues:\n• wont work on M5, A19 and A19 Pro due to MTE\n• on iOS 17.x, the kernel may panic when lara is closed from the app switcher.\n• downloading OTA updates does not work.\n• dirtyzero does not work.\n• ui is buggy on 17.x\n• .aea ota updates do not work.\n• A16+ and M-series devices dont support RemoteCall (yet)\n• apps don't detect JIT enabled however they are enabled.\n\ntips:\ndeleting and redownloading kernelcache is known to fix many issues. do this before asking me for support. closing and reopening the app can fix font change issues. respringing is needed to apply springboard changes such as font changes.\n\ncredits:\n• opa334 for the kernel exploit poc, ChOma and XPF\n• AppInstaller iOS for help with offsets\n• AlfieCG for libgrabkernel2",
       "iconURL": "https://raw.githubusercontent.com/rooootdev/lara/refs/heads/main/lara.png",
       "tintColor": "#5CA399",
       "versions": [
         {
           "version": "1.2",
-          "date": "2026-04-5",
+          "date": "2026-04-05",
           "size": 3450675,
           "downloadURL": "https://github.com/rooootdev/lara/releases/download/latest/lara.ipa",
           "localizedDescription": "",
-          "minOSVersion": "15.0"
+          "minOSVersion": "17.0"
         }
       ],
       "appPermissions": {}
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 8904a18: fix(source.json): correct minOSVersion, broken headerURL, stale feature list
- Apr 23, 2026: 🔴 Closed by @rooootdev


---

## Comments (1)

### Comment #1 — [@rooootdev](https://github.com/rooootdev) · Apr 23, 2026

`ID: 4305566659`


pull request closed, addressed by changing the offsets_init() exit to 16.0 and lower.


---

*Exported on 10/14/2026, 3:00:51 PM from [https://github.com/rooootdev/lara/pull/177](https://github.com/rooootdev/lara/pull/177)*
