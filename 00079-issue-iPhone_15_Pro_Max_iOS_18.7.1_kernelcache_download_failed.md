# iPhone 15 Pro Max iOS 18.7.1 kernelcache download failed

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#79](https://github.com/rooootdev/lara/issues/79)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@rooootdev](https://github.com/rooootdev)  
> **Created:** Apr 11, 2026  
> **Updated:** Apr 16, 2026  
> **Closed:** Apr 16, 2026  

---

## Description

*Opened by [@osama-alwaeli](https://github.com/osama-alwaeli)* *on Apr 11, 2026*


downloading kernelcache → /var/mobile/Containers/Data/Application/B12FF385-7B79-4866-9550-F08E12A5FB51/Documents/kernelcache

libgrabkernel2: Found firmware URL: https://updates.cdn-apple.com/2025FallFCS/mobileassets/093-53595/C30A3B9D-50E1-4F56-B302-A98FE22F32EA/com_apple_MobileAsset_SoftwareUpdate/b2b3a37382aae7b85968106820a5e324f5ee27b6634a894426707fc9a505dede.aea (OTA: yes)

libgrabkernel2: ERROR: Failed to open zip file! End of central directory signature is invalid! Expected 6054b50, got 88dccb90

kernelcache download failed

downloading kernelcache → /var/mobile/Containers/Data/Application/B12FF385-7B79-4866-9550-F08E12A5FB51/Documents/kernelcache

libgrabkernel2: Found firmware URL: https://updates.cdn-apple.com/2025FallFCS/mobileassets/093-53595/C30A3B9D-50E1-4F56-B302-A98FE22F32EA/com_apple_MobileAsset_SoftwareUpdate/b2b3a37382aae7b85968106820a5e324f5ee27b6634a894426707fc9a505dede.aea (OTA: yes)

libgrabkernel2: ERROR: Failed to open zip file! End of central directory signature is invalid! Expected 6054b50, got 88dccb90

kernelcache download failed

downloading kernelcache → /var/mobile/Containers/Data/Application/B12FF385-7B79-4866-9550-F08E12A5FB51/Documents/kernelcache

libgrabkernel2: Found firmware URL: https://updates.cdn-apple.com/2025FallFCS/mobileassets/093-53595/C30A3B9D-50E1-4F56-B302-A98FE22F32EA/com_apple_MobileAsset_SoftwareUpdate/b2b3a37382aae7b85968106820a5e324f5ee27b6634a894426707fc9a505dede.aea (OTA: yes)

libgrabkernel2: ERROR: Failed to open zip file! End of central directory signature is invalid! Expected 6054b50, got 88dccb90

kernelcache download failed

downloading kernelcache → /var/mobile/Containers/Data/Application/B12FF385-7B79-4866-9550-F08E12A5FB51/Documents/kernelcache

libgrabkernel2: Found firmware URL: https://updates.cdn-apple.com/2025FallFCS/mobileassets/093-53595/C30A3B9D-50E1-4F56-B302-A98FE22F32EA/com_apple_MobileAsset_SoftwareUpdate/b2b3a37382aae7b85968106820a5e324f5ee27b6634a894426707fc9a505dede.aea (OTA: yes)

libgrabkernel2: ERROR: Failed to open zip file! End of central directory signature is invalid! Expected 6054b50, got 88dccb90

kernelcache download failed

<img width="1290" height="2796" alt="Image" src="https://github.com/user-attachments/assets/e8970fc8-c64b-4998-a9fa-acb7ca071e13" />


👍 1


---

## Timeline (Verbose)

- Apr 16, 2026: 🔴 Closed by @rooootdev


---

## Comments (2)

### Comment #1 — [@GZ-920](https://github.com/GZ-920) · Apr 14, 2026

`ID: 4244827485`


**lara kernelcache 下载失败解决方法**

如果 lara 一直提示 kernelcache 下载失败，需要手动导入。首先访问 `ipsw.dev`，找到你的设备型号和 iOS build 号对应的 IPSW 文件下载（iPad Air M3 对应 `iPad15,3` 或 `iPad15,5`）。下载后以压缩包形式打开，找到其中名为 `kernelcache.release.ipadXX` 的文件，重命名为 `kernelcache`（无后缀）。然后用Filza，将文件放入 lara 的沙盒目录：`/var/mobile/Containers/Data/Application/[lara的UUID]/Documents/`。UUID 可以从 lara 的日志里找到。放好后重新打开 lara 点击下载即可识别。


👍 1


---

### Comment #2 — [@rooootdev](https://github.com/rooootdev) · Apr 16, 2026

`ID: 4263165138`


.aea ota updates are currently broken. use the temporary fix (see readme) in the meantime.


---

*Exported on 10/14/2026, 2:52:35 PM from [https://github.com/rooootdev/lara/issues/79](https://github.com/rooootdev/lara/issues/79)*
