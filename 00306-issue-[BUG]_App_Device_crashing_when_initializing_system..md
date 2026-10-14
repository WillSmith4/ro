# [BUG] App/Device crashing when initializing system.

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#306](https://github.com/rooootdev/lara/issues/306)  
> **State:** 🟢 open  
> **Labels:** `bug`  
> **Created:** Jun 26, 2026  
> **Updated:** Jul 26, 2026  

---

## Description

*Opened by [@brainmanthe](https://github.com/brainmanthe)* *on Jun 26, 2026*


I successfully run the exploit, upon initializing, app/device crashes.

### To Reproduce:
Steps to reproduce the behavior:
1. Tap 'Run Exploit'
2. Tap 'Fetch KernelCache'
3. Tap 'Initialize VFS' or 'Initialize System'
4. Crashes Immediately

### Expected behavior:
<!-- What you expected to happen. -->
``[A successful initialization process without any crashing]``

### Screenshots:
<!-- If applicable, add screenshots to help explain your problem. --> [Screen recording of the issue ](https://drive.google.com/file/d/1bCZUcQ9JeWDSyy1ChWLQwVhDlna1mfSi/view?usp=drivesdk)

### Device Info:
- **Device:** ``[iPhone 16]`
- **Chip:** ``[A18]``
- **iOS Version:** `iOS 26.0.1 (23A355) 
- **Jailbroken before?** ``[No - First attempt on this device]``
- **Lara version / commit:** ``[v0.2]``

### Logs:
```
lara started: 2026-06-26 08:27:40
(utils) darksword not ready
(rc) Unable to find process: youtube
(utils) T1SZ_BOOT: 0x0
(utils) TASK_TNEXT_OFFSET: 0x50
(utils) THREAD_MUPCB_OFFSET: 0x108
(utils) PROC_PID_OFFSET: 0x60
(offs) initialized offsets
xpf dict failed, continuing without offsets: Set "translation" failed on "kernelSymbol.cpu_ttep" ([src/common.c:53] Failed assert in xpf_find_pmap_bootstrap: pmap_asid_plru_stringAddr)
(offs) kernel: Darwin Kernel Version 25.0.0: Thu Sep 25 15:05:36 PDT 2025; root:xnu-12377.2.9~1/RELEASE_ARM64_T8140
(offs) kernbase: 0xfffffff007004000
(offs) kernentry: 0xfffffff00afb4000
(offs) allproc: 0x42f6a18
(offs) kernproc: 0xca8d80
(offs) rootvnode: 0x42f7090
(offs) procsize: 0x748
(offs) t1szboot: 0x0
```

### Additional context:
<!-- Anything else — recent iOS updates, tweaks installed, prior jailbreak state, etc. -->
``[This is my first time jailbreaking any device, besides using tweaks like KSign. I also tried setting T1SZ_BOOT to 0x11(original value), 0x13 and 0x19, neither work and give the same result.]``

### Pre-submission checklist:
<!-- Please confirm all of the following before submitting. Issues missing these may be closed. -->
- [X] I have searched existing issues (open and closed) and confirmed this is not a duplicate
- [X] I can reproduce this on the latest version / commit of Lara
- [X] I have provided sufficient detail (device info, logs, reproduction steps) for a maintainer to investigate
- [X] I agree to communicate respectfully with the developers and understand that harassment may result in restricted support
<!-- To Mark a check Put an "x" inside the brackets like so [x] -->


---

## Timeline (Verbose)

- Jun 26, 2026: 🏷️ @brainmanthe added label `bug`


---

## Comments (3)

### Comment #1 — [@ahmadmouhand10-oss](https://github.com/ahmadmouhand10-oss) · Jul 1, 2026

`ID: 4849239219`


same problem 
Device: IPHONE 16 PRO MAX (A18)
IOS 26.0


---

### Comment #2 — [@komarxvi](https://github.com/komarxvi) · Jul 20, 2026

`ID: 5018535318`


Turn off Wi-Fi and close all other apps and try again; it should work.


---

### Comment #3 — [@brainmanthe](https://github.com/brainmanthe) · Jul 26, 2026

`ID: 5080992810`


> Turn off Wi-Fi and close all other apps and try again; it should work.

Doesn't work, same issue occurs.


---

*Exported on 10/14/2026, 3:11:59 PM from [https://github.com/rooootdev/lara/issues/306](https://github.com/rooootdev/lara/issues/306)*
