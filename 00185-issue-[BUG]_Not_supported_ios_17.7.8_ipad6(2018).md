# [BUG] Not supported ios 17.7.8 ipad6(2018)

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#185](https://github.com/rooootdev/lara/issues/185)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@rooootdev](https://github.com/rooootdev)  
> **Labels:** `bug`  
> **Assignees:** [@Pro42good](https://github.com/Pro42good)  
> **Created:** Apr 24, 2026  
> **Updated:** May 9, 2026  
> **Closed:** May 9, 2026  

---

## Description

*Opened by [@Da8m0n-Vye11in](https://github.com/Da8m0n-Vye11in)* *on Apr 24, 2026*


### Describe the bug:
app developer need update the app so that  it can install on your device.ONLY ON IOS 17.8 IPAD6!!!
### To Reproduce:
Steps to reproduce the behavior:
1. Sign and install lara by Company Cert
2. Tap lara
3. See error

### Expected behavior:
Open it Normally

### Screenshots:![image](https://github.com/user-attachments/assets/fd1746e1-211c-4f7b-b70b-ded4bc160d7d)

### Device Info:
- **Device:** Ipad 6 (2018)
- **Chip:** A10
- **iOS Version:** Ipad OS 17.8
- **Jailbroken before?** No
- **Lara version / commit:** 1.2

### Pre-submission checklist:
<!-- Please confirm all of the following before submitting. Issues missing these may be closed. -->
- [ x] I have searched existing issues (open and closed) and confirmed this is not a duplicate
- [ ] I can reproduce this on the latest version / commit of Lara
- [ ] I have provided sufficient detail (device info, logs, reproduction steps) for a maintainer to investigate
- [ x] I agree to communicate respectfully with the developers and understand that harassment may result in restricted support
<!-- To Mark a check Put an "x" inside the brackets like so [x] -->


---

## Timeline (Verbose)

- Apr 24, 2026: 👤 @Da8m0n-Vye11in assigned @Pro42good
- Apr 24, 2026: 🏷️ @Da8m0n-Vye11in added label `bug`
- Apr 24, 2026: ✏️ @Da8m0n-Vye11in renamed: "[BUG] Not support ios 17.8 ipad6(2018)" → "[BUG] Not supported ios 17.8 ipad6(2018)"
- Apr 24, 2026: ✏️ @Da8m0n-Vye11in renamed: "[BUG] Not supported ios 17.8 ipad6(2018)" → "[BUG] Not supported ios 17.7.8 ipad6(2018)"
- May 9, 2026: 🔴 Closed by @rooootdev


---

## Comments (5)

### Comment #1 — [@Huofeinb](https://github.com/Huofeinb) · Apr 30, 2026

`ID: 4349835002`


it's same in iOS 17.7.9


---

### Comment #2 — [@Pro42good](https://github.com/Pro42good) · May 4, 2026

`ID: 4373330740`


Hello, This is a copy paste response, Im just trying to close issues.

But could you try on the latest official version and or on the latest nightly version of lara?

Please let me know if its fixed it's self or you can close the issue.


---

### Comment #3 — [@Huofeinb](https://github.com/Huofeinb) · May 5, 2026

`ID: 4376464845`


> Hello, This is a copy paste response, Im just trying to close issues.
> 
> But could you try on the latest official version and or on the latest nightly version of lara?
> 
> Please let me know if its fixed it's self or you can close the issue.

i have tried the latest version and the latest nightly version, but either works.


---

### Comment #4 — [@kechuan](https://github.com/kechuan) · May 7, 2026

`ID: 4396629759`


ipad pro 10.5 (a10x) 17.4 same issue.
use iloader will throw 
```unknown error `IncorrectArchitecture (Failed to find matching arch for input file: /var/installd/Library/Caches/com.apple.mobile.installd.staging/temp.8kUfX4/extracted/lara.app/lara)` returned from device```

use livecontainer will throw
```
dlopen(/private/var/mobile/Containers/Data/Application/30C49231-E25D-4DF7-BAC1-80802F14D688/Documents/Applications/com.roooot.lara.app/lara, 0x0109): tried: 
'/private/var/mobile/Containers/Data/Application/30C49231-E25D-4DF7-BAC1-80802F14D688/Documents/Applications/com.roooot.lara.app/lara' (mach-o file, but is an incompatible architecture (have 'arm64e', need 'arm64')), 
'/private/preboot/Cryptexes/OS/private/var/mobile/Containers/Data/Application/30C49231-E25D-4DF7-BAC1-80802F14D688/Documents/Applications/com.roooot.lara.app/lara' (no such file), 
'/private/var/mobile/Containers/Data/Application/30C49231-E25D-4DF7-BAC1-80802F14D688/Documents/Applications/com.roooot.lara.app/lara' (mach-o file, but is an incompatible architecture (have 'arm64e', need 'arm64'))
```

nightly version is the same situlation


---

### Comment #5 — [@rooootdev](https://github.com/rooootdev) · May 9, 2026

`ID: 4412613114`


arm64 is not supported currently.


---

*Exported on 10/14/2026, 3:01:43 PM from [https://github.com/rooootdev/lara/issues/185](https://github.com/rooootdev/lara/issues/185)*
