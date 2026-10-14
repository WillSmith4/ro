# [BUG] Incompatible with 17.0.3 on A14 (iPhone 12)

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#254](https://github.com/rooootdev/lara/issues/254)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@eris-webserv](https://github.com/eris-webserv)  
> **Labels:** `bug`  
> **Created:** May 19, 2026  
> **Updated:** May 31, 2026  
> **Closed:** May 31, 2026  

---

## Description

*Opened by [@eris-webserv](https://github.com/eris-webserv)* *on May 19, 2026*


### Describe the bug:
<!-- A clear and concise description of what the bug is. -->
Lara fails to exploit DarkSword on my iPhone 12, specifically during the search mapping phase.

### To Reproduce:
Steps to reproduce the behavior:
1. Hit Run Exploit
2. Watch the logs
3. Eternal `looking in search mapping: 1-7/8` damnation

### Expected behavior:
1. Hit Run Exploit
2. Exploit finishes
3. I can FINALLY ENABLE JIT ON MY APPS

### Screenshots:
What am I supposed to screenshot, the logs of it floundering?

### Device Info:
- **Device:**  iPhone 12
- **Chip:** A14
- **iOS Version:** 17.0.3 (21A360) <!-- e.g. 16.5 (20F66) — include the build number -->
- **Jailbroken before?** No (waiting for Dopamine) <!-- Yes / No / First attempt on this device -->
- **Lara version / commit:** Nightly as of 5/18/2026 (not exactly sure the commit ID since I don't have enough internet to check the commit logs)

### Logs:
<!-- Paste crash logs, console output, or kernel panic text. Use ``` fences. -->
```
lara started: 2026-05-19 10:58:28
(utils) darksword not ready
(rc) Unable to find process: youtube
(utils) T1SZ_BOOT: 0x19
(utils) TASK_TNEXT_OFFSET: 0x58
(utils) THREAD_MUPCB_OFFSET: 0xb0
(utils) PROC_PID_OFFSET: 0x28
(offs) (user-set) t1sz_boot = 0x19 (default: 0x0)
(offs) loaded user-saved offsets above
(offs) initialized offsets
(offs) kernel: Darwin Kernel Version 23.0.0: Sat Sep 30 17:17:13 PDT 2023; root:xnu-10002.2.13~1/RELEASE_ARM64_T8101
(offs) kernbase: 0xfffffff007004000
(offs) kernentry: 0xfffffff007d20540
(offs) allproc: 0x32c7078
(offs) kernproc: 0x9277b0
(offs) rootvnode: 0x32c7670
(offs) procsize: 0x730
(offs) t1szboot: 0x19
(offs) initialized offsets
(ds) (progress update)
(ds) starting darksword
(ds) (progress update)
(ds) device: iPhone13,2
(ds) ispac: yes
(ds) running on non-a18 device
(ds) read_fd: 0xa
(ds) write_fd: 0xb
(ds) executable_path:       /private/var/containers/Bundle/Application/0BFE391F-F20A-4BD5-9B4A-E87BCA1CC376/lara.app/lara
(ds) host_executable_path:  /private/var/containers/Bundle/Application/0BFE391F-F20A-4BD5-9B4A-E87BCA1CC376/lara.app/lara
(ds) guest_executable_name: lara
(ds) host_executable_name:  lara
(ds) kernel_process_name:   lara
(ds) livecontainer_bundle:  no
(ds) livecontainer_guest:   no
(ds) rehosted_process:      no
(ds) process_marker 0: lara
(ds) executable_name: lara
(ds) free_thread_arg: 0x112418000
(ds) (progress update)
(ds) physical_mapping_address: 0x112430000
(ds) pc_object: 0x6b0b
(ds) pc_address: 0x52732c000
(ds) spraying 22528 sockets...
(ds) (progress update)
(ds) (progress update)
(ds) (progress update)
(ds) (progress update)
(ds) (progress update)
(ds) (progress update)
(ds) (progress update)
(ds) (progress update)
(ds) (progress update)
(ds) (progress update)
(ds) (progress update)
(ds) (progress update)
(ds) (progress update)
(ds) (progress update)
(ds) (progress update)
(ds) (progress update)
(ds) (progress update)
(ds) (progress update)
(ds) (progress update)
(ds) (progress update)
(ds) (progress update)
(ds) (progress update)
(ds) socket_ports_count: 0x5800
(ds) start_pcb_id: 0x84020
(ds) end_pcb_id: 0x8f01e
(ds) looking in search mapping: 0 / 8
(ds) (progress update)
(ds) (progress update)
(ds) (progress update)
(ds) (progress update)
(ds) (progress update)
(ds) looking in search mapping: 1 / 8
(ds) looking in search mapping: 2 / 8
(ds) looking in search mapping: 3 / 8
(ds) looking in search mapping: 4 / 8
(ds) looking in search mapping: 5 / 8
(ds) looking in search mapping: 6 / 8
(ds) looking in search mapping: 7 / 8
(ds) retrying ds...
(ds) spraying 22528 sockets...
(ds) socket_ports_count: 0x5800
(ds) start_pcb_id: 0x9a020
(ds) end_pcb_id: 0xa501e
(ds) looking in search mapping: 0 / 8
(It just keeps looping through looking in the search mapping from here)
```

### Additional context:
This device has been through the ringer of random things done to it, I have no idea if that would affect the exploit but here's a small list:
- Mobilegestalt tweaks done via Nugget
- Usage of the Coruna website (34306.lol) [i'm sorry i was impatient forgive me]

### Pre-submission checklist:
<!-- Please confirm all of the following before submitting. Issues missing these may be closed. -->
- [X] I have searched existing issues (open and closed) and confirmed this is not a duplicate
- [X] I can reproduce this on the latest version / commit of Lara
- [X] I have provided sufficient detail (device info, logs, reproduction steps) for a maintainer to investigate
- [X] I agree to communicate respectfully with the developers and understand that harassment may result in restricted support
- [X] I agree to sell my soul to rooootdev in the event that lara works on my device after a fix

### Anything else?
Would it help if I attached the kernelcache file I used?


---

## Timeline (Verbose)

- May 19, 2026: 🏷️ @eris-webserv added label `bug`
- May 31, 2026: 🔴 Closed by @eris-webserv


---

## Comments (1)

### Comment #1 — [@eris-webserv](https://github.com/eris-webserv) · May 31, 2026

`ID: 4585491674`


This issue appears to have resolved itself.


---

*Exported on 10/14/2026, 3:07:37 PM from [https://github.com/rooootdev/lara/issues/254](https://github.com/rooootdev/lara/issues/254)*
