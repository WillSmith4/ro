# Kernelcache not downloading

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#24](https://github.com/rooootdev/lara/issues/24)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@rooootdev](https://github.com/rooootdev)  
> **Created:** Apr 3, 2026  
> **Updated:** Apr 16, 2026  
> **Closed:** Apr 16, 2026  

---

## Description

*Opened by [@clogan9019-dotcom](https://github.com/clogan9019-dotcom)* *on Apr 3, 2026*


It says Kernelcache is missing I click download nothing happens I sideloaded it with Esign im on 18.7.1 on a 16e. Here are the logs 

lara started: 2026-04-03 09:13:09


livecontainer detected: nah

getprocsize() returned 0x0. defaulting to 0x740.

PROC_PID_OFFSET: 0x60

PROC_STRUCT_SIZE: 0x740

downloading kernelcache to: /var/mobile/Containers/Data/Application/9CD0D506-312F-4257-8C2D-E5443B4C654F/Documents/kernelcache

libgrabkernel2: Found firmware URL: https://updates.cdn-apple.com/2025FallFCS/mobileassets/093-53276/D29FA64F-E12C-43DE-97DF-25BD041FCA34/com_apple_MobileAsset_SoftwareUpdate/c2da33e15b92b5df9284b05a225c974108343182dbb6984dc508f49dde8586c0.aea (OTA: yes)

libgrabkernel2: ERROR: Failed to open zip file! End of central directory signature is invalid! Expected 6054b50, got 70e09292

failed to download kernelcache


---

## Timeline (Verbose)

- Apr 16, 2026: 🔴 Closed by @rooootdev


---

## Comments (7)

### Comment #1 — [@bakirfatah99](https://github.com/bakirfatah99) · Apr 4, 2026

`ID: 4187560287`


Dont work for iphone 17 pro max ios 26.0. Why?


---

### Comment #2 — [@Ltsparten88](https://github.com/Ltsparten88) · Apr 4, 2026

`ID: 4187614398`


Similar error on my iPad pro 2nd gen (17.7.10)


---

### Comment #3 — [@kickTROLL](https://github.com/kickTROLL) · Apr 6, 2026

`ID: 4189876472`


Same for me on iPhone 16 18.7.1 

here are my logs 
downloading kernelcache → /var/mobile/Containers/Data/Application/7141EC39-1C09-42E3-A46F-F87FB792928F/Documents/kernelcache

libgrabkernel2: Found firmware URL: https://updates.cdn-apple.com/2025FallFCS/mobileassets/093-53638/254E281E-1777-4007-B057-1D6273577A87/com_apple_MobileAsset_SoftwareUpdate/f10c5dfe7f3f3c713a093cc786a3cf54c9289a4a38db2c3b7f6c9b46b11e75ea.aea (OTA: yes)

libgrabkernel2: ERROR: Failed to open zip file! End of central directory signature is invalid! Expected 6054b50, got cb39edf7

kernelcache download failed


---

### Comment #4 — [@AppropriateNet2928](https://github.com/AppropriateNet2928) · Apr 6, 2026

`ID: 4192121868`


> It says Kernelcache is missing I click download nothing happens I sideloaded it with Esign im on 18.7.1 on a 16e. Here are the logs
> 
> lara started: 2026-04-03 09:13:09
> 
> livecontainer detected: nah
> 
> getprocsize() returned 0x0. defaulting to 0x740.
> 
> PROC_PID_OFFSET: 0x60
> 
> PROC_STRUCT_SIZE: 0x740
> 
> downloading kernelcache to: /var/mobile/Containers/Data/Application/9CD0D506-312F-4257-8C2D-E5443B4C654F/Documents/kernelcache
> 
> libgrabkernel2: Found firmware URL: https://updates.cdn-apple.com/2025FallFCS/mobileassets/093-53276/D29FA64F-E12C-43DE-97DF-25BD041FCA34/com_apple_MobileAsset_SoftwareUpdate/c2da33e15b92b5df9284b05a225c974108343182dbb6984dc508f49dde8586c0.aea (OTA: yes)
> 
> libgrabkernel2: ERROR: Failed to open zip file! End of central directory signature is invalid! Expected 6054b50, got 70e09292
> 
> failed to download kernelcache


BC you have OTA, and the dev said that OTAs are his biggest enemies..................


---

### Comment #5 — [@AppropriateNet2928](https://github.com/AppropriateNet2928) · Apr 6, 2026

`ID: 4192123517`


> Same for me on iPhone 16 18.7.1
> 
> here are my logs downloading kernelcache → /var/mobile/Containers/Data/Application/7141EC39-1C09-42E3-A46F-F87FB792928F/Documents/kernelcache
> 
> libgrabkernel2: Found firmware URL: https://updates.cdn-apple.com/2025FallFCS/mobileassets/093-53638/254E281E-1777-4007-B057-1D6273577A87/com_apple_MobileAsset_SoftwareUpdate/f10c5dfe7f3f3c713a093cc786a3cf54c9289a4a38db2c3b7f6c9b46b11e75ea.aea (OTA: yes)
> 
> libgrabkernel2: ERROR: Failed to open zip file! End of central directory signature is invalid! Expected 6054b50, got cb39edf7
> 
> kernelcache download failed




BC you have OTA, and the dev said that OTAs are his biggest enemies..................


---

### Comment #6 — [@GZ-920](https://github.com/GZ-920) · Apr 14, 2026

`ID: 4244909896`


**lara kernelcache 下载失败解决方法**

如果 lara 一直提示 kernelcache 下载失败，需要手动导入。首先访问 `ipsw.dev`，找到你的设备型号和 iOS build 号对应的 IPSW 文件下载（iPad Air M3 对应 `iPad15,3` 或 `iPad15,5`）。下载后以压缩包形式打开，找到其中名为 `kernelcache.release.ipadXX` 的文件，重命名为 `kernelcache`（无后缀）。然后用Filza，将文件放入 lara 的沙盒目录：`/var/mobile/Containers/Data/Application/[lara的UUID]/Documents/`。UUID 可以从 lara 的日志里找到。放好后重新打开 lara 点击下载即可识别。


---

### Comment #7 — [@rooootdev](https://github.com/rooootdev) · Apr 16, 2026

`ID: 4262721667`


added to known issues, a temporary fix will be available soon.


---

*Exported on 10/14/2026, 2:48:07 PM from [https://github.com/rooootdev/lara/issues/24](https://github.com/rooootdev/lara/issues/24)*
