# iPhone 15 Pro (A17)

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#7](https://github.com/rooootdev/lara/issues/7)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@rooootdev](https://github.com/rooootdev)  
> **Created:** Mar 28, 2026  
> **Updated:** Mar 29, 2026  
> **Closed:** Mar 29, 2026  

---

## Description

*Opened by [@ghost](https://github.com/ghost)* *on Mar 28, 2026*


iphone 15 pro a17
ios 17.2.1 
tested:
kernelcache download (works)

run exploit (works)

initialise kfs (works) 

font override (fails -
(kfs) vm_map_entry not found for 0x113f3c00 
(kfs) using font: (filepath location)
failed to change font )
^ changing font fails for all fonts

Panic! (works, takes on avg 3.4s-4.3s to panic and reboot device)
gettask (works)

Logs (all working)


---

## Timeline (Verbose)

- Mar 29, 2026: 🔴 Closed by @rooootdev


---

## Comments (5)

### Comment #1 — [@ghost](https://github.com/ghost) · Mar 28, 2026

`ID: 4148296306`


hope i could be of help, glad you got this working and thank you for your contribution


---

### Comment #2 — [@rooootdev](https://github.com/rooootdev) · Mar 28, 2026

`ID: 4148896335`


could you share logs?


---

### Comment #3 — [@ghost](https://github.com/ghost) · Mar 29, 2026

`ID: 4148981986`


> could you share logs?

Yes i can, just updated to latest (sha256:8d2b77fc8c8d44cd88e61f6697440a0519a066eeb22795f0d7638a36c47fcfd2)
Did not test DirtyZero tweaks.

lara started: 2026-03-28 19:04:11

PROC_PID_OFFSET: 0x28

downloading kernelcache to: /var/mobile/Containers/Data/Application/E19A7E23-2427-410C-8F90-D177D5332372/Documents/kernelcache

libgrabkernel2: Found firmware URL: https://updates.cdn-apple.com/2023FallFCS/fullrestores/052-18109/661DD240-3D1B-4CB3-BB47-168916048AC1/iPhone16,1_17.2.1_21C66_Restore.ipsw (OTA: no)

libgrabkernel2: Downloading BuildManifest.plist...

libgrabkernel2: Downloading kernelcache.release.iphone16 to /var/mobile/Containers/Data/Application/E19A7E23-2427-410C-8F90-D177D5332372/Documents/kernelcache...

libgrabkernel2: Downloaded kernelcache!

kernel base: 0xfffffff027004000

kernel entry: 0xfffffff02a354000

kernproc symbol: 0xfffffff027980668

rootvnode symbol: 0xfffffff02a5f4e80

kernproc offset: 0x97c668

rootvnode offset: 0x35f0e80

starting darksword
device: iPhone16,1
running on non-a18 device
read_fd: 0x12
write_fd: 0x15
executable_name: lara
free_thread_arg: 0x10dbb8000
physical_mapping_address: 0x114df0000
pc_object: 0xff03
pc_address: 0x363528000
starting darksword
device: iPhone16,1
running on non-a18 device
read_fd: 0x12
write_fd: 0x15
executable_name: lara
free_thread_arg: 0x10dbb8000
physical_mapping_address: 0x114df0000
pc_object: 0xff03
pc_address: 0x363528000
spraying 22528 sockets...
spraying 22528 sockets...
socket_ports_count: 0x5800
start_pcb_id: 0x8
end_pcb_id: 0xb006
looking in search mapping: 0
socket_ports_count: 0x5800
start_pcb_id: 0x8
end_pcb_id: 0xb006
looking in search mapping: 0
pcb_start_offset: 0x0
target_inp_gencnt: 0x33ee
inp_list_next_pointer: 0xffffffe16dda0400
icmp6filter: 0xffffffe388a648e0
pcb_start_offset: 0x0
Corrupting icmp6filter pointer...
target_inp_gencnt: 0x33ee
inp_list_next_pointer: 0xffffffe16dda0400
icmp6filter: 0xffffffe388a648e0
Corrupting icmp6filter pointer...
target corrupted: 0xffffffe16dda0548
found control_socket at idx: 0x19f3
target corrupted: 0xffffffe16dda0548
found control_socket at idx: 0x19f3
highest_success_idx: 498
success_read_count: 267
highest_success_idx: 498

kernel proc: 0xffffffe08c307290

looking for pid: 1399
Walking kernel structures...
control_socket_pcb: 0xffffffe16dda0000
pcbinfo_pointer: 0xfffffff063ccc948
ipi_zone: 0xfffffff0610612f0
zv_name: 0xfffffff06074865d
searching for kernel Mach-O header from 0xfffffff060748000...
candidate Mach-O at 0xfffffff0606e4000: filetype=2 cpuinfo=0x2c0000002 (iter=25)

found proc:  (pid=1399 uid=501 gid=501) @ 0xffffffe08a288710
candidate Mach-O at 0xfffffff0606dc000: filetype=12 cpuinfo=0xc00000002 (iter=27)
found MH_FILESET header at 0xfffffff0606dc000
kernel_base:  0xfffffff0606dc000
kernel_slide: 0x596d8000
iOS 17: using so_count offset 0x24c
kernel r/w is ready!
success_read_count: 267
Walking kernel structures...
control_socket_pcb: 0xffffffe16dda0000
pcbinfo_pointer: 0xfffffff063ccc948
ipi_zone: 0xfffffff0610612f0
zv_name: 0xfffffff06074865d
searching for kernel Mach-O header from 0xfffffff060748000...
our_proc: 0xffffffe08a288710
our_task: 0xffffffe08a288e50
exploit success!
kernel_base:  0xfffffff0606dc000
kernel_slide: 0x596d8000

candidate Mach-O at 0xfffffff0606e4000: filetype=2 cpuinfo=0x2c0000002 (iter=25)
candidate Mach-O at 0xfffffff0606dc000: filetype=12 cpuinfo=0xc00000002 (iter=27)
found MH_FILESET header at 0xfffffff0606dc000
kernel_base:  0xfffffff0606dc000
kernel_slide: 0x596d8000
iOS 17: using so_count offset 0x24c
kernel r/w is ready!
our_proc: 0xffffffe08a288710
our_task: 0xffffffe08a288e50


exploit success!

kernel_base:  0xfffffff0606dc000

kernel_slide: 0x596d8000


(vfs) vfs_init starting...

(vfs) Extracted heap PAC prefix: 0xffffff0000000000

(vfs) proc=0xffffffe08a288710 task=0xffffffe08a288e50 (from exploit)

(vfs) file overwrite ready!

(vfs) rootvnode offset probe: 0xffffffdfa58c0a00 (sym=0xfffffff063ccce80)

(vfs) rootvnode via offset: 0xffffffdfa58c0a00

(vfs) ncache offsets mismatch

(vfs) ncache verification failed

(vfs) vfs_init done
(vfs) vfs_init starting...
(vfs) Extracted heap PAC prefix: 0xffffff0000000000
(vfs) proc=0xffffffe08a288710 task=0xffffffe08a288e50 (from exploit)
(vfs) file overwrite ready!
(vfs) rootvnode offset probe: 0xffffffdfa58c0a00 (sym=0xfffffff063ccce80)
(vfs) rootvnode via offset: 0xffffffdfa58c0a00
(vfs) ncache offsets mismatch
(vfs) ncache verification failed
(vfs) vfs_init done

vfs ready!


(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/0E0848DE-81A7-49B0-A2CB-8165DAE24649/lara.app/Comic Sans MS.ttf
(vfs) using font: /private/var/containers/Bundle/Application/0E0848DE-81A7-49B0-A2CB-8165DAE24649/lara.app/Comic Sans MS.ttf
failed to change font

(vfs) target mmap'd at 0x1179c0000 (size 8334556)

(vfs) task (from exploit): 0xffffffe08a288e50

(vfs) vm_map: 0xffffffe258e1d100 (task+0x98, nentries=24910)

(vfs) vm_map entries: 24930, looking for 0x1179c0000

(vfs) vm_map_entry not found for 0x1179c0000
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/0E0848DE-81A7-49B0-A2CB-8165DAE24649/lara.app/Comic Sans MS.ttf
(vfs) target mmap'd at 0x1179c0000 (size 8334556)
(vfs) task (from exploit): 0xffffffe08a288e50
(vfs) vm_map: 0xffffffe258e1d100 (task+0x98, nentries=24910)
(vfs) vm_map entries: 24930, looking for 0x1179c0000
(vfs) vm_map_entry not found for 0x1179c0000


---

### Comment #4 — [@ghost](https://github.com/ghost) · Mar 29, 2026

`ID: 4148995044`


Most notably; ncache offsets mismatch and ncache verification fails, as well as vm_map_entry not found for 0x1179c0000


---

### Comment #5 — [@rooootdev](https://github.com/rooootdev) · Mar 29, 2026

`ID: 4149357081`


yeah. a couple of offsets are wrong for your version. I might add support for your version in a future update, no eta currently.


---

*Exported on 10/14/2026, 2:46:39 PM from [https://github.com/rooootdev/lara/issues/7](https://github.com/rooootdev/lara/issues/7)*
