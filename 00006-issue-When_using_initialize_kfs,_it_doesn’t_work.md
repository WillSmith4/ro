# When using initialize kfs, it doesn’t work

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#6](https://github.com/rooootdev/lara/issues/6)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@rooootdev](https://github.com/rooootdev)  
> **Created:** Mar 28, 2026  
> **Updated:** Mar 28, 2026  
> **Closed:** Mar 28, 2026  

---

## Description

*Opened by [@Raed3300](https://github.com/Raed3300)* *on Mar 28, 2026*


PROC_PID_OFFSET: 0x60

downloading kernelcache to: /var/mobile/Containers/Data/Application/4D10A778-52AC-4563-A6F5-98BEAA09FCB6/Documents/kernelcache

libgrabkernel2: Found firmware URL: https://updates.cdn-apple.com/2025SpringFCS/fullrestores/082-46429/1368AE9F-7042-4559-884F-586C3A5550D6/iPad_10.2_2021_18.5_22F76_Restore.ipsw (OTA: no)

libgrabkernel2: Downloading BuildManifest.plist...

libgrabkernel2: Downloading kernelcache.release.ipad12p to /var/mobile/Containers/Data/Application/4D10A778-52AC-4563-A6F5-98BEAA09FCB6/Documents/kernelcache...

libgrabkernel2: Downloaded kernelcache!

kernel base: 0xfffffff007004000

kernel entry: 0xfffffff007ea44f0

kernproc symbol: 0xfffffff007aaa298

kernproc offset: 0xaa6298
starting darksword
device: iPad12,1
running on non-a18 device
read_fd: 0x11
write_fd: 0x12
executable_name: lara
free_thread_arg: 0x13db14000
physical_mapping_address: 0x114d64000
pc_object: 0x521b
pc_address: 0x152bf8000
starting darksword
device: iPad12,1
running on non-a18 device
read_fd: 0x11
write_fd: 0x12
executable_name: lara
free_thread_arg: 0x13db14000
physical_mapping_address: 0x114d64000
pc_object: 0x521b
pc_address: 0x152bf8000
spraying 22528 sockets...
spraying 22528 sockets...
socket_ports_count: 0x5800
start_pcb_id: 0x57ff4
end_pcb_id: 0x62ff2
looking in search mapping: 0
socket_ports_count: 0x5800
start_pcb_id: 0x57ff4
end_pcb_id: 0x62ff2
looking in search mapping: 0
pcb_start_offset: 0x0
target_inp_gencnt: 0x60698
pcb_start_offset: 0x0
inp_list_next_pointer: 0xffffffe1825f8400
icmp6filter: 0xffffffe39d657d80
Corrupting icmp6filter pointer...
target_inp_gencnt: 0x60698
inp_list_next_pointer: 0xffffffe1825f8400
icmp6filter: 0xffffffe39d657d80
Corrupting icmp6filter pointer...
target corrupted: 0xffffffe1825f8548
found control_socket at idx: 0x4352
target corrupted: 0xffffffe1825f8548
found control_socket at idx: 0x4352
highest_success_idx: 491
success_read_count: 147
highest_success_idx: 491
success_read_count: 147

kernel proc: 0xffffffe185127b68

looking for pid: 678

found our proc at 0xffffffe1864f9240

pid: 678

name: lara

uid: 501

gid: 501
Walking kernel structures...
control_socket_pcb: 0xffffffe1825f8000
pcbinfo_pointer: 0xfffffff01b3bfd50
ipi_zone: 0xfffffff01899b590
zv_name: 0xfffffff017f6242a
searching for kernel Mach-O header...
kernel_base:  0xfffffff017eec000
kernel_slide: 0x10ee8000
iOS 18: using so_count offset 0x254
kernel r/w is ready!
our_proc: 0xffffffe1864f9240
our_task: 0xffffffe1864f9980
exploit success!
kernel_base:  0xfffffff017eec000
kernel_slide: 0x10ee8000

Walking kernel structures...
control_socket_pcb: 0xffffffe1825f8000
pcbinfo_pointer: 0xfffffff01b3bfd50
ipi_zone: 0xfffffff01899b590
zv_name: 0xfffffff017f6242a
searching for kernel Mach-O header...
kernel_base:  0xfffffff017eec000
kernel_slide: 0x10ee8000
iOS 18: using so_count offset 0x254
kernel r/w is ready!
our_proc: 0xffffffe1864f9240
our_task: 0xffffffe1864f9980


exploit success!

kernel_base:  0xfffffff017eec000

kernel_slide: 0x10ee8000


(kfs) kfs_init starting...

(kfs) Extracted heap PAC prefix: 0xffffff0000000000

(kfs) proc=0xffffffe1864f9240 task=0xffffffe1864f9980 (from exploit)

(kfs) file overwrite ready!

(kfs) our proc: 0xffffffe1864f9240

kernel proc: 0xffffffe185127b68

looking for process: launchd

found process: launchd (PID: 1, UID: 0, GID: 0) at 0xffffffe185125240

(kfs) launchd proc: 0xffffffe185125240

(kfs) scanning launchd proc for p_textvp...

(kfs) check textvp=0xffffffe185127bf0 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffe186432db0 name_ptr=0xffffffe184f1a300 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffdfb873a2e0 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffdfb92c9d30 name_ptr=0x5055000b596b (OV_NAME=0xb8)

(kfs) check textvp=0xffffffe26b749c00 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffdfb8236800 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffe09ef4c230 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffe26b2b6000 name_ptr=0xffffffe39e837d60 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffdfb82d6600 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffdfb82d5300 name_ptr=0xffffffded26c9700 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffdfb8074c00 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffe185125810 name_ptr=0x28 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffe185125820 name_ptr=0x3c0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffded2374f80 name_ptr=0xd (OV_NAME=0xb8)

(kfs) check textvp=0xffffffdfb8c60300 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffe1850acb00 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffdfb82c3000 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffe185132bd0 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffe1851282a8 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffdfb8739e18 name_ptr=0x55e1e8dfb8ae91a0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffdfb92c9750 name_ptr=0x68a736dfb9fc1b90 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffe09e820100 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffdfb839c200 name_ptr=0x1c946519 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffe26b700090 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) rootvnode not found via launchd (scanned proc+0x80 to proc+0x800)

