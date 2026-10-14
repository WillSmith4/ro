# 3 app bypass

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#42](https://github.com/rooootdev/lara/issues/42)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@rooootdev](https://github.com/rooootdev)  
> **Created:** Apr 6, 2026  
> **Updated:** Apr 10, 2026  
> **Closed:** Apr 10, 2026  

---

## Description

*Opened by [@Nnnnnnn274](https://github.com/Nnnnnnn274)* *on Apr 6, 2026*


It is broken it shows i have no apps but i have 3 and it freezes when i press bypass


---

## Timeline (Verbose)

- Apr 10, 2026: 🔴 Closed by @rooootdev


---

## Comments (5)

### Comment #1 — [@neonmodder123](https://github.com/neonmodder123) · Apr 6, 2026

`ID: 4192428575`


The feature is literally labeled as broken


---

### Comment #2 — [@Pro42good](https://github.com/Pro42good) · Apr 6, 2026

`ID: 4192555407`


Can Confirm, the feature is literally labeled broken within the app.


---

### Comment #3 — [@ctrssss](https://github.com/ctrssss) · Apr 6, 2026

`ID: 4192849723`


broken, logs literally says:

`(sbx) copy back failed /private/var/containers/Bundle/Application/2EE7EA4B-854C-4ECE-92D3-ACC75CF97B28/lara.app: “lara” couldn’t be removed because you don’t have permission to access it.`


more logs:

```
(sbx) copying /private/var/containers/Bundle/Application/2EE7EA4B-854C-4ECE-92D3-ACC75CF97B28/lara.app -> /private/var/mobile/Containers/Data/Application/B8AC16B4-7191-4300-BAC2-5A90730BDF4D/tmp/sbx_copy_test/lara.app
(sbx) copy success /private/var/mobile/Containers/Data/Application/B8AC16B4-7191-4300-BAC2-5A90730BDF4D/tmp/sbx_copy_test/lara.app
(sbx) set test xattr on staged copy: /private/var/mobile/Containers/Data/Application/B8AC16B4-7191-4300-BAC2-5A90730BDF4D/tmp/sbx_copy_test/lara.app
(sbx) verified test xattr exists on staged copy: /private/var/mobile/Containers/Data/Application/B8AC16B4-7191-4300-BAC2-5A90730BDF4D/tmp/sbx_copy_test/lara.app size=3
(sbx) removing original app: /private/var/containers/Bundle/Application/2EE7EA4B-854C-4ECE-92D3-ACC75CF97B28/lara.app
(sbx) copy back failed /private/var/containers/Bundle/Application/2EE7EA4B-854C-4ECE-92D3-ACC75CF97B28/lara.app: “lara” couldn’t be removed because you don’t have permission to access it.
```


👍 3


---

### Comment #4 — [@Majhool](https://github.com/Majhool) · Apr 7, 2026

`ID: 4197021350`


Broken also on iPhone 11 with iOS18.0.1

This is log for proccess:

(vfs) target /private/var/mobile/Containers/Data/Application/F6761B0B-79EA-4E49-A8A7-6294A5B20108/tmp/vfs_src_3786809165.bin -> /private/var/db/MobileIdentityData/Rejections.plist

(vfs) vfs_overwritefile returned: 0

(vfs) file overwritten

(vfs) target /private/var/mobile/Containers/Data/Application/F6761B0B-79EA-4E49-A8A7-6294A5B20108/tmp/vfs_src_3977461729.bin -> /private/var/db/MobileIdentityData/AuthListBannedUpps.plist

(vfs) vfs_overwritefile returned: 0

(vfs) file overwritten

(vfs) target /private/var/mobile/Containers/Data/Application/F6761B0B-79EA-4E49-A8A7-6294A5B20108/tmp/vfs_src_2967987821.bin -> /private/var/db/MobileIdentityData/AuthListBannedCdHashes.plist

(vfs) vfs_overwritefile returned: 0

(vfs) file overwritten
(vfs) overwrite: /private/var/db/MobileIdentityData/Rejections.plist <- /private/var/mobile/Containers/Data/Application/F6761B0B-79EA-4E49-A8A7-6294A5B20108/tmp/vfs_src_3786809165.bin
(vfs) target mmap'd at 0x10bffc000 (size 182)
(vfs) task (from exploit): 0xffffffdefb8f5120
(vfs) vm_map: 0xffffffdc00add980 (task+0x28, nentries=176)
(vfs) exploit task looks valid (nentries=176)
(vfs) vm_map: 0xffffffdc00add980 (task+0x28, nentries=176)
(vfs) vm_map entries: 176, looking for 0x10bffc000
(vfs) found entry 0xffffffdce5c07060: 0x10bffc000-0x10c000000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) writing 181 bytes...
(vfs) zeroed 1 bytes of trailing garbage
(vfs) overwrite done!
(vfs) overwrite: /private/var/db/MobileIdentityData/AuthListBannedUpps.plist <- /private/var/mobile/Containers/Data/Application/F6761B0B-79EA-4E49-A8A7-6294A5B20108/tmp/vfs_src_3977461729.bin
(vfs) target mmap'd at 0x10bffc000 (size 181)
(vfs) task (from exploit): 0xffffffdefb8f5120
(vfs) vm_map: 0xffffffdc00add980 (task+0x28, nentries=176)
(vfs) exploit task looks valid (nentries=176)
(vfs) vm_map: 0xffffffdc00add980 (task+0x28, nentries=176)
(vfs) vm_map entries: 176, looking for 0x10bffc000
(vfs) found entry 0xffffffdce548d3f0: 0x10bffc000-0x10c000000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) writing 181 bytes...
(vfs) overwrite done!
(vfs) overwrite: /private/var/db/MobileIdentityData/AuthListBannedCdHashes.plist <- /private/var/mobile/Containers/Data/Application/F6761B0B-79EA-4E49-A8A7-6294A5B20108/tmp/vfs_src_2967987821.bin
(vfs) target mmap'd at 0x10bffc000 (size 181)
(vfs) task (from exploit): 0xffffffdefb8f5120
(vfs) vm_map: 0xffffffdc00add980 (task+0x28, nentries=176)
(vfs) exploit task looks valid (nentries=176)
(vfs) vm_map: 0xffffffdc00add980 (task+0x28, nentries=176)
(vfs) vm_map entries: 176, looking for 0x10bffc000
(vfs) found entry 0xffffffdce4c48cc0: 0x10bffc000-0x10c000000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) writing 181 bytes...
(vfs) overwrite done!


👍 1


---

### Comment #5 — [@rooootdev](https://github.com/rooootdev) · Apr 10, 2026

`ID: 4218322841`


added


---

*Exported on 10/14/2026, 2:49:25 PM from [https://github.com/rooootdev/lara/issues/42](https://github.com/rooootdev/lara/issues/42)*
