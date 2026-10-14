# Hello, everyone! I've been trying some offsets recently, hoping to find the right value for me iPhone16Pro Max iOS18.6.2

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#173](https://github.com/rooootdev/lara/issues/173)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@rooootdev](https://github.com/rooootdev)  
> **Labels:** `enhancement`  
> **Assignees:** [@Pro42good](https://github.com/Pro42good)  
> **Created:** Apr 23, 2026  
> **Updated:** May 10, 2026  
> **Closed:** May 10, 2026  

---

## Description

*Opened by [@mychenliu-jpg](https://github.com/mychenliu-jpg)* *on Apr 23, 2026*


lara started: 2026-04-23 06:56:48

(utils) TASK_TNEXT_OFFSET: 0x50

(utils) THREAD_MUPCB_OFFSET: 0xb8

(utils) PROC_PID_OFFSET: 0x60

(offs) loaded saved t1sz_boot: 0x19

(offs) t1sz_boot: 0x19

(offs) saved t1sz_boot: 0x19

initialized offsets

(offs) loaded saved t1sz_boot: 0x19

(offs) t1sz_boot: 0x19

(offs) saved t1sz_boot: 0x19

initialized offsets

(ds) starting darksword
(ds) device: iPhone17,2
(ds) ispac: yes
(ds) running on a18 device
(ds) read_fd: 0x11
(ds) write_fd: 0x12
(ds) executable_path: /private/var/containers/Bundle/Application/F12F397F-2147-4D67-AD3E-FDEA37052109/lara.app/lara
(ds) host_executable_path: /private/var/containers/Bundle/Application/F12F397F-2147-4D67-AD3E-FDEA37052109/lara.app/lara
(ds) guest_executable_name: lara
(ds) host_executable_name: lara
(ds) kernel_process_name: lara
(ds) livecontainer_bundle: no
(ds) livecontainer_guest: no
(ds) rehosted_process: no
(ds) process_marker[0]: lara
(ds) executable_name: lara
(ds) free_thread_arg: 0x112708000
(ds) physical_mapping_address: 0x10c898000
(ds) pc_object: 0xf007
(ds) pc_address: 0x32c284000
(ds) allocating wired memory (131072 entries)...
(ds) allocating wired memory done
(ds) seeking_offset: 0x0: Found wired_page: 0x153c0c000
(ds) seeking_offset: 0x18000: Found wired_page: 0x10f2d8000
(ds) seeking_offset: 0x124000: Found wired_page: 0x158fdc000
(ds) seeking_offset: 0x88000: Found wired_page: 0x16a204000
(ds) seeking_offset: 0x50000: Found wired_page: 0x30fc64000
(ds) seeking_offset: 0x10000: Found wired_page: 0x11dd64000
(ds) seeking_offset: 0xc000: Found wired_page: 0x16ef58000
(ds) seeking_offset: 0x14000: Found wired_page: 0x10eacc000
(ds) waiting for zone trimming...
(ds) seeking_offset: 0x60000: Found wired_page: 0x14fcc4000
(ds) seeking_offset: 0x70000: Found wired_page: 0x14c1fc000
(ds) seeking_offset: 0x84000: Found wired_page: 0x30d454000
(ds) seeking_offset: 0xdc000: Found wired_page: 0x11ea10000
(ds) seeking_offset: 0xec000: Found wired_page: 0x1688f8000
(ds) seeking_offset: 0xf8000: Found wired_page: 0x30b4dc000
(ds) seeking_offset: 0x1c0000: Found wired_page: 0x10edec000
(ds) seeking_offset: 0x48000: Found wired_page: 0x1778d4000
(ds) waiting for zone trimming...
(ds) seeking_offset: 0x160000: Found wired_page: 0x3011d4000
(ds) seeking_offset: 0x12c000: Found wired_page: 0x30b318000
(ds) seeking_offset: 0xc0000: Found wired_page: 0x3019a0000
(ds) seeking_offset: 0xd4000: Found wired_page: 0x11ea08000
(ds) seeking_offset: 0x1ec000: Found wired_page: 0x30fcfc000
(ds) seeking_offset: 0x1e8000: Found wired_page: 0x16ed64000
(ds) seeking_offset: 0x254000: Found wired_page: 0x3056d4000
(ds) Matched PCB via process marker: lara
(ds) pcb_start_offset: 0x0
(ds) target_inp_gencnt: 0x32eca
(ds) inp_list_next_pointer: 0xffffffe115398400
(ds) icmp6filter: 0xffffffe40d693600
(ds) Corrupting icmp6filter pointer...
(ds) target corrupted: 0xffffffe115398548
(ds) found control_socket at idx: 0x375f
(ds) seeking_offset: 0xf8000: Found PCB page

(utils) socket fallback: pcb=0xffffffe115398400 pid=560

(utils) socket found via pcb+0x28 = 0xfffffff04bc20b58 (backptr at +0x0)

(utils) socket scan found nothing (pcb=0xffffffe115398400 sock=0xfffffff04bc20b58)
(ds) highest_success_idx: 499
(ds) exploit success!
(ds) kernel_base:  0xfffffff047dc8000
(ds) kernel_slide: 0x40dc4000

(ds) success_read_count: 370
(ds) Walking kernel structures...
(ds) control_socket_pcb: 0xffffffe115398000
(ds) pcbinfo_pointer: 0xfffffff04bc2e790
(ds) ipi_zone: 0xfffffff0489f0f20
(ds) zv_name: 0xfffffff047e425c3
(ds) searching for kernel Mach-O header from 0xfffffff047e40000...
(ds) candidate Mach-O at 0xfffffff047dd0000: filetype=2 cpuinfo=0x2c0000002 (iter=28)
(ds) candidate Mach-O at 0xfffffff047dc8000: filetype=12 cpuinfo=0xc00000002 (iter=30)
(ds) found MH_FILESET header at 0xfffffff047dc8000
(ds) kernel_base:  0xfffffff047dc8000
(ds) kernel_slide: 0x40dc4000
(ds) iOS 18: using so_count offset 0x254
(ds) kernel r/w is ready!
(ds) our_proc: 0xffffffe1f9635618
(ds) our_task: 0xffffffe1f9635d58

(ds) exploit success!
(ds) kernel_base:  0xfffffff047dc8000
(ds) kernel_slide: 0x40dc4000


---

## Timeline (Verbose)

- Apr 23, 2026: 👤 @mychenliu-jpg assigned @Pro42good
- Apr 23, 2026: 🏷️ @mychenliu-jpg added label `enhancement`
- May 10, 2026: 🔴 Closed by @rooootdev


---

## Comments (12)

### Comment #1 — [@mychenliu-jpg](https://github.com/mychenliu-jpg) · Apr 23, 2026

`ID: 4300554226`


Hello, everyone! I've been trying some offsets recently, hoping to find a value that suits me. It crashes every time I run, and the phone restarts, which is a little annoying. However, I have successfully found some offset values, hoping to help you. The following is the offset value I found:


---

### Comment #2 — [@rooootdev](https://github.com/rooootdev) · Apr 23, 2026

`ID: 4301169032`


well the exploit succeeds from what you've provided. what ends up panicking your phone?


---

### Comment #3 — [@Mrchenliu](https://github.com/Mrchenliu) · Apr 23, 2026

`ID: 4301191938`


>well the exploit succeeds from what you‘ve provided. what ends up panicking your phone?

Clicking to use it will cause the app to crash; the success rate is extremely low.


---

### Comment #4 — [@Mrchenliu](https://github.com/Mrchenliu) · Apr 23, 2026

`ID: 4301215928`


> well the exploit succeeds from what you've provided. what ends up panicking your phone?

After clicking "Run Exploit," the phone automatically restarted after loading to 39%. This is likely due to a conflicting offset being exploited.


---

### Comment #5 — [@rooootdev](https://github.com/rooootdev) · Apr 23, 2026

`ID: 4301217764`


what device+version are you on?


---

### Comment #6 — [@Mrchenliu](https://github.com/Mrchenliu) · Apr 23, 2026

`ID: 4301222747`


> what device+version are you on?
> 

iPhone16Pro Max iOS18.6.2


---

### Comment #7 — [@Mrchenliu](https://github.com/Mrchenliu) · Apr 23, 2026

`ID: 4301250534`


> what device+version are you on?
> 

Sometimes after restarting 3-4 times and clicking "Run Exploit," the progress bar quickly reaches 99%, and then the sandbox escape is successfully exploited. However, I'm unsure which offset was used to succeed. Each time I run Lara, it crashes many times before finally succeeding.


---

### Comment #8 — [@rooootdev](https://github.com/rooootdev) · Apr 23, 2026

`ID: 4301351606`


oh weird
I though ds was deterministic


---

### Comment #9 — [@mychenliu-jpg](https://github.com/mychenliu-jpg) · Apr 23, 2026

`ID: 4301398348`


This crash is not an isolated incident. Could you please help optimize the
system and try adding a feature to automatically save the offset after
successful exploitation?

roooot ***@***.***>于2026年4月23日 周四10:31写道：

> *rooootdev* left a comment (rooootdev/lara#173)
> <https://github.com/rooootdev/lara/issues/173#issuecomment-4301351606>
>
> oh weird
> I though ds was deterministic
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/rooootdev/lara/issues/173?email_source=notifications&email_token=B24EZUYQTUOSHQV4SAWA54D4XF57HA5CNFSNUABFM5UWIORPF5TWS5BNNB2WEL2JONZXKZKDN5WW2ZLOOQXTIMZQGEZTKMJWGA3KM4TFMFZW63VGMF2XI2DPOKSWK5TFNZ2KYZTPN52GK4S7MNWGSY3L#issuecomment-4301351606>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/B24EZU4CN7QXHNNIOAEG4D34XF57HAVCNFSM6AAAAACYC646I6VHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHM2DGMBRGM2TCNRQGY>
> .
> You are receiving this because you authored the thread.Message ID:
> ***@***.***>
>


---

### Comment #10 — [@rooootdev](https://github.com/rooootdev) · Apr 23, 2026

`ID: 4305542610`


these offsets aren't constants


---

### Comment #11 — [@Amin-70](https://github.com/Amin-70) · Apr 24, 2026

`ID: 4310826595`


I have several versions of lara, I don't know which version I downloaded

Of all the old and latest lara versions, only one works on my iphone 15PM ios version 17.5.1

<img width="1290" height="2796" alt="Image" src="https://github.com/user-attachments/assets/4587400e-3f3c-4f3b-be3c-49f055ae35dd" />


---

### Comment #12 — [@Amin-70](https://github.com/Amin-70) · Apr 24, 2026

`ID: 4310862677`


In this successful version of the lara, the remote control also works only if the lara is not closed from the app switcher or respring from the app

Here's the lara link that works on my iphone 15PM ios 17.5.1

https://drive.google.com/file/d/1tnP7msVL97x3hqWsCIu_Vtt4OR4UkBkM/view?usp=drivesdk

<img width="1290" height="2796" alt="Image" src="https://github.com/user-attachments/assets/7e88c879-6d6d-486b-8389-07271fb56fd8" />


---

*Exported on 10/14/2026, 3:00:28 PM from [https://github.com/rooootdev/lara/issues/173](https://github.com/rooootdev/lara/issues/173)*
