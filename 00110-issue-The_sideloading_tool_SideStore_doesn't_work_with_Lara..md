# The sideloading tool SideStore doesn't work with Lara.

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#110](https://github.com/rooootdev/lara/issues/110)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@Pro42good](https://github.com/Pro42good)  
> **Labels:** `bug`, `sufficient info`  
> **Created:** Apr 17, 2026  
> **Updated:** Apr 17, 2026  
> **Closed:** Apr 17, 2026  

---

## Description

*Opened by [@706984971](https://github.com/706984971)* *on Apr 17, 2026*


Hi there,

I've been trying to install and use SideStore (the iOS sideloading app) with the latest version of your Lara tool, but I keep running into issues. The installation either fails completely, or the tool won't work properly after I try to set it up.

Could you please advise if there are any older or specific versions of Lara (or SideStore) that are confirmed to work together and might fix this problem? Any guidance on which build to use would be greatly appreciated.


---

## Timeline (Verbose)

- Apr 17, 2026: 🔴 Closed by @Pro42good
- Apr 17, 2026: 🟢 Reopened by @Pro42good
- Apr 17, 2026: 🏷️ @Pro42good added label `bug`
- Apr 17, 2026: 🔴 Closed by @Pro42good
- Apr 17, 2026: 🟢 Reopened by @Pro42good
- Apr 17, 2026: 🏷️ @Pro42good added label `need info`
- Apr 17, 2026: 🏷️ @Pro42good removed label `need info`
- Apr 17, 2026: 🏷️ @Pro42good added label `sufficient info`
- Apr 17, 2026: 🔴 Closed by @Pro42good


---

## Comments (21)

### Comment #1 — [@panininis](https://github.com/panininis) · Apr 17, 2026

`ID: 4265190877`


could you try iloader or esign


---

### Comment #2 — [@xsxs18-dev](https://github.com/xsxs18-dev) · Apr 17, 2026

`ID: 4266067454`


On me it works fine. Just use iLoader


---

### Comment #3 — [@rooootdev](https://github.com/rooootdev) · Apr 17, 2026

`ID: 4266937539`


im going to need logs or a screenshot


---

### Comment #4 — [@CjoveKing](https://github.com/CjoveKing) · Apr 17, 2026

`ID: 4267450883`


<img width="1179" height="2556" alt="Image" src="https://github.com/user-attachments/assets/354f9c50-d32b-4ca7-a915-f521c5b5d3e9" />


---

### Comment #5 — [@CjoveKing](https://github.com/CjoveKing) · Apr 17, 2026

`ID: 4267481543`


My mobile phone is A16, ios18.7.1


---

### Comment #6 — [@Pro42good](https://github.com/Pro42good) · Apr 17, 2026

`ID: 4267514468`


I fixed it in this commit 210336ac4045a2b6d9059e82dea421d2503b2fb3 .


---

### Comment #7 — [@Pro42good](https://github.com/Pro42good) · Apr 17, 2026

`ID: 4267517717`


Try Again and if it's resolved I'm closing the issue


👍 1


---

### Comment #8 — [@CjoveKing](https://github.com/CjoveKing) · Apr 17, 2026

`ID: 4267553459`


I didn't download it before. Can I download it again now?


---

### Comment #9 — [@Pro42good](https://github.com/Pro42good) · Apr 17, 2026

`ID: 4267582981`


Yeah, or at least try it and tell me if it succeeds.


👍 1


---

### Comment #10 — [@706984971](https://github.com/706984971) · Apr 17, 2026

`ID: 4268194153`


> Yeah, or at least try it and tell me if it succeeds.

![image](https://github.com/user-attachments/assets/b9f141b6-2bbb-495a-b351-d91e0234c80c)


---

### Comment #11 — [@706984971](https://github.com/706984971) · Apr 17, 2026

`ID: 4268195974`


> Yeah, or at least try it and tell me if it succeeds.

![image](https://github.com/user-attachments/assets/33c1cddb-cbdd-4020-b696-bad63cbbaa48)


---

### Comment #12 — [@CjoveKing](https://github.com/CjoveKing) · Apr 17, 2026

`ID: 4268438947`


I don't know if it's a network problem or something else. I encountered these problems from it: 1. I downloaded Lara with the source in sidestoe and the progress was stuck, and the error was reported as follows: 1.1 asked me to check Wi-Fi and LocalDevVpn; 1.2 is timeout. 2. I downloaded Lara from Livecontainer, and I will display the problem of the following picture during use.

<img width="1179" height="2556" alt="Image" src="https://github.com/user-attachments/assets/422aad4c-123a-42e3-8546-6c7afcdb654a" />


---

### Comment #13 — [@706984971](https://github.com/706984971) · Apr 17, 2026

`ID: 4268864824`


I can't afford a certificate, so I can only use the all-in-one sideload tool SideStore or LiveContainer. Hope you understand.


---

### Comment #14 — [@CjoveKing](https://github.com/CjoveKing) · Apr 17, 2026

`ID: 4268882515`


That's not what I meant, sorry. You misunderstood. I don't know whether it's a network problem or an equipment problem that I can't use it.


---

### Comment #15 — [@Pro42good](https://github.com/Pro42good) · Apr 17, 2026

`ID: 4268898022`


Ohhhh, Im sorry about that, That's my bad for not understanding.


---

### Comment #16 — [@Pro42good](https://github.com/Pro42good) · Apr 17, 2026

`ID: 4268917904`


The problemm here is that your device has an OTA partially or fully downloaded waiting to be installed

```
libgrabkernel2: Found firmware URL: https://updates.cdn-apple.com/
2025Fal1FCS/mobileassets/093-54758/ A7E57E9F-F319-4083-BAF9-AA745C83FF1A/ com_apple_MobileAsset_SoftwareUpdate/
9bc73f7f0eb296a16e53ea7b0dd3459b0dd12243
60a7ddfee26094c6f29892dc.aea (OTA: yes)
```

The thing is OTA currently breaks lara and is a known issue that's being worked on.
So in this case it's neither an equipment or network issue. It's just a Software issue currently with lara.


---

### Comment #17 — [@CjoveKing](https://github.com/CjoveKing) · Apr 17, 2026

`ID: 4268969100`


> 這裡的問題是，您的裝置有部分或全部下載的OTA等待安裝
> 
> ```
> libgrabkernel2: Found firmware URL: https://updates.cdn-apple.com/
> 2025Fal1FCS/mobileassets/093-54758/ A7E57E9F-F319-4083-BAF9-AA745C83FF1A/ com_apple_MobileAsset_SoftwareUpdate/
> 9bc73f7f0eb296a16e53ea7b0dd3459b0dd12243
> 60a7ddfee26094c6f29892dc.aea (OTA: yes)
> ```
> 
> 问题是OTA目前破坏了lara，这是一个已知的问题，正在处理中。因此，在这种情况下，这既不是设备问题，也不是网络问题。这只是目前与拉拉的软件问题。
It's okay, because I also use livecontainer.


---

### Comment #18 — [@706984971](https://github.com/706984971) · Apr 17, 2026

`ID: 4269509767`


> The problemm here is that your device has an OTA partially or fully downloaded waiting to be installed
> 
> ```
> libgrabkernel2: Found firmware URL: https://updates.cdn-apple.com/
> 2025Fal1FCS/mobileassets/093-54758/ A7E57E9F-F319-4083-BAF9-AA745C83FF1A/ com_apple_MobileAsset_SoftwareUpdate/
> 9bc73f7f0eb296a16e53ea7b0dd3459b0dd12243
> 60a7ddfee26094c6f29892dc.aea (OTA: yes)
> ```
> 
> The thing is OTA currently breaks lara and is a known issue that's being worked on.
> So in this case it's neither an equipment or network issue. It's just a Software issue currently with lara.

Got it! Thanks for letting me know it's a known issue. I'


---

### Comment #19 — [@tiziodied](https://github.com/tiziodied) · Apr 17, 2026

`ID: 4269617917`


> I don't know if it's a network problem or something else. I encountered these problems from it: 1. I downloaded Lara with the source in sidestoe and the progress was stuck, and the error was reported as follows: 1.1 asked me to check Wi-Fi and LocalDevVpn; 1.2 is timeout. 2. I downloaded Lara from Livecontainer, and I will display the problem of the following picture during use.
> 
> <img width="1179" height="2556" alt="Image" src="https://github.com/user-attachments/assets/422aad4c-123a-42e3-8546-6c7afcdb654a" />

1.1 and 1.2 Sidestore is really weird sometimes so it can happen
2 check the README for a guide (you do need a PC)


---

### Comment #20 — [@CjoveKing](https://github.com/CjoveKing) · Apr 17, 2026

`ID: 4269652910`


> > 我不知道这是网络问题还是其他问题。我从中遇到了这些问题：1.我下载了Lara，源代码在侧边，进度卡住了，错误报告如下：1.1要求我检查Wi-Fi和LocalDevVpn；1.2超时。2.我从Livecontainer下载了Lara，在使用过程中我会显示以下图片的问题。
> > <img alt="形象" width="1179" height="2000" src="https://private-user-images.githubusercontent.com/131689788/579936813-422aad4c-123a-42e3-8546-6c7afcdb654a.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NzY0NDMyMzQsIm5iZiI6MTc3NjQ0MjkzNCwicGF0aCI6Ii8xMzE2ODk3ODgvNTc5OTM2ODEzLTQyMmFhZDRjLTEyM2EtNDJlMy04NTQ2LTZjN2FmY2RiNjU0YS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjYwNDE3JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI2MDQxN1QxNjIyMTRaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1hZDAzNzZhNjQ1MDFmNTc4MTE4YWM4M2NjOTc1NzM5ODE4YTczMGRkOWE0NzUwMzI2ZjVmNTRhYmE5ODIxY2JjJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZyZXNwb25zZS1jb250ZW50LXR5cGU9aW1hZ2UlMkZwbmcifQ.OLGchalzBRNCjKKyxrFTCt1LJ5r3SAKRuCw7aXfgGL0">
> 
> 1.1和1.2 Sidestore有时真的很奇怪，所以它可能发生2 查看README以获取指南（您确实需要一台电脑）

I have a computer with me. Can you tell me what to do?


---

### Comment #21 — [@Pro42good](https://github.com/Pro42good) · Apr 17, 2026

`ID: 4269679589`


> > The problemm here is that your device has an OTA partially or fully downloaded waiting to be installed
> > ```
> > libgrabkernel2: Found firmware URL: https://updates.cdn-apple.com/
> > 2025Fal1FCS/mobileassets/093-54758/ A7E57E9F-F319-4083-BAF9-AA745C83FF1A/ com_apple_MobileAsset_SoftwareUpdate/
> > 9bc73f7f0eb296a16e53ea7b0dd3459b0dd12243
> > 60a7ddfee26094c6f29892dc.aea (OTA: yes)
> > ```
> > 
> > 
> >     
> >       
> >     
> > 
> >       
> >     
> > 
> >     
> >   
> > The thing is OTA currently breaks lara and is a known issue that's being worked on.
> > So in this case it's neither an equipment or network issue. It's just a Software issue currently with lara.
> 
> Got it! Thanks for letting me know it's a known issue. I'

Yeah well since the Original issue got responded to (by the original poster) im going to be closing this.

probably a ping or a github @ if it gets fixed


---

*Exported on 10/14/2026, 2:55:09 PM from [https://github.com/rooootdev/lara/issues/110](https://github.com/rooootdev/lara/issues/110)*
