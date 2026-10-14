# [BUG] SpringBoard Tools: Dock / Notification Banner Shadow color and disable options not working

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#255](https://github.com/rooootdev/lara/issues/255)  
> **State:** 🟢 open  
> **Labels:** `bug`  
> **Created:** May 20, 2026  

---

## Description

*Opened by [@yamazakiteikoku](https://github.com/yamazakiteikoku)* *on May 20, 2026*


### Describe the bug:
<!-- A clear and concise description of what the bug is. -->
``SpringBoard Tools: Dock / Notification Banner Shadow color and disable options not working``

### To Reproduce:
Steps to reproduce the behavior:
1.	Open SpringBoard Tools in Lara
2.	Go to Dock / Notification Banner Shadow settings
3.	Disable them or change color values
4.	Tap “Save”
5.	Restart SpringBoard or do not restart it
6.	Changes are not applied


### Expected behavior:
<!-- What you expected to happen. -->
``Settings (Dock color, Notification Banner Shadow color, and disable options) should be applied after saving and respringing SpringBoard.``

### Screenshots:
<!-- If applicable, add screenshots to help explain your problem. -->

### Device Info:
- **Device:** ``iPhone 11 Pro Max`` <!-- e.g. iPhone 13 Pro, iPad Air 5 -->
- **Chip:** ``A13`` <!-- e.g. A15, A14, M1 -->
- **iOS Version:** ``iOS18.6.1`` <!-- e.g. 16.5 (20F66) — include the build number -->
- **Jailbroken before?** ``Yes`` <!-- Yes / No / First attempt on this device -->
- **Lara version / commit:** ``All Version`` <!-- e.g. v0.3 or commit hash -->

### Logs:
<!-- Paste crash logs, console output, or kernel panic text. Use ``` fences. -->
```
lara started: 2026-05-20 07:57:08

(utils) darksword not ready

(rc) Unable to find process: youtube

(utils) T1SZ_BOOT: 0x19

(utils) TASK_TNEXT_OFFSET: 0x50

(utils) THREAD_MUPCB_OFFSET: 0xb8

(utils) PROC_PID_OFFSET: 0x60

(offs) (user-set) t1sz_boot = 0x19 (default: 0x0)

(offs) loaded user-saved offsets above

(offs) initialized offsets

(offs) kernel: Darwin Kernel Version 24.6.0: Tue Jul 15 21:53:57 PDT 2025; root:xnu-11417.140.69~16/RELEASE_ARM64_T8030

(offs) kernbase: 0xfffffff007004000

(offs) kernentry: 0xfffffff007ef04f0

(offs) allproc: 0x3653cb8

(offs) kernproc: 0xada3a8

(offs) rootvnode: 0x3654300

(offs) procsize: 0x740

(offs) t1szboot: 0x19

(offs) initialized offsets

(ds) starting darksword
(ds) device: iPhone12,5
(ds) ispac: yes
(ds) running on non-a18 device
(ds) read_fd: 0xb
(ds) write_fd: 0xc
(ds) executable_path:       /private/var/containers/Bundle/Application/C7E85257-B41F-4DF8-964C-49BE06C2097D/lara.app/lara
(ds) host_executable_path:  /private/var/containers/Bundle/Application/C7E85257-B41F-4DF8-964C-49BE06C2097D/lara.app/lara
(ds) guest_executable_name: lara
(ds) host_executable_name:  lara
(ds) kernel_process_name:   lara
(ds) livecontainer_bundle:  no
(ds) livecontainer_guest:   no
(ds) rehosted_process:      no
(ds) process_marker 0: lara
(ds) executable_name: lara
(ds) free_thread_arg: 0x105c3c000
(ds) physical_mapping_address: 0x105608000
(ds) pc_object: 0x8503
(ds) pc_address: 0x3f5628000
(ds) spraying 22528 sockets...
(ds) socket_ports_count: 0x5800
(ds) start_pcb_id: 0xc5fec
(ds) end_pcb_id: 0xd0fea
(ds) looking in search mapping: 0 / 8
(ds) matched PCB via process marker: lara
(ds) pcb_start_offset: 0x0
(ds) target_inp_gencnt: 0xcb72a
(ds) inp_list_next_pointer: 0xffffffe54e5e0400
(ds) icmp6filter: 0xffffffe637edbec0
(ds) corrupting icmp6filter pointer...
(ds) target corrupted: 0xffffffe54e5e0548
(ds) found control_socket at idx: 0x2b9f

(utils) socket fallback: pcb=0xffffffe54e5e0400 pid=14767

(utils) socket found via pcb+0x28 = 0xfffffff02c9721f8 (backptr at +0x0)

(utils) socket scan found nothing (pcb=0xffffffe54e5e0400 sock=0xfffffff02c9721f8)
(ds) exploit success!
(ds) kernel_base:  0xfffffff02932c000
(ds) kernel_slide: 0x22328000

(ds) highest_success_idx: 241
(ds) success_read_count: 8
(ds) walking kernel structures...
(ds) control_socket_pcb: 0xffffffe54e5e0000
(ds) pcbinfo_pointer: 0xfffffff02c97fd90
(ds) ipi_zone: 0xfffffff029e0f6a0
(ds) zv_name: 0xfffffff0293a2303
(ds) searching for kernel mach-o header from 0xfffffff0293a0000...
(ds) candidate mach-o at 0xfffffff029334000: filetype=2 cpuinfo=0x2c0000002 (iter=27)
(ds) candidate mach-o at 0xfffffff02932c000: filetype=12 cpuinfo=0xc00000002 (iter=29)
(ds) found mh_fileset header at 0xfffffff02932c000
(ds) t1sz_boot: 0xffffffffffffffff
(ds) kernel_base:  0xfffffff02932c000
(ds) kernel_slide: 0x22328000
(ds) iOS 18: using so_count offset 0x254
(ds) kernel r/w is ready!
(ds) our_proc: 0xffffffe469338920
(ds) our_task: 0xffffffe469339060

(ds) exploit success!
(ds) kernel_base:  0xfffffff02932c000
(ds) kernel_slide: 0x22328000

(vfs) vfs_init starting...
(vfs) heap PAC prefix: 0xffffff0000000000
(vfs) proc=0xffffffe469338920 task=0xffffffe469339060 (from exploit)
(vfs) file overwrite ready!
(sbx) proc=0xffffffe469338920 proc_ro_raw=0xffffffe250c94d00 proc_ro=0xffffffe250c94d00
(sbx) scanning proc_ro for ucred...
(sbx)   proc_ro+0x10: raw=0x39af smr=0x39af pac=0x39af
(sbx)   proc_ro+0x18: raw=0x10000a095 smr=0x10000a095 pac=0x10000a095
(sbx)   proc_ro+0x20: raw=0x2200230100000001 smr=0x100000001 pac=0x100000001
(sbx)   proc_ro+0x28: raw=0xffffffe250c68610 smr=0xffffffe250c68610 pac=0xffffffe250c68610
(sbx) found ucred at proc_ro+0x28 (SMR) = 0xffffffe250c68610
(sbx) ucred=0xffffffe250c68610 label=0xffffffe29d8f5760 sandbox=0xffffffe250da8820 ext_set=0xffffffe469a60dc0
(vfs) g_rootvnode = 0xffffffe29df08100
(vfs) first_nc = 0xffffffe383540038
(vfs) nc_vp = 0xffffffe29df08100
(vfs) nc_name = 0x9c30d0
(vfs) nc_name offset probe: 0x60
(vfs) ncache ok: first child='preboot'
(vfs) vfs_init done

vfs ready!

(sbx) patched 5 extensions
(sbx) changed 2 extension classes
(sbx) filled 12 empty hash slots
(sbx) escaped!

sandbox escape ready!


(rc) Unable to find process: youtube

(rc) process: SpringBoard, pid: 14593

(rc) firstExceptionPort: 0x2c13, secondExceptionPort: 0x4107

(rc) Unable to find process: youtube
initializing remote call on SpringBoard...

(rc) Unable to find process: youtube

(rc) Unable to find process: youtube

(rc) [iter 0] kstack=0xffffffe0f584ff30 kernelSP=0xffffffe0f584f880

(rc) [iter 0] Found TRO at offset=0x39f0

(rc) [iter 0] checkVal=0x104e checkVal2=0xffffffe29d8fcca0 (expecting 0x104e)

(rc) [iter 0] TRO swap: writing target tro=0xffffffe29d86dbe8 to addr=0xffffffe0f584f9f0

(rc) TRO swap SUCCESS!

(rc) set_exception_port_on_thread returning success=1

(rc) Inject EXC_GUARD on thread:0xffffffe637248980 OK

(rc) Valid threads: 1

(rc) Injected threads: 1

(rc) Finish clearing EXC_GUARD from all other threads...

(rc) trojanMemTemp: 0x16fdf9d40

(rc) vmMap: 0xffffffe16a6004f0

(pac) found pacia gadget, gadget addr = 0x189b36b30

(pac) remotepac: addr=0x101 mod=0x748100009f000000 gadget=0x189b36b30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xa40f590000000101

(pac) remotepac: addr=0x201 mod=0x77d300009f000000 gadget=0x189b36b30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xe32c1b8000000201

(rc) pcAddr for getpid: 0x1dc002f9c

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying

(pac) remotepac: addr=0x1dc002f9c mod=0x74810000cd000000 gadget=0x189b36b30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x601caa81dc002f9c

(pac) remotepac: addr=0x201 mod=0x77d30000cd000000 gadget=0x189b36b30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x9c3fa10000000201

(rc) getpid func's retValue = 0x3901(14593)

(pac) remotepac: addr=0x301 mod=0x0 gadget=0x189b36b30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xa62ed98000000301

(rc) remoteCrashSigned: 0xa62ed98000000301

(rc) pcAddr for pthread_create_suspended_np: 0x215634bd0

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying

(pac) remotepac: addr=0x215634bd0 mod=0x748100000e000000 gadget=0x189b36b30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x1b606f0215634bd0

(pac) remotepac: addr=0x201 mod=0x77d300000e000000 gadget=0x189b36b30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x2119cd8000000201

(rc) pthread_create_suspended_np func's retValue = 0x0(0)

(rc) trojanMemTemp: 0x16fdf9d40

(rc) pthreadAddr: 0x17002b000

(rc) pcAddr for pthread_mach_thread_np: 0x2156302d4

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying

(pac) remotepac: addr=0x2156302d4 mod=0x748100005d000000 gadget=0x189b36b30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x37440d82156302d4

(pac) remotepac: addr=0x201 mod=0x77d300005d000000 gadget=0x189b36b30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x911fc68000000201

(rc) pthread_mach_thread_np func's retValue = 0x7b607(505351)

(rc) callThreadPort: 0x7b607

(rc) [iter 0] kstack=0xffffffe0f57d7f30 kernelSP=0xffffffe0f57d7880

(rc) [iter 0] Found TRO at offset=0x39f0

(rc) [iter 0] checkVal=0x104e checkVal2=0xffffffe29d8f5820 (expecting 0x104e)

(rc) [iter 0] TRO swap: writing target tro=0xffffffe29d85fbe0 to addr=0xffffffe0f57d79f0

(rc) TRO swap SUCCESS!

(rc) set_exception_port_on_thread returning success=1

(rc) All good! Resuming trojan thread...

(rc) pcAddr for thread_resume: 0x1dbfff0b0

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying

(pac) remotepac: addr=0x1dbfff0b0 mod=0x748100001e000000 gadget=0x189b36b30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xa30c5481dbfff0b0

(pac) remotepac: addr=0x201 mod=0x77d300001e000000 gadget=0x189b36b30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x7e569c8000000201

(rc) thread_resume func's retValue = 0x0(0)

(rc) New thread created, resuming original

(pac) remotepac: addr=0x1115fc81dbffdce4 mod=0x748100008f000000 gadget=0x189b36b30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x32399081dbffdce4

(pac) remotepac: addr=0xe60d1601dc00139c mod=0x77d300008f000000 gadget=0x189b36b30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x8564c101dc00139c

(rc) Original thread restored

(rc) pcAddr for getpid: 0x1dc002f9c

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying

(pac) remotepac: addr=0x1dc002f9c mod=0x74810000a0000000 gadget=0x189b36b30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x147eb481dc002f9c

(pac) remotepac: addr=0x401 mod=0x77d30000a0000000 gadget=0x189b36b30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x9169bf8000000401

(rc) getpid func's retValue = 0x3901(14593)

(rc) Task pid: 14593

(rc) pcAddr for mmap: 0x1dc003bcc

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying

(pac) remotepac: addr=0x1dc003bcc mod=0x748100006b000000 gadget=0x189b36b30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x531b9001dc003bcc

(pac) remotepac: addr=0x401 mod=0x77d300006b000000 gadget=0x189b36b30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x215ddf0000000401

(rc) mmap func's retValue = 0x87f648000(36497031168)

(rc) pcAddr for memset: 0x215582120

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying

(pac) remotepac: addr=0x215582120 mod=0x74810000e9000000 gadget=0x189b36b30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x7d07868215582120

(pac) remotepac: addr=0x401 mod=0x77d30000e9000000 gadget=0x189b36b30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0xfa53b90000000401

(rc) memset func's retValue = 0x87f648000(36497031168)

(rc) Finished successfully

(rc) pcAddr for getpid: 0x1dc002f9c

(rc) Waiting for exception...

(rc) Exception received, setting up args and replying

(pac) remotepac: addr=0x1dc002f9c mod=0x74810000a2000000 gadget=0x189b36b30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x6341b281dc002f9c

(pac) remotepac: addr=0x401 mod=0x77d30000a2000000 gadget=0x189b36b30

(pac) pac thread state set, swapping keys and resuming

(pac) remotepac result: 0x7a57f30000000401

(rc) getpid func's retValue = 0x3901(14593)

(rc) Unable to find process: youtube

(rc) Unable to find process: youtube
remote call initialized on SpringBoard
rc init succeeded!
remote getpid() returned: 14593

(rc) Unable to find process: youtube

(rc) Unable to find process: youtube

Applying tweak "Dock"

(vfs) target /private/var/mobile/Containers/Data/Application/813639DF-E843-49B8-B174-785B25360BCA/tmp/vfs_src_3468452141.bin -> /System/Library/PrivateFrameworks/CoreMaterial.framework/dockDark.materialrecipe

(vfs) vfs_overwritefile returned: 0

(vfs) file overwritten

The operation couldn’t be completed. (Swift.String error 1.)

(vfs) target /private/var/mobile/Containers/Data/Application/813639DF-E843-49B8-B174-785B25360BCA/tmp/vfs_src_103051093.bin -> /System/Library/PrivateFrameworks/CoreMaterial.framework/dockLight.materialrecipe

(vfs) vfs_overwritefile returned: 0

(vfs) file overwritten

The operation couldn’t be completed. (Swift.String error 1.)

Success

Successfully applied tweak "Dock"

Applying tweak "Home Bar"

(vfs) target /private/var/mobile/Containers/Data/Application/813639DF-E843-49B8-B174-785B25360BCA/tmp/vfs_src_1060243257.bin -> /System/Library/PrivateFrameworks/MaterialKit.framework/Assets.car

(vfs) vfs_overwritefile returned: 0

(vfs) file overwritten

i hope it worked

Successfully applied tweak "Home Bar"

Applying tweak "Folder Background"

(vfs) target /private/var/mobile/Containers/Data/Application/813639DF-E843-49B8-B174-785B25360BCA/tmp/vfs_src_3430797326.bin -> /System/Library/PrivateFrameworks/SpringBoardHome.framework/folderDark.materialrecipe

(vfs) vfs_overwritefile returned: 0

(vfs) file overwritten

Failed to apply tweak "Folder Background"!!!

The operation couldn’t be completed. (Swift.String error 1.)

Applying tweak "Folder Blur"

(vfs) target /private/var/mobile/Containers/Data/Application/813639DF-E843-49B8-B174-785B25360BCA/tmp/vfs_src_3338704243.bin -> /System/Library/PrivateFrameworks/SpringBoardHome.framework/folderExpandedBackgroundHome.materialrecipe

(vfs) vfs_overwritefile returned: 0

(vfs) file overwritten

Failed to apply tweak "Folder Blur"!!!

The operation couldn’t be completed. (Swift.String error 1.)

Applying tweak "CC Module Background"

(vfs) target /private/var/mobile/Containers/Data/Application/813639DF-E843-49B8-B174-785B25360BCA/tmp/vfs_src_2520290500.bin -> /System/Library/PrivateFrameworks/CoreMaterial.framework/modules.materialrecipe

(vfs) vfs_overwritefile returned: 0

(vfs) file overwritten

Failed to apply tweak "CC Module Background"!!!

The operation couldn’t be completed. (Swift.String error 1.)

Applying tweak "CC Background Blur"

(vfs) target /private/var/mobile/Containers/Data/Application/813639DF-E843-49B8-B174-785B25360BCA/tmp/vfs_src_1217158318.bin -> /System/Library/PrivateFrameworks/CoreMaterial.framework/modulesBackground.materialrecipe

(vfs) vfs_overwritefile returned: 0

(vfs) file overwritten

Failed to apply tweak "CC Background Blur"!!!

The operation couldn’t be completed. (Swift.String error 1.)

Applying tweak "App Switcher Blur"

(vfs) target /private/var/mobile/Containers/Data/Application/813639DF-E843-49B8-B174-785B25360BCA/tmp/vfs_src_3051250605.bin -> /System/Library/PrivateFrameworks/SpringBoard.framework/homeScreenBackdrop-application.materialrecipe

(vfs) vfs_overwritefile returned: 0

(vfs) file overwritten

The operation couldn’t be completed. (Swift.String error 1.)

“homeScreenBackdrop-application.materialrecipe” couldn’t be removed.

Failed to apply tweak "App Switcher Blur"!!!

Applying tweak "Library Pod Background"

(vfs) target /private/var/mobile/Containers/Data/Application/813639DF-E843-49B8-B174-785B25360BCA/tmp/vfs_src_547121356.bin -> /System/Library/PrivateFrameworks/SpringBoardHome.framework/podBackgroundViewDark.visualstyleset

(vfs) vfs_overwritefile returned: 0

(vfs) file overwritten

Failed to apply tweak "Library Pod Background"!!!

The operation couldn’t be completed. (Swift.String error 1.)

Applying tweak "Notification Banner Background"

(vfs) target /private/var/mobile/Containers/Data/Application/813639DF-E843-49B8-B174-785B25360BCA/tmp/vfs_src_2529180106.bin -> /System/Library/PrivateFrameworks/CoreMaterial.framework/plattersDark.materialrecipe

(vfs) vfs_overwritefile returned: 0

(vfs) file overwritten

Failed to apply tweak "Notification Banner Background"!!!

The operation couldn’t be completed. (Swift.String error 1.)

Applying tweak "Notification Banner Shadow"

(vfs) target /private/var/mobile/Containers/Data/Application/813639DF-E843-49B8-B174-785B25360BCA/tmp/vfs_src_180351856.bin -> /System/Library/PrivateFrameworks/PlatterKit.framework/platterVibrantShadowDark.visualstyleset

(vfs) vfs_overwritefile returned: 0

(vfs) file overwritten

The operation couldn’t be completed. (Swift.String error 1.)

(vfs) target /private/var/mobile/Containers/Data/Application/813639DF-E843-49B8-B174-785B25360BCA/tmp/vfs_src_7876519.bin -> /System/Library/PrivateFrameworks/PlatterKit.framework/platterVibrantShadowLight.visualstyleset

(vfs) vfs_overwritefile returned: 0

(vfs) file overwritten

The operation couldn’t be completed. (Swift.String error 1.)

Success

Successfully applied tweak "Notification Banner Shadow"
(vfs) overwrite: /System/Library/PrivateFrameworks/CoreMaterial.framework/dockDark.materialrecipe <- /private/var/mobile/Containers/Data/Application/813639DF-E843-49B8-B174-785B25360BCA/tmp/vfs_src_3468452141.bin
(vfs) target mmap'd at 0x10edac000 (size 298)
(vfs) task (from exploit): 0xffffffe469339060
(vfs) vm_map: 0xffffffe24fe8cf40 (task+0x28, nentries=213)
(vfs) exploit task looks valid (nentries=213)
(vfs) vm_map: 0xffffffe24fe8cf40 (task+0x28, nentries=213)
(vfs) vm_map entries: 213, looking for 0x10edac000
(vfs) found entry 0xffffffe250a87dd0: 0x10edac000-0x10edb0000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) writing 298 bytes...
(vfs) overwrite done!
(vfs) overwrite: /System/Library/PrivateFrameworks/CoreMaterial.framework/dockLight.materialrecipe <- /private/var/mobile/Containers/Data/Application/813639DF-E843-49B8-B174-785B25360BCA/tmp/vfs_src_103051093.bin
(vfs) target mmap'd at 0x10edc8000 (size 347)
(vfs) task (from exploit): 0xffffffe469339060
(vfs) vm_map: 0xffffffe24fe8cf40 (task+0x28, nentries=214)
(vfs) exploit task looks valid (nentries=214)
(vfs) vm_map: 0xffffffe24fe8cf40 (task+0x28, nentries=214)
(vfs) vm_map entries: 214, looking for 0x10edc8000
(vfs) found entry 0xffffffe25085a980: 0x10edc8000-0x10edcc000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) writing 347 bytes...
(vfs) overwrite done!
(vfs) overwrite: /System/Library/PrivateFrameworks/MaterialKit.framework/Assets.car <- /private/var/mobile/Containers/Data/Application/813639DF-E843-49B8-B174-785B25360BCA/tmp/vfs_src_1060243257.bin
(vfs) target mmap'd at 0x10edc8000 (size 20536)
(vfs) task (from exploit): 0xffffffe469339060
(vfs) vm_map: 0xffffffe24fe8cf40 (task+0x28, nentries=214)
(vfs) exploit task looks valid (nentries=214)
(vfs) vm_map: 0xffffffe24fe8cf40 (task+0x28, nentries=214)
(vfs) vm_map entries: 214, looking for 0x10edc8000
(vfs) found entry 0xffffffe2502f3330: 0x10edc8000-0x10edd0000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) writing 3 bytes...
(vfs) zeroed 20533 trailing bytes
(vfs) overwrite done!
(vfs) overwrite: /System/Library/PrivateFrameworks/SpringBoardHome.framework/folderDark.materialrecipe <- /private/var/mobile/Containers/Data/Application/813639DF-E843-49B8-B174-785B25360BCA/tmp/vfs_src_3430797326.bin
(vfs) target mmap'd at 0x10edc8000 (size 347)
(vfs) task (from exploit): 0xffffffe469339060
(vfs) vm_map: 0xffffffe24fe8cf40 (task+0x28, nentries=214)
(vfs) exploit task looks valid (nentries=214)
(vfs) vm_map: 0xffffffe24fe8cf40 (task+0x28, nentries=214)
(vfs) vm_map entries: 214, looking for 0x10edc8000
(vfs) found entry 0xffffffe25085a980: 0x10edc8000-0x10edcc000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) writing 347 bytes...
(vfs) overwrite done!
(vfs) overwrite: /System/Library/PrivateFrameworks/SpringBoardHome.framework/folderExpandedBackgroundHome.materialrecipe <- /private/var/mobile/Containers/Data/Application/813639DF-E843-49B8-B174-785B25360BCA/tmp/vfs_src_3338704243.bin
(vfs) target mmap'd at 0x10edc8000 (size 250)
(vfs) task (from exploit): 0xffffffe469339060
(vfs) vm_map: 0xffffffe24fe8cf40 (task+0x28, nentries=214)
(vfs) exploit task looks valid (nentries=214)
(vfs) vm_map: 0xffffffe24fe8cf40 (task+0x28, nentries=214)
(vfs) vm_map entries: 214, looking for 0x10edc8000
(vfs) found entry 0xffffffe24faa0220: 0x10edc8000-0x10edcc000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) writing 250 bytes...
(vfs) overwrite done!
(vfs) overwrite: /System/Library/PrivateFrameworks/CoreMaterial.framework/modules.materialrecipe <- /private/var/mobile/Containers/Data/Application/813639DF-E843-49B8-B174-785B25360BCA/tmp/vfs_src_2520290500.bin
(vfs) target mmap'd at 0x10edc8000 (size 514)
(vfs) task (from exploit): 0xffffffe469339060
(vfs) vm_map: 0xffffffe24fe8cf40 (task+0x28, nentries=214)
(vfs) exploit task looks valid (nentries=214)
(vfs) vm_map: 0xffffffe24fe8cf40 (task+0x28, nentries=214)
(vfs) vm_map entries: 214, looking for 0x10edc8000
(vfs) found entry 0xffffffe250012de0: 0x10edc8000-0x10edcc000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) writing 514 bytes...
(vfs) overwrite done!
(vfs) overwrite: /System/Library/PrivateFrameworks/CoreMaterial.framework/modulesBackground.materialrecipe <- /private/var/mobile/Containers/Data/Application/813639DF-E843-49B8-B174-785B25360BCA/tmp/vfs_src_1217158318.bin
(vfs) target mmap'd at 0x10edc8000 (size 343)
(vfs) task (from exploit): 0xffffffe469339060
(vfs) vm_map: 0xffffffe24fe8cf40 (task+0x28, nentries=214)
(vfs) exploit task looks valid (nentries=214)
(vfs) vm_map: 0xffffffe24fe8cf40 (task+0x28, nentries=214)
(vfs) vm_map entries: 214, looking for 0x10edc8000
(vfs) found entry 0xffffffe24f83b100: 0x10edc8000-0x10edcc000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) writing 343 bytes...
(vfs) overwrite done!
(vfs) overwrite: /System/Library/PrivateFrameworks/SpringBoard.framework/homeScreenBackdrop-application.materialrecipe <- /private/var/mobile/Containers/Data/Application/813639DF-E843-49B8-B174-785B25360BCA/tmp/vfs_src_3051250605.bin
(vfs) target mmap'd at 0x10edc8000 (size 141)
(vfs) task (from exploit): 0xffffffe469339060
(vfs) vm_map: 0xffffffe24fe8cf40 (task+0x28, nentries=214)
(vfs) exploit task looks valid (nentries=214)
(vfs) vm_map: 0xffffffe24fe8cf40 (task+0x28, nentries=214)
(vfs) vm_map entries: 214, looking for 0x10edc8000
(vfs) found entry 0xffffffe25085a980: 0x10edc8000-0x10edcc000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) writing 141 bytes...
(vfs) overwrite done!
(vfs) overwrite: /System/Library/PrivateFrameworks/SpringBoardHome.framework/podBackgroundViewDark.visualstyleset <- /private/var/mobile/Containers/Data/Application/813639DF-E843-49B8-B174-785B25360BCA/tmp/vfs_src_547121356.bin
(vfs) target mmap'd at 0x10edc8000 (size 3107)
(vfs) task (from exploit): 0xffffffe469339060
(vfs) vm_map: 0xffffffe24fe8cf40 (task+0x28, nentries=215)
(vfs) exploit task looks valid (nentries=215)
(vfs) vm_map: 0xffffffe24fe8cf40 (task+0x28, nentries=215)
(vfs) vm_map entries: 215, looking for 0x10edc8000
(vfs) found entry 0xffffffe250012de0: 0x10edc8000-0x10edcc000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) writing 3107 bytes...
(vfs) overwrite done!
(vfs) overwrite: /System/Library/PrivateFrameworks/CoreMaterial.framework/plattersDark.materialrecipe <- /private/var/mobile/Containers/Data/Application/813639DF-E843-49B8-B174-785B25360BCA/tmp/vfs_src_2529180106.bin
(vfs) target mmap'd at 0x10edc8000 (size 416)
(vfs) task (from exploit): 0xffffffe469339060
(vfs) vm_map: 0xffffffe24fe8cf40 (task+0x28, nentries=215)
(vfs) exploit task looks valid (nentries=215)
(vfs) vm_map: 0xffffffe24fe8cf40 (task+0x28, nentries=215)
(vfs) vm_map entries: 215, looking for 0x10edc8000
(vfs) found entry 0xffffffe24f690fe0: 0x10edc8000-0x10edcc000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) writing 416 bytes...
(vfs) overwrite done!
(vfs) overwrite: /System/Library/PrivateFrameworks/PlatterKit.framework/platterVibrantShadowDark.visualstyleset <- /private/var/mobile/Containers/Data/Application/813639DF-E843-49B8-B174-785B25360BCA/tmp/vfs_src_180351856.bin
(vfs) target mmap'd at 0x10edc8000 (size 392)
(vfs) task (from exploit): 0xffffffe469339060
(vfs) vm_map: 0xffffffe24fe8cf40 (task+0x28, nentries=215)
(vfs) exploit task looks valid (nentries=215)
(vfs) vm_map: 0xffffffe24fe8cf40 (task+0x28, nentries=215)
(vfs) vm_map entries: 215, looking for 0x10edc8000
(vfs) found entry 0xffffffe24fc9d760: 0x10edc8000-0x10edcc000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) writing 392 bytes...
(vfs) overwrite done!
(vfs) overwrite: /System/Library/PrivateFrameworks/PlatterKit.framework/platterVibrantShadowLight.visualstyleset <- /private/var/mobile/Containers/Data/Application/813639DF-E843-49B8-B174-785B25360BCA/tmp/vfs_src_7876519.bin
(vfs) target mmap'd at 0x10edc8000 (size 295)
(vfs) task (from exploit): 0xffffffe469339060
(vfs) vm_map: 0xffffffe24fe8cf40 (task+0x28, nentries=215)
(vfs) exploit task looks valid (nentries=215)
(vfs) vm_map: 0xffffffe24fe8cf40 (task+0x28, nentries=215)
(vfs) vm_map entries: 215, looking for 0x10edc8000
(vfs) found entry 0xffffffe24f68d440: 0x10edc8000-0x10edcc000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) writing 295 bytes...
(vfs) overwrite done!

