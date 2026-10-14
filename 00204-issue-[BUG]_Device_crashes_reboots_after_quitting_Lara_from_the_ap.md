# [BUG] Device crashes/reboots after quitting Lara from the app switcher

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#204](https://github.com/rooootdev/lara/issues/204)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@dookcss](https://github.com/dookcss)  
> **Labels:** `bug`  
> **Assignees:** [@Pro42good](https://github.com/Pro42good)  
> **Created:** Apr 27, 2026  
> **Updated:** Sep 27, 2026  
> **Closed:** Apr 30, 2026  

---

## Description

*Opened by [@dookcss](https://github.com/dookcss)* *on Apr 27, 2026*


### Describe the bug:
After using the exploit, quitting Lara from the background/app switcher triggers a crash and causes the device to reboot.

### To Reproduce:
Steps to reproduce the behavior:
1. Run the exploit.
2. Use a feature, for example bypassing the 3-app sideloading limit.
3. Exit Lara and swipe away the app card from the app switcher.

### Expected behavior:
The app should exit normally without triggering a crash or rebooting the device.

### Screenshots:
Not applicable.

### Device Info:
- **Device:** iPhone 14 Pro Max
- **Chip:** A16
- **iOS Version:** 17.4
- **Jailbroken before?** no
- **Lara version / commit:** 1.2

### Logs:
Unable to provide logs because the crash/reboot prevents collecting them.

### Additional context:
The crash/reboot happens after the app is swiped away from the app switcher following exploit usage.

### Pre-submission checklist:
- [x] I have searched existing issues (open and closed) and confirmed this is not a duplicate
- [x] I can reproduce this on the latest version / commit of Lara
- [x] I have provided sufficient detail (device info, logs, reproduction steps) for a maintainer to investigate
- [x] I agree to communicate respectfully with the developers and understand that harassment may result in restricted support


---

## Timeline (Verbose)

- Apr 27, 2026: 👤 @dookcss assigned @Pro42good
- Apr 27, 2026: 🏷️ @dookcss added label `bug`
- Apr 30, 2026: 🔴 Closed by @dookcss


---

## Comments (3)

### Comment #1 — [@TheDiamondSquidy](https://github.com/TheDiamondSquidy) · Apr 28, 2026

`ID: 4334549832`


Happens to me on ios 17.6.1, iphone 13. Did not happen before


---

### Comment #2 — [@hxhlb](https://github.com/hxhlb) · Apr 30, 2026

`ID: 4349003617`


<img width="629" height="295" alt="Image" src="https://github.com/user-attachments/assets/12db232a-c0a4-4ee5-a6d6-c8bdb78e0fce" />


---

### Comment #3 — [@buzz-lightsnack-2007](https://github.com/buzz-lightsnack-2007) · Sep 26, 2026

`ID: 5847097331`


It seems that by allowing the app to crash itself by using the Crasher (to crash the "Lara" process), it can then be safely closed from the app switcher on affected iOS versions. Maybe there could be a button to automate this crash on those versions?

Also, having RemoteCall active when crashing the process seems to guarantee a proper respring instead of a kernel panic, at least on iOS 17.6.1.


---

*Exported on 10/14/2026, 3:03:26 PM from [https://github.com/rooootdev/lara/issues/204](https://github.com/rooootdev/lara/issues/204)*
