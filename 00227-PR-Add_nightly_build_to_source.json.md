# Add nightly build to source.json

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#227](https://github.com/rooootdev/lara/pull/227)  
> **State:** 🔴 closed  
> **Branch:** `jurre111:altsource` → `rooootdev:main`  
> **Changes:** +20 / -0 across 1 files  
> **Created:** May 6, 2026  
> **Updated:** May 6, 2026  
> **Closed:** May 6, 2026  

---

## Description

*Opened by [@jurre111](https://github.com/jurre111)* *on May 6, 2026*


Someone requested it, so if you think it's useful aswell you can merge it


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/25406024368/job/74517262316) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/25406024368/job/74517040843) — **success** — *GitHub Actions*


---

## Reviews (1)

### 💬 COMMENTED — [@copilot-pull-request-reviewer[bot]](https://github.com/apps/copilot-pull-request-reviewer) · May 6, 2026

## Pull request overview

Adds a “nightly” distribution entry to the AltStore-style `source.json` so users can install a nightly build via the source.

**Changes:**
- Add a new `apps[]` entry named `lara (nightly)` that points to the `releases/download/nightly/lara.ipa` artifact.
- Duplicate the stable app metadata (description, icon, colors) for the new nightly entry.





---

💡 <a href="/rooootdev/lara/new/main?filename=.github/instructions/*.instructions.md" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Add Copilot custom instructions</a> for smarter, more guided reviews. <a href="https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Learn how to get started</a>.


---

## Review Comments (3)

*Inline comments on code changes*

### `source.json`

**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · May 6, 2026 (line 35) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -29,6 +29,26 @@
         }
       ],
       "appPermissions": {}
+    },
+    {
+      "name": "lara (nightly)",
+      "bundleIdentifier": "com.roooot.lara",
```

</details>

This new entry reuses the same `bundleIdentifier` (`com.roooot.lara`) as the existing stable app. In AltStore-style sources this identifier is effectively the unique key, so having two app entries with the same bundle ID can cause duplicate/ambiguous listings and installs overwriting each other. Consider either using a distinct bundle ID for the nightly build (if it’s a separately signed app) or keeping a single app entry and adding an additional version pointing at the nightly URL.


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · May 6, 2026 (line 43) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -29,6 +29,26 @@
         }
       ],
       "appPermissions": {}
+    },
+    {
+      "name": "lara (nightly)",
+      "bundleIdentifier": "com.roooot.lara",
+      "developerName": "rooootdev",
+      "subtitle": "iOS 18.7.1- & 26.0.1-",
+      "localizedDescription": "lara will at its absolute best only ever support versions up to iOS 26.0.1/iOS 18.7.1. the exploit was patched after those versions.\n\nCurrently tested on iOS 17.2.1 - 26.0.1. If you run lara on your device, and it ends up working, please contact me on discord (@roooot.dev) and tell me:\n\n1. your device\n2. your iOS version\n3. what you tested in lara (eg. Run Exploit, Init KFS, etc.)\n\nIf lara doesnt work on your device, and you want to help the project, please also provide your logs and iOS version.\n\nFeatures:\n\nimplemented:\n• Font Overwrite\n• Custom Overwrite\n• Card Overwrite\n• File Manager (Full Disk r/w)\n• MobileGestalt Editor\n• 3 App Bypass\n• DirtyZero 2\n• 5 App Dock\n• Status Bar Tweaks\n• Hide labels\n• Upside Down\n• Floating Dock (Broken)\n• Grid App Switcher\n• Performance\n\ncoming soon:\n• JIT\n• App Decrypt\n\nknown issues:\n• wont work on M5, A19 and A19 Pro due to MTE\n• on iOS 17.x, the kernel may panic when lara is closed from the app switcher.\n• downloading OTA updates does not work.\n• dirtyzero does not work.\n• ui is buggy on 17.x\n• doesnt work on ipad m1-m4?\n• .aea ota updates do not work.\n\ntips:\ndeleting and redownloading kernelcache is known to fix many issues. do this before asking me for support. closing and reopening the app can fix font change issues. respringing is needed to apply springboard changes such as font changes.\n\ncredits:\n• opa334 for the kernel exploit poc, ChOma and XPF\n• AppInstaller iOS for help with offsets\n• AlfieCG for libgrabkernel2",
+      "iconURL": "https://raw.githubusercontent.com/rooootdev/lara/refs/heads/main/lara.png",
+      "tintColor": "#5CA399",
+      "versions": [
+        {
+          "version": "1.2",
```

</details>

The nightly entry uses the same `version` value as the stable entry (both are `1.2`). If the nightly asset is updated in-place, clients that rely on semantic version comparison may not detect updates correctly, and users may not be able to tell which build they’re installing. Consider using a distinct version scheme for nightly (e.g., `1.2-nightly.<build>` or a date-based version) and updating it per build.


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · May 6, 2026 (line 44) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -29,6 +29,26 @@
         }
       ],
       "appPermissions": {}
