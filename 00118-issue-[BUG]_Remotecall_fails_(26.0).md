# [BUG] Remotecall fails (26.0)

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#118](https://github.com/rooootdev/lara/issues/118)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@rooootdev](https://github.com/rooootdev)  
> **Labels:** `bug`, `sufficient info`  
> **Assignees:** [@Pro42good](https://github.com/Pro42good)  
> **Created:** Apr 17, 2026  
> **Updated:** Apr 21, 2026  
> **Closed:** Apr 21, 2026  

---

## Description

*Opened by [@wijathemidja](https://github.com/wijathemidja)* *on Apr 17, 2026*


On iPhone 15 Pro on 26.0, all RemoteCall features fail including JIT (though this is marked as broken).

1. Install Lara via Sidestore / LC from the latest Github release IPA.

2. Run the exploit (you can initialise VFS and get the same issue)

3. Try and use any RemoteCall feature in Lara

Phone restarts completely

No screenshots of the shutdown.

- **Device:** iPhone 15 Pro

- **Chip:** A17 Pro
- **iOS Version:** iOS 26.0 (23A341)
- **Jailbroken before?** No (Lara has been used before)
- **Lara version / commit:** 1.2

Log doesn't save as it breaks 
This is after the kexploit:

lara started: 2026-04-17 15:26:03

(utils) TASK_TNEXT_OFFSET: 0x50

(utils) THREAD_MUPCB_OFFSET: 0x108

(utils) PROC_PID_OFFSET: 0x60

(utils) PROC_STRUCT_SIZE: 0x748

initialized offsets

initialized offsets

(ds) starting darksword
(ds) device: iPhone16,1
(ds) ispac: yes
(ds) running on non-a18 device
(ds) read_fd: 0x9
(ds) write_fd: 0xa
(ds) executable_path: /private/var/mobile/Containers/Data/Application/3ABD37F0-A0DA-481D-93B8-4A04EB579464/Documents/Applications/com.roooot.laraapp.app/lara
(ds) host_executable_path: /private/var/containers/Bundle/Application/B85E7893-F5C9-4C1E-BFA7-BEF5A410F440/LiveContainer.app/LiveContainer
(ds) guest_executable_name: lara
(ds) host_executable_name: LiveContainer
(ds) kernel_process_name: LiveContainer
(ds) livecontainer_bundle: yes
(ds) livecontainer_guest: yes
(ds) rehosted_process: yes
(ds) process_marker[0]: LiveProcess
(ds) process_marker[1]: LiveContainer
(ds) process_marker[2]: lara
(ds) executable_name: lara
(ds) free_thread_arg: 0x1131e0000
(ds) physical_mapping_address: 0x10d2b0000
(ds) pc_object: 0x8d03
(ds) pc_address: 0x39a640000
(ds) spraying 22528 sockets...
(ds) socket_ports_count: 0x5800
(ds) start_pcb_id: 0x4
(ds) end_pcb_id: 0xb002
(ds) looking in search mapping: 0
(ds) Matched PCB via process marker: LiveContainer
(ds) pcb_start_offset: 0x0
(ds) target_inp_gencnt: 0x3e42
(ds) inp_list_next_pointer: 0xffffffe2d57e8400
(ds) icmp6filter: 0xffffffe40a40b700
(ds) Corrupting icmp6filter pointer...
(ds) target corrupted: 0xffffffe2d57e8548
(ds) found control_socket at idx: 0x1f1f

(utils) kernel proc: 0xffffffe1f253f240

(utils) looking for pid: 482

(utils) found proc: LiveContainer (pid=482 uid=501 gid=501) @ 0xffffffe1f14056c0
(ds) highest_success_idx: 500
(ds) success_read_count: 815
exploit success!
kernel_base:  0xfffffff04fd60000
kernel_slide: 0x48d5c000

(ds) Walking kernel structures...
(ds) control_socket_pcb: 0xffffffe2d57e8000
(ds) pcbinfo_pointer: 0xfffffff053ccea40
(ds) ipi_zone: 0xfffffff050936170
(ds) zv_name: 0xfffffff04fdd3558
(ds) searching for kernel Mach-O header from 0xfffffff04fdd0000...
(ds) candidate Mach-O at 0xfffffff04fd68000: filetype=2 cpuinfo=0x2c0000002 (iter=26)
(ds) candidate Mach-O at 0xfffffff04fd60000: filetype=12 cpuinfo=0xcc0000002 (iter=28)
(ds) found MH_FILESET header at 0xfffffff04fd60000
(ds) kernel_base:  0xfffffff04fd60000
(ds) kernel_slide: 0x48d5c000
(ds) iOS 26: using so_count offset 0x23c
(ds) kernel r/w is ready!
(ds) our_proc: 0xffffffe1f14056c0
(ds) our_task: 0xffffffe1f1405e08

exploit success!
kernel_base:  0xfffffff04fd60000
kernel_slide: 0x48d5c000




- [x] I have searched existing issues (open and closed) and confirmed this is not a duplicate
- There was an issue but that was after initiation. 
- A kernel panic was reported but this was on the old broken remoteCall version
- [x] I can reproduce this on the latest version / commit of Lara
- [x] I have provided sufficient detail (device info, logs, reproduction steps) for a maintainer to investigate
- [x] I agree to communicate respectfully with the developers and understand that harassment may result in restricted support


---

## Timeline (Verbose)

- Apr 17, 2026: 👤 @wijathemidja assigned @Pro42good
- Apr 17, 2026: 🏷️ @wijathemidja added label `bug`
- Apr 17, 2026: 🏷️ @Pro42good added label `sufficient info`
- Apr 19, 2026: ✏️ @wijathemidja renamed: "[BUG] Remotecall fails on 26.0" → "[BUG] Remotecall fails (26.0)"
- Apr 21, 2026: 🔴 Closed by @rooootdev


---

## Comments (10)

### Comment #1 — [@Pro42good](https://github.com/Pro42good) · Apr 17, 2026

`ID: 4268840166`


Yeah this is a known issue, Remotecall is kinda broken on the latest versions. Sorry about that, It's getting fixed soon.


---

### Comment #2 — [@CjoveKing](https://github.com/CjoveKing) · Apr 17, 2026

`ID: 4269587520`


> 是的，这是一个已知的问题，Remotecall在最新版本上有点坏了。对此很抱歉，它很快就会修好。

Okay, I'm glad I found a problem, and I'm also sorry that I didn't make my subjective thoughts clear.


---

### Comment #3 — [@m1337v](https://github.com/m1337v) · Apr 17, 2026

`ID: 4271517966`


Can reproduce the same issue on 18.5. Will have a look as well


---

### Comment #4 — [@tryh4rd3rr](https://github.com/tryh4rd3rr) · Apr 19, 2026

`ID: 4275559148`


same with 16e 26.0.1


---

### Comment #5 — [@rooootdev](https://github.com/rooootdev) · Apr 19, 2026

`ID: 4276902210`


please retry with the latest build.


---

### Comment #6 — [@boierito](https://github.com/boierito) · Apr 20, 2026

`ID: 4276999191`


> please retry with the latest build.

it doesnt work. Just the same as every build. iPhone 16 Pro, iOS 26.


---

### Comment #7 — [@CjoveKing](https://github.com/CjoveKing) · Apr 20, 2026

`ID: 4277232433`


> please retry with the latest build.

Thank you very much. It can be used normally.


---

### Comment #8 — [@rooootdev](https://github.com/rooootdev) · Apr 20, 2026

`ID: 4284115436`


try again now.


---

### Comment #9 — [@CjoveKing](https://github.com/CjoveKing) · Apr 21, 2026

`ID: 4285021143`


It's all do.

发自我的iPhone


------------------ Original ------------------
From: roooot ***@***.***&gt;
Date: Tue,Apr 21,2026 4:37 AM
To: rooootdev/lara ***@***.***&gt;
Cc: CjoveKing ***@***.***&gt;, Comment ***@***.***&gt;
Subject: Re: [rooootdev/lara] [BUG] Remotecall fails (26.0) (Issue #118)



rooootdev left a comment (rooootdev/lara#118)
 
try again now.
 
—
Reply to this email directly, view it on GitHub, or unsubscribe.
You are receiving this because you commented.Message ID: ***@***.***&gt;


---

### Comment #10 — [@rooootdev](https://github.com/rooootdev) · Apr 21, 2026

`ID: 4289251589`


remote call doesn't support A15+ or M1-4. closed as this is a known issue.


---

*Exported on 10/14/2026, 2:55:47 PM from [https://github.com/rooootdev/lara/issues/118](https://github.com/rooootdev/lara/issues/118)*
