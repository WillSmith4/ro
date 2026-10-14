# [BUG] Ds crashing on iPhone XR

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#246](https://github.com/rooootdev/lara/issues/246)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@rooootdev](https://github.com/rooootdev)  
> **Labels:** `bug`  
> **Created:** May 14, 2026  
> **Updated:** May 17, 2026  
> **Closed:** May 17, 2026  

---

## Description

*Opened by [@pixelatic-gif](https://github.com/pixelatic-gif)* *on May 14, 2026*


### Describe the bug:
<!-- A clear and concise description of what the bug is. -->
``[Insert here]``

### To Reproduce:
Steps to reproduce the behavior:
1. Go to '...'
2. Tap '...'
3. Scroll down to '...'
4. See error

### Expected behavior:
<!-- What you expected to happen. -->
``[Insert here]``

### Screenshots:
<!-- If applicable, add screenshots to help explain your problem. -->

### Device Info:
- **Device:** ``[IPhone XR]``
- **Chip:** ``[non-a18]``
- **iOS Version:** ``[iOS 18.7.7]``(20F66)
- **Jailbroken before?** ``[no]``
- **Lara version / commit:** ``[version 0.1]``

### Logs:

lara started: 2026-05-14 10:47:03

(utils) TASK_TNEXT_OFFSET: 0x50

(utils) THREAD_MUPCB_OFFSET: 0xb8

(utils) PROC_PID_OFFSET: 0x60

(offs) loaded saved t1sz_boot: 0x19

(offs) t1sz_boot: 0x19

(offs) saved t1sz_boot: 0x19

initialized offsets

(offs) loaded saved t1sz_boot: 0x19

(offs) t1sz_boot: 0x19

(offs) saved t1sz_boot: (CRASH LOG!)
(ds) looking in search mapping: 1 / 8 (12.50%)
(ds) looking in search mapping: 2 / 8 (25.00%)
(ds) looking in search mapping: 3 / 8 (37.50%)
(ds) looking in search mapping: 4 / 8 (50.00%)
(ds) looking in search mapping: 5 / 8 (62.50%)
(ds) looking in search mapping: 6 / 8 (75.00%)
(ds) looking in search mapping: 7 / 8 (87.50%)
(ds) retrying ds...
(ds) spraying 22528 sockets...
(ds) socket_ports_count: 0x5800
(ds) start_pcb_id: 0x6e004
(ds) end_pcb_id: 0x79002
(ds) looking in search mapping: 0 / 8 (0.00%)
(its starts lagging so much and lara crashing with black screen)
```
(logs here)
```lara started: 2026-05-14 10:47:03

(utils) TASK_TNEXT_OFFSET: 0x50

(utils) THREAD_MUPCB_OFFSET: 0xb8

(utils) PROC_PID_OFFSET: 0x60

(offs) loaded saved t1sz_boot: 0x19

(offs) t1sz_boot: 0x19

(offs) saved t1sz_boot: 0x19

initialized offsets

(offs) loaded saved t1sz_boot: 0x19

(offs) t1sz_boot: 0x19

(offs) saved t1sz_boot: 0x19

initialized offsets

(ds) starting darksword
(ds) device: iPhone11,8
(ds) ispac: yes
(ds) running on non-a18 device
(ds) read_fd: 0x9
(ds) write_fd: 0xa
(ds) executable_path: /private/var/containers/Bundle/Application/71D39396-D4B5-4C79-972E-6D3E1277D31F/lara.app/lara
(ds) host_executable_path: /private/var/containers/Bundle/Application/71D39396-D4B5-4C79-972E-6D3E1277D31F/lara.app/lara
(ds) guest_executable_name: lara
(ds) host_executable_name: lara
(ds) kernel_process_name: lara
(ds) livecontainer_bundle: no
(ds) livecontainer_guest: no
(ds) rehosted_process: no
(ds) process_marker[0]: lara
(ds) executable_name: lara
(ds) free_thread_arg: 0x10f400000
(ds) physical_mapping_address: 0x10f214000
(ds) pc_object: 0x8503
(ds) pc_address: 0x34a47c000
(ds) spraying 22528 sockets...
(ds) socket_ports_count: 0x5800
(ds) start_pcb_id: 0x58004
(ds) end_pcb_id: 0x63002
(ds) looking in search mapping: 0 / 8 (0.00%)
(ds) looking in search mapping: 1 / 8 (12.50%)
(ds) looking in search mapping: 2 / 8 (25.00%)
(ds) looking in search mapping: 3 / 8 (37.50%)
(ds) looking in search mapping: 4 / 8 (50.00%)
(ds) looking in search mapping: 5 / 8 (62.50%)
(ds) looking in search mapping: 6 / 8 (75.00%)
(ds) looking in search mapping: 7 / 8 (87.50%)
(ds) retrying ds...
(ds) spraying 22528 sockets...
(ds) socket_ports_count: 0x5800
(ds) start_pcb_id: 0x6e004
(ds) end_pcb_id: 0x79002
(ds) looking in search mapping: 0 / 8 (0.00%)```

### Additional context:
<!-- Anything else — recent iOS updates, tweaks installed, prior jailbreak state, etc. -->
``[no tweaks, 18.7.9 (im not updated ) no jailbreak]``

### Pre-submission checklist:
<!-- Please confirm all of the following before submitting. Issues missing these may be closed. -->
- [x] I have searched existing issues (open and closed) and confirmed this is not a duplicate
- [x] I can reproduce this on the latest version / commit of Lara
- [x] I have provided sufficient detail (device info, logs, reproduction steps) for a maintainer to investigate
- [x] I agree to communicate respectfully with the developers and understand that harassment may result in restricted support
<!-- To Mark a check Put an "x" inside the brackets like so [x] -->


---

## Timeline (Verbose)

- May 14, 2026: 🏷️ @pixelatic-gif added label `bug`
- May 14, 2026: ✏️ @pixelatic-gif renamed: "[BUG] Insert Title Here" → "[BUG] Ds crashing on iPhone XR"
- May 17, 2026: 🔴 Closed by @rooootdev


---

## Comments (2)

### Comment #1 — [@pixelatic-gif](https://github.com/pixelatic-gif) · May 14, 2026

`ID: 4448032139`


bug:crashing when retrying ds, and when im trying to restart its going to search mapping [7] and lagg


---

### Comment #2 — [@A14xp](https://github.com/A14xp) · May 15, 2026

`ID: 4461601693`


Darksword is patched on IOS 18.7.7


---

*Exported on 10/14/2026, 3:06:55 PM from [https://github.com/rooootdev/lara/issues/246](https://github.com/rooootdev/lara/issues/246)*
