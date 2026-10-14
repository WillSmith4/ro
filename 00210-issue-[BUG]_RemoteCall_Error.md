# [BUG] RemoteCall Error

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#210](https://github.com/rooootdev/lara/issues/210)  
> **State:** 🟢 open  
> **Labels:** `bug`, `sufficient info`  
> **Assignees:** [@Pro42good](https://github.com/Pro42good)  
> **Created:** Apr 28, 2026  
> **Updated:** May 25, 2026  

---

## Description

*Opened by [@Servant9527](https://github.com/Servant9527)* *on Apr 28, 2026*


### Describe the bug:
<!-- A clear and concise description of what the bug is. -->
``Red error message: Set target kaddr got invalid kaddr 0xffffffa7b38ecc38 ``

### To Reproduce:
Steps to reproduce the behavior:
1. Go to 'run exploit'
2. Tap 'remotecall'
3. Scroll down to 'Set target kaddr got invalid kaddr 0xffffffa7b38ecc38'
4. See error

### Expected behavior:
<!-- What you expected to happen. -->


### Screenshots:
<!-- If applicable, add screenshots to help explain your problem. -->

![image](https://github.com/user-attachments/assets/fa6768f0-aee8-4122-9258-2dda3153493b)

### Device Info:
- **Device:** ``[iphone 16 pro max]`` <!-- e.g. iPhone 13 Pro, iPad Air 5 -->
- **Chip:** ``[A18 pro]`` <!-- e.g. A15, A14, M1 -->
- **iOS Version:** ``IOS26.0.1(23A355)`` <!-- e.g. 16.5 (20F66) — include the build number -->
- **Jailbroken before?** ``No`` <!-- Yes / No / First attempt on this device -->
- **Lara version / commit:** ``v1.2`` <!-- e.g. v0.3 or commit hash -->

### Logs:
<!-- Paste crash logs, console output, or kernel panic text. Use ``` fences. -->
```

lara started: 2026-04-29 03:10:26

(utils) TASK_TNEXT_OFFSET: 0x50

(utils) THREAD_MUPCB_OFFSET: 0xb8

(utils) PROC_PID_OFFSET: 0x60

(offs) loaded saved t1sz_boot: 0x11

(offs) t1sz_boot: 0x11

(offs) saved t1sz_boot: 0x11

initialized offsets

(offs) loaded saved t1sz_boot: 0x11

(offs) t1sz_boot: 0x11

(offs) saved t1sz_boot: 0x11

initialized offsets

(persist) sandbox_extension_consume failed

(persist) bootstrap_look_up failed: unknown error code

(ds) starting darksword
(ds) recovering primitives from launchd...
(ds) device: iPhone17,2
(ds) ispac: yes
(ds) running on a18 device
(ds) read_fd: 0x9
(ds) write_fd: 0xa
(ds) executable_path: /private/var/containers/Bundle/Application/167467DC-12E0-42DD-A1DF-65A0F30BCFA4/App.app/lara
(ds) host_executable_path: /private/var/containers/Bundle/Application/167467DC-12E0-42DD-A1DF-65A0F30BCFA4/App.app/lara
(ds) guest_executable_name: lara
(ds) host_executable_name: lara
(ds) kernel_process_name: lara
(ds) livecontainer_bundle: no
(ds) livecontainer_guest: no
(ds) rehosted_process: no
(ds) process_marker[0]: lara
(ds) executable_name: lara
(ds) free_thread_arg: 0x120c2c000
(ds) physical_mapping_address: 0x10dac0000
(ds) pc_object: 0x2d0f
(ds) pc_address: 0x4314b8000
(ds) allocating wired memory (131072 entries)...
(ds) allocating wired memory done
(ds) seeking_offset: 0x148000: found wired_page: 0x16c51c000
(ds) seeking_offset: 0x34000: found wired_page: 0x152e2c000
(ds) seeking_offset: 0x28000: found wired_page: 0x168118000
(ds) seeking_offset: 0x4000: found wired_page: 0x30e970000
(ds) seeking_offset: 0x1a0000: found wired_page: 0x1621ac000
(ds) seeking_offset: 0x10000: found wired_page: 0x11a868000
(ds) seeking_offset: 0x9c000: found wired_page: 0x30bf48000
(ds) Matched PCB via process marker: lara
(ds) pcb_start_offset: 0x0
(ds) target_inp_gencnt: 0x32f22
(ds) inp_list_next_pointer: 0xffffffde835d0400
(ds) icmp6filter: 0xffffffe26a089b20
(ds) Corrupting icmp6filter pointer...
(ds) target corrupted: 0xffffffde835d0548
(ds) found control_socket at idx: 0x32c5
(ds) seeking_offset: 0x28000: Found PCB page
(ds) highest_success_idx: 498

(utils) socket fallback: pcb=0xffffffde835d0400 pid=4152
(ds) success_read_count: 51

(utils) socket found via pcb+0x28 = 0xfffffff054340e38 (backptr at +0x0)

(utils) socket scan found nothing (pcb=0xffffffde835d0400 sock=0xfffffff054340e38)

(rc) process: launchd, pid: 1

(rc) firstExceptionPort: 0x3d1b, secondExceptionPort: 0x425913

(ds) kaddr isn't valid: 0xffffffa7b38ec5d8

(persist) Failed to create RemoteCall for launchd
(ds) Walking kernel structures...
(ds) control_socket_pcb: 0xffffffde835d0000
(ds) pcbinfo_pointer: 0xfffffff05434eaf0
(ds) ipi_zone: 0xfffffff050d0a950
(ds) zv_name: 0xfffffff0500d465c
(ds) searching for kernel Mach-O header from 0xfffffff0500d4000...
(ds) exploit success!
(ds) kernel_base:  0xfffffff050058000
(ds) kernel_slide: 0x49054000

(ds) candidate Mach-O at 0xfffffff050060000: filetype=2 cpuinfo=0x2c0000002 (iter=29)
(ds) candidate Mach-O at 0xfffffff050058000: filetype=12 cpuinfo=0xcc0000002 (iter=31)
(ds) found MH_FILESET header at 0xfffffff050058000
(ds) kernel_base:  0xfffffff050058000
(ds) kernel_slide: 0x49054000
(ds) iOS 26: using so_count offset 0x23c
(ds) kernel r/w is ready!
(ds) our_proc: 0xffffffe05756baa0
(ds) our_task: 0xffffffe05756c1e8
(ds) transferring primitives to launchd...

(ds) exploit success!
(ds) kernel_base:  0xfffffff050058000
(ds) kernel_slide: 0x49054000


(rc) process: SpringBoard, pid: 35

(rc) firstExceptionPort: 0x811f, secondExceptionPort: 0xa223

(ds) kaddr isn't valid: 0xffffffa7b38ecc38
T
initializing remote call on SpringBoard...
remote call init failed on SpringBoard
remote call init failed on SpringBoard: set_target_kaddr got invalid kaddr 0xffffffa7b38ecc38
rc init failed

```

### Additional context:
<!-- Anything else — recent iOS updates, tweaks installed, prior jailbreak state, etc. -->

### Pre-submission checklist:
<!-- Please confirm all of the following before submitting. Issues missing these may be closed. -->
- [x] I have searched existing issues (open and closed) and confirmed this is not a duplicate
- [ ] I can reproduce this on the latest version / commit of Lara
- [x] I have provided sufficient detail (device info, logs, reproduction steps) for a maintainer to investigate
- [x] I agree to communicate respectfully with the developers and understand that harassment may result in restricted support
<!-- To Mark a check Put an "x" inside the brackets like so [x] -->


---

## Timeline (Verbose)

- Apr 28, 2026: 👤 @Servant9527 assigned @Pro42good
- Apr 28, 2026: 🏷️ @Servant9527 added label `bug`
- May 4, 2026: 🏷️ @Pro42good added label `sufficient info`


---

## Comments (6)

### Comment #1 — [@Pro42good](https://github.com/Pro42good) · May 4, 2026

`ID: 4373233099`


Could you try on the latest build or latest nightly build?


---

### Comment #2 — [@Servant9527](https://github.com/Servant9527) · May 6, 2026

`ID: 4384534643`


The latest nightly version also throws an error and can’t be used, but the app doesn’t crash, so I can see the logs as follows.

lara started: 2026-05-06 09:21:58

(ka) enabled keepalive

(utils) no kernelcache...

xpf start error: Failed to open kernelcache

(utils) T1SZ_BOOT: 0x0

(utils) TASK_TNEXT_OFFSET: 0x50

(utils) THREAD_MUPCB_OFFSET: 0xb8

(utils) PROC_PID_OFFSET: 0x60

(offs) initialized offsets

(offs) initialized offsets

(persist) sandbox_extension_consume failed

(persist) bootstrap_look_up failed: unknown error code
(ds) starting darksword
(ds) recovering primitives from launchd...
(ds) device: iPhone17,2
(ds) ispac: yes
(ds) running on a18 device
(ds) read_fd: 0xa
(ds) write_fd: 0xb
(ds) executable_path: /private/var/containers/Bundle/Application/7752DD4D-5F6E-4494-9ACC-B3A0C21610CB/App.app/lara
(ds) host_executable_path: /private/var/containers/Bundle/Application/7752DD4D-5F6E-4494-9ACC-B3A0C21610CB/App.app/lara
(ds) guest_executable_name: lara
(ds) host_executable_name: lara
(ds) kernel_process_name: lara
(ds) livecontainer_bundle: no
(ds) livecontainer_guest: no
(ds) rehosted_process: no
(ds) process_marker[0]: lara
(ds) executable_name: lara
(ds) free_thread_arg: 0x138d28000
(ds) physical_mapping_address: 0x1113f8000
(ds) pc_object: 0x8207
(ds) pc_address: 0x4474e4000
(ds) allocating wired memory (131072 entries)...
(ds) allocating wired memory done
(ds) seeking_offset: 0xc8000: found wired_page: 0x16be44000
(ds) seeking_offset: 0x0: found wired_page: 0x313a18000
(ds) seeking_offset: 0xcc000: found wired_page: 0x117728000
(ds) seeking_offset: 0x9c000: found wired_page: 0x311800000
(ds) seeking_offset: 0xa4000: found wired_page: 0x310d34000
(ds) seeking_offset: 0xdc000: found wired_page: 0x11cb4c000
(ds) seeking_offset: 0x1a4000: found wired_page: 0x1735c0000
(ds) matched PCB via process marker: lara
(ds) pcb_start_offset: 0x0
(ds) target_inp_gencnt: 0xa8fa
(ds) inp_list_next_pointer: 0xffffffde22444400
(ds) icmp6filter: 0xffffffe20039efc0
(ds) corrupting icmp6filter pointer...
(ds) target corrupted: 0xffffffde22444548
(ds) found control_socket at idx: 0x4ac7
(ds) seeking_offset: 0x9c000: found PCB page

(utils) socket fallback: pcb=0xffffffde22444400 pid=5878

(utils) socket found via pcb+0x28 = 0xfffffff04d10ce38 (backptr at +0x0)

(utils) socket scan found nothing (pcb=0xffffffde22444400 sock=0xfffffff04d10ce38)
(ds) highest_success_idx: 495
(ds) success_read_count: 110

(rc) process: launchd, pid: 1
(ds) walking kernel structures...
(ds) control_socket_pcb: 0xffffffde22444000

(rc) firstExceptionPort: 0xa827, secondExceptionPort: 0x425a07

(ds) kaddr isn't valid: 0xffffffa2044ecfe0
(ds) pcbinfo_pointer: 0xfffffff04d11aaf0
(ds) ipi_zone: 0xfffffff049ad6950
(ds) zv_name: 0xfffffff048ea065c
(ds) searching for kernel Mach-O header from 0xfffffff048ea0000...

(persist) Failed to create RemoteCall for launchd
(ds) candidate Mach-O at 0xfffffff048e2c000: filetype=2 cpuinfo=0x2c0000002 (iter=29)
(ds) candidate Mach-O at 0xfffffff048e24000: filetype=12 cpuinfo=0xcc0000002 (iter=31)
(ds) found MH_FILESET header at 0xfffffff048e24000
(ds) kernel_base:  0xfffffff048e24000
(ds) kernel_slide: 0x41e20000
(ds) iOS 26: using so_count offset 0x23c
(ds) kernel r/w is ready!
(ds) our_proc: 0xffffffe0cc107220
(ds) our_task: 0xffffffe0cc107968
(ds) transferring primitives to launchd...
(ds) exploit success!
(ds) kernel_base:  0xfffffff048e24000
(ds) kernel_slide: 0x41e20000


(ds) exploit success!
(ds) kernel_base:  0xfffffff048e24000
(ds) kernel_slide: 0x41e20000


(rc) process: SpringBoard, pid: 35

(rc) firstExceptionPort: 0x8e841b, secondExceptionPort: 0x8c190f

(ds) kaddr isn't valid: 0xffffffa2045c1c80
T
initializing remote call on SpringBoard...
remote call init failed on SpringBoard
remote call init failed on SpringBoard: set_target_kaddr got invalid kaddr 0xffffffa2045c1c80
rc init failed


---

### Comment #3 — [@Luderas](https://github.com/Luderas) · May 15, 2026

`ID: 4459453533`


**remote call init failed on SpringBoard – iPhone 16 / iOS 18.5**

Device: iPhone 16

iOS: 18.5

Version tested: latest, nightly

Issue:
remote call init always fails on SpringBoard with invalid kaddr
Error:`(ds) kaddr isn't valid: 0xffffbfeb7aa305d8
remote call init failed on SpringBoard: set_target_kaddr got invalid kaddr
rc init failed
`

Steps to reproduce:
	1.	Open lara
	2.	Run exploit
	3.	Exploit succeeds (KR/W ready, sandbox escaped)
	4.	Remote call on SpringBoard fails every attempt
Additional info:
	•	Tried multiple times with reboot between attempts
	•	Happens consistently, never succeeds


---

### Comment #4 — [@NsCrosu](https://github.com/NsCrosu) · May 24, 2026

`ID: 4527709698`


same


---

### Comment #5 — [@Luderas](https://github.com/Luderas) · May 24, 2026

`ID: 4527751906`


Delete every related app first.
Install the Nightly build, don’t fetch a kernel cache manually.
Run the exploit directly, then fetch the kernelcache and test remote call.
Repeat until remote call finally works.

When it works, save the kernelcache from the Lara folder in the Files app as a backup.
Then you can stay on Nightly or reinstall Lara 2.0 and import that cache there.

Sometimes the cache stops working again later.

For me, fetching in Lara 2.0 (latest version) never worked, only in Nightly.
Maybe it works for you though, just experiment a bit.

You can also try Lara in LiveContainer.
Remote call usually won’t work there, but you might still be able to get the  kernelcache.
Just test around a little.


---

### Comment #6 — [@Servant9527](https://github.com/Servant9527) · May 25, 2026

`ID: 4533603095`


Thanks for the reply. Lara v0.2 is already working properly for me. I only need it to display the date and weekday in RemoteCall.


---

*Exported on 10/14/2026, 3:03:58 PM from [https://github.com/rooootdev/lara/issues/210](https://github.com/rooootdev/lara/issues/210)*
