# fix altstore/sidestore source.json

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#146](https://github.com/rooootdev/lara/pull/146)  
> **State:** 🟣 merged  
> **Branch:** `mrtacoguy01:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 20, 2026  
> **Changes:** +3 / -3 across 1 files  
> **Created:** Apr 20, 2026  
> **Updated:** Apr 20, 2026  
> **Closed:** Apr 20, 2026  

---

## Description

*Opened by [@mrtacoguy01](https://github.com/mrtacoguy01)* *on Apr 20, 2026*


source.json still has "com.roooot.laraapp" instead of "com.roooot.lara"


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (3)

- [`549e4eb`](https://github.com/rooootdev/lara/commit/549e4ebbfccdafdf6117918d8e5368e71885b54a) fix altstore/sidestore — *t* · Apr 20, 2026
- [`58ca307`](https://github.com/rooootdev/lara/commit/58ca3074264de31e30ad435b0a7cc63272363881) update description to whats in readme — *t* · Apr 20, 2026
- [`dbdf395`](https://github.com/rooootdev/lara/commit/dbdf395eb3af83cff894e9f2de17e3f05726af38) update source.json with updated features — *t* · Apr 20, 2026


---

## Changed Files (1)

- 🟡 `source.json` (modified) — +3 / -3
  <details><summary>Diff</summary>

  ```diff
  @@ -7,15 +7,15 @@
   "website": "https://github.com/rooootdev/lara",
   "tintColor": "#4185A9",
   "featuredApps": [
-    "com.roooot.laraapp"
+    "com.roooot.lara"
   ],
   "apps": [
     {
       "name": "lara",
-      "bundleIdentifier": "com.roooot.laraapp",
+      "bundleIdentifier": "com.roooot.lara",
       "developerName": "rooootdev",
       "subtitle": "iOS 18.7.1- & 26.0.1-",
-      "localizedDescription": "lara will at its absolute best only ever support versions up to iOS 26.0.1/iOS 18.7.1. the exploit was patched after those versions.\n\nCurrently tested on iOS 17.2.1 - 26.0.1. If you run lara on your device, and it ends up working, please contact me on discord (@roooot.dev) and tell me:\n\n1. your device\n2. your iOS version\n3. what you tested in lara (eg. Run Exploit, Init KFS, etc.)\n\nIf lara doesnt work on your device, and you want to help the project, please also provide your logs and iOS version.\n\nFeatures:\n\nimplemented:\n• Font overwrite (VFS)\n• 3 app limit bypass\n• Unblacklisting\n• File Manager (Full Disk r/w)\n• dirtyZero 2 (Broken)\n\ncoming soon:\n• Mobilegestalt Editor\n\nknown issues:\n• on iOS 17.x, the kernel may panic when lara is closed from the app switcher.\n• dirtyZero does not work.\n• ui is buggy on 17.x\n• doesnt work on ipad m2?\n• doesnt work on a18 pro?\n• kernelcache download broken for some versions.\n\ntips:\ndeleting and redownloading kernelcache is known to fix many issues. do this before asking me for support. closing and reopening the app can fix font change issues. respringing is needed to apply springboard changes such as font changes.\n\ncredits:\n• opa334 for the kernel exploit poc, ChOma and XPF\n• AppInstaller iOS for help with offsets\n• AlfieCG for libgrabkernel2",
+      "localizedDescription": "lara will at its absolute best only ever support versions up to iOS 26.0.1/iOS 18.7.1. the exploit was patched after those versions.\n\nCurrently tested on iOS 17.2.1 - 26.0.1. If you run lara on your device, and it ends up working, please contact me on discord (@roooot.dev) and tell me:\n\n1. your device\n2. your iOS version\n3. what you tested in lara (eg. Run Exploit, Init KFS, etc.)\n\nIf lara doesnt work on your device, and you want to help the project, please also provide your logs and iOS version.\n\nFeatures:\n\nimplemented:\n• Font Overwrite\n• Custom Overwrite\n• Card Overwrite\n• File Manager (Full Disk r/w)\n• MobileGestalt Editor\n• 3 App Bypass\n• DirtyZero 2\n• 5 App Dock\n• Status Bar Tweaks\n• Hide labels\n• Upside Down\n• Floating Dock (Broken)\n• Grid App Switcher\n• Performance\n\ncoming soon:\n• JIT\n• App Decrypt\n\nknown issues:\n• wont work on M5, A19 and A19 Pro due to MTE\n• on iOS 17.x, the kernel may panic when lara is closed from the app switcher.\n• downloading OTA updates does not work.\n• dirtyzero does not work.\n• ui is buggy on 17.x\n• doesnt work on ipad m1-m4?\n• .aea ota updates do not work.\n\ntips:\ndeleting and redownloading kernelcache is known to fix many issues. do this before asking me for support. closing and reopening the app can fix font change issues. respringing is needed to apply springboard changes such as font changes.\n\ncredits:\n• opa334 for the kernel exploit poc, ChOma and XPF\n• AppInstaller iOS for help with offsets\n• AlfieCG for libgrabkernel2"
       "iconURL": "https://raw.githubusercontent.com/rooootdev/lara/refs/heads/main/lara.png",
       "tintColor": "#5CA399",
       "versions": [
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 549e4eb: fix altstore/sidestore
- : 📝 Commit 58ca307: update description to whats in readme
- : 📝 Commit dbdf395: update source.json with updated features
- Apr 20, 2026: 🟣 @rooootdev merged
- Apr 20, 2026: 🔴 Closed by @rooootdev


---

*Exported on 10/14/2026, 2:58:05 PM from [https://github.com/rooootdev/lara/pull/146](https://github.com/rooootdev/lara/pull/146)*
