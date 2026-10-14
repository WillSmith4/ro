# [BUG] The remotecall button just crashes lara

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#149](https://github.com/rooootdev/lara/issues/149)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@rooootdev](https://github.com/rooootdev)  
> **Labels:** `bug`  
> **Assignees:** [@Pro42good](https://github.com/Pro42good)  
> **Created:** Apr 21, 2026  
> **Updated:** Apr 21, 2026  
> **Closed:** Apr 21, 2026  

---

## Description

*Opened by [@Nnnnnnn274](https://github.com/Nnnnnnn274)* *on Apr 21, 2026*


### Describe the bug:
<!-- A clear and concise description of what the bug is. -->
``[Insert here]``

### To Reproduce:
Steps to reproduce the behavior:
1. Go to 'remotecall'
2. Tap 'remotecall'
3. Scroll down to '...'
4. See error

### Expected behavior:
<!-- What you expected to happen. -->
``[Crash]``

### Screenshots:
<!-- If applicable, add screenshots to help explain your problem. -->

### Device Info:
- **Device:** ``Iphone 16
- **Chip:** `a18
- **iOS Version:** ``ios 18.5`` how do i get a build number...
- **Jailbroken before?**
- lara commit > the newest

### Logs:
<!-- Paste crash logs, console output, or kernel panic text. Use ``` fences. -->
```
(logs here)no logs
```

Just saying i have am on a 18 for some reason the t1st boot is on 0x19 is it ok 

### Pre-submission checklist:
<!-- Please confirm all of the following before submitting. Issues missing these may be closed. -->
- [x] I have searched existing issues (open and closed) and confirmed this is not a duplicate
- [x] I can reproduce this on the latest version / commit of Lara
- [x] I have provided sufficient detail (device info, logs, reproduction steps) for a maintainer to investigate
- [x] I agree to communicate respectfully with the developers and understand that harassment may result in restricted support
<!-- To Mark a check Put an "x" inside the brackets like so [x] -->


---

## Timeline (Verbose)

- Apr 21, 2026: 👤 @Nnnnnnn274 assigned @Pro42good
- Apr 21, 2026: 🏷️ @Nnnnnnn274 added label `bug`
- Apr 21, 2026: 🔴 Closed by @rooootdev


---

## Comments (7)

### Comment #1 — [@Nnnnnnn274](https://github.com/Nnnnnnn274) · Apr 21, 2026

`ID: 4286935444`


and it worked before the compatability update with m ipads


---

### Comment #2 — [@Nnnnnnn274](https://github.com/Nnnnnnn274) · Apr 21, 2026

`ID: 4286935960`


pefectly


---

### Comment #3 — [@verbes4](https://github.com/verbes4) · Apr 21, 2026

`ID: 4287702021`


same here on iphone 15 pro max, 18.5 (22F76), only started happening after the update


---

### Comment #4 — [@tiziodied](https://github.com/tiziodied) · Apr 21, 2026

`ID: 4287734930`


first make sure you have the newest version, then go into settings and scroll down until you see T1SZ_BOOT, if the value is 0x11 change it to 0x19, if it's 0x19 change it to 0x11


👍 2


---

### Comment #5 — [@verbes4](https://github.com/verbes4) · Apr 21, 2026

`ID: 4287753120`


> first make sure you have the newest version, then go into settings and scroll down until you see T1SZ_BOOT, if the value is 0x11 change it to 0x19, if it's 0x19 change it to 0x11

thanks, works perfectly again, i had no idea the offsets could be manually set like that lol


---

### Comment #6 — [@doraorak](https://github.com/doraorak) · Apr 21, 2026

`ID: 4289141426`


> first make sure you have the newest version, then go into settings and scroll down until you see T1SZ_BOOT, if the value is 0x11 change it to 0x19, if it's 0x19 change it to 0x11

0x19 works on a16 14pm 26.0


---

### Comment #7 — [@rooootdev](https://github.com/rooootdev) · Apr 21, 2026

`ID: 4289446665`


> first make sure you have the newest version, then go into settings and scroll down until you see T1SZ_BOOT, if the value is 0x11 change it to 0x19, if it's 0x19 change it to 0x11

use this temp fix.


---

*Exported on 10/14/2026, 2:58:21 PM from [https://github.com/rooootdev/lara/issues/149](https://github.com/rooootdev/lara/issues/149)*
