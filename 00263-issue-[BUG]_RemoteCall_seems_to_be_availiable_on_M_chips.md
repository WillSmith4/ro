# [BUG] RemoteCall seems to be availiable on M chips

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#263](https://github.com/rooootdev/lara/issues/263)  
> **State:** 🟢 open  
> **Labels:** `bug`  
> **Created:** May 23, 2026  
> **Updated:** May 24, 2026  

---

## Description

*Opened by [@henryyuyhy](https://github.com/henryyuyhy)* *on May 23, 2026*


### Describe the bug:
<!-- A clear and concise description of what the bug is. -->
It's not about the app but about issue #118
You closed the issue with "remote call doesn't support A15+ or M1-4. closed as this is a known issue.", but i succeeded to initialize and use remotecall on my ipad pro(gen 4), with M2 chip.
Actually most of my attempts failed but it succeeded when i try immediately after i reboot the device (two times in a row)
i'm sure it's functionable because i successfully disabled ota updates

<img width="1119" height="1080" alt="Image" src="https://github.com/user-attachments/assets/d82d2dc3-b3cd-4c44-b188-a5a8b8b24ede" />

### To Reproduce:
Steps to reproduce the behavior:
1. get a ipad pro(gen 4)
2. reboot the device (may not be necessary)
3. open lara
4. init remotecall

### Expected behavior:
<!-- What you expected to happen. -->
Ummm... 
idk

### Screenshots:
<!-- If applicable, add screenshots to help explain your problem. -->

### Device Info:
- **Device:** ``ipad pro (gen 4)`` <!-- e.g. iPhone 13 Pro, iPad Air 5 -->
- **Chip:** ``M2`` <!-- e.g. A15, A14, M1 -->
- **iOS Version:** ``26.0.1 23A355`` <!-- e.g. 16.5 (20F66) — include the build number -->
- **Jailbroken before?** ``no`` <!-- Yes / No / First attempt on this device -->
- **Lara version / commit:** ``a nightly release around May.15th`` <!-- e.g. v0.3 or commit hash -->

### Logs:
<!-- Paste crash logs, console output, or kernel panic text. Use ``` fences. -->

[lara.log](https://github.com/user-attachments/files/28175288/lara.log)



### Additional context:
<!-- Anything else — recent iOS updates, tweaks installed, prior jailbreak state, etc. -->
``[Insert here]``

### Pre-submission checklist:
<!-- Please confirm all of the following before submitting. Issues missing these may be closed. -->
- [x ] I have searched existing issues (open and closed) and confirmed this is not a duplicate
- [x ] I can reproduce this on the latest version / commit of Lara
- [x ] I have provided sufficient detail (device info, logs, reproduction steps) for a maintainer to investigate
- [x ] I agree to communicate respectfully with the developers and understand that harassment may result in restricted support
<!-- To Mark a check Put an "x" inside the brackets like so [x] -->


---

## Timeline (Verbose)

- May 23, 2026: 🏷️ @henryyuyhy added label `bug`


---

## Comments (1)

### Comment #1 — [@henryyuyhy](https://github.com/henryyuyhy) · May 24, 2026

`ID: 4528033852`


rebooting seems not necessary


---

*Exported on 10/14/2026, 3:08:22 PM from [https://github.com/rooootdev/lara/issues/263](https://github.com/rooootdev/lara/issues/263)*
