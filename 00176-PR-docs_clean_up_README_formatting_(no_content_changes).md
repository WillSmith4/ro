# docs: clean up README formatting (no content changes)

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#176](https://github.com/rooootdev/lara/pull/176)  
> **State:** 🟣 merged  
> **Branch:** `tyhallcsu:docs/readme-formatting` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 23, 2026  
> **Changes:** +65 / -33 across 1 files  
> **Created:** Apr 23, 2026  
> **Updated:** Apr 23, 2026  
> **Closed:** Apr 23, 2026  

---

## Description

*Opened by [@tyhallcsu](https://github.com/tyhallcsu)* *on Apr 23, 2026*


## Summary

Two commits on `README.md`:

1. **Formatting-only pass** — no wording changes.
2. **Explain what the kernelcache is for** — new readers previously hit the "kernelcache download fix" steps with no context about what the kernelcache is, why lara needs it, or when these steps apply. Added a short subsection sourced from the code (`lara/kexploit/offsets.m` + `lara/views/SettingsView.swift`).

### Commit 1 — formatting

**Content-adjacent (minor) fixes**
- Fix the numbered-list typo in the "please contact me" block — steps were `1, 2, 4`, now `1, 2, 3`.
- Fix the split link in the kernelcache fix step 1 — the `[here](...)` markdown was broken across two lines so the anchor text rendered with a dangling "for your device" on the previous line.

**Pure formatting**
- Drop trailing colons on headings (`## features:` → `## features`) so they match `## support` / `## compatibility` and yield cleaner anchors.
- Blank line after headings (CommonMark-friendly).
- Fence shell commands in ```` ```sh ```` code blocks (the `./ipsw extract …` line was prose, the `cd` line was inline-code — now consistent).
- Wrap `t1sz_boot` in backticks in the CAUTION block (it's a setting name, matches how `0x11` is already quoted).
- Center install buttons in `<p align="center">` so the two badges sit together like the shields row at the top.
- Align the compatibility-table columns in the source (same rendered output).
- Convert `tips` from three lines-with-trailing-spaces into a proper bulleted list.
- Expand the top nav to cover all sections (was 3/7).
- Horizontal rule below the nav.

### Commit 2 — kernelcache purpose

Adds an `### fixes → about the kernelcache` subsection before the existing fix steps, explaining:

- What the kernelcache is: the iOS kernel binary for the user's exact iOS version + device.
- Why lara needs it: runs a patchfinder ([opa334's XPF](https://github.com/opa334/ChOma) via `libgrabkernel2`) against it to resolve kernel symbols (`kernproc`, `rootvnode`) and struct sizes (`proc.struct_size`, etc.) — these change every iOS release and every SoC, so nothing is hardcoded. See `lara/kexploit/offsets.m:1037-1091` (`resolvekernoffsets`).
- When the manual fix steps apply: the in-app **Download Kernelcache** button (`dlkerncache()` → `grab_kernelcache`) tries Apple's IPSW servers first; the manual path is the fallback for when that fails, combined with **Import Kernelcache from Files**.
- What **Delete Kernelcache Data** in Settings does — ties the "delete and redownload" tip back to something concrete.

Doc-only — no code or behavior changes.

### What is NOT changed

- No changes to wording outside the two minor fixes in commit 1 and the new subsection in commit 2.
- No changes to badges, images, links, caution copy, feature list, known-issues list, fix steps themselves, credits, or the closing "a beautiful kexploit ♥️".
- No changes outside `README.md`.

## Test plan

- [x] `README.md` renders on GitHub with all sections, images, and the CAUTION admonition intact.
- [x] All anchor links in the top nav resolve.
- [x] Code blocks render as fenced `sh` blocks.
- [x] New "about the kernelcache" subsection sits cleanly between known-issues and the fix steps.

---

Not affiliated with the project — drive-by cleanup. Feel free to cherry-pick and drop the rest.


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (2)

- [`df53a1a`](https://github.com/rooootdev/lara/commit/df53a1a9727e19ea0e819ae962bb1bef53e2ed90) docs: clean up README formatting — *tyhallcsu* · Apr 23, 2026
- [`a25af29`](https://github.com/rooootdev/lara/commit/a25af29d3f84218e123fb31be7a91ea130fa52b6) docs: explain why lara needs a kernelcache — *tyhallcsu* · Apr 23, 2026


---

## Changed Files (1)

- 🟡 `README.md` (modified) — +65 / -33
  <details><summary>Diff</summary>

  ```diff
  @@ -24,40 +24,50 @@
 
 <p align="center">
   <a href="#support">support</a> •
+  <a href="#compatibility">compatibility</a> •
   <a href="#features">features</a> •
-  <a href="#installation">installation</a>
+  <a href="#known-issues">known issues</a> •
+  <a href="#installation">installation</a> •
+  <a href="#tips">tips</a> •
+  <a href="#credits">credits</a>
 </p>
 
+---
+
 ## support
+
 lara will at its absolute best only ever support versions up to iOS 26.0.1/iOS 18.7.1. the exploit was patched after those versions.
 
 currently tested on iOS 17.1 - 26.0.1, up to iOS 18.7.1 only on the 18.7 series.
 
 ## compatibility
 
-| series | version / chip | status |
-| :--- | :--- | :--- |
-| **iOS 17** | all versions | supported |
-| **iOS 18** | 18.0 — 18.7.1 | supported |
-| **iOS 26.0/26.0.1** | 26.0 — 26.0.1 **only** | supported |
-| **iOS 26.1+** | 26.1+ | **patched** |
-| **M-series chips** | M1 - M4  | partially supported. YMMV |
+| series              | version / chip    | status                    |
+| :------------------ | :---------------- | :------------------------ |
+| **iOS 17**          | all versions      | supported                 |
+| **iOS 18**          | 18.0 — 18.7.1     | supported                 |
+| **iOS 26.0/26.0.1** | 26.0 — 26.0.1 **only** | supported            |
+| **iOS 26.1+**       | 26.1+             | **patched**               |
+| **M-series chips**  | M1 - M4           | partially supported. YMMV |
 
 > [!CAUTION]
-> if you are on an M-series device, go to lara settings, scroll down set t1sz_boot to `0x11`. if you are on any iOS version higher than 26.0.1 the app will crash on launch. this isn't a bug, lara just doesnt support those devices.
-> 
+> if you are on an M-series device, go to lara settings, scroll down set `t1sz_boot` to `0x11`. if you are on any iOS version higher than 26.0.1 the app will crash on launch. this isn't a bug, lara just doesnt support those devices.
+>
 > **ISSUES THAT INVOLVE LARA NOT WORKING ON UNSUPPORTED VERSIONS WILL BE CLOSED IMMEDIATELY.**<br>
 > **Issues related to lara not working on versions that the exploit DOES technically support will be closed and added to the known issues section**
 
 If you run lara on your device, and it ends up working, please contact me on [discord](https://discord.gg/gw8PcRF3Jr) and tell me:
+
 1. your device
 2. your iOS version
-4. what you tested in lara (eg. Run Exploit, Init KFS, etc.)
+3. what you tested in lara (eg. Run Exploit, Init KFS, etc.)
 
 If lara doesnt work on your device, and you want to help the project, please also provide your logs and iOS version.
 
-## features:
-### implemented:
+## features
+
+### implemented
+
 - Font Overwrite
 - Custom Overwrite
 - Card Overwrite
@@ -74,10 +84,12 @@ If lara doesnt work on your device, and you want to help the project, please als
 - Performance
 - JIT
 
-### coming soon:
+### coming soon
+
 - App Decrypt
 
-## known issues:
+## known issues
+
 - wont work on M5, A19 and A19 Pro due to MTE
 - on iOS 17.x, the kernel may panic when lara is closed from the app switcher.
 - downloading OTA updates does not work.
@@ -87,35 +99,55 @@ If lara doesnt work on your device, and you want to help the project, please als
 - A16+ and M-series devices dont support RemoteCall (yet)
 - apps don't detect JIT enabled however they are enabled.
 
-### fixes:
-kernelcache download fix:
-1. Download the IPSW tool for your device
-[here](https://github.com/blacktop/ipsw/releases/tag/v3.1.671).
+### fixes
+
+#### about the kernelcache
+
+lara needs the **kernelcache** (the iOS kernel binary from your exact iOS version + device) to run. on first launch it runs a patchfinder ([opa334's XPF](https://github.com/opa334/ChOma) via libgrabkernel2) against the kernelcache to locate the kernel symbols and struct offsets the exploit touches — `kernproc`, `rootvnode`, `proc` size, etc. these move on every iOS release and every SoC, so lara can't ship them hardcoded.
+
+the app tries to download the kernelcache for you automatically (the **Download Kernelcache** button in Settings hits Apple's IPSW servers). when that fails — usually a network/CDN hiccup or an unusual device/build combo — grab one manually with the steps below and import it via **Import Kernelcache from Files**.
+
+if things get weird later, **Delete Kernelcache Data** in Settings wipes the cached kernelcache and the saved offsets, and you start over. that's what the "delete and redownload" line in [tips](#tips) is about.
+
+**kernelcache download fix (manual fallback):**
+
+1. Download the IPSW tool for your device [here](https://github.com/blacktop/ipsw/releases/tag/v3.1.671).
 2. Extract the archive.
 3. Open Terminal.
-4. Navigate to the extracted folder:<br>
-`cd /path/to/ipsw_3.1.671_something_something/`
+4. Navigate to the extracted folder:
+   ```sh
+   cd /path/to/ipsw_3.1.671_something_something/
+   ```
 5. Extract the kernel:
-./ipsw extract --kernel [drag your ipsw here]
+   ```sh
+   ./ipsw extract --kernel [drag your ipsw here]
+   ```
 6. Get the kernelcache file.
 7. Transfer the kernelcache to your iPhone.
 8. In the Files app:
    - Go to "On My iPhone" > "lara"
    - Place the kernelcache file there.
-9. Rename the file to "kernelcache" (without extension).
+9. Rename the file to `kernelcache` (without extension).
+
+## installation
+
+<p align="center">
+  <a href="https://celloserenity.github.io/altdirect/?url=https://raw.githubusercontent.com/rooootdev/lara/refs/heads/main/source.json" target="_blank">
+    <img src="https://github.com/CelloSerenity/altdirect/blob/main/assets/png/AltSource_Blue.png?raw=true" alt="Add AltSource" width="200">
+  </a>
+  <a href="https://github.com/rooootdev/lara/releases/download/latest/lara.ipa" target="_blank">
+    <img src="https://github.com/CelloSerenity/altdirect/blob/main/assets/png/Download_Blue.png?raw=true" alt="Download .ipa" width="200">
+  </a>
+</p>
+
+## tips
 
-## installation:
-<a href="https://celloserenity.github.io/altdirect/?url=https://raw.githubusercontent.com/rooootdev/lara/refs/heads/main/source.json" target="_blank">
-   <img src="https://github.com/CelloSerenity/altdirect/blob/main/assets/png/AltSource_Blue.png?raw=true" alt="Add AltSource" width="200">
-</a>
-<a href="https://github.com/rooootdev/lara/releases/download/latest/lara.ipa" target="_blank"><img src="https://github.com/CelloSerenity/altdirect/blob/main/assets/png/Download_Blue.png?raw=true" alt="Download .ipa" width="200"></a>
+- deleting and redownloading kernelcache is known to fix many issues. do this before asking me for support.
+- closing and reopening the app can fix font change issues.
+- respringing is needed to apply springboard changes such as font changes.
 
-## tips:
-deleting and redownloading kernelcache is known to fix many issues. do this before asking me for support.  
-closing and reopening the app can fix font change issues.
-respringing is needed to apply springboard changes such as font changes.
+## credits
 
-## credits:
 - opa334 for the kernel exploit poc, ChOma and XPF
 - AppInstaller iOS for help with offsets
 - AlfieCG for libgrabkernel2
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit df53a1a: docs: clean up README formatting
- : 📝 Commit a25af29: docs: explain why lara needs a kernelcache
- Apr 23, 2026: 🟣 @rooootdev merged
- Apr 23, 2026: 🔴 Closed by @rooootdev


---

*Exported on 10/14/2026, 3:00:44 PM from [https://github.com/rooootdev/lara/pull/176](https://github.com/rooootdev/lara/pull/176)*
