# [BUG] App crashes after initialise VFS

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#157](https://github.com/rooootdev/lara/issues/157)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@marzexis](https://github.com/marzexis)  
> **Labels:** `bug`  
> **Assignees:** [@Pro42good](https://github.com/Pro42good)  
> **Created:** Apr 21, 2026  
> **Updated:** Apr 21, 2026  
> **Closed:** Apr 21, 2026  

---

## Description

*Opened by [@marzexis](https://github.com/marzexis)* *on Apr 21, 2026*


### Describe the bug:
<!-- A clear and concise description of what the bug is. -->
``App crashes after initialise VFS,  SBX works ``

### To Reproduce:
Steps to reproduce the behavior:
1. Go to '...'
2. Tap '...'
3. Scroll down to '...'
4. See error

### Expected behavior:
<!-- What you expected to happen. -->
``Initialise VFS Correctly executed without Lara crash``

### Screenshots:
<!-- If applicable, add screenshots to help explain your problem. -->

### Device Info:
- **Device:** ``iPhone 15 Pro`` <!-- e.g. iPhone 13 Pro, iPad Air 5 -->
- **Chip:** ``A17 Pro`` <!-- e.g. A15, A14, M1 -->
- **iOS Version:** ``26.0.0 (23A341)`` <!-- e.g. 16.5 (20F66) — include the build number -->
- **Jailbroken before?** ``No`` <!-- Yes / No / First attempt on this device -->
- **Lara version / commit:** ``791d063`` <!-- e.g. v0.3 or commit hash -->

### Logs:
<!-- Paste crash logs, console output, or kernel panic text. Use ``` fences. -->
```
lara started: 2026-04-21 13:44:21

(utils) TASK_TNEXT_OFFSET: 0x50

(utils) THREAD_MUPCB_OFFSET: 0x108

(utils) PROC_PID_OFFSET: 0x60

(offs) loaded saved t1sz_boot: 0x11

(offs) t1sz_boot: 0x11

(offs) saved t1sz_boot: 0x11

initialized offsets

(offs) loaded saved t1sz_boot: 0x11

(offs) t1sz_boot: 0x11

(offs) saved t1sz_boot: 0x11

initialized offsets

(ds) starting darksword
(ds) device: iPhone16,1
(ds) ispac: yes
(ds) running on non-a18 device
(ds) read_fd: 0x9
(ds) write_fd: 0xa
(ds) executable_path: /private/var/containers/Bundle/Application/A1244DD0-7D52-4111-AAD9-CB41B2ABB0A5/lara.app/lara
(ds) host_executable_path: /private/var/containers/Bundle/Application/A1244DD0-7D52-4111-AAD9-CB41B2ABB0A5/lara.app/lara
(ds) guest_executable_name: lara
(ds) host_executable_name: lara
(ds) kernel_process_name: lara
(ds) livecontainer_bundle: no
(ds) livecontainer_guest: no
(ds) rehosted_process: no
(ds) process_marker[0]: lara
(ds) executable_name: lara
(ds) free_thread_arg: 0x1393ac000
(ds) physical_mapping_address: 0x10d100000
(ds) pc_object: 0x4b0f
(ds) pc_address: 0x15cf28000
(ds) spraying 22528 sockets...
(ds) socket_ports_count: 0x5800
(ds) start_pcb_id: 0x84803
(ds) end_pcb_id: 0x8f801
(ds) looking in search mapping: 0
(ds) Matched PCB via process marker: lara
(ds) pcb_start_offset: 0x0
(ds) target_inp_gencnt: 0x8daef
(ds) inp_list_next_pointer: 0xffffffe30dcf4400
(ds) icmp6filter: 0xffffffe4d7ed3b80
(ds) Corrupting icmp6filter pointer...
(ds) target corrupted: 0xffffffe30dcf4548
(ds) found control_socket at idx: 0x4976

(utils) socket fallback: pcb=0xffffffe30dcf4400 pid=2096

(utils) socket found via pcb+0x28 = 0xfffffff04e1e4d48 (backptr at +0x0)

(utils) socket scan found nothing (pcb=0xffffffe30dcf4400 sock=0xfffffff04e1e4d48)
(ds) exploit success!
(ds) kernel_base:  0xfffffff04a284000
(ds) kernel_slide: 0x43280000

(ds) highest_success_idx: 491
(ds) success_read_count: 118
(ds) Walking kernel structures...
(ds) control_socket_pcb: 0xffffffe30dcf4000
(ds) pcbinfo_pointer: 0xfffffff04e1f2a40
(ds) ipi_zone: 0xfffffff04ae5a170
(ds) zv_name: 0xfffffff04a2f7558
(ds) searching for kernel Mach-O header from 0xfffffff04a2f4000...
(ds) candidate Mach-O at 0xfffffff04a28c000: filetype=2 cpuinfo=0x2c0000002 (iter=26)
(ds) candidate Mach-O at 0xfffffff04a284000: filetype=12 cpuinfo=0xcc0000002 (iter=28)
(ds) found MH_FILESET header at 0xfffffff04a284000
(ds) kernel_base:  0xfffffff04a284000
(ds) kernel_slide: 0x43280000
(ds) iOS 26: using so_count offset 0x23c
(ds) kernel r/w is ready!
(ds) our_proc: 0xffffffe3f295fb40
(ds) our_task: 0xffffffe3f2960288

(ds) exploit success!
(ds) kernel_base:  0xfffffff04a284000
(ds) kernel_slide: 0x43280000

(sbx) proc=0xffffffe3f295fb40 proc_ro_raw=0xffffffe13dff1718 proc_ro=0xffffffe13dff1718
(sbx) scanning proc_ro for ucred...
(sbx)   proc_ro+0x10: raw=0x830 smr=0x830 pac=0x830
(sbx)   proc_ro+0x18: raw=0x10000162e smr=0x10000162e pac=0x10000162e
(sbx)   proc_ro+0x20: raw=0x2200230500000001 smr=0x500000001 pac=0x500000001
(sbx)   proc_ro+0x28: raw=0xffffffe0f0032c50 smr=0xffffffe0f0032c50 pac=0xffffffe0f0032c50
(sbx) found ucred at proc_ro+0x28 (SMR) = 0xffffffe0f0032c50
(sbx) ucred=0xffffffe0f0032c50 label=0xffffffe0f021f2c0 sandbox=0xffffffe0f0150880 ext_set=0xffffffe3efad6800
(sbx) patched 5 extensions
(sbx) changed 3 extension classes
(sbx) filled 11 empty hash slots
(sbx) escaped!

sandbox escape ready!
```

### Additional context:
<!-- Anything else — recent iOS updates, tweaks installed, prior jailbreak state, etc. -->
``[Insert here]``

### Pre-submission checklist:
<!-- Please confirm all of the following before submitting. Issues missing these may be closed. -->
- [ X] I have searched existing issues (open and closed) and confirmed this is not a duplicate
- [ X] I can reproduce this on the latest version / commit of Lara
- [ X] I have provided sufficient detail (device info, logs, reproduction steps) for a maintainer to investigate
- [ X] I agree to communicate respectfully with the developers and understand that harassment may result in restricted support
<!-- To Mark a check Put an "x" inside the brackets like so [x] -->


---

## Timeline (Verbose)

- Apr 21, 2026: 👤 @marzexis assigned @Pro42good
- Apr 21, 2026: 🏷️ @marzexis added label `bug`
- Apr 21, 2026: ✏️ @marzexis renamed: "[BUG] Insert Title Here" → "[BUG] App crashes after initialise VFS"
- Apr 21, 2026: 🔴 Closed by @marzexis


---

## Comments (6)

### Comment #1 — [@tiziodied](https://github.com/tiziodied) · Apr 21, 2026

`ID: 4288303511`


first make sure you are updated to the latest version, then go to settings and scroll down until you find T1SZ_BOOT and change the value from 0x11 to 0x19 and tell me if it works


---

### Comment #2 — [@marzexis](https://github.com/marzexis) · Apr 21, 2026

`ID: 4288315534`


It works! T1SZ_BOOT now is 0x19 ,
more test will follow 
Thanks a lot!


---

### Comment #3 — [@tiziodied](https://github.com/tiziodied) · Apr 21, 2026

`ID: 4288323429`


if you have more problems please ask, if not then close this issue


---

### Comment #4 — [@marzexis](https://github.com/marzexis) · Apr 21, 2026

`ID: 4288331137`


these options won't appear: 

5 App Dock
Status Bar Tweaks
Hide labels
Upside Down
Floating Dock (Broken)
Grid App Switcher
Performance

Is it correct?


---

### Comment #5 — [@tiziodied](https://github.com/tiziodied) · Apr 21, 2026

`ID: 4288352080`


they should appear after you initialise RemoteCall, also i highly advise you to switch over to Hybrid because it's better


---

### Comment #6 — [@marzexis](https://github.com/marzexis) · Apr 21, 2026

`ID: 4288391800`


Yes i was in SFX only to check if the problem persist. 
after initialise RemoteCall, options appeared. 
Thanks a lot
I'll close the issue


---

*Exported on 10/14/2026, 2:58:51 PM from [https://github.com/rooootdev/lara/issues/157](https://github.com/rooootdev/lara/issues/157)*
