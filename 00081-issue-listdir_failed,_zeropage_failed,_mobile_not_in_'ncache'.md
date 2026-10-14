# listdir failed, zeropage failed, mobile not in 'ncache'

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#81](https://github.com/rooootdev/lara/issues/81)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@rooootdev](https://github.com/rooootdev)  
> **Labels:** `bug`  
> **Created:** Apr 11, 2026  
> **Updated:** Apr 17, 2026  
> **Closed:** Apr 17, 2026  

---

## Description

*Opened by [@ghost](https://github.com/ghost)* *on Apr 11, 2026*


Device: iPhone 15 Pro (iOS 17.1.2) , tested:
Font overwrite* works.

Card overwrite* works 
(only for first card due to  listdir failed (/private/var/mobile/Library/Passes/Cards/o-ZmYdzb8oe4ffr2UJfz2w7vl9k=.cache/FrontFace) r=-1
(vfs) 'mobile' not in ncache  , maybe due to the folder containing non-unicode chars? )
Custom overwrite NOT tested.
MobileGestAlt* works
Whitelist does not work on my end.
DirtyZero works, (minus the duplicated feats)
3 App Bypass works* (takes 2-3 tries but 85%+ succession)

Used sbx + vfs for all feats. great work root, you brought a community back to life, and blossomed a garden.

lara started: 2026-04-11 10:55:31
livecontainer detected: nah
(utils) TASK_TNEXT_OFFSET: 0x58
(utils) THREAD_MUPCB_OFFSET: 0x100
(utils) PROC_PID_OFFSET: 0x28
(utils) PROC_STRUCT_SIZE: 0x730
(ds) starting darksword
(ds) device: iPhone16,1
(ds) ispac: yes
(ds) running on non-a18 device
(ds) read_fd: 0x12
(ds) write_fd: 0x13
(ds) executable_name: lara
(ds) free_thread_arg: 0x10f798000
(ds) physical_mapping_address: 0x10f7ac000
(ds) pc_object: 0xa10b
(ds) pc_address: 0x318b04000
(ds) spraying 22528 sockets...
(ds) socket_ports_count: 0x5800
(ds) start_pcb_id: 0x35a4ec
(ds) end_pcb_id: 0x3654ea
(ds) looking in search mapping: 0
(ds) pcb_start_offset: 0x0
(ds) target_inp_gencnt: 0x35ad70
(ds) inp_list_next_pointer: 0xffffffde16de0400
(ds) icmp6filter: 0xffffffe1ff071420
(ds) Corrupting icmp6filter pointer...
(ds) target corrupted: 0xffffffde16de0548
(ds) found control_socket at idx: 0x442
initialized offsetsinitialized offsets(utils) kernel proc: 0xffffffe0cd81fcd0
(utils) looking for pid: 8286
(utils) found proc:  (pid=8286 uid=501 gid=501) @ 0xffffffe0cc46d7b0
exploit success!
kernel_base:  0xfffffff0516e0000
kernel_slide: 0x4a6dc000
(ds) highest_success_idx: 213
(ds) success_read_count: 24
(ds) Walking kernel structures...
(ds) control_socket_pcb: 0xffffffde16de0000
(ds) pcbinfo_pointer: 0xfffffff054cd0948
(ds) ipi_zone: 0xfffffff0520652f0
(ds) zv_name: 0xfffffff05174c65d
(ds) searching for kernel Mach-O header from 0xfffffff05174c000...
(ds) candidate Mach-O at 0xfffffff0516e8000: filetype=2 cpuinfo=0x2c0000002 (iter=25)
(ds) candidate Mach-O at 0xfffffff0516e0000: filetype=12 cpuinfo=0xc00000002 (iter=27)
(ds) found MH_FILESET header at 0xfffffff0516e0000
(ds) kernel_base:  0xfffffff0516e0000
(ds) kernel_slide: 0x4a6dc000
(ds) iOS 17: using so_count offset 0x24c
(ds) kernel r/w is ready!
(ds) our_proc: 0xffffffe0cc46d7b0
(ds) our_task: 0xffffffe0cc46dee0
exploit success!
kernel_base:  0xfffffff0516e0000
kernel_slide: 0x4a6dc000
(sbx) proc=0xffffffe0cc46d7b0 proc_ro_raw=0xffffffdd35fbb900 proc_ro=0xffffffdd35fbb900
(sbx) scanning proc_ro for ucred...
(sbx)   proc_ro+0x10: raw=0x205e smr=0x205e pac=0x205e
(sbx)   proc_ro+0x18: raw=0x220023050000595a smr=0x23050000595a pac=0x23050000595a
(sbx)   proc_ro+0x20: raw=0xffffffdce801ca90 smr=0xffffffdce801ca90 pac=0xffffffdce801ca90
(sbx) found ucred at proc_ro+0x20 (SMR) = 0xffffffdce801ca90
(sbx) ucred=0xffffffdce801ca90 label=0xffffffdd35eaf0e0 sandbox=0xffffffdce81af608 ext_set=0xffffffe0cd37f880
(sbx) patched 2 extensions
(sbx) changed 2 extension classes
(sbx) filled 14 empty hash slots
(sbx) escaped!
sandbox escape ready!
(vfs) vfs_init starting...
(vfs) Extracted heap PAC prefix: 0xffffff0000000000
(vfs) proc=0xffffffe0cc46d7b0 task=0xffffffe0cc46dee0 (from exploit)
(vfs) file overwrite ready!
(vfs) rootvnode offset missing; trying kernelcache resolve
(vfs) rootvnode offset probe: 0xffffffde1b79bc00 (sym=0xfffffff054cd0e80)
(vfs) rootvnode via offset: 0xffffffde1b79bc00
(vfs) g_rootvnode = 0xffffffde1b79bc00
(vfs) first_nc = 0xffffffdfe5e7ec18
(vfs) nc_vp = 0xffffffde1b79bc00
(vfs) nc_name = 0x7f379b6e
(vfs) nc_name offset probe: 0x60
(vfs) ncache ok: first child='.tmp.KRjpT17U7p'
(vfs) vfs_init done
vfs ready!
 listdir failed (/var/mobile/Library/Passes/Cards/sww92DxgRc4l4dl-EHM2FBiMbk8=.cache/FrontFace) r=-1
 listdir failed (/private/var/mobile/Library/Passes/Cards/sww92DxgRc4l4dl-EHM2FBiMbk8=.cache/FrontFace) r=-1
 listdir failed (/var/mobile/Library/Passes/Cards/fcS90qre5c-OJ2GFkMEyyXpXGfQ=.cache/FrontFace) r=-1
 listdir failed (/private/var/mobile/Library/Passes/Cards/fcS90qre5c-OJ2GFkMEyyXpXGfQ=.cache/FrontFace) r=-1
 listdir failed (/var/mobile/Library/Passes/Cards/o-ZmYdzb8oe4ffr2UJfz2w7vl9k=.cache/FrontFace) r=-1
 listdir failed (/private/var/mobile/Library/Passes/Cards/o-ZmYdzb8oe4ffr2UJfz2w7vl9k=.cache/FrontFace) r=-1
(vfs) 'mobile' not in ncache
(vfs) vfs_listdir resolvepath failed path=/var/mobile/Library/Passes/Cards/sww92DxgRc4l4dl-EHM2FBiMbk8=.cache/FrontFace dvn=0x0
(vfs) 'mobile' not in ncache
(vfs) vfs_listdir resolvepath failed path=/private/var/mobile/Library/Passes/Cards/sww92DxgRc4l4dl-EHM2FBiMbk8=.cache/FrontFace dvn=0x0
(vfs) 'mobile' not in ncache
(vfs) vfs_listdir resolvepath failed path=/var/mobile/Library/Passes/Cards/fcS90qre5c-OJ2GFkMEyyXpXGfQ=.cache/FrontFace dvn=0x0
(vfs) 'mobile' not in ncache
(vfs) vfs_listdir resolvepath failed path=/private/var/mobile/Library/Passes/Cards/fcS90qre5c-OJ2GFkMEyyXpXGfQ=.cache/FrontFace dvn=0x0
(vfs) 'mobile' not in ncache
(vfs) vfs_listdir resolvepath failed path=/var/mobile/Library/Passes/Cards/o-ZmYdzb8oe4ffr2UJfz2w7vl9k=.cache/FrontFace dvn=0x0
(vfs) 'mobile' not in ncache
(vfs) vfs_listdir resolvepath failed path=/private/var/mobile/Library/Passes/Cards/o-ZmYdzb8oe4ffr2UJfz2w7vl9k=.cache/FrontFace dvn=0x0
(vfs) zeroed first page of /System/Library/PrivateFrameworks/CoreMaterial.framework/dockDark.materialrecipe
(vfs) zeroed first page of /System/Library/PrivateFrameworks/CoreMaterial.framework/dockLight.materialrecipe
(vfs) zeroed first page of /System/Library/PrivateFrameworks/SpringBoardHome.framework/folderDark.materialrecipe
(vfs) zeroed first page of /System/Library/PrivateFrameworks/SpringBoardHome.framework/folderLight.materialrecipe
(vfs) zeroed first page of /System/Library/PrivateFrameworks/SpringBoardHome.framework/stackConfigurationBackground.materialrecipe
(vfs) zeroed first page of /System/Library/PrivateFrameworks/SpringBoardHome.framework/stackConfigurationForeground.materialrecipe
(vfs) zeropage failed
(vfs) zeroed first page of /System/Library/PrivateFrameworks/SpringBoardHome.framework/homeScreenOverlay.materialrecipe
(vfs) zeroed first page of /System/Library/PrivateFrameworks/CoreMaterial.framework/platterStrokeLight.visualstyleset
(vfs) zeroed first page of /System/Library/PrivateFrameworks/CoreMaterial.framework/platterStrokeDark.visualstyleset
(vfs) zeroed first page of /System/Library/PrivateFrameworks/CoreMaterial.framework/plattersDark.materialrecipe
(vfs) zeroed first page of /System/Library/PrivateFrameworks/CoreMaterial.framework/platters.materialrecipe
(vfs) zeroed first page of /System/Library/PrivateFrameworks/UserNotificationsUIKit.framework/stackDimmingLight.visualstyleset
(vfs) zeroed first page of /System/Library/PrivateFrameworks/UserNotificationsUIKit.framework/stackDimmingDark.visualstyleset
(vfs) zeroed first page of /System/Library/PrivateFrameworks/SpringBoard.framework/homeScreenBackdrop-application.materialrecipe
(vfs) zeroed first page of /System/Library/PrivateFrameworks/SpringBoard.framework/homeScreenBackdrop-switcher.materialrecipe
(vfs) zeroed first page of /System/Library/ControlCenter/Bundles/FocusUIModule.bundle/Info.plist
(vfs) task (from exploit): 0xffffffe0cc46dee0
(vfs) vm_map: 0xffffffdc003837e0 (task+0x28, nentries=562)
(vfs) exploit task looks valid (nentries=562)
(vfs) vm_map: 0xffffffdc003837e0 (task+0x28, nentries=562)
(vfs) vm_map entries: 562, looking for 0x116458000
(vfs) found entry 0xffffffdce63fd030: 0x116458000-0x11645c000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) zeroed mapped page (shared) containing offset 0x0 (page 0x0)
(vfs) task (from exploit): 0xffffffe0cc46dee0
(vfs) vm_map: 0xffffffdc003837e0 (task+0x28, nentries=562)
(vfs) exploit task looks valid (nentries=562)
(vfs) vm_map: 0xffffffdc003837e0 (task+0x28, nentries=562)
(vfs) vm_map entries: 562, looking for 0x116458000
(vfs) found entry 0xffffffdce6092a70: 0x116458000-0x11645c000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) zeroed mapped page (shared) containing offset 0x0 (page 0x0)
(vfs) task (from exploit): 0xffffffe0cc46dee0
(vfs) vm_map: 0xffffffdc003837e0 (task+0x28, nentries=562)
(vfs) exploit task looks valid (nentries=562)
(vfs) vm_map: 0xffffffdc003837e0 (task+0x28, nentries=562)
(vfs) vm_map entries: 562, looking for 0x116458000
(vfs) found entry 0xffffffdce6155030: 0x116458000-0x11645c000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) zeroed mapped page (shared) containing offset 0x0 (page 0x0)
(vfs) task (from exploit): 0xffffffe0cc46dee0
(vfs) vm_map: 0xffffffdc003837e0 (task+0x28, nentries=562)
(vfs) exploit task looks valid (nentries=562)
(vfs) vm_map: 0xffffffdc003837e0 (task+0x28, nentries=562)
(vfs) vm_map entries: 562, looking for 0x116458000
(vfs) found entry 0xffffffdce60282c0: 0x116458000-0x11645c000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) zeroed mapped page (shared) containing offset 0x0 (page 0x0)
(vfs) task (from exploit): 0xffffffe0cc46dee0
(vfs) vm_map: 0xffffffdc003837e0 (task+0x28, nentries=562)
(vfs) exploit task looks valid (nentries=562)
(vfs) vm_map: 0xffffffdc003837e0 (task+0x28, nentries=562)
(vfs) vm_map entries: 562, looking for 0x116458000
(vfs) found entry 0xffffffdce5e348b0: 0x116458000-0x11645c000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) zeroed mapped page (shared) containing offset 0x0 (page 0x0)
(vfs) task (from exploit): 0xffffffe0cc46dee0
(vfs) vm_map: 0xffffffdc003837e0 (task+0x28, nentries=562)
(vfs) exploit task looks valid (nentries=562)
(vfs) vm_map: 0xffffffdc003837e0 (task+0x28, nentries=562)
(vfs) vm_map entries: 562, looking for 0x116458000
(vfs) found entry 0xffffffdce603d760: 0x116458000-0x11645c000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) zeroed mapped page (shared) containing offset 0x0 (page 0x0)
(vfs) cant open: No such file or directory
(vfs) task (from exploit): 0xffffffe0cc46dee0
(vfs) vm_map: 0xffffffdc003837e0 (task+0x28, nentries=562)
(vfs) exploit task looks valid (nentries=562)
(vfs) vm_map: 0xffffffdc003837e0 (task+0x28, nentries=562)
(vfs) vm_map entries: 562, looking for 0x116458000
(vfs) found entry 0xffffffdce63ce980: 0x116458000-0x11645c000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) zeroed mapped page (shared) containing offset 0x0 (page 0x0)
(vfs) task (from exploit): 0xffffffe0cc46dee0
(vfs) vm_map: 0xffffffdc003837e0 (task+0x28, nentries=562)
(vfs) exploit task looks valid (nentries=562)
(vfs) vm_map: 0xffffffdc003837e0 (task+0x28, nentries=562)
(vfs) vm_map entries: 562, looking for 0x116458000
(vfs) found entry 0xffffffdce60a4a40: 0x116458000-0x11645c000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) zeroed mapped page (shared) containing offset 0x0 (page 0x0)
(vfs) task (from exploit): 0xffffffe0cc46dee0
(vfs) vm_map: 0xffffffdc003837e0 (task+0x28, nentries=562)
(vfs) exploit task looks valid (nentries=562)
(vfs) vm_map: 0xffffffdc003837e0 (task+0x28, nentries=562)
(vfs) vm_map entries: 562, looking for 0x116458000
(vfs) found entry 0xffffffdce5d2b650: 0x116458000-0x11645c000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) zeroed mapped page (shared) containing offset 0x0 (page 0x0)
(vfs) task (from exploit): 0xffffffe0cc46dee0
(vfs) vm_map: 0xffffffdc003837e0 (task+0x28, nentries=562)
(vfs) exploit task looks valid (nentries=562)
(vfs) vm_map: 0xffffffdc003837e0 (task+0x28, nentries=562)
(vfs) vm_map entries: 562, looking for 0x116458000
(vfs) found entry 0xffffffdce60a4a40: 0x116458000-0x11645c000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) zeroed mapped page (shared) containing offset 0x0 (page 0x0)
(vfs) task (from exploit): 0xffffffe0cc46dee0
(vfs) vm_map: 0xffffffdc003837e0 (task+0x28, nentries=562)
(vfs) exploit task looks valid (nentries=562)
(vfs) vm_map: 0xffffffdc003837e0 (task+0x28, nentries=562)
(vfs) vm_map entries: 562, looking for 0x116458000
(vfs) found entry 0xffffffdce603d760: 0x116458000-0x11645c000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) zeroed mapped page (shared) containing offset 0x0 (page 0x0)
(vfs) task (from exploit): 0xffffffe0cc46dee0
(vfs) vm_map: 0xffffffdc003837e0 (task+0x28, nentries=562)
(vfs) exploit task looks valid (nentries=562)
(vfs) vm_map: 0xffffffdc003837e0 (task+0x28, nentries=562)
(vfs) vm_map entries: 562, looking for 0x116458000
(vfs) found entry 0xffffffdce62cf7e0: 0x116458000-0x11645c000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) zeroed mapped page (shared) containing offset 0x0 (page 0x0)
(vfs) task (from exploit): 0xffffffe0cc46dee0
(vfs) vm_map: 0xffffffdc003837e0 (task+0x28, nentries=562)
(vfs) exploit task looks valid (nentries=562)
(vfs) vm_map: 0xffffffdc003837e0 (task+0x28, nentries=562)
(vfs) vm_map entries: 562, looking for 0x116458000
(vfs) found entry 0xffffffdce5b5c770: 0x116458000-0x11645c000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) zeroed mapped page (shared) containing offset 0x0 (page 0x0)
(vfs) task (from exploit): 0xffffffe0cc46dee0
(vfs) vm_map: 0xffffffdc003837e0 (task+0x28, nentries=562)
(vfs) exploit task looks valid (nentries=562)
(vfs) vm_map: 0xffffffdc003837e0 (task+0x28, nentries=562)
(vfs) vm_map entries: 562, looking for 0x116458000
(vfs) found entry 0xffffffdce5f0a840: 0x116458000-0x11645c000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) zeroed mapped page (shared) containing offset 0x0 (page 0x0)
(vfs) task (from exploit): 0xffffffe0cc46dee0
(vfs) vm_map: 0xffffffdc003837e0 (task+0x28, nentries=562)
(vfs) exploit task looks valid (nentries=562)
(vfs) vm_map: 0xffffffdc003837e0 (task+0x28, nentries=562)
(vfs) vm_map entries: 562, looking for 0x116458000
(vfs) found entry 0xffffffdce5d2b650: 0x116458000-0x11645c000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) zeroed mapped page (shared) containing offset 0x0 (page 0x0)
(vfs) task (from exploit): 0xffffffe0cc46dee0
(vfs) vm_map: 0xffffffdc003837e0 (task+0x28, nentries=562)
(vfs) exploit task looks valid (nentries=562)
(vfs) vm_map: 0xffffffdc003837e0 (task+0x28, nentries=562)
(vfs) vm_map entries: 562, looking for 0x116458000
(vfs) found entry 0xffffffdce60a4a40: 0x116458000-0x11645c000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) zeroed mapped page (shared) containing offset 0x0 (page 0x0)
changed owner of /private/var/containers/Bundle/Application/5936ECBB-6BE3-49B8-824F-B43475AE03BC/lara.app to 501:501!
changed owner of /private/var/containers/Bundle/Application/2A7D2CC3-766E-4048-A3CA-D01C6AD5001E/Filza.app to 501:501!
changed owner of /private/var/containers/Bundle/Application/96076234-233A-48A9-96C0-9DA5501AD406/PancakeStore.app to 501:501!
(sbx) set ownership on: /private/var/containers/Bundle/Application/5936ECBB-6BE3-49B8-824F-B43475AE03BC/lara.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/5936ECBB-6BE3-49B8-824F-B43475AE03BC/lara.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/5936ECBB-6BE3-49B8-824F-B43475AE03BC/lara.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/2A7D2CC3-766E-4048-A3CA-D01C6AD5001E/Filza.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/2A7D2CC3-766E-4048-A3CA-D01C6AD5001E/Filza.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/2A7D2CC3-766E-4048-A3CA-D01C6AD5001E/Filza.app size=3
(sbx) set ownership on: /private/var/containers/Bundle/Application/96076234-233A-48A9-96C0-9DA5501AD406/PancakeStore.app
(sbx) set xattr on: /private/var/containers/Bundle/Application/96076234-233A-48A9-96C0-9DA5501AD406/PancakeStore.app
(sbx) verified xattr on: /private/var/containers/Bundle/Application/96076234-233A-48A9-96C0-9DA5501AD406/PancakeStore.app size=3
(sbx) processed 3 app(s)
(utils) kernel proc: 0xffffffe0cd81fcd0
(utils) looking for pid: 1
(utils) found proc: chd (pid=1 uid=0 gid=0) @ 0xffffffe0cd81d480
(utils) (aslr) refreshed. aslr is off
(utils) kernel proc: 0xffffffe0cd81fcd0
(utils) looking for pid: 1
(utils) found proc: chd (pid=1 uid=0 gid=0) @ 0xffffffe0cd81d480
(utils) (aslr) refreshed. aslr is off
(utils) kernel proc: 0xffffffe0cd81fcd0
(utils) looking for pid: 1
(utils) found proc: chd (pid=1 uid=0 gid=0) @ 0xffffffe0cd81d480
(utils) (aslr) aslr is now on
(utils) kernel proc: 0xffffffe0cd81fcd0
(utils) looking for pid: 1
(utils) found proc: chd (pid=1 uid=0 gid=0) @ 0xffffffe0cd81d480
(utils) (aslr) refreshed. aslr is on
(utils) kernel proc: 0xffffffe0cd81fcd0
(utils) looking for pid: 1
(utils) found proc: chd (pid=1 uid=0 gid=0) @ 0xffffffe0cd81d480
(utils) (aslr) aslr is now off
(utils) kernel proc: 0xffffffe0cd81fcd0
(utils) looking for pid: 1
(utils) found proc: chd (pid=1 uid=0 gid=0) @ 0xffffffe0cd81d480
(utils) (aslr) refreshed. aslr is off
(utils) kernel proc: 0xffffffe0cd81fcd0
(utils) looking for pid: 1
(utils) found proc: chd (pid=1 uid=0 gid=0) @ 0xffffffe0cd81d480
(utils) (aslr) refreshed. aslr is off


---

## Timeline (Verbose)

- Apr 11, 2026: ✏️ someone renamed: "listdir failed, zeropage failed, mobile not in 'nache'" → "listdir failed, zeropage failed, mobile not in 'ncache'"
- Apr 17, 2026: 🏷️ @Pro42good added label `bug`
- Apr 17, 2026: 🔴 Closed by @rooootdev


---

## Comments (1)

### Comment #1 — [@rooootdev](https://github.com/rooootdev) · Apr 17, 2026

`ID: 4267644604`


I dont see an issue here... reopen if the issue still occurs.


---

*Exported on 10/14/2026, 2:52:43 PM from [https://github.com/rooootdev/lara/issues/81](https://github.com/rooootdev/lara/issues/81)*
