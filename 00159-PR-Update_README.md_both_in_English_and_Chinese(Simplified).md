# Update README.md both in English and Chinese(Simplified)

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#159](https://github.com/rooootdev/lara/pull/159)  
> **State:** 🔴 closed  
> **Branch:** `Da8m0n-Vye11in:patch-2` → `rooootdev:main`  
> **Changes:** +85 / -88 across 1 files  
> **Created:** Apr 21, 2026  
> **Updated:** Apr 21, 2026  
> **Closed:** Apr 21, 2026  

---

## Description

*Opened by [@Da8m0n-Vye11in](https://github.com/Da8m0n-Vye11in)* *on Apr 21, 2026*


*No description provided.*


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (1)

- [`d1a1d3a`](https://github.com/rooootdev/lara/commit/d1a1d3aa994d2ba64fea773b1cfc8f7def328406) Update README.md — *Yankeelucas* · Apr 21, 2026


---

## Changed Files (1)

- 🟡 `README.md` (modified) — +85 / -88
  <details><summary>Diff</summary>

  ```diff
  @@ -4,7 +4,8 @@
   <br>
   <h1>LARA</h1>
 
-  <p>star this repo please :P</p>
+  <p>Please follow the official repository of LARA :P<br>
+  请关注lara的官方仓库 :P</p>
 </div>
 
 <p align="center">
@@ -23,102 +24,98 @@
 </p>
 
 <p align="center">
-  <a href="#support">support</a> •
-  <a href="#features">features</a> •
-  <a href="#installation">installation</a>
+  <a href="#support">support 支持</a> •
+  <a href="#features">features 功能</a> •
+  <a href="#installation">installation 安装</a>
 </p>
 
-## support
-lara will at its absolute best only ever support versions up to iOS 26.0.1/iOS 18.7.1. the exploit was patched after those versions.
+## Support 支持范围
+LARA will at its absolute best only ever support versions up to iOS 26.0.1/iOS 18.7.1. The exploit was patched after those versions.<br>
+LARA 只在 iOS26.0.1和iOS18.7.1上处于最好的状态。这个漏洞在此之后的几个版本更新中被修复。
 
-currently tested on iOS 17.1 - 26.0.1, up to iOS 18.7.1 only on the 18.7 series.
+Currently tested on iOS 17.1 - 26.0.1, up to iOS 18.7.1 only on the 18.7 series.<br>
+只在iOS17.1-26.0.1上进行过完整测试，在18.7系列里，只有iOS18.7.1支持。
 
-## compatibility
-
-| series | version / chip | status |
+## Comparison 对比
+| Series 系列 | Version/Chip 版本/芯片 | Status 状态 |
 | :--- | :--- | :--- |
-| **iOS 17** | all versions | supported |
-| **iOS 18** | 18.0 — 18.7.1 | supported |
-| **iOS 26.0/26.0.1** | 26.0 — 26.0.1 **only** | supported |
-| **iOS 26.1+** | 26.1+ | **patched** |
-| **M-series chips** | M1, M2, M3, etc...  | **unsupported** |
+| **iOS 17** | All versions(unsupport 17.8 - 17.10 所有版本 (不支持17.8 - 17.10 | Supported 支持 |
+| **iOS 18** | 18.0 – 18.7.1 | Supported 支持 |
+| **iOS 26.0/26.0.1** | 26.0 – 26.0.1 only 只支持26.0 – 26.0.1 | Supported 支持 |
+| **iOS 26.1+** | 26.1+ | **Patched 已修补** |
+| **M-series Chips** | M1, M2, M3, etc... | **Not Supported 不支持** |
 
 > [!CAUTION]
-> if you are on an M-series device or any iOS version higher than 26.0.1, the app will crash on launch. this isn't a bug, lara just doesnt support those devices yet.
+> If you are on an M-series device or any iOS version higher than 26.0.1, the app will crash on launch.
+> This isn't a bug; LARA just doesn’t support those devices yet.<br>
 > 
-> **ISSUES THAT INVOLVE LARA NOT WORKING ON UNSUPPORTED VERSIONS WILL BE CLOSED IMMEDIATELY.**<br>
-> **Issues related to lara not working on versions that the exploit DOES technically support will be closed and added to the known issues section**
-
-If you run lara on your device, and it ends up working, please contact me on [discord](https://discord.gg/gw8PcRF3Jr) and tell me:
-1. your device
-2. your iOS version
-4. what you tested in lara (eg. Run Exploit, Init KFS, etc.)
-
-If lara doesnt work on your device, and you want to help the project, please also provide your logs and iOS version.
-
-## features:
-### implemented:
-- Font Overwrite
-- Custom Overwrite
-- Card Overwrite
-- File Manager (Full Disk r/w)
-- MobileGestalt Editor
-- 3 App Bypass
-- DirtyZero 2
-- 5 App Dock
-- Status Bar Tweaks
-- Hide labels
-- Upside Down
-- Floating Dock (Broken)
-- Grid App Switcher
-- Performance 
-
-### coming soon:
-- JIT
-- App Decrypt
-
-## known issues:
-- wont work on M5, A19 and A19 Pro due to MTE
-- on iOS 17.x, the kernel may panic when lara is closed from the app switcher.
-- downloading OTA updates does not work.
-- dirtyzero does not work.
-- ui is buggy on 17.x
-- .aea ota updates do not work.
-- A16+ and M-series devices dont support RemoteCall (yet)
-
-### fixes:
-kernelcache download fix:
-1. Download the IPSW tool for your device
-[here](https://github.com/blacktop/ipsw/releases/tag/v3.1.671).
-2. Extract the archive.
-3. Open Terminal.
-4. Navigate to the extracted folder:<br>
-`cd /path/to/ipsw_3.1.671_something_something/`
-5. Extract the kernel:
-./ipsw extract --kernel [drag your ipsw here]
-6. Get the kernelcache file.
-7. Transfer the kernelcache to your iPhone.
-8. In the Files app:
-   - Go to "On My iPhone" > "lara"
-   - Place the kernelcache file there.
-9. Rename the file to "kernelcache" (without extension).
-
-## installation:
+> 如果你拥有M系列设备搭载任意iOS版本或者系统版本高于26.0.1，那么LARA会在启动时崩溃，这不是bug，只是LARA不支持在这些版本上运行。
+>
+> **ANY ISSUES THAT INVOLVE LARA NOT WORKING ON UNSUPPORTED VERSIONS WILL BE CLOSED IMMEDIATELY.**<br>
+> **任何说LARA在不支持的版本上启动崩溃的议题，都会被迅速关闭。**
+
+If you run LARA on your device, and it ends up working, please contact me on [Discord](https://discord.gg/gw8PcRF3Jr) and tell me:
+1. Your device
+2. Your iOS version
+3. What you tested in LARA (e.g., Run Exploit, Init KFS, etc.)<br>
+如果你在设备上运行LARA且成功，请通过[Discord](https://discord.gg/gw8PcRF3Jr)联系我并告诉我：
+- 你的设备
+- 你的iOS版本
+- 在LARA中测试了什么（例如：运行Exploit, 初始化KFS等）。
+
+If LARA doesn’t work on your device, and you want to help the project, please also provide your logs and iOS version.<br>
+如果LARA在你的设备上无法运行并希望帮助改善项目，请提供日志和iOS系统版本。
+
+## Features 功能
+### Implemented 已实现
+- Font Overwrite 字体覆盖
+- Custom Overwrite 自定义覆盖
+- File Manager (Full Disk r/w) 文件管理器（完整磁盘读写）
+- MobileGestalt Editor MobileGestalt编辑器
+- 3 App Bypass 3 应用绕过
+- DirtyZero 2 (Broken) DirtyZero 2 (损坏)
+
+### Coming Soon 即将到来:
+- remotecall????
+
+## Known Issues 已知问题
+- Won't work on M5, A19, and A19 Pro due to MTE<br>
+  因为MTE无法在M5, A19和A19 Pro 上运行
+- On iOS 17.x, the kernel may panic when LARA is closed from the app switcher.<br>
+  在iOS 17.x 上，从应用程序切换器关闭LARA时可能会导致内核崩溃
+- Downloading OTA updates does not work.<br>
+  OTA更新下载无效
+- DirtyZero does not work.<br>
+  DirtyZero 功能无效。
+- UI is buggy on 17.x<br>
+  在17.x版本的UI存在问题。
+- Doesn’t work on iPad M2?<br>
+  可能无法在iPad M2运行
+- Kernelcache download broken for some versions.<br>
+  某些版本的Kernelcache下载受损。
+
+## Installation 安装
 <a href="https://celloserenity.github.io/altdirect/?url=https://raw.githubusercontent.com/rooootdev/lara/refs/heads/main/source.json" target="_blank">
    <img src="https://github.com/CelloSerenity/altdirect/blob/main/assets/png/AltSource_Blue.png?raw=true" alt="Add AltSource" width="200">
 </a>
-<a href="https://github.com/rooootdev/lara/releases/download/latest/lara.ipa" target="_blank"><img src="https://github.com/CelloSerenity/altdirect/blob/main/assets/png/Download_Blue.png?raw=true" alt="Download .ipa" width="200"></a>
-
-## tips:
-deleting and redownloading kernelcache is known to fix many issues. do this before asking me for support.  
-closing and reopening the app can fix font change issues.
-respringing is needed to apply springboard changes such as font changes.
-
-## credits:
-- opa334 for the kernel exploit poc, ChOma and XPF
-- AppInstaller iOS for help with offsets
-- AlfieCG for libgrabkernel2
-- Everyone who contributed!
+<a href="https://github.com/rooootdev/lara/releases/download/latest/lara.ipa" target="_blank">
+   <img src="https://github.com/CelloSerenity/altdirect/blob/main/assets/png/Download_Blue.png?raw=true" alt="Download IPA" width="200">
+</a>
 
-<br><br><br><br>
-<div align="center">a beautiful kexploit ♥️</div>
+## Tips 贴士
+- Deleting and redownloading Kernelcache is known to fix many issues. Do this before asking me for support.<br>
+  删除并重新下载Kernelcache可以解决大多数问题。在向我求助之前，请尝试这样做。
+- Closing and reopening the app can fix font change issues.<br>
+  关闭并重新打开应用程序可以解决字体更改问题。
+- Respringing is needed to apply Springboard changes such as font changes.<br>
+  Respring可以应用Springboard更改，例如字体更改。
+
+## Credits 鸣谢
+- opa334 for the Kernel Exploit POC, ChOma, and XPF<br>
+  感谢opa334提供的内核漏洞验证、ChOma 和 XPF
+- AppInstaller iOS for help with offsets<br>
+  感谢AppInstaller iOS在offsets方面的帮助。
+- AlfieCG for libgrabkernel2<br>
+  感谢AlfieCG提供libgrabkernel2
+- Everyone who contributed!<br>
+  每一位贡献者！
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit d1a1d3a: Update README.md
- Apr 21, 2026: 🔴 Closed by @Da8m0n-Vye11in


---

*Exported on 10/14/2026, 2:59:03 PM from [https://github.com/rooootdev/lara/pull/159](https://github.com/rooootdev/lara/pull/159)*
