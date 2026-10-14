# iPhone 14 18.4.1 - VFS failure

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#21](https://github.com/rooootdev/lara/issues/21)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@rooootdev](https://github.com/rooootdev)  
> **Created:** Apr 2, 2026  
> **Updated:** Apr 2, 2026  
> **Closed:** Apr 2, 2026  

---

## Description

*Opened by [@ficool2](https://github.com/ficool2)* *on Apr 2, 2026*


Exploit works but not Font Overwrite
```
lara started: 2026-04-02 12:38:48


livecontainer detected: nah

getprocsize() returned 0x0. defaulting to 0x740.

PROC_PID_OFFSET: 0x60

PROC_STRUCT_SIZE: 0x740

downloading kernelcache to: /var/mobile/Containers/Data/Application/62BF2D9D-97A0-4A87-B4B4-10C39ED442CC/Documents/kernelcache

libgrabkernel2: Found firmware URL: https://updates.cdn-apple.com/2025SpringFCS/fullrestores/082-30563/AC21E0CD-9427-4F5E-9FE7-9D39765D1304/iPhone14,7_18.4.1_22E252_Restore.ipsw (OTA: no)

libgrabkernel2: Downloading BuildManifest.plist...

libgrabkernel2: Downloading kernelcache.release.iphone14b to /var/mobile/Containers/Data/Application/62BF2D9D-97A0-4A87-B4B4-10C39ED442CC/Documents/kernelcache...

libgrabkernel2: Downloaded kernelcache!

kernel base: 0xfffffff007004000

kernel entry: 0xfffffff00a374000

kernproc symbol: 0xfffffff007a6f2a0

kernprocsize: 0x740

rootvnode symbol: 0xfffffff00a64f5b0

kernproc offset: 0xa6b2a0

rootvnode offset: 0x364b5b0
(ds) starting darksword
(ds) device: iPhone14,7
(ds) running on non-a18 device
(ds) read_fd: 0x13
(ds) write_fd: 0x14
(ds) executable_name: lara
(ds) free_thread_arg: 0x139bfc000
(ds) physical_mapping_address: 0x115c48000
(ds) pc_object: 0xa423
(ds) pc_address: 0x129f28000
(ds) spraying 22528 sockets...
(ds) socket_ports_count: 0x5800
(ds) start_pcb_id: 0x4
(ds) end_pcb_id: 0xb002
(ds) looking in search mapping: 0
(ds) pcb_start_offset: 0x0
(ds) target_inp_gencnt: 0xa5e4
(ds) inp_list_next_pointer: 0xffffffdd41604400
(ds) icmp6filter: 0xffffffe0d7311140
(ds) Corrupting icmp6filter pointer...
(ds) target corrupted: 0xffffffdd41604548
(ds) found control_socket at idx: 0x52f0

kernel proc: 0xffffffdd3e059f18

looking for pid: 357

found proc: lara (pid=357 uid=501 gid=501) @ 0xffffffdd3f93b228
exploit success!
kernel_base:  0xfffffff02df4c000
kernel_slide: 0x26f48000

(ds) highest_success_idx: 18
(ds) success_read_count: 4
(ds) Walking kernel structures...
(ds) control_socket_pcb: 0xffffffdd41604000
(ds) pcbinfo_pointer: 0xfffffff031597060
(ds) ipi_zone: 0xfffffff02e9c0650
(ds) zv_name: 0xfffffff02dfbbef2
(ds) searching for kernel Mach-O header from 0xfffffff02dfb8000...
(ds) candidate Mach-O at 0xfffffff02df54000: filetype=2 cpuinfo=0x2c0000002 (iter=25)
(ds) candidate Mach-O at 0xfffffff02df4c000: filetype=12 cpuinfo=0xc00000002 (iter=27)
(ds) found MH_FILESET header at 0xfffffff02df4c000
(ds) kernel_base:  0xfffffff02df4c000
(ds) kernel_slide: 0x26f48000
(ds) iOS 18: using so_count offset 0x254
(ds) kernel r/w is ready!
(ds) our_proc: 0xffffffdd3f93b228
(ds) our_task: 0xffffffdd3f93b968

exploit success!
kernel_base:  0xfffffff02df4c000
kernel_slide: 0x26f48000

(vfs) vfs_init starting...
(vfs) Extracted heap PAC prefix: 0xffffff0000000000
(vfs) proc=0xffffffdd3f93b228 task=0xffffffdd3f93b968 (from exploit)
(vfs) file overwrite ready!
(ds) setsockopt failed (set_target_kaddr)!
(vfs) rootvnode offset probe: 0x75000001f5 (sym=0xfffffff0315975b0)
(vfs) rootvnode pointer invalid (offset=0x364b5b0)

kernel base: 0xfffffff007004000

kernel entry: 0xfffffff00a374000

kernproc symbol: 0xfffffff007a6f2a0

kernprocsize: 0x740

rootvnode symbol: 0xfffffff00a64f5b0

kernproc offset: 0xa6b2a0

rootvnode offset: 0x364b5b0
(vfs) rootvnode discovery disabled; listdir will be unavailable
(vfs) vfs_init done

vfs ready!


(vfs) target /var/mobile/Containers/Data/Application/62BF2D9D-97A0-4A87-B4B4-10C39ED442CC/Documents/FontRepos/larafonts/Comic Sans MS.ttf -> /System/Library/Fonts/Core/SFUI.ttf

(vfs) vfs_overwritefile returned: -1

(ds) setsockopt failed (set_target_kaddr)! (93x)
(vfs) vm_map not found in task 0xffffffdd3f93b968
(ds) setsockopt failed (set_target_kaddr)!
(vfs) bad proc_ro

(vfs) target /var/mobile/Containers/Data/Application/62BF2D9D-97A0-4A87-B4B4-10C39ED442CC/Documents/FontRepos/larafonts/Comic Sans MS.ttf -> /System/Library/Fonts/Core/SFUI.ttf
failed to change font

(vfs) vfs_overwritefile returned: -1

(ds) setsockopt failed (set_target_kaddr)! (93x)
(vfs) vm_map not found in task 0xffffffdd3f93b968
(ds) setsockopt failed (set_target_kaddr)!
(vfs) bad proc_ro

(vfs) target /var/mobile/Containers/Data/Application/62BF2D9D-97A0-4A87-B4B4-10C39ED442CC/Documents/FontRepos/larafonts/Chococooky.ttf -> /System/Library/Fonts/Core/SFUI.ttf
failed to change font

(vfs) vfs_overwritefile returned: -1

(ds) setsockopt failed (set_target_kaddr)! (93x)
(vfs) vm_map not found in task 0xffffffdd3f93b968
(ds) setsockopt failed (set_target_kaddr)!
(vfs) bad proc_ro

(vfs) target /var/mobile/Containers/Data/Application/62BF2D9D-97A0-4A87-B4B4-10C39ED442CC/Documents/FontRepos/larafonts/Comic Sans MS.ttf -> /System/Library/Fonts/Core/SFUI.ttf

(vfs) vfs_overwritefile returned: -1

(ds) setsockopt failed (set_target_kaddr)! (93x)
(vfs) vm_map not found in task 0xffffffdd3f93b968
(ds) setsockopt failed (set_target_kaddr)!
(vfs) bad proc_ro

(vfs) target /var/mobile/Containers/Data/Application/62BF2D9D-97A0-4A87-B4B4-10C39ED442CC/Documents/FontRepos/larafonts/DejaVuSerif.ttf -> /System/Library/Fonts/Core/SFUI.ttf
failed to change font

(vfs) vfs_overwritefile returned: -1

(ds) setsockopt failed (set_target_kaddr)! (93x)
(vfs) vm_map not found in task 0xffffffdd3f93b968
(ds) setsockopt failed (set_target_kaddr)!
(vfs) bad proc_ro

(vfs) target /var/mobile/Containers/Data/Application/62BF2D9D-97A0-4A87-B4B4-10C39ED442CC/Documents/FontRepos/larafonts/Go-Regular.ttf -> /System/Library/Fonts/Core/SFUI.ttf

(vfs) vfs_overwritefile returned: -1

(ds) setsockopt failed (set_target_kaddr)! (93x)
(vfs) vm_map not found in task 0xffffffdd3f93b968
(ds) setsockopt failed (set_target_kaddr)!
(vfs) bad proc_ro

(vfs) target /var/mobile/Containers/Data/Application/62BF2D9D-97A0-4A87-B4B4-10C39ED442CC/Documents/FontRepos/larafonts/segoeui.ttf -> /System/Library/Fonts/Core/SFUI.ttf
failed to change font

(vfs) vfs_overwritefile returned: -1

(ds) setsockopt failed (set_target_kaddr)! (93x)
(vfs) vm_map not found in task 0xffffffdd3f93b968
(ds) setsockopt failed (set_target_kaddr)!
(vfs) bad proc_ro

(vfs) target /var/mobile/Containers/Data/Application/62BF2D9D-97A0-4A87-B4B4-10C39ED442CC/Documents/FontRepos/larafonts/Comic Sans MS.ttf -> /System/Library/Fonts/Core/SFUI.ttf

(vfs) vfs_overwritefile returned: -1

(ds) setsockopt failed (set_target_kaddr)! (93x)
(vfs) vm_map not found in task 0xffffffdd3f93b968
(ds) setsockopt failed (set_target_kaddr)!
(vfs) bad proc_ro

(vfs) target /var/mobile/Containers/Data/Application/62BF2D9D-97A0-4A87-B4B4-10C39ED442CC/Documents/FontRepos/larafonts/DejaVuSansMono.ttf -> /System/Library/Fonts/Core/SFUI.ttf
failed to change font

(vfs) vfs_overwritefile returned: -1

(ds) setsockopt failed (set_target_kaddr)! (93x)
(vfs) vm_map not found in task 0xffffffdd3f93b968
(ds) setsockopt failed (set_target_kaddr)!
(vfs) bad proc_ro

(vfs) target /var/mobile/Containers/Data/Application/62BF2D9D-97A0-4A87-B4B4-10C39ED442CC/Documents/FontRepos/larafonts/DejaVuSerif.ttf -> /System/Library/Fonts/Core/SFUI.ttf
failed to change font

(vfs) vfs_overwritefile returned: -1

(ds) setsockopt failed (set_target_kaddr)! (93x)
(vfs) vm_map not found in task 0xffffffdd3f93b968
(ds) setsockopt failed (set_target_kaddr)!
(vfs) bad proc_ro

(vfs) target /var/mobile/Containers/Data/Application/62BF2D9D-97A0-4A87-B4B4-10C39ED442CC/Documents/FontRepos/larafonts/Go-Mono.ttf -> /System/Library/Fonts/Core/SFUI.ttf

(vfs) vfs_overwritefile returned: -1

(ds) setsockopt failed (set_target_kaddr)! (93x)
(vfs) vm_map not found in task 0xffffffdd3f93b968
(ds) setsockopt failed (set_target_kaddr)!
(vfs) bad proc_ro

(vfs) target /var/mobile/Containers/Data/Application/62BF2D9D-97A0-4A87-B4B4-10C39ED442CC/Documents/FontRepos/larafonts/Go-Regular.ttf -> /System/Library/Fonts/Core/SFUI.ttf
failed to change font

(vfs) vfs_overwritefile returned: -1

(ds) setsockopt failed (set_target_kaddr)! (93x)
(vfs) vm_map not found in task 0xffffffdd3f93b968
(ds) setsockopt failed (set_target_kaddr)!
(vfs) bad proc_ro

(vfs) target /var/mobile/Containers/Data/Application/62BF2D9D-97A0-4A87-B4B4-10C39ED442CC/Documents/FontRepos/larafonts/QuickSand.ttf -> /System/Library/Fonts/Core/SFUI.ttf
failed to change font

(vfs) vfs_overwritefile returned: -1

(ds) setsockopt failed (set_target_kaddr)! (93x)
(vfs) vm_map not found in task 0xffffffdd3f93b968
(ds) setsockopt failed (set_target_kaddr)!
(vfs) bad proc_ro

(vfs) target /var/mobile/Containers/Data/Application/62BF2D9D-97A0-4A87-B4B4-10C39ED442CC/Documents/FontRepos/larafonts/segoeui.ttf -> /System/Library/Fonts/Core/SFUI.ttf
failed to change font

(vfs) vfs_overwritefile returned: -1

(ds) setsockopt failed (set_target_kaddr)! (93x)
(vfs) vm_map not found in task 0xffffffdd3f93b968
(ds) setsockopt failed (set_target_kaddr)!
(vfs) bad proc_ro

(vfs) target /var/mobile/Containers/Data/Application/62BF2D9D-97A0-4A87-B4B4-10C39ED442CC/Documents/FontRepos/larafonts/SFUI.ttf -> /System/Library/Fonts/Core/SFUI.ttf
failed to change font

(vfs) vfs_overwritefile returned: -1

(ds) setsockopt failed (set_target_kaddr)! (93x)
(vfs) vm_map not found in task 0xffffffdd3f93b968
(ds) setsockopt failed (set_target_kaddr)!
(vfs) bad proc_ro

(vfs) target /var/mobile/Containers/Data/Application/62BF2D9D-97A0-4A87-B4B4-10C39ED442CC/Documents/FontRepos/larafonts/Comic Sans MS.ttf -> /System/Library/Fonts/Core/SFUI.ttf
failed to change font

(vfs) vfs_overwritefile returned: -1

(ds) setsockopt failed (set_target_kaddr)! (93x)
(vfs) vm_map not found in task 0xffffffdd3f93b968
(ds) setsockopt failed (set_target_kaddr)!
(vfs) bad proc_ro

(vfs) target /var/mobile/Containers/Data/Application/62BF2D9D-97A0-4A87-B4B4-10C39ED442CC/Documents/FontRepos/larafonts/Comic Sans MS.ttf -> /System/Library/Fonts/Core/SFUI.ttf

(vfs) vfs_overwritefile returned: -1

(ds) setsockopt failed (set_target_kaddr)! (93x)
(vfs) vm_map not found in task 0xffffffdd3f93b968
(ds) setsockopt failed (set_target_kaddr)!
(vfs) bad proc_ro
```


---

## Timeline (Verbose)

- Apr 2, 2026: ✏️ @ficool2 renamed: "iPhone 18.4.1 - VFS failure" → "iPhone 14 18.4.1 - VFS failure"
- Apr 2, 2026: 🔴 Closed by @rooootdev


---

## Comments (4)

### Comment #1 — [@Pro42good](https://github.com/Pro42good) · Apr 2, 2026

`ID: 4177265505`


You need to close and re-open the Lara app from my experience, If that also doesn't work then try redownloading the kernel cache that you initially downloaded. And once it works you need to respring springboard anyways.


---

### Comment #2 — [@ficool2](https://github.com/ficool2) · Apr 2, 2026

`ID: 4177482836`


Re-opening the app and running the exploit again worked, although I don't see a way to force a respring


---

### Comment #3 — [@Pro42good](https://github.com/Pro42good) · Apr 2, 2026

`ID: 4177845830`


I have you covered, I got this from a conversation from the developer yesterday as I was having a similar issue.

https://github.com/34306/mdc0/releases/download/1.0/respringapp.ipa


---

### Comment #4 — [@ficool2](https://github.com/ficool2) · Apr 2, 2026

`ID: 4178139732`


Nice, thanks


👍 1


---

*Exported on 10/14/2026, 2:47:51 PM from [https://github.com/rooootdev/lara/issues/21](https://github.com/rooootdev/lara/issues/21)*
