# [BUG] 06/05/26 Nightly App Crash iOS 17.1

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#241](https://github.com/rooootdev/lara/issues/241)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@seba4316](https://github.com/seba4316)  
> **Labels:** `bug`  
> **Created:** May 10, 2026  
> **Updated:** May 11, 2026  
> **Closed:** May 11, 2026  

---

## Description

*Opened by [@seba4316](https://github.com/seba4316)* *on May 10, 2026*


### Describe the bug:
``When running the exploit in any mode the spinner reaches 50% and then the app crashes``

### To Reproduce:
Steps to reproduce the behavior:
1. Go to the main tab
2. Tap 'Run Exploit'
3. Wait for completion
4. See progress get stuck at 50% for 1-3 seconds
5. The application crashes

### Expected behavior:
``The exploit should complete like it does on release 0.1``

### Screenshots:
<!-- If applicable, add screenshots to help explain your problem. -->

### Device Info:
- **Device:** ``iPhone 12 Pro Max``
- **Chip:** ``A14``
- **iOS Version:** ``17.1 (21B74)``
- **Jailbroken before?** ``Yes``
- **Lara version / commit:** ``Nightly (7fde60a)``

### Logs:
Last console logs before the crash
```

(offs) alright, t1sz_boot is still 0x0, every man for himself!(offs) initialized offsets
(ds) starting darksword
(ds) device: iPhone13,4
(ds) ispac: yes
(ds) running on non-a18 device
(ds) read_fd: Oxa
(ds) write_fd: Oxb
(ds) executable_path: /private/var/ containers/Bundle/Application/ C9B0E993-5AFF-40CA-93AC-3A7C451FCF4C/
App. app/lara
(ds) host_executable_path: /private/var/ containers/Bundle/Application/ C9B0E993-5AFF-40CA-93AC-3A7C451FCF4C/
App. app/lara
(ds) guest_executable_name: lara
(ds) host_executable_name: lara (ds) kernel_process_name: lara
(ds) livecontainer_bundle: no
(ds) livecontainer_guest: no
(ds) rehosted_process: no
(ds) process_marker[01: lara
(ds) executable_name: lara (ds) free_thread.
Larg: 0x116090000
(ds) physical_mapping_address: 0x1160ac000
(ds) pc_object: 0x9c07
(ds) pc_address: Oxafb320000
(ds) spraying 22528 sockets...
(ds) socket_ports_count: 0x5800
(ds) start_pob.
_id: 0x9a4a0
(ds) end
_pob_id: 0xa549e
(ds) looking in search mapping: 0 / 8
```

### Additional context:
``It worked fine on release 0.1 and prior prerelease versions. This phone has previously been jailbroken with unc0ver on iOS 14.1. I have tried deleting and redownloading kernelcache``

### Pre-submission checklist:
- [x] I have searched existing issues (open and closed) and confirmed this is not a duplicate
- [x] I can reproduce this on the latest version / commit of Lara
- [x] I have provided sufficient detail (device info, logs, reproduction steps) for a maintainer to investigate
- [x] I agree to communicate respectfully with the developers and understand that harassment may result in restricted support


---

## Timeline (Verbose)

- May 10, 2026: 🏷️ @seba4316 added label `bug`
- May 11, 2026: 🔴 Closed by @seba4316


---

## Comments (1)

### Comment #1 — [@seba4316](https://github.com/seba4316) · May 11, 2026

`ID: 4425478038`


Can confirm that this issue seems to have been resolved on the latest nightly build, perhaps by commit 9455d26242c0437f32716666abfc4334968b8a46?


---

*Exported on 10/14/2026, 3:06:26 PM from [https://github.com/rooootdev/lara/issues/241](https://github.com/rooootdev/lara/issues/241)*
