# [BUG] Purple flash reboot on iPadOS18.7.1

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#196](https://github.com/rooootdev/lara/issues/196)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@jfri102](https://github.com/jfri102)  
> **Labels:** `bug`, `sufficient info`  
> **Assignees:** [@Pro42good](https://github.com/Pro42good)  
> **Created:** Apr 26, 2026  
> **Updated:** May 13, 2026  
> **Closed:** May 13, 2026  

---

## Description

*Opened by [@jfri102](https://github.com/jfri102)* *on Apr 26, 2026*


### Describe the bug:
<!-- A clear and concise description of what the bug is. -->
``System reboots on iPadOS 18.7.1 after running exploit.
Tried to re-download kernelcache, but didn't work.``

### To Reproduce:
Steps to reproduce the behavior:
1. Go to 'lara'
2. Tap 'Run exploit'
3. Wait a few seconds.
4. See error.

### Expected behavior:
<!-- What you expected to happen. -->
``The progress stuck at 30% and soon the System shutdowns after a purple flash.``

### Screenshots:
<!-- If applicable, add screenshots to help explain your problem. -->
Sorry, the iPad shuts down after running exploit, so there's no way to obtain one.

### Device Info:
- **Device:** ``iPad Pro 3`` <!-- e.g. iPhone 13 Pro, iPad Air 5 -->
- **Chip:** ``M1`` <!-- e.g. A15, A14, M1 -->
- **iOS Version:** ``18.7.1 (22H31)`` <!-- e.g. 16.5 (20F66) — include the build number -->
- **Jailbroken before?** ``No`` <!-- Yes / No / First attempt on this device -->
- **Lara version / commit:** ``1.2`` <!-- e.g. v0.3 or commit hash -->

### Logs:
<!-- Paste crash logs, console output, or kernel panic text. Use ``` fences. -->
```
Too long.

https://pastebin.com/L1XChEiP

```

### Additional context:
<!-- Anything else — recent iOS updates, tweaks installed, prior jailbreak state, etc. -->
``None``

### Pre-submission checklist:
<!-- Please confirm all of the following before submitting. Issues missing these may be closed. -->
- [ x] I have searched existing issues (open and closed) and confirmed this is not a duplicate
- [x ] I can reproduce this on the latest version / commit of Lara
- [ x] I have provided sufficient detail (device info, logs, reproduction steps) for a maintainer to investigate
- [x ] I agree to communicate respectfully with the developers and understand that harassment may result in restricted support
<!-- To Mark a check Put an "x" inside the brackets like so [x] -->


---

## Timeline (Verbose)

- Apr 26, 2026: 👤 @jfri102 assigned @Pro42good
- Apr 26, 2026: 🏷️ @jfri102 added label `bug`
- Apr 26, 2026: ✏️ @jfri102 renamed: "[BUG] Insert Title Here" → "[BUG] Purple flash reboot on iPadOS18.7.1"
- May 4, 2026: 🏷️ @Pro42good added label `sufficient info`
- May 13, 2026: 🔴 Closed by @jfri102


---

## Comments (3)

### Comment #1 — [@BlueOutside](https://github.com/BlueOutside) · Apr 27, 2026

`ID: 4330575378`


Same thing happening on an iPad Air M3 on iPadOS 18.6 (22G86)


---

### Comment #2 — [@BlueOutside](https://github.com/BlueOutside) · May 1, 2026

`ID: 4360315363`


Managed to capture Lara's logs right before the reboot

<img width="1080" height="1920" alt="Image" src="https://github.com/user-attachments/assets/43c9ce42-7508-449a-bb5b-5cccdcb0f697" />


---

### Comment #3 — [@lowiro102-dotcom](https://github.com/lowiro102-dotcom) · May 5, 2026

`ID: 4376400958`


Sorry for not reading the README, and I didn't set the correct t1sz_boot value.

<img width="1170" height="120" alt="Image" src="https://github.com/user-attachments/assets/05cdd8c5-6319-454d-92c7-78bc57a99536" />

Sorry again for bothering🙏


---

*Exported on 10/14/2026, 3:02:36 PM from [https://github.com/rooootdev/lara/issues/196](https://github.com/rooootdev/lara/issues/196)*
