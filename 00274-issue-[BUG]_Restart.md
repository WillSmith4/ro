# [BUG] Restart

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#274](https://github.com/rooootdev/lara/issues/274)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@TYXH2024](https://github.com/TYXH2024)  
> **Labels:** `bug`  
> **Created:** May 28, 2026  
> **Updated:** May 28, 2026  
> **Closed:** May 28, 2026  

---

## Description

*Opened by [@TYXH2024](https://github.com/TYXH2024)* *on May 28, 2026*


No matter what T1SZ_BOOT is changed, it shows that 0x11 is used in the log.
Using the latest version 0.2, try more than 20 times before and after. Each time the progress bar is loaded to %60 or lower, it will restart.
#2c1bb60 was the last version where both Run Exploit and Remote Call worked normally.
#425 was the last version where Run Exploit worked normally.
In all versions after these, using Run Exploit causes an immediate reboot.

[lara.log](https://github.com/user-attachments/files/28334573/lara.log)
[panic.txt](https://github.com/user-attachments/files/28334574/panic.txt)


---

## Timeline (Verbose)

- May 28, 2026: 🏷️ @TYXH2024 added label `bug`
- May 28, 2026: 🔴 Closed by @TYXH2024


---

*Exported on 10/14/2026, 3:09:13 PM from [https://github.com/rooootdev/lara/issues/274](https://github.com/rooootdev/lara/issues/274)*
