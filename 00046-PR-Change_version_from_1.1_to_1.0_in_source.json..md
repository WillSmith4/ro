# Change version from 1.1 to 1.0 in source.json.

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#46](https://github.com/rooootdev/lara/pull/46)  
> **State:** 🟣 merged  
> **Branch:** `Pro42good:patch-3` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 6, 2026  
> **Changes:** +3 / -3 across 1 files  
> **Created:** Apr 6, 2026  
> **Updated:** Apr 6, 2026  
> **Closed:** Apr 6, 2026  

---

## Description

*Opened by [@Pro42good](https://github.com/Pro42good)* *on Apr 6, 2026*


For the love of god, do not touch the version number on both this JSON and when compiling the IPA, it does not like a version mismatch when installing on third party stores.

Just mark the version number within the app.


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (1)

- 🔴 [build](https://github.com/rooootdev/lara/actions/runs/24040942954/job/70112262525) — **failure** — *GitHub Actions*


---

## Reviews (2)

### 💬 COMMENTED — [@copilot-pull-request-reviewer[bot]](https://github.com/apps/copilot-pull-request-reviewer) · Apr 6, 2026

## Pull request overview

This PR updates the AltSource metadata to avoid an app version mismatch between `source.json` and the compiled IPA during installation.

**Changes:**
- Set the published version in `source.json` from `1.1` to `1.0`.
- Cleared the per-version `localizedDescription` value.





---

💡 <a href="/rooootdev/lara/new/main?filename=.github/instructions/*.instructions.md" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Add Copilot custom instructions</a> for smarter, more guided reviews. <a href="https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Learn how to get started</a>.

---

### 💬 COMMENTED — [@Pro42good](https://github.com/Pro42good) · Apr 6, 2026


---

## Review Comments (2)

*Inline comments on code changes*

### `source.json`

**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 6, 2026 (lines 26-28) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -20,11 +20,11 @@
       "tintColor": "#5CA399",
       "versions": [
         {
-          "version": "1.1",
+          "version": "1.0",
           "date": "2025-05-19",
           "size": 3450675,
           "downloadURL": "https://github.com/rooootdev/lara/releases/download/latest/lara.ipa",
-          "localizedDescription": "lara will at its absolute best only ever support versions up to iOS 26.0.1/iOS 18.7.1. the exploit was patched after those versions.\n\nCurrently tested on iOS 17.2.1 - 26.0.1. If you run lara on your device, and it ends up working, please contact me on discord (@roooot.dev) and tell me:\n\n1. your device\n2. your iOS version\n3. what you tested in lara (eg. Run Exploit, Init KFS, etc.)\n\nIf lara doesnt work on your device, and you want to help the project, please also provide your logs and iOS version.\n\nFeatures:\n\nimplemented:\n• Font overwrite (VFS)\n• 3 app limit bypass\n• Unblacklisting\n• File Manager (Full Disk r/w)\n• dirtyZero 2 (Broken)\n\ncoming soon:\n• Mobilegestalt Editor\n\nknown issues:\n• on iOS 17.x, the kernel may panic when lara is closed from the app switcher.\n• dirtyZero does not work.\n• ui is buggy on 17.x\n• doesnt work on ipad m2?\n• doesnt work on a18 pro?\n• kernelcache download broken for some versions.\n\ntips:\ndeleting and redownloading kernelcache is known to fix many issues. do this before asking me for support. closing and reopening the app can fix font change issues. respringing is needed to apply springboard changes such as font changes.\n\ncredits:\n• opa334 for the kernel exploit poc, ChOma and XPF\n• AppInstaller iOS for help with offsets\n• AlfieCG for libgrabkernel2",
+          "localizedDescription": "",
           "minOSVersion": "15.0"
```

</details>

This change set also clears the per-version `localizedDescription` (now an empty string). The PR title/description only mention adjusting the version number; if this was unintended, keep the previous release notes text or remove the `localizedDescription` field entirely (some clients may prefer the version-level description and show blank details when it’s present-but-empty).

  **[@Pro42good](https://github.com/Pro42good)** · Apr 6, 2026 (lines 26-28) *(reply)* · review: COMMENTED

  @copilot apply changes based on [this feedback](https://github.com/rooootdev/lara/pull/46#discussion_r3040319046)



---

## PR Commits (3)

- [`ad42b5e`](https://github.com/rooootdev/lara/commit/ad42b5e58cfe0bbf152996ce062692e5005a28de) Change version from 1.1 to 1.0 in source.json — *Pro42good DQ* · Apr 6, 2026
- [`4119b30`](https://github.com/rooootdev/lara/commit/4119b303d6aa3783adedd206c93d8d34a318aa76) Update localizedDescription in source.json — *Pro42good DQ* · Apr 6, 2026
- [`a308ccd`](https://github.com/rooootdev/lara/commit/a308ccd5658e2c04349054483c90d591324f2769) Update icon URL and release date in source.json — *Pro42good DQ* · Apr 6, 2026


---

## Changed Files (1)

- 🟡 `source.json` (modified) — +3 / -3
  <details><summary>Diff</summary>

  ```diff
  @@ -16,12 +16,12 @@
       "developerName": "rooootdev",
       "subtitle": "iOS 18.7.1- & 26.0.1-",
       "localizedDescription": "lara will at its absolute best only ever support versions up to iOS 26.0.1/iOS 18.7.1. the exploit was patched after those versions.\n\nCurrently tested on iOS 17.2.1 - 26.0.1. If you run lara on your device, and it ends up working, please contact me on discord (@roooot.dev) and tell me:\n\n1. your device\n2. your iOS version\n3. what you tested in lara (eg. Run Exploit, Init KFS, etc.)\n\nIf lara doesnt work on your device, and you want to help the project, please also provide your logs and iOS version.\n\nFeatures:\n\nimplemented:\n• Font overwrite (VFS)\n• 3 app limit bypass\n• Unblacklisting\n• File Manager (Full Disk r/w)\n• dirtyZero 2 (Broken)\n\ncoming soon:\n• Mobilegestalt Editor\n\nknown issues:\n• on iOS 17.x, the kernel may panic when lara is closed from the app switcher.\n• dirtyZero does not work.\n• ui is buggy on 17.x\n• doesnt work on ipad m2?\n• doesnt work on a18 pro?\n• kernelcache download broken for some versions.\n\ntips:\ndeleting and redownloading kernelcache is known to fix many issues. do this before asking me for support. closing and reopening the app can fix font change issues. respringing is needed to apply springboard changes such as font changes.\n\ncredits:\n• opa334 for the kernel exploit poc, ChOma and XPF\n• AppInstaller iOS for help with offsets\n• AlfieCG for libgrabkernel2",
-      "iconURL": "https://raw.githubusercontent.com/rooootdev/lara/refs/heads/main/icon.png",
+      "iconURL": "https://raw.githubusercontent.com/rooootdev/lara/refs/heads/main/lara.png",
       "tintColor": "#5CA399",
       "versions": [
         {
-          "version": "1.1",
-          "date": "2025-05-19",
+          "version": "1.0",
+          "date": "2026-04-5",
           "size": 3450675,
           "downloadURL": "https://github.com/rooootdev/lara/releases/download/latest/lara.ipa",
           "localizedDescription": "lara will at its absolute best only ever support versions up to iOS 26.0.1/iOS 18.7.1. the exploit was patched after those versions.\n\nCurrently tested on iOS 17.2.1 - 26.0.1. If you run lara on your device, and it ends up working, please contact me on discord (@roooot.dev) and tell me:\n\n1. your device\n2. your iOS version\n3. what you tested in lara (eg. Run Exploit, Init KFS, etc.)\n\nIf lara doesnt work on your device, and you want to help the project, please also provide your logs and iOS version.\n\nFeatures:\n\nimplemented:\n• Font overwrite (VFS)\n• 3 app limit bypass\n• Unblacklisting\n• File Manager (Full Disk r/w)\n• dirtyZero 2 (Broken)\n\ncoming soon:\n• Mobilegestalt Editor\n\nknown issues:\n• on iOS 17.x, the kernel may panic when lara is closed from the app switcher.\n• dirtyZero does not work.\n• ui is buggy on 17.x\n• doesnt work on ipad m2?\n• doesnt work on a18 pro?\n• kernelcache download broken for some versions.\n\ntips:\ndeleting and redownloading kernelcache is known to fix many issues. do this before asking me for support. closing and reopening the app can fix font change issues. respringing is needed to apply springboard changes such as font changes.\n\ncredits:\n• opa334 for the kernel exploit poc, ChOma and XPF\n• AppInstaller iOS for help with offsets\n• AlfieCG for libgrabkernel2",
  ```

  </details>


---

## References

### Commits (1)

- [e95df52](https://github.com/danosito/lara-m-chips-fix/commit/e95df529ae2e8272d758dd4c84792999ead28af3) — Merge pull request #46 from Pro42good/patch-3


---

## Timeline (Verbose)

- : 📝 Commit ad42b5e: Change version from 1.1 to 1.0 in source.json
- Apr 6, 2026: 👀 @Pro42good requested review from @Copilot
- : 💬 someone reviewed: commented
- : 📝 Commit 4119b30: Update localizedDescription in source.json
- : 📝 Commit a308ccd: Update icon URL and release date in source.json
- Apr 6, 2026: 🟣 @rooootdev merged
- Apr 6, 2026: 🔴 Closed by @rooootdev
- Apr 19, 2026: 🔗 Commit e95df52 — Merge pull request #46 from Pro42good/patch-3


---

*Exported on 10/14/2026, 2:49:49 PM from [https://github.com/rooootdev/lara/pull/46](https://github.com/rooootdev/lara/pull/46)*