+    },
+    {
+      "name": "lara (nightly)",
+      "bundleIdentifier": "com.roooot.lara",
+      "developerName": "rooootdev",
+      "subtitle": "iOS 18.7.1- & 26.0.1-",
+      "localizedDescription": "lara will at its absolute best only ever support versions up to iOS 26.0.1/iOS 18.7.1. the exploit was patched after those versions.\n\nCurrently tested on iOS 17.2.1 - 26.0.1. If you run lara on your device, and it ends up working, please contact me on discord (@roooot.dev) and tell me:\n\n1. your device\n2. your iOS version\n3. what you tested in lara (eg. Run Exploit, Init KFS, etc.)\n\nIf lara doesnt work on your device, and you want to help the project, please also provide your logs and iOS version.\n\nFeatures:\n\nimplemented:\n• Font Overwrite\n• Custom Overwrite\n• Card Overwrite\n• File Manager (Full Disk r/w)\n• MobileGestalt Editor\n• 3 App Bypass\n• DirtyZero 2\n• 5 App Dock\n• Status Bar Tweaks\n• Hide labels\n• Upside Down\n• Floating Dock (Broken)\n• Grid App Switcher\n• Performance\n\ncoming soon:\n• JIT\n• App Decrypt\n\nknown issues:\n• wont work on M5, A19 and A19 Pro due to MTE\n• on iOS 17.x, the kernel may panic when lara is closed from the app switcher.\n• downloading OTA updates does not work.\n• dirtyzero does not work.\n• ui is buggy on 17.x\n• doesnt work on ipad m1-m4?\n• .aea ota updates do not work.\n\ntips:\ndeleting and redownloading kernelcache is known to fix many issues. do this before asking me for support. closing and reopening the app can fix font change issues. respringing is needed to apply springboard changes such as font changes.\n\ncredits:\n• opa334 for the kernel exploit poc, ChOma and XPF\n• AppInstaller iOS for help with offsets\n• AlfieCG for libgrabkernel2",
+      "iconURL": "https://raw.githubusercontent.com/rooootdev/lara/refs/heads/main/lara.png",
+      "tintColor": "#5CA399",
+      "versions": [
+        {
+          "version": "1.2",
+          "date": "2026-04-5",
```

</details>

`date` is set to `2026-04-5` (non-zero-padded day). If this field is parsed as an ISO-8601 date by clients/tools, the missing leading zero can cause inconsistent parsing. Prefer `YYYY-MM-DD` (e.g., `2026-04-05`).



---

## PR Commits (3)

- [`6f58702`](https://github.com/rooootdev/lara/commit/6f5870211a0cff4eaf401c0f34c13773c130cad3) add nightly to source.json — *jurre111* · May 6, 2026
- [`459d17a`](https://github.com/rooootdev/lara/commit/459d17a6b68b09157b4587759f9d70a1a0b4f149) Merge branch 'rooootdev:main' into altsource — *Jurre* · May 6, 2026
- [`b0d5f99`](https://github.com/rooootdev/lara/commit/b0d5f99014051c2cfa4b564d604fa6845b799076) Merge branch 'rooootdev:main' into altsource — *Jurre* · May 6, 2026


---

## Changed Files (1)

- 🟡 `source.json` (modified) — +20 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -29,6 +29,26 @@
         }
       ],
       "appPermissions": {}
+    },
+    {
+      "name": "lara (nightly)",
+      "bundleIdentifier": "com.roooot.lara",
+      "developerName": "rooootdev",
+      "subtitle": "iOS 18.7.1- & 26.0.1-",
+      "localizedDescription": "lara will at its absolute best only ever support versions up to iOS 26.0.1/iOS 18.7.1. the exploit was patched after those versions.\n\nCurrently tested on iOS 17.2.1 - 26.0.1. If you run lara on your device, and it ends up working, please contact me on discord (@roooot.dev) and tell me:\n\n1. your device\n2. your iOS version\n3. what you tested in lara (eg. Run Exploit, Init KFS, etc.)\n\nIf lara doesnt work on your device, and you want to help the project, please also provide your logs and iOS version.\n\nFeatures:\n\nimplemented:\n• Font Overwrite\n• Custom Overwrite\n• Card Overwrite\n• File Manager (Full Disk r/w)\n• MobileGestalt Editor\n• 3 App Bypass\n• DirtyZero 2\n• 5 App Dock\n• Status Bar Tweaks\n• Hide labels\n• Upside Down\n• Floating Dock (Broken)\n• Grid App Switcher\n• Performance\n\ncoming soon:\n• JIT\n• App Decrypt\n\nknown issues:\n• wont work on M5, A19 and A19 Pro due to MTE\n• on iOS 17.x, the kernel may panic when lara is closed from the app switcher.\n• downloading OTA updates does not work.\n• dirtyzero does not work.\n• ui is buggy on 17.x\n• doesnt work on ipad m1-m4?\n• .aea ota updates do not work.\n\ntips:\ndeleting and redownloading kernelcache is known to fix many issues. do this before asking me for support. closing and reopening the app can fix font change issues. respringing is needed to apply springboard changes such as font changes.\n\ncredits:\n• opa334 for the kernel exploit poc, ChOma and XPF\n• AppInstaller iOS for help with offsets\n• AlfieCG for libgrabkernel2",
+      "iconURL": "https://raw.githubusercontent.com/rooootdev/lara/refs/heads/main/lara.png",
+      "tintColor": "#5CA399",
+      "versions": [
+        {
+          "version": "1.2",
+          "date": "2026-04-5",
+          "size": 3450675,
+          "downloadURL": "https://github.com/rooootdev/lara/releases/download/nightly/lara.ipa",
+          "localizedDescription": "",
+          "minOSVersion": "15.0"
+        }
+      ],
+      "appPermissions": {}
     }
   ]
 }
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 6f58702: add nightly to source.json
- : 📝 Commit 459d17a: Merge branch 'rooootdev:main' into altsource
- May 6, 2026: 👀 @jurre111 requested review from @Copilot
- May 6, 2026: ⚪ @jurre111 converted to draft
- : 💬 someone reviewed: commented
- May 6, 2026: 🔴 Closed by @jurre111
- May 6, 2026: 🟢 Reopened by @jurre111
- May 6, 2026: 🟢 @jurre111 marked ready for review
- : 📝 Commit b0d5f99: Merge branch 'rooootdev:main' into altsource
- May 6, 2026: 🔴 Closed by @jurre111


---

## Comments (2)

### Comment #1 — [@jurre111](https://github.com/jurre111) · May 6, 2026

`ID: 4383583922`


doesnt work


---

### Comment #2 — [@jurre111](https://github.com/jurre111) · May 6, 2026

`ID: 4383603498`


works again lol


---

*Exported on 10/14/2026, 3:05:18 PM from [https://github.com/rooootdev/lara/pull/227](https://github.com/rooootdev/lara/pull/227)*