(kfs) p_textvp offset may have changed on this iOS version

(kfs) rootvnode discovery failed; listdir will be unavailable

(kfs) kfs_init done
(kfs) kfs_init starting...
(kfs) Extracted heap PAC prefix: 0xffffff0000000000
(kfs) proc=0xffffffe1864f9240 task=0xffffffe1864f9980 (from exploit)
(kfs) file overwrite ready!
(kfs) our proc: 0xffffffe1864f9240
(kfs) launchd proc: 0xffffffe185125240
(kfs) scanning launchd proc for p_textvp...
(kfs) check textvp=0xffffffe185127bf0 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffe186432db0 name_ptr=0xffffffe184f1a300 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffdfb873a2e0 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffdfb92c9d30 name_ptr=0x5055000b596b (OV_NAME=0xb8)
(kfs) check textvp=0xffffffe26b749c00 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffdfb8236800 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffe09ef4c230 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffe26b2b6000 name_ptr=0xffffffe39e837d60 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffdfb82d6600 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffdfb82d5300 name_ptr=0xffffffded26c9700 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffdfb8074c00 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffe185125810 name_ptr=0x28 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffe185125820 name_ptr=0x3c0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffded2374f80 name_ptr=0xd (OV_NAME=0xb8)
(kfs) check textvp=0xffffffdfb8c60300 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffe1850acb00 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffdfb82c3000 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffe185132bd0 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffe1851282a8 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffdfb8739e18 name_ptr=0x55e1e8dfb8ae91a0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffdfb92c9750 name_ptr=0x68a736dfb9fc1b90 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffe09e820100 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffdfb839c200 name_ptr=0x1c946519 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffe26b700090 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) rootvnode not found via launchd (scanned proc+0x80 to proc+0x800)
(kfs) p_textvp offset may have changed on this iOS version
(kfs) rootvnode discovery failed; listdir will be unavailable
(kfs) kfs_init done

kfs ready!


kernel proc: 0xffffffe185127b68

looking for pid: 678

found our proc at 0xffffffe1864f9240

pid: 678

name: lara

uid: 501

gid: 501

kernel proc: 0xffffffe185127b68

looking for pid: 678

found our proc at 0xffffffe1864f9240

pid: 678

name: lara

uid: 501

gid: 501

This is on iPadOS 18.5 iPad 9th generation Wi-Fi only, misakaReborn also doesn’t work, I installed via enterprise certificate since my dev certificate is full


---

## Timeline (Verbose)

- Mar 28, 2026: 🔴 Closed by @rooootdev


---

## Comments (3)

### Comment #1 — [@rooootdev](https://github.com/rooootdev) · Mar 28, 2026

`ID: 4148054031`


looks like everything is working? what are you trying to do?


---

### Comment #2 — [@Raed3300](https://github.com/Raed3300) · Mar 28, 2026

`ID: 4148056799`


Can i revert the font back to og after using overwrite?


---

### Comment #3 — [@rooootdev](https://github.com/rooootdev) · Mar 28, 2026

`ID: 4148058644`


yes.


---

*Exported on 10/14/2026, 2:46:35 PM from [https://github.com/rooootdev/lara/issues/6](https://github.com/rooootdev/lara/issues/6)*
