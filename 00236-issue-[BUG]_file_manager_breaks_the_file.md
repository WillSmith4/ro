# [BUG] file manager breaks the file

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#236](https://github.com/rooootdev/lara/issues/236)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@rooootdev](https://github.com/rooootdev)  
> **Labels:** `bug`  
> **Created:** May 8, 2026  
> **Updated:** May 14, 2026  
> **Closed:** May 14, 2026  

---

## Description

*Opened by [@henryyuyhy](https://github.com/henryyuyhy)* *on May 8, 2026*


### Describe the bug:
<!-- A clear and concise description of what the bug is. -->
When i use the file manager in lara to edit a text file (a plots for example), after i save the file, it broke, the file manager views the edited file as a binary and other apps cannot properly read it.

### To Reproduce:
Steps to reproduce the behavior:
1.go to file manager in lara and find any plist file
2.open it
3.tap edit
4.don't change anything
5.tap save
6.re-open the file


### Expected behavior:
<!-- What you expected to happen. -->
``editing the file without breaking it``

### Screenshots:
<!-- If applicable, add screenshots to help explain your problem. -->

### Device Info:
- **Device:** ``ipad pro 2022`` <!-- e.g. iPhone 13 Pro, iPad Air 5 -->
- **Chip:** ``don't know`` <!-- e.g. A15, A14, M1 -->
- **iOS Version:** ``ipados 26.0.1`` <!-- e.g. 16.5 (20F66) — include the build number -->
- **Jailbroken before?** `no`` <!-- Yes / No / First attempt on this device -->
- **Lara version / commit:** ``v0.1`` <!-- e.g. v0.3 or commit hash -->

### Logs:
<!-- Paste crash logs, console output, or kernel panic text. Use ``` fences. -->
```

```

### Additional context:
<!-- Anything else — recent iOS updates, tweaks installed, prior jailbreak state, etc. -->
``[Insert here]``

### Pre-submission checklist:
<!-- Please confirm all of the following before submitting. Issues missing these may be closed. -->
- [x ] I have searched existing issues (open and closed) and confirmed this is not a duplicate
- [x ] I can reproduce this on the latest version / commit of Lara
- [ x] I have provided sufficient detail (device info, logs, reproduction steps) for a maintainer to investigate
- [ x] I agree to communicate respectfully with the developers and understand that harassment may result in restricted support
<!-- To Mark a check Put an "x" inside the brackets like so [x] -->


---

## References

### Commits (1)

- [d09212c](https://github.com/rooootdev/lara/commit/d09212c6f47b59678e73a19080ef998cb93fe9fc) — fix issue #236


---

## Timeline (Verbose)

- May 8, 2026: 🏷️ @henryyuyhy added label `bug`
- May 14, 2026: 🔴 Closed by @rooootdev
- May 14, 2026: 🔗 Commit d09212c — fix issue #236


---

## Comments (1)

### Comment #1 — [@rooootdev](https://github.com/rooootdev) · May 14, 2026

`ID: 4445754614`


should be fixed in latest nightly build.


---

*Exported on 10/14/2026, 3:06:05 PM from [https://github.com/rooootdev/lara/issues/236](https://github.com/rooootdev/lara/issues/236)*
