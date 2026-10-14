# Is it possible to have r/w on files owned by root?

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#49](https://github.com/rooootdev/lara/issues/49)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@rooootdev](https://github.com/rooootdev)  
> **Created:** Apr 7, 2026  
> **Updated:** Apr 7, 2026  
> **Closed:** Apr 7, 2026  

---

## Description

*Opened by [@isacucho](https://github.com/isacucho)* *on Apr 7, 2026*


According to my testing, we have r/w on mobile-owned paths but on those owned by root, we can only read. Is writing to root-owned paths in the scope of Darksword, or is there no way to write there with this exploit chain?


---

## Timeline (Verbose)

- Apr 7, 2026: 🔴 Closed by @rooootdev


---

## Comments (2)

### Comment #1 — [@Pro42good](https://github.com/Pro42good) · Apr 7, 2026

`ID: 4195550969`


There's no way to, these paths are protected with things such as PPL and SPTM at the hardware level marking them read only, so for the time being it's not possible. (until a ppl bypass)


---

### Comment #2 — [@rooootdev](https://github.com/rooootdev) · Apr 7, 2026

`ID: 4196805971`


Not quite, the reason why we can’t write to them is thatcher are, well, owned by root. Standard Unix ownership applies, even if escape the sandbox, we still can’t write to files owned by root


---

*Exported on 10/14/2026, 2:50:04 PM from [https://github.com/rooootdev/lara/issues/49](https://github.com/rooootdev/lara/issues/49)*
