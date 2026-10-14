# [Feature] Enable EU marketplaces

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#127](https://github.com/rooootdev/lara/issues/127)  
> **State:** 🟢 open  
> **Labels:** `enhancement`, `sufficient info`  
> **Assignees:** [@Pro42good](https://github.com/Pro42good)  
> **Created:** Apr 19, 2026  
> **Updated:** Apr 20, 2026  

---

## Description

*Opened by [@isacucho](https://github.com/isacucho)* *on Apr 19, 2026*


Hey!

I’ve noticed Huy figured out how to turn read-only paths to mobile-owned to allow modifications on FilzaJailedDS. I was wondering if this would allow something like EUEnabler or MarketplaceEnabler to work, since IIRC it’s just modifying a couple of protected files. 

Is this within the scope of this project? Could it be possible to enable the installation of AltStore PAL, Epic Games Store, etc.?

Thanks!


---

## Timeline (Verbose)

- Apr 19, 2026: 👤 @isacucho assigned @Pro42good
- Apr 19, 2026: 🏷️ @isacucho added label `enhancement`
- Apr 20, 2026: 🏷️ @Pro42good added label `sufficient info`


---

## Comments (2)

### Comment #1 — [@rooootdev](https://github.com/rooootdev) · Apr 19, 2026

`ID: 4276905977`


possibly. if someone can give me a confirmed list of files to be modified for an eu enabler, I will add it to lara.


---

### Comment #2 — [@isacucho](https://github.com/isacucho) · Apr 20, 2026

`ID: 4276912392`


EUEnabler modified the files on /var/db/os_elegibility and another path I can’t exactly remember at the moment, but apparently that doesn’t work with iOS 18. 

Marketplace Enabler hooks some system functions to always return elegible. Is this possible to do with Lara? https://github.com/ichitaso/marketplaceenabler/

Edit: I think it’s this file that does the job: https://github.com/ichitaso/MarketplaceEnabler/blob/main/BypassMarketplace.x


---

*Exported on 10/14/2026, 2:56:34 PM from [https://github.com/rooootdev/lara/issues/127](https://github.com/rooootdev/lara/issues/127)*
