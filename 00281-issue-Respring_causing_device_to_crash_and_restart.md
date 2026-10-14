# Respring causing device to crash and restart

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#281](https://github.com/rooootdev/lara/issues/281)  
> **State:** 🟢 open  
> **Labels:** `bug`  
> **Created:** May 30, 2026  
> **Updated:** Oct 9, 2026  

---

## Description

*Opened by [@MegaFTW3](https://github.com/MegaFTW3)* *on May 30, 2026*


### Describe the bug:
<!-- A clear and concise description of what the bug is. -->
``[after running the exploit by pressing run exploit and initialize system and respringing afterwards it causes the device to crash and restart ]``

### To Reproduce:
Steps to reproduce the behavior:
1. Go to 'lara app'
2. Tap 'Run Exploit and Initialize System'
3. Tap 'Respring'
4. See screen turn black and device getting restarted

### Expected behavior:
<!-- What you expected to happen. -->
``[device to get respringed successfully instead of the device crashing and restarting ]``

### Screenshots:
<!-- If applicable, add screenshots to help explain your problem. -->

### Device Info:
- **Device:** ``[iPad 8]`` <!-- e.g. iPhone 13 Pro, iPad Air 5 -->
- **Chip:** ``[A12]`` <!-- e.g. A15, A14, M1 -->
- **iOS Version:** ``[17.6.1 (21G93)]`` <!-- e.g. 16.5 (20F66) — include the build number -->
- **Jailbroken before?** ``[No]`` <!-- Yes / No / First attempt on this device -->
- **Lara version / commit:** ``[0.2]`` <!-- e.g. v0.3 or commit hash -->

### Logs:
<!-- Paste crash logs, console output, or kernel panic text. Use ``` fences. -->
```
(lara started: 2026-05-30 11:45:10

(utils) darksword not ready

(rc) Unable to find process: youtube

(utils) T1SZ_BOOT: 0x19

(utils) TASK_TNEXT_OFFSET: 0x58

(utils) THREAD_MUPCB_OFFSET: 0xb0

(utils) PROC_PID_OFFSET: 0x28

(offs) (user-set) t1sz_boot = 0x19 (default: 0x0)

(offs) loaded user-saved offsets above

(offs) initialized offsets

(offs) kernel: Darwin Kernel Version 23.6.0: Fri Jul  5 18:02:04 PDT 2024; root:xnu-10063.142.1~1/RELEASE_ARM64_T8020

(offs) kernbase: 0xfffffff007004000

(offs) kernentry: 0xfffffff007ca4588

(offs) allproc: 0x2fc2e00

(offs) kernproc: 0x8ec108

(offs) rootvnode: 0x2fc33f0

(offs) procsize: 0x740

(offs) t1szboot: 0x19)
```

### Additional context:
<!-- Anything else — recent iOS updates, tweaks installed, prior jailbreak state, etc. -->
``[Insert here]``

### Pre-submission checklist:
<!-- Please confirm all of the following before submitting. Issues missing these may be closed. -->
- [x] I have searched existing issues (open and closed) and confirmed this is not a duplicate
- [x] I can reproduce this on the latest version / commit of Lara
- [x] I have provided sufficient detail (device info, logs, reproduction steps) for a maintainer to investigate
- [x] I agree to communicate respectfully with the developers and understand that harassment may result in restricted support
<!-- To Mark a check Put an "x" inside the brackets like so [x] -->


---

## Timeline (Verbose)

- May 30, 2026: 🏷️ @MegaFTW3 added label `bug`


---

## Comments (3)

### Comment #1 — [@zhen737](https://github.com/zhen737) · Jun 3, 2026

`ID: 4609617933`


The same issue occurred, so instead of opening a new issue, imma just tag along here.

___
___

### Describe the bug:
<!-- A clear and concise description of what the bug is. -->
``[after running the exploit by pressing run exploit and initialize system and respringing afterwards it causes the device to crash and restart ]``

### To Reproduce:
Steps to reproduce the behavior (1):
1. Go to 'lara app'
2. Tap 'Run Exploit'
3. Tap 'Respring'
4. See screen turn black and device getting restarted

Steps to reproduce the behavior (2):
1. Go to 'lara app'
2. Tap 'Run Exploit and Initialize System'
3. Tap 'Respring'
4. See screen turn black and device getting restarted

Steps to reproduce the behavior (3):
1. Go to 'lara app'
2. Tap 'Respring' before 'Run Exploit and Initialize System'
3. Respring normally without issue

### Expected behavior:
<!-- What you expected to happen. -->
``[device to get respringed successfully instead of the device crashing and restarting ]``

### Screenshots:
<!-- If applicable, add screenshots to help explain your problem. -->

### Device Info:
- **Device:** ``[iPhone 11 (iPhone12,1)]`` <!-- e.g. iPhone 13 Pro, iPad Air 5 -->
- **Chip:** ``[A13]`` <!-- e.g. A15, A14, M1 -->
- **iOS Version:** ``[17.5.1 (21F90)]`` <!-- e.g. 16.5 (20F66) — include the build number -->
- **Jailbroken before?** ``[No]`` <!-- Yes / No / First attempt on this device -->
- **Lara version / commit:** ``[0.2]`` <!-- e.g. v0.3 or commit hash -->

### Logs:
<!-- Paste crash logs, console output, or kernel panic text. Use ``` fences. -->
```
lara started: 2026-06-03 2:02:13 PM


(ka) enabled keepalive


(utils) darksword not ready


(rc) Unable to find process: youtube


(utils) T1SZ_BOOT: 0x19


(utils) TASK_TNEXT_OFFSET: 0x58


(utils) THREAD_MUPCB_OFFSET: 0xb0


(utils) PROC_PID_OFFSET: 0x28


(offs) (user-set) t1sz_boot = 0x19 (default: 0x0)


(offs) loaded user-saved offsets above


(offs) initialized offsets


(offs) kernel: Darwin Kernel Version 23.5.0: Wed May  1 20:34:59 PDT 2024; root:xnu-10063.122.3~3/RELEASE_ARM64_T8030


(offs) kernbase: 0xfffffff007004000


(offs) kernentry: 0xfffffff007d4c4f0


(offs) allproc: 0x328edf0


(offs) kernproc: 0x9780f0


(offs) rootvnode: 0x328f3e0


(offs) procsize: 0x740


(offs) t1szboot: 0x19


(offs) initialized offsets
(ds) starting darksword
(ds) device: iPhone12,1
(ds) ispac: yes
(ds) running on non-a18 device
(ds) read_fd: 0xb
(ds) write_fd: 0xc
(ds) executable_path:       /private/var/containers/Bundle/Application/E1ED6931-0167-4E63-8C2B-7C59DD0BEDF4/App.app/lara
(ds) host_executable_path:  /private/var/containers/Bundle/Application/E1ED6931-0167-4E63-8C2B-7C59DD0BEDF4/App.app/lara
(ds) guest_executable_name: lara
(ds) host_executable_name:  lara
(ds) kernel_process_name:   lara
(ds) livecontainer_bundle:  no
(ds) livecontainer_guest:   no
(ds) rehosted_process:      no
(ds) process_marker 0: lara
(ds) executable_name: lara
(ds) free_thread_arg: 0x12f000000
(ds) physical_mapping_address: 0x10f440000
(ds) pc_object: 0x6e23
(ds) pc_address: 0x331dd4000
(ds) spraying 22528 sockets...
(ds) socket_ports_count: 0x5800
(ds) start_pcb_id: 0xa
(ds) end_pcb_id: 0xb008
(ds) looking in search mapping: 0 / 8
(ds) matched PCB via process marker: lara
(ds) pcb_start_offset: 0x0
(ds) target_inp_gencnt: 0x1b8c
(ds) inp_list_next_pointer: 0xffffffdffe420400
(ds) icmp6filter: 0xffffffe21283ba00
(ds) corrupting icmp6filter pointer...


(utils) socket fallback: pcb=0xffffffdffe420400 pid=300


(utils) socket found via pcb+0x28 = 0xfffffff0196907b0 (backptr at +0x0)


(utils) socket scan found nothing (pcb=0xffffffdffe420400 sock=0xfffffff0196907b0)
(ds) target corrupted: 0xffffffdffe420548
(ds) found control_socket at idx: 0xdc1
(ds) exploit success!
(ds) kernel_base:  0xfffffff01640c000
(ds) kernel_slide: 0xf408000


(ds) highest_success_idx: 357
(ds) success_read_count: 313
(ds) walking kernel structures...
(ds) control_socket_pcb: 0xffffffdffe420000
(ds) pcbinfo_pointer: 0xfffffff01969aeb8
(ds) ipi_zone: 0xfffffff016d8d040
(ds) zv_name: 0xfffffff01647e94d
(ds) searching for kernel mach-o header from 0xfffffff01647c000...
(ds) candidate mach-o at 0xfffffff016414000: filetype=2 cpuinfo=0x2c0000002 (iter=26)
(ds) candidate mach-o at 0xfffffff01640c000: filetype=12 cpuinfo=0xc00000002 (iter=28)
(ds) found mh_fileset header at 0xfffffff01640c000
(ds) t1sz_boot: 0xffffffffffffffff
(ds) kernel_base:  0xfffffff01640c000
(ds) kernel_slide: 0xf408000
(ds) iOS 17: using so_count offset 0x24c
(ds) kernel r/w is ready!
(ds) our_proc: 0xffffffdffde52f90
(ds) our_task: 0xffffffdffde536d0


(ds) exploit success!
(ds) kernel_base:  0xfffffff01640c000
(ds) kernel_slide: 0xf408000


(vfs) vfs_init starting...
(vfs) heap PAC prefix: 0xffffff0000000000
(vfs) proc=0xffffffdffde52f90 task=0xffffffdffde536d0 (from exploit)
(vfs) file overwrite ready!
(sbx) proc=0xffffffdffde52f90 proc_ro_raw=0xffffffdcf9b75400 proc_ro=0xffffffdcf9b75400
(sbx) scanning proc_ro for ucred...
(sbx)   proc_ro+0x10: raw=0x12c smr=0x12c pac=0x12c
(sbx)   proc_ro+0x18: raw=0x220023050000034e smr=0x50000034e pac=0x50000034e
(sbx)   proc_ro+0x20: raw=0xffffffdd46639e40 smr=0xffffffdd46639e40 pac=0xffffffdd46639e40
(sbx) found ucred at proc_ro+0x20 (SMR) = 0xffffffdd46639e40
(sbx) ucred=0xffffffdd46639e40 label=0xffffffdcf9b48cc0 sandbox=0xffffffdd466c3fa8 ext_set=0xffffffdff8fdfc40
(vfs) g_rootvnode = 0xffffffdf12d94f00
(vfs) first_nc = 0xffffffde2c3e3778
(vfs) nc_vp = 0xffffffdf12d94f00
(vfs) nc_name = 0x3e628b16
(vfs) nc_name offset probe: 0x60
(vfs) ncache ok: first child='.tmp.v8LSaFnnU0'
(vfs) vfs_init done


vfs ready!


(sbx) patched 3 extensions
(sbx) changed 2 extension classes
(sbx) filled 13 empty hash slots
(sbx) escaped!


sandbox escape ready!

```

### Additional context:
<!-- Anything else — recent iOS updates, tweaks installed, prior jailbreak state, etc. -->
``[Insert here]``

### Pre-submission checklist:
<!-- Please confirm all of the following before submitting. Issues missing these may be closed. -->
- [x] I have searched existing issues (open and closed) and confirmed this is not a duplicate
- [x] I can reproduce this on the latest version / commit of Lara
- [x] I have provided sufficient detail (device info, logs, reproduction steps) for a maintainer to investigate
- [x] I agree to communicate respectfully with the developers and understand that harassment may result in restricted support
<!-- To Mark a check Put an "x" inside the brackets like so [x] -->


---

### Comment #2 — [@maxblox2999](https://github.com/maxblox2999) · Sep 18, 2026

`ID: 5735765680`


> ### Describe the bug:
> <!-- A clear and concise description of what the bug is. -->
> ``[after running the exploit by pressing run exploit and initialize system and respringing afterwards it causes the device to crash and restart ]``
> 
> ### To Reproduce:
> Steps to reproduce the behavior:
> 1. Go to 'lara app'
> 2. Tap 'Run Exploit and Initialize System'
> 3. Tap 'Respring'
> 4. See screen turn black and device getting restarted
> 
> ### Expected behavior:
> <!-- What you expected to happen. -->
> ``[device to get respringed successfully instead of the device crashing and restarting ]``
> 
> ### Screenshots:
> <!-- If applicable, add screenshots to help explain your problem. -->
> 
> ### Device Info:
> - **Device:** ``[iPad 8]`` <!-- e.g. iPhone 13 Pro, iPad Air 5 -->
> - **Chip:** ``[A12]`` <!-- e.g. A15, A14, M1 -->
> - **iOS Version:** ``[17.6.1 (21G93)]`` <!-- e.g. 16.5 (20F66) — include the build number -->
> - **Jailbroken before?** ``[No]`` <!-- Yes / No / First attempt on this device -->
> - **Lara version / commit:** ``[0.2]`` <!-- e.g. v0.3 or commit hash -->
> 
> ### Logs:
> <!-- Paste crash logs, console output, or kernel panic text. Use ``` fences. -->
> ```
> (lara started: 2026-05-30 11:45:10
> 
> (utils) darksword not ready
> 
> (rc) Unable to find process: youtube
> 
> (utils) T1SZ_BOOT: 0x19
> 
> (utils) TASK_TNEXT_OFFSET: 0x58
> 
> (utils) THREAD_MUPCB_OFFSET: 0xb0
> 
> (utils) PROC_PID_OFFSET: 0x28
> 
> (offs) (user-set) t1sz_boot = 0x19 (default: 0x0)
> 
> (offs) loaded user-saved offsets above
> 
> (offs) initialized offsets
> 
> (offs) kernel: Darwin Kernel Version 23.6.0: Fri Jul  5 18:02:04 PDT 2024; root:xnu-10063.142.1~1/RELEASE_ARM64_T8020
> 
> (offs) kernbase: 0xfffffff007004000
> 
> (offs) kernentry: 0xfffffff007ca4588
> 
> (offs) allproc: 0x2fc2e00
> 
> (offs) kernproc: 0x8ec108
> 
> (offs) rootvnode: 0x2fc33f0
> 
> (offs) procsize: 0x740
> 
> (offs) t1szboot: 0x19)
> ```
> 
> ### Additional context:
> <!-- Anything else — recent iOS updates, tweaks installed, prior jailbreak state, etc. -->
> ``[Insert here]``
> 
> ### Pre-submission checklist:
> <!-- Please confirm all of the following before submitting. Issues missing these may be closed. -->
> - [x] I have searched existing issues (open and closed) and confirmed this is not a duplicate
> - [x] I can reproduce this on the latest version / commit of Lara
> - [x] I have provided sufficient detail (device info, logs, reproduction steps) for a maintainer to investigate
> - [x] I agree to communicate respectfully with the developers and understand that harassment may result in restricted support
> <!-- To Mark a check Put an "x" inside the brackets like so [x] -->
> 

 you know you can jailbreak with dopamine on that device right ( if ipados 15-26.01 )


---

### Comment #3 — [@buzz-lightsnack-2007](https://github.com/buzz-lightsnack-2007) · Sep 27, 2026

`ID: 5853204197`


I appear to encounter similar issues with iOS 17.6.1, on an iPhone 14 Plus, even when using a nightly build. However, it seems that crashing Lara (from itself, using the Crasher) while RemoteCall is enabled guarantees a respring without causing a kernel panic.


---

*Exported on 10/14/2026, 3:10:01 PM from [https://github.com/rooootdev/lara/issues/281](https://github.com/rooootdev/lara/issues/281)*
