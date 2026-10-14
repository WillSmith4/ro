# [BUG] Respring crashes

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#316](https://github.com/rooootdev/lara/issues/316)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@lewis0917](https://github.com/lewis0917)  
> **Labels:** `bug`  
> **Created:** Aug 12, 2026  
> **Updated:** Aug 20, 2026  
> **Closed:** Aug 20, 2026  

---

## Description

*Opened by [@lewis0917](https://github.com/lewis0917)* *on Aug 12, 2026*


### Describe the bug:
<!-- Applying a tweak such as ‘hide/disable home bar’ and respringing fails to apply as it crashes to Lock Screen. -->
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
- **Device:** ``[iPhone xr]`` <!-- e.g. iPhone 13 Pro, iPad Air 5 -->
- **Chip:** ``[A12]`` <!-- e.g. A15, A14, M1 -->
- **iOS Version:** ``[18.2]`` <!-- e.g. 16.5 (20F66) — include the build number -->
- **Jailbroken before?** ``[Yes]`` <!-- Yes / No / First attempt on this device -->
- **Lara version / commit:** ``[0.2]`` <!-- e.g. v0.3 or commit hash -->

### Logs:
<!-- Paste crash logs, console output, or kernel panic text. Use ``` fences. -->
```
(logs here)
```

### Additional context:
<!-- Anything else — recent iOS updates, tweaks installed, prior jailbreak state, etc. -->
``[It worked just once or maybe twice. Seems to conflict with being in a jailbroken state — especially at initial fetch kernel cache stage when it would instantly reboot. Still seldom applies tweaks either jailbroken or jailed.]``

### Pre-submission checklist:
<!-- Please confirm all of the following before submitting. Issues missing these may be closed. -->
- [x] I have searched existing issues (open and closed) and confirmed this is not a duplicate
- [x] I can reproduce this on the latest version / commit of Lara
- [x] I have provided sufficient detail (device info, logs, reproduction steps) for a maintainer to investigate
- [x] I agree to communicate respectfully with the developers and understand that harassment may result in restricted support
<!-- To Mark a check Put an "x" inside the brackets like so [x] -->


---

## Timeline (Verbose)

- Aug 12, 2026: 🏷️ @lewis0917 added label `bug`
- Aug 20, 2026: 🔴 Closed by @lewis0917


---

## Comments (1)

### Comment #1 — [@lewis0917](https://github.com/lewis0917) · Aug 20, 2026

`ID: 5361515128`


To backtrack on this, I got it working with jailbreak. First device must be jailed when fetching kernel on Lara. Once that’s done, it should work when jailbroken. 

As for the respring crash, that’s still occurring. But I this time I respringed via Dopamine after setting my Lara tweaks and it worked. Hit apply tweaks on Lara. Switch to Dopamine app and respring from there.


---

*Exported on 10/14/2026, 3:12:47 PM from [https://github.com/rooootdev/lara/issues/316](https://github.com/rooootdev/lara/issues/316)*