(rc) Unable to find process: youtube

(rc) Unable to find process: youtube

(rc) Unable to find process: youtube

(rc) Unable to find process: youtube
```

### Additional context:
<!-- Anything else — recent iOS updates, tweaks installed, prior jailbreak state, etc. -->
``SpringBoard Tools seems to attempt file overwrites in system frameworks (CoreMaterial, PlatterKit, SpringBoardHome), and logs show “overwrite done”, but UI changes are not reflected.

Other tweaks sometimes report success but are not visually applied after respring.``

### Pre-submission checklist:
<!-- Please confirm all of the following before submitting. Issues missing these may be closed. -->
- [x ] I have searched existing issues (open and closed) and confirmed this is not a duplicate
- [x ] I can reproduce this on the latest version / commit of Lara
- [x ] I have provided sufficient detail (device info, logs, reproduction steps) for a maintainer to investigate
- [x ] I agree to communicate respectfully with the developers and understand that harassment may result in restricted support
<!-- To Mark a check Put an "x" inside the brackets like so [x] -->


---

## Timeline (Verbose)

- May 20, 2026: 🏷️ @yamazakiteikoku added label `bug`


---

*Exported on 10/14/2026, 3:07:40 PM from [https://github.com/rooootdev/lara/issues/255](https://github.com/rooootdev/lara/issues/255)*
