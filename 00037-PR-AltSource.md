# AltSource

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#37](https://github.com/rooootdev/lara/pull/37)  
> **State:** 🟣 merged  
> **Branch:** `Maxpro131:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 6, 2026  
> **Changes:** +37 / -1 across 3 files  
> **Created:** Apr 6, 2026  
> **Updated:** Apr 6, 2026  
> **Closed:** Apr 6, 2026  

---

## Description

*Opened by [@Maxpro131](https://github.com/Maxpro131)* *on Apr 6, 2026*


I'm sick of copying the download link 10000 times


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (2)

- [`7f8c4cd`](https://github.com/rooootdev/lara/commit/7f8c4cde595ca02ceef61c6c281af24b60355124) Add AltSource — *Maxpro131* · Apr 6, 2026
- [`07023ac`](https://github.com/rooootdev/lara/commit/07023ace34db4053ce6b820e1a6e0199e47419fc) Add AltSource button — *Maxpro131* · Apr 6, 2026


---

## Changed Files (3)

- 🟡 `README.md` (modified) — +3 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -3,7 +3,9 @@ a beautiful kexploit.
 > star this repo please :P
 
 ## support
-lara will at its absolute best only ever support versions up to iOS 26.0.1/iOS 18.7.1. the exploit was patched after those versions.
+lara will at its absolute best only ever support versions up to iOS 26.0.1/iOS 18.7.1. the exploit was patched after those versions.<br><br><a href="https://celloserenity.github.io/altdirect/?url=https://raw.githubusercontent.com/rooootdev/lara/refs/heads/main/source.json" target="_blank">
+   <img src="https://github.com/CelloSerenity/altdirect/blob/main/assets/png/AltSource_Blue.png?raw=true" alt="Add AltSource" width="200">
+</a>
 
 Currently tested on iOS 17.2.1 - 26.0.1.
 If you run lara on your device, and it ends up working, please contact me on [discord](https://discord.com/users/919268666305024010) (@roooot.dev) and tell me:
  ```

  </details>
- 🟢 `icon.png` (added) — +0 / -0
- 🟢 `source.json` (added) — +34 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -0,0 +1,34 @@
+{
+  "name": "lara",
+  "subtitle": "WIP darksword kexploit implement",
+  "description": "",
+  "iconURL": "https://avatars.githubusercontent.com/u/103732419?v=4",
+  "headerURL": "https://raw.githubusercontent.com/rooootdev/lara/refs/heads/main/icon.png",
+  "website": "https://github.com/rooootdev/lara",
+  "tintColor": "#4185A9",
+  "featuredApps": [
+    "com.roooot.laraapp"
+  ],
+  "apps": [
+    {
+      "name": "lara",
+      "bundleIdentifier": "com.roooot.laraapp",
+      "developerName": "rooootdev",
+      "subtitle": "iOS 18.7.1- & 26.0.1-",
+      "localizedDescription": "lara will at its absolute best only ever support versions up to iOS 26.0.1/iOS 18.7.1. the exploit was patched after those versions.\n\nCurrently tested on iOS 17.2.1 - 26.0.1. If you run lara on your device, and it ends up working, please contact me on discord (@roooot.dev) and tell me:\n\n1. your device\n2. your iOS version\n3. what you tested in lara (eg. Run Exploit, Init KFS, etc.)\n\nIf lara doesnt work on your device, and you want to help the project, please also provide your logs and iOS version.\n\nFeatures:\n\nimplemented:\n• Font overwrite (VFS)\n• 3 app limit bypass\n• Unblacklisting\n• File Manager (Full Disk r/w)\n• dirtyZero 2 (Broken)\n\ncoming soon:\n• Mobilegestalt Editor\n\nknown issues:\n• on iOS 17.x, the kernel may panic when lara is closed from the app switcher.\n• dirtyZero does not work.\n• ui is buggy on 17.x\n• doesnt work on ipad m2?\n• doesnt work on a18 pro?\n• kernelcache download broken for some versions.\n\ntips:\ndeleting and redownloading kernelcache is known to fix many issues. do this before asking me for support. closing and reopening the app can fix font change issues. respringing is needed to apply springboard changes such as font changes.\n\ncredits:\n• opa334 for the kernel exploit poc, ChOma and XPF\n• AppInstaller iOS for help with offsets\n• AlfieCG for libgrabkernel2",
+      "iconURL": "https://raw.githubusercontent.com/rooootdev/lara/refs/heads/main/icon.png",
+      "tintColor": "#5CA399",
+      "versions": [
+        {
+          "version": "1.0",
+          "date": "",
+          "size": 2000,
+          "downloadURL": "https://github.com/rooootdev/lara/releases/download/latest/lara.ipa",
+          "localizedDescription": "",
+          "minOSVersion": "15.0"
+        }
+      ],
+      "appPermissions": {}
+    }
+  ]
+}
  ```

  </details>


---

## References

### Commits (1)

- [007be0e](https://github.com/danosito/lara-m-chips-fix/commit/007be0e67d1c7b6e5dcbc3898206788d397be914) — Merge pull request #37 from Maxpro131/main


---

## Timeline (Verbose)

- : 📝 Commit 7f8c4cd: Add AltSource
- : 📝 Commit 07023ac: Add AltSource button
- Apr 6, 2026: 🟣 @rooootdev merged
- Apr 6, 2026: 🔴 Closed by @rooootdev
- Apr 19, 2026: 🔗 Commit 007be0e — Merge pull request #37 from Maxpro131/main


---

*Exported on 10/14/2026, 2:49:06 PM from [https://github.com/rooootdev/lara/pull/37](https://github.com/rooootdev/lara/pull/37)*
