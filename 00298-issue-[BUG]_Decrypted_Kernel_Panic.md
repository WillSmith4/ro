# [BUG] Decrypted Kernel Panic

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#298](https://github.com/rooootdev/lara/issues/298)  
> **State:** 🟢 open  
> **Labels:** `bug`  
> **Created:** Jun 17, 2026  
> **Updated:** Jun 18, 2026  

---

## Description

*Opened by [@TYXH2024](https://github.com/TYXH2024)* *on Jun 17, 2026*


Device：iPadAirM3
System：18.3.2
Lara：Latest Nightly、Latest Actions(github.com/rooootdev/lara/actions/runs/27710052926)
After successfully run exploit, it restarts when decrypting the app. It only takes one or two seconds from the beginning of decryption to restart.

[panic-full-2026-06-18-023042.0002.txt](https://github.com/user-attachments/files/29061758/panic-full-2026-06-18-023042.0002.txt)

[lara.log](https://github.com/user-attachments/files/29061733/lara.log)


---

## Timeline (Verbose)

- Jun 17, 2026: 🏷️ @TYXH2024 added label `bug`


---

## Comments (2)

### Comment #1 — [@neonmodder123](https://github.com/neonmodder123) · Jun 18, 2026

`ID: 4737396004`


@TYXH2024 Could you please go to Lara's settings, click Remove Kernelcache, then Fetch Kernelcache and try again?


---

### Comment #2 — [@TYXH2024](https://github.com/TYXH2024) · Jun 18, 2026

`ID: 4738557774`


> [@TYXH2024](https://github.com/TYXH2024) Could you please go to Lara's settings, click Remove Kernelcache, then Fetch Kernelcache and try again?

@neonmodder123 After trying again, the fault situation is still the same.


---

*Exported on 10/14/2026, 3:11:31 PM from [https://github.com/rooootdev/lara/issues/298](https://github.com/rooootdev/lara/issues/298)*
