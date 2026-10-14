# /var/wireless/Library/Preferences

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#310](https://github.com/rooootdev/lara/issues/310)  
> **State:** 🟢 open  
> **Labels:** `bug`  
> **Created:** Jul 5, 2026  
> **Updated:** Jul 6, 2026  

---

## Description

*Opened by [@fateh660](https://github.com/fateh660)* *on Jul 5, 2026*


/var/wireless/Library/Preferences
lacks writing permission


---

## Timeline (Verbose)

- Jul 5, 2026: 🏷️ @fateh660 added label `bug`


---

## Comments (4)

### Comment #1 — [@neonmodder123](https://github.com/neonmodder123) · Jul 5, 2026

`ID: 4886620345`


Have you tried overwriting the file using the Custom Overwrite feature in Tweaks?


👀 1


---

### Comment #2 — [@fateh660](https://github.com/fateh660) · Jul 5, 2026

`ID: 4886843303`


> Have you tried overwriting the file using the Custom Overwrite feature in Tweaks?

I try it but show some error 

<img width="1170" height="1331" alt="Image" src="https://github.com/user-attachments/assets/2a3c1276-b383-4168-9f95-82efe4628a93" />


---

### Comment #3 — [@fateh660](https://github.com/fateh660) · Jul 5, 2026

`ID: 4886858188`


Maybe file size is not matching


👀 1


---

### Comment #4 — [@neonmodder123](https://github.com/neonmodder123) · Jul 6, 2026

`ID: 4891541434`


Yes, you currently cannot overwrite files with a greater file size than the current size. Ex. if the file you are trying to overwrite is currently 10Kb, and your modified file is 11Kb it will not work. However if your modified file is the exact same size or less, it will work.


---

*Exported on 10/14/2026, 3:12:18 PM from [https://github.com/rooootdev/lara/issues/310](https://github.com/rooootdev/lara/issues/310)*
