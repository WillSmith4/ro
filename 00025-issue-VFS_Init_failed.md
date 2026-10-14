# VFS Init failed

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#25](https://github.com/rooootdev/lara/issues/25)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@rooootdev](https://github.com/rooootdev)  
> **Created:** Apr 4, 2026  
> **Updated:** Apr 4, 2026  
> **Closed:** Apr 4, 2026  

---

## Description

*Opened by [@Zer0xDev](https://github.com/Zer0xDev)* *on Apr 4, 2026*


VFS Init failed. iPhone 12 Pro Max iOS 18.1. I'm using the latest version of lara that came out literally 15 minutes ago. On the older version everything works perfectly. I've tried reinstalling the app, but it doesn't help.

Logs:
-------

lara started: 2026-04-04 20:24:26


livecontainer detected: nah

getprocsize() returned 0x0. defaulting to 0x740.

PROC_PID_OFFSET: 0x28

PROC_STRUCT_SIZE: 0x740

downloading kernelcache → /var/mobile/Containers/Data/Application/AEDD005F-23B3-4101-A142-04958A31FA3A/Documents/kernelcache

libgrabkernel2: Found firmware URL: https://updates.cdn-apple.com/2024FallFCS/fullrestores/062-24453/8D9BFD36-BCD8-4E50-B794-FDF7D979FB0A/iPhone13,4_18.1_22B82_Restore.ipsw (OTA: no)

libgrabkernel2: Downloading BuildManifest.plist...

libgrabkernel2: Downloading kernelcache.release.iphone13 to /var/mobile/Containers/Data/Application/AEDD005F-23B3-4101-A142-04958A31FA3A/Documents/kernelcache...

libgrabkernel2: Downloaded kernelcache!

kernel: Darwin Kernel Version 24.1.0: Mon Oct  7 21:11:47 PDT 2024; root:xnu-11215.42.1~1/RELEASE_ARM64_T8101

kernbase: 0xfffffff007004000

kernentry: 0xfffffff007e48548

kernproc: 0xa30d18

rootvnode: 0x34e0e80

procsize: 0x740

darksword not ready

(aslr) failed. could not find launchd proc

darksword not ready

(aslr) failed. could not find launchd proc

darksword not ready

(aslr) failed. could not find launchd proc
(ds) starting darksword
(ds) device: iPhone13,4
(ds) running on non-a18 device
(ds) read_fd: 0x12
(ds) write_fd: 0x13
(ds) executable_name: lara
(ds) free_thread_arg: 0x10c2d8000
(ds) physical_mapping_address: 0x1037f8000
(ds) pc_object: 0x8a07
(ds) pc_address: 0x17b864000

darksword not ready

(aslr) failed. could not find launchd proc
(ds) spraying 22528 sockets...

darksword not ready

(aslr) failed. could not find launchd proc
(ds) socket_ports_count: 0x5800
(ds) start_pcb_id: 0x57ff8
(ds) end_pcb_id: 0x62ff6
(ds) looking in search mapping: 0

darksword not ready

(aslr) failed. could not find launchd proc
(ds) pcb_start_offset: 0x0
(ds) target_inp_gencnt: 0x5d780

darksword not ready

(aslr) failed. could not find launchd proc
(ds) inp_list_next_pointer: 0xffffffe1ee590400
(ds) icmp6filter: 0xffffffe6b7455b00
(ds) Corrupting icmp6filter pointer...

darksword not ready

(aslr) failed. could not find launchd proc
(ds) target corrupted: 0xffffffe1ee590548
(ds) found control_socket at idx: 0x2bc4

darksword not ready

(aslr) failed. could not find launchd proc

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(aslr) failed. could not find launchd proc
exploit success!
kernel_base:  0xfffffff0285e0000
kernel_slide: 0x215dc000

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(aslr) failed. could not find launchd proc

(ds) highest_success_idx: 500
(ds) success_read_count: 278
(ds) Walking kernel structures...
(ds) control_socket_pcb: 0xffffffe1ee590000
(ds) pcbinfo_pointer: 0xfffffff02bac0940
(ds) ipi_zone: 0xfffffff029019dc0
(ds) zv_name: 0xfffffff028654638
(ds) searching for kernel Mach-O header from 0xfffffff028654000...
(ds) candidate Mach-O at 0xfffffff0285e8000: filetype=2 cpuinfo=0x2c0000002 (iter=27)
(ds) candidate Mach-O at 0xfffffff0285e0000: filetype=12 cpuinfo=0xc00000002 (iter=29)
(ds) found MH_FILESET header at 0xfffffff0285e0000
(ds) kernel_base:  0xfffffff0285e0000
(ds) kernel_slide: 0x215dc000
(ds) iOS 18: using so_count offset 0x254
(ds) kernel r/w is ready!
(ds) our_proc: 0x0
(ds) our_task: 0x0

exploit success!
kernel_base:  0xfffffff0285e0000
kernel_slide: 0x215dc000


kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(aslr) failed. could not find launchd proc

kernel proc pointer invalid

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(aslr) failed. could not find launchd proc
(vfs) vfs_init starting...
(vfs) exploit didn't find proc/task, trying vfs scan...
(vfs) ds_get_our_proc() failed
(vfs) procbyname("launchd") failed
(vfs) proc not found - file overwrite won't work
(vfs) vfs_init done

vfs init failed.


kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(aslr) failed. could not find launchd proc

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(aslr) failed. could not find launchd proc

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(ds) setsockopt failed (set_target_kaddr)! (21x)

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(ds) setsockopt failed (set_target_kaddr)! (22x)

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(ds) setsockopt failed (set_target_kaddr)! (25x)

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(ds) setsockopt failed (set_target_kaddr)! (26x)

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(aslr) failed. could not find launchd proc

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(ds) setsockopt failed (set_target_kaddr)! (28x)

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(aslr) failed. could not find launchd proc

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(ds) setsockopt failed (set_target_kaddr)! (30x)

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(aslr) failed. could not find launchd proc

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(ds) setsockopt failed (set_target_kaddr)! (32x)

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(aslr) failed. could not find launchd proc

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(ds) setsockopt failed (set_target_kaddr)! (34x)

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(aslr) failed. could not find launchd proc

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(ds) setsockopt failed (set_target_kaddr)! (36x)

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(aslr) failed. could not find launchd proc

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(ds) setsockopt failed (set_target_kaddr)! (38x)

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(aslr) failed. could not find launchd proc

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(ds) setsockopt failed (set_target_kaddr)! (40x)

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(aslr) failed. could not find launchd proc

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(ds) setsockopt failed (set_target_kaddr)! (42x)

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(aslr) failed. could not find launchd proc

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(ds) setsockopt failed (set_target_kaddr)! (44x)

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(aslr) failed. could not find launchd proc

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(ds) setsockopt failed (set_target_kaddr)! (46x)

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(aslr) failed. could not find launchd proc

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(ds) setsockopt failed (set_target_kaddr)! (48x)

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(aslr) failed. could not find launchd proc

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(ds) setsockopt failed (set_target_kaddr)! (50x)

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(aslr) failed. could not find launchd proc

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(ds) setsockopt failed (set_target_kaddr)! (52x)

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(aslr) failed. could not find launchd proc

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(ds) setsockopt failed (set_target_kaddr)! (54x)

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(aslr) failed. could not find launchd proc

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(ds) setsockopt failed (set_target_kaddr)! (56x)

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(aslr) failed. could not find launchd proc

kernel proc: 0x28df1af029a9000c

kernel proc pointer invalid

(aslr) failed. could not find launchd proc


---

## Timeline (Verbose)

- Apr 4, 2026: 🔴 Closed by @rooootdev


---

## Comments (2)

### Comment #1 — [@lentas1](https://github.com/lentas1) · Apr 4, 2026

`ID: 4187462758`


also getting vfs errors


---

### Comment #2 — [@rooootdev](https://github.com/rooootdev) · Apr 4, 2026

`ID: 4187514991`


should be fixed in latest. thanks @jurre111


---

*Exported on 10/14/2026, 2:48:11 PM from [https://github.com/rooootdev/lara/issues/25](https://github.com/rooootdev/lara/issues/25)*
