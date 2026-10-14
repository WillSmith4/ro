# [BUG] Initialise RemoteCall crashes the app

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#183](https://github.com/rooootdev/lara/issues/183)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@rooootdev](https://github.com/rooootdev)  
> **Labels:** `bug`, `need info`  
> **Assignees:** [@Pro42good](https://github.com/Pro42good)  
> **Created:** Apr 23, 2026  
> **Updated:** May 14, 2026  
> **Closed:** May 14, 2026  

---

## Description

*Opened by [@xelahot](https://github.com/xelahot)* *on Apr 23, 2026*


### Describe the bug:
Earlier versions worked fine. Since a couple of merges, it crashes


### Device Info:
- **Device:** iPhone 15 Pro Max
- **iOS Version:** 18.3.2
- **Lara version / commit:** 1.2


---

## Timeline (Verbose)

- Apr 23, 2026: 👤 @xelahot assigned @Pro42good
- Apr 23, 2026: 🏷️ @xelahot added label `bug`
- Apr 25, 2026: 🏷️ @Pro42good added label `need info`
- May 14, 2026: 🔴 Closed by @rooootdev


---

## Comments (16)

### Comment #1 — [@hostedbyjustus](https://github.com/hostedbyjustus) · Apr 24, 2026

`ID: 4309225451`


True, I have no idea what happened here.


---

### Comment #2 — [@m1337v](https://github.com/m1337v) · Apr 24, 2026

`ID: 4311500628`


Can confirm iPhone 15 18.5 (installed in Livecontainer)

edit: when installed with SideStore it works


---

### Comment #3 — [@xelahot](https://github.com/xelahot) · Apr 24, 2026

`ID: 4315989178`


It's installed with Sidestore for me too but it crashes when trying to initialise RemoteCall. Even the "Run exploit" crashes if the new option "Stash KRW primitives" is enabled.


---

### Comment #4 — [@fall3nang3l](https://github.com/fall3nang3l) · Apr 24, 2026

`ID: 4316404645`


same problem
crashes on iOS 18.1 iPhone 16 pro max


---

### Comment #5 — [@3075559368](https://github.com/3075559368) · Apr 25, 2026

`ID: 4319702548`


> ### 描述错误：我也是注销后崩溃
> 
> 早期版本运行良好。自从几次合并后，它就崩溃了
> 
> ### 设备信息：
> 
> * **设备：**iPhone 14 Pro 
> * **iOS 版本：**18.5
> * **Lara版本/提交：**1.2


---

### Comment #6 — [@mostafa89maa](https://github.com/mostafa89maa) · Apr 26, 2026

`ID: 4320762732`


Same problem 
iphone 16 pro max 
ios 18.5


---

### Comment #7 — [@m1337v](https://github.com/m1337v) · Apr 26, 2026

`ID: 4321680604`


Test this version (remotecall doesn't work in livecontainer tho, haven't found a fix for that yet): https://github.com/m1337v/lara/releases/download/release/lara.ipa


🎉 1


---

### Comment #8 — [@xelahot](https://github.com/xelahot) · Apr 26, 2026

`ID: 4322201345`


This version works! Are you able to open a PR on this repo?


---

### Comment #9 — [@Amin-70](https://github.com/Amin-70) · Apr 27, 2026

`ID: 4324411044`


For me it always fails when pressing the respring button, it will reboot the userspace and remove all applied tweaks

Only in this version does respring work for me
https://drive.google.com/file/d/1tnP7msVL97x3hqWsCIu_Vtt4OR4UkBkM/view?usp=drivesdk

Iphone 15PM ios 17.5.1


---

### Comment #10 — [@xelahot](https://github.com/xelahot) · Apr 27, 2026

`ID: 4326627606`


Don't respring. These runtime modifications don't survive process restarts.

________________________________
De : Amin-70 ***@***.***>
Envoyé : Monday, April 27, 2026 1:32:22 AM
À : rooootdev/lara ***@***.***>
Cc : xelahot ***@***.***>; Author ***@***.***>
Objet : Re: [rooootdev/lara] [BUG] Initialise RemoteCall crashes the app (Issue #183)

[https://avatars.githubusercontent.com/u/216113727?s=20&v=4]Amin-70 left a comment (rooootdev/lara#183)<https://github.com/rooootdev/lara/issues/183#issuecomment-4324411044>

For me it always fails when pressing the respring button, it will reboot the userspace and remove all applied tweaks

Only in this version does respring work for me
https://drive.google.com/file/d/1tnP7msVL97x3hqWsCIu_Vtt4OR4UkBkM/view?usp=drivesdk

Iphone 15PM ios 17.5.1

—
Reply to this email directly, view it on GitHub<https://github.com/rooootdev/lara/issues/183#issuecomment-4324411044>, or unsubscribe<https://github.com/notifications/unsubscribe-auth/AKRKYMN75RHHE32U7FCGVR34X3WGNAVCNFSM6AAAAACYD6HN5GVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHM2DGMRUGQYTCMBUGQ>.
You are receiving this because you authored the thread.Message ID: ***@***.***>


---

### Comment #11 — [@Amin-70](https://github.com/Amin-70) · Apr 27, 2026

`ID: 4327111241`


on iOS 17.x, the kernel may panic when lara is closed from the app switcher

The problem is that when I let it run in the background, sometimes it closes the lara app itself which causes the kernel panic and restart


---

### Comment #12 — [@Amin-70](https://github.com/Amin-70) · Apr 27, 2026

`ID: 4327129802`


> For me it always fails when pressing the respring button, it will reboot the userspace and remove all applied tweaks
> 
> Only in this version does respring work for me https://drive.google.com/file/d/1tnP7msVL97x3hqWsCIu_Vtt4OR4UkBkM/view?usp=drivesdk
> 
> Iphone 15PM ios 17.5.1

This version of lara respring works perfectly, and after respring I can remove the lara app from the app switcher without kernel panic


---

### Comment #13 — [@xelahot](https://github.com/xelahot) · Apr 27, 2026

`ID: 4327224991`


I don't think tweaks are meant to survive respring in Lara. The respring is kind of useless for RemoteCall as the changes live in memory until the process restarts.

________________________________
De : Amin-70 ***@***.***>
Envoyé : Monday, April 27, 2026 8:56:24 AM
À : rooootdev/lara ***@***.***>
Cc : xelahot ***@***.***>; Author ***@***.***>
Objet : Re: [rooootdev/lara] [BUG] Initialise RemoteCall crashes the app (Issue #183)

[https://avatars.githubusercontent.com/u/216113727?s=20&v=4]Amin-70 left a comment (rooootdev/lara#183)<https://github.com/rooootdev/lara/issues/183#issuecomment-4327129802>

For me it always fails when pressing the respring button, it will reboot the userspace and remove all applied tweaks

Only in this version does respring work for me https://drive.google.com/file/d/1tnP7msVL97x3hqWsCIu_Vtt4OR4UkBkM/view?usp=drivesdk

Iphone 15PM ios 17.5.1

This version of lara respring works perfectly, and after respring I can remove the lara app from the app switcher without kernel panic

—
Reply to this email directly, view it on GitHub<https://github.com/rooootdev/lara/issues/183#issuecomment-4327129802>, or unsubscribe<https://github.com/notifications/unsubscribe-auth/AKRKYMJLLRV2UBWPXONYKW34X5KHJAVCNFSM6AAAAACYD6HN5GVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHM2DGMRXGEZDSOBQGI>.
You are receiving this because you authored the thread.Message ID: ***@***.***>


---

### Comment #14 — [@Pro42good](https://github.com/Pro42good) · May 4, 2026

`ID: 4373324375`


Hello, This is a copy paste response, Im just trying to close issues.

But could you try on the latest official version and or on the latest nightly version of lara?

Please let me know if its fixed it's self or you can close the issue.


---

### Comment #15 — [@rooootdev](https://github.com/rooootdev) · May 14, 2026

`ID: 4445768024`


I think this is solved? reopen if its not.


---

### Comment #16 — [@Amin-70](https://github.com/Amin-70) · May 14, 2026

`ID: 4445961655`


> I think this is solved? reopen if its not.

The latest commits are working, commits nightly 7fde60a


---

*Exported on 10/14/2026, 3:01:28 PM from [https://github.com/rooootdev/lara/issues/183](https://github.com/rooootdev/lara/issues/183)*
