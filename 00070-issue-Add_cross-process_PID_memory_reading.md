# Add cross-process PID memory reading

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#70](https://github.com/rooootdev/lara/issues/70)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@rooootdev](https://github.com/rooootdev)  
> **Created:** Apr 11, 2026  
> **Updated:** Apr 17, 2026  
> **Closed:** Apr 16, 2026  

---

## Description

*Opened by [@H9GG](https://github.com/H9GG)* *on Apr 11, 2026*


Great work on this project!
Is it possible to implement cross-process memory reading? I want to read the pid memory of other app processes under jailed iOS.
Thanks a lot!


---

## Timeline (Verbose)

- Apr 16, 2026: 🔴 Closed by @rooootdev


---

## Comments (3)

### Comment #1 — [@Pro42good](https://github.com/Pro42good) · Apr 11, 2026

`ID: 4227189907`


It's most likely Feasible, I'll Put word in to recommend this to the main developer. (And He'll also see this issue created)


---

### Comment #2 — [@rooootdev](https://github.com/rooootdev) · Apr 16, 2026

`ID: 4263151324`


how would you want this feature to be implemented?


---

### Comment #3 — [@H9GG](https://github.com/H9GG) · Apr 17, 2026

`ID: 4266996882`


> 您希望如何實現此功能？
DarkSword leverages DOP socket UAF to obtain kernel physical page r/w primitive, reads process PCB & TTBR0 page table directly, bypass PAC/TPRO hardware protections, then walks MMU page table to get ASLR base without task port.


---

*Exported on 10/14/2026, 2:51:56 PM from [https://github.com/rooootdev/lara/issues/70](https://github.com/rooootdev/lara/issues/70)*
