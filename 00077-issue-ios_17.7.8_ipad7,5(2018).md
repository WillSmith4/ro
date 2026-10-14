# 解析关键内核字符失败。在ios 17.7.8 ipad7,5(2018)

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#77](https://github.com/rooootdev/lara/issues/77)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@rooootdev](https://github.com/rooootdev)  
> **Labels:** `bug`  
> **Created:** Apr 11, 2026  
> **Updated:** Apr 17, 2026  
> **Closed:** Apr 17, 2026  

---

## Description

*Opened by [@Da8m0n-Vye11in](https://github.com/Da8m0n-Vye11in)* *on Apr 11, 2026*


lara started: 2026-04-11 20:31:29


livecontainer detected: nah

(utils) getprocsize()返回0x0, 默认0x740。

(utils) TASK_TNEXT偏移: 0x58

(utils) THREAD_MUPCB偏移: 0xf8

(utils) PROC_PID偏移: 0x28

(utils) PROC结构大小: 0x740

偏移已初始化下载kernelcache -> /var/mobile/Containers/Data/Application/258ABFFC-505C-42DC-A88D-A5470A60EE2F/Documents/kernelcache

libgrabkernel2: 已找到固件URL: https://updates.cdn-apple.com/2025SpringFCS/fullrestores/082-53165/70E3BE3A-3C25-43A0-A8D9-8271C1AA802B/iPad_64bit_TouchID_ASTC_17.7.8_21H440_Restore.ipsw (OTA: no)

libgrabkernel2: 正在下载BuildManifest.plist...

libgrabkernel2: 正在下载kernelcache.release.ipad7b到/var/mobile/Containers/Data/Application/258ABFFC-505C-42DC-A88D-A5470A60EE2F/Documents/kernelcache...

libgrabkernel2: 已下载kernelcache!

内核: Darwin Kernel Version 23.6.0: Thu Apr 24 20:24:42 PDT 2025; root:xnu-10063.142.1.703.7~1/RELEASE_ARM64_T8010

kernbase: 0xfffffff007004000

kernentry: 0xfffffff0071d04e8

解析关键内核符号失败

使用缓存kernelcache

内核: Darwin Kernel Version 23.6.0: Thu Apr 24 20:24:42 PDT 2025; root:xnu-10063.142.1.703.7~1/RELEASE_ARM64_T8010

kernbase: 0xfffffff007004000

kernentry: 0xfffffff0071d04e8

解析关键内核符号失败


---

## Timeline (Verbose)

- Apr 17, 2026: 🏷️ @Pro42good added label `bug`
- Apr 17, 2026: 🔴 Closed by @rooootdev


---

## Comments (1)

### Comment #1 — [@rooootdev](https://github.com/rooootdev) · Apr 17, 2026

`ID: 4267664067`


try the temporary fix in the readme.


---

*Exported on 10/14/2026, 2:52:24 PM from [https://github.com/rooootdev/lara/issues/77](https://github.com/rooootdev/lara/issues/77)*
