# Kernel Cache Fails to resolve kernel symbols on iPadOS 17.7.10

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#95](https://github.com/rooootdev/lara/issues/95)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@rooootdev](https://github.com/rooootdev)  
> **Created:** Apr 13, 2026  
> **Updated:** Apr 16, 2026  
> **Closed:** Apr 16, 2026  

---

## Description

*Opened by [@SudoPigeon223](https://github.com/SudoPigeon223)* *on Apr 13, 2026*


When trying to download the kernel cache on a A10X iPad Pro Gen 2 running 17.7.10, it fails with the error “failed to resolve important kernel symbols.” The device was previously jailbroken with palera1n but was not in a jailbroken state when tested. Deleting and redownloading the cache along with manually placing the kernel kernelcache extracted from a .ipsw file on Mac also results in this issue.

Log:
```
lara started: 2026-04-12 06:00:54


livecontainer detected: nah

(utils) getprocsize() returned 0x0. defaulting to 0x740.

(utils) TASK_TNEXT_OFFSET: 0x58

(utils) THREAD_MUPCB_OFFSET: 0xf8

(utils) PROC_PID_OFFSET: 0x28

(utils) PROC_STRUCT_SIZE: 0x740

initialized offsetsdownloading kernelcache → /var/mobile/Containers/Data/Application/084D5CFF-3386-4549-8F6E-F56DB0F2DEDE/Documents/kernelcache

libgrabkernel2: Found firmware URL: https://updates.cdn-apple.com/2025SpringFCS/fullrestores/093-21857/D64B129B-269D-42A5-9637-DE16D6843DA1/iPad_Pro_HFR_17.7.10_21H450_Restore.ipsw (OTA: no)

libgrabkernel2: Downloading BuildManifest.plist...

libgrabkernel2: Downloading kernelcache.release.ipad7 to /var/mobile/Containers/Data/Application/084D5CFF-3386-4549-8F6E-F56DB0F2DEDE/Documents/kernelcache...

libgrabkernel2: Downloaded kernelcache!

kernel: Darwin Kernel Version 23.6.0: Fri Jul 11 17:39:39 PDT 2025; root:xnu-10063.142.1.703.8~1/RELEASE_ARM64_T8010

kernbase: 0xfffffff007004000

kernentry: 0xfffffff0071d04e8

failed to resolve important kernel symbols
```


---

## Timeline (Verbose)

- Apr 15, 2026: ✏️ @SudoPigeon223 renamed: "Kernel Cache Fails to Download on iPadOS 17.7.10" → "Kernel Cache Fails to resolve kernel symbols on iPadOS 17.7.10"
- Apr 16, 2026: 🔴 Closed by @rooootdev


---

## Comments (4)

### Comment #1 — [@julian-win](https://github.com/julian-win) · Apr 14, 2026

`ID: 4242835472`


Same problem same ipad same version


---

### Comment #2 — [@GZ-920](https://github.com/GZ-920) · Apr 14, 2026

`ID: 4244904460`


**lara kernelcache 下载失败解决方法**

如果 lara 一直提示 kernelcache 下载失败，需要手动导入。首先访问 `ipsw.dev`，找到你的设备型号和 iOS build 号对应的 IPSW 文件下载（iPad Air M3 对应 `iPad15,3` 或 `iPad15,5`）。下载后以压缩包形式打开，找到其中名为 `kernelcache.release.ipadXX` 的文件，重命名为 `kernelcache`（无后缀）。然后用Filza，将文件放入 lara 的沙盒目录：`/var/mobile/Containers/Data/Application/[lara的UUID]/Documents/`。UUID 可以从 lara 的日志里找到。放好后重新打开 lara 点击下载即可识别。


👍 2


---

### Comment #3 — [@julian-win](https://github.com/julian-win) · Apr 14, 2026

`ID: 4245278052`


> **lara kernelcache 下载失败解决方法**
> 
> 如果 lara 一直提示 kernelcache 下载失败，需要手动导入。首先访问 `ipsw.dev`，找到你的设备型号和 iOS build 号对应的 IPSW 文件下载（iPad Air M3 对应 `iPad15,3` 或 `iPad15,5`）。下载后以压缩包形式打开，找到其中名为 `kernelcache.release.ipadXX` 的文件，重命名为 `kernelcache`（无后缀）。然后用Filza，将文件放入 lara 的沙盒目录：`/var/mobile/Containers/Data/Application/[lara的UUID]/Documents/`。UUID 可以从 lara 的日志里找到。放好后重新打开 lara 点击下载即可识别。

failed to resolve important kernel symbols   

not download failed, please see logs first.


👍 1


---

### Comment #4 — [@rooootdev](https://github.com/rooootdev) · Apr 16, 2026

`ID: 4263190482`


kernel cache download is broken on some versions. use the temporary fix (see readme) in the meantime.


---

*Exported on 10/14/2026, 2:53:52 PM from [https://github.com/rooootdev/lara/issues/95](https://github.com/rooootdev/lara/issues/95)*
