# iPhone 14 Pro | iOS 17.7.1

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#5](https://github.com/rooootdev/lara/issues/5)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@rooootdev](https://github.com/rooootdev)  
> **Created:** Mar 28, 2026  
> **Updated:** Mar 28, 2026  
> **Closed:** Mar 28, 2026  

---

## Description

*Opened by [@by3lish](https://github.com/by3lish)* *on Mar 28, 2026*


Although it does not work in the system interface, there is a font change in the application interfaces

Edit: I forgot to respring so it didn't work in the system interface. It also works in the system interface after respringing

LOGS:

PROC_PID_OFFSET: 0x28
starting darksword
device: iPhone15,2
running on non-a18 device
read_fd: 0x11
write_fd: 0x12
executable_name: lara
free_thread_arg: 0x1344b4000
physical_mapping_address: 0x10e1ac000
pc_object: 0xac07
pc_address: 0x3b7ce4000
starting darksword
device: iPhone15,2
running on non-a18 device
read_fd: 0x11
write_fd: 0x12
executable_name: lara
free_thread_arg: 0x1344b4000
physical_mapping_address: 0x10e1ac000
pc_object: 0xac07
pc_address: 0x3b7ce4000
spraying 22528 sockets...
spraying 22528 sockets...
socket_ports_count: 0x5800
start_pcb_id: 0x4
end_pcb_id: 0xb002
looking in search mapping: 0
socket_ports_count: 0x5800
start_pcb_id: 0x4
end_pcb_id: 0xb002
looking in search mapping: 0
pcb_start_offset: 0x0
target_inp_gencnt: 0x3484
inp_list_next_pointer: 0xffffffec1be88400
icmp6filter: 0xffffffeffe566600
Corrupting icmp6filter pointer...
pcb_start_offset: 0x0
target_inp_gencnt: 0x3484
inp_list_next_pointer: 0xffffffec1be88400
icmp6filter: 0xffffffeffe566600
Corrupting icmp6filter pointer...
target corrupted: 0xffffffec1be88548
target corrupted: 0xffffffec1be88548
found control_socket at idx: 0x1a40
found control_socket at idx: 0x1a40
highest_success_idx: 438
success_read_count: 64
highest_success_idx: 438
success_read_count: 64

kernel proc: 0xffffffede67e3280

looking for pid: 435

found our proc at 0xffffffede84f0680

pid: 435

name: lara

uid: 501

gid: 501
Walking kernel structures...
control_socket_pcb: 0xffffffec1be88000
pcbinfo_pointer: 0xfffffff046d29108
ipi_zone: 0xfffffff044331c90
zv_name: 0xfffffff043a7453e
searching for kernel Mach-O header...
kernel_base:  0xfffffff043a08000
kernel_slide: 0x3ca04000
iOS 17: using so_count offset 0x24c
kernel r/w is ready!
our_proc: 0xffffffede84f0680
our_task: 0xffffffede84f0dc0
exploit success!
kernel_base:  0xfffffff043a08000
kernel_slide: 0x3ca04000

Walking kernel structures...
control_socket_pcb: 0xffffffec1be88000
pcbinfo_pointer: 0xfffffff046d29108
ipi_zone: 0xfffffff044331c90
zv_name: 0xfffffff043a7453e
searching for kernel Mach-O header...
kernel_base:  0xfffffff043a08000
kernel_slide: 0x3ca04000
iOS 17: using so_count offset 0x24c
kernel r/w is ready!
our_proc: 0xffffffede84f0680
our_task: 0xffffffede84f0dc0


exploit success!

kernel_base:  0xfffffff043a08000

kernel_slide: 0x3ca04000


(kfs) kfs_init starting...

(kfs) Extracted heap PAC prefix: 0xffffff0000000000

(kfs) proc=0xffffffede84f0680 task=0xffffffede84f0dc0 (from exploit)

(kfs) file overwrite ready!

(kfs) our proc: 0xffffffede84f0680

kernel proc: 0xffffffede67e3280

looking for process: launchd

found process: launchd (PID: 1, UID: 0, GID: 0) at 0xffffffede67dd400

(kfs) launchd proc: 0xffffffede67dd400

(kfs) scanning launchd proc for p_textvp...

(kfs) check textvp=0xffffffede67e3308 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffede8739400 name_ptr=0xffffffed00e1c420 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffede61ee990 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffede0bc2aa8 name_ptr=0x50550008ad27 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffefff7e5880 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffed00910000 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffec199a4230 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffec19b1a300 name_ptr=0xffffffeeccc9ac60 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffed0033ce00 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffed0033cc00 name_ptr=0xffffffec18f35600 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffecffe50800 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffede67dd9d0 name_ptr=0x28 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffede67dd9e0 name_ptr=0x3c0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffec1999c700 name_ptr=0x10b (OV_NAME=0xb8)

(kfs) check textvp=0xffffffede4574300 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffec19ccb480 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffed0032e600 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffede67b68c0 name_ptr=0xa022000000 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffede67e39c0 name_ptr=0xa022000000 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffede61ee4c0 name_ptr=0xffffffede4882d50 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffede0bc24c0 name_ptr=0xffffffede0b8ba90 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffec199be500 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffeae5e45580 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffede67f8220 name_ptr=0x92f155 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffed004400e0 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) rootvnode not found via launchd (scanned proc+0x80 to proc+0x800)

(kfs) p_textvp offset may have changed on this iOS version

(kfs) rootvnode discovery failed; listdir will be unavailable

(kfs) kfs_init done
(kfs) kfs_init starting...
(kfs) Extracted heap PAC prefix: 0xffffff0000000000
(kfs) proc=0xffffffede84f0680 task=0xffffffede84f0dc0 (from exploit)
(kfs) file overwrite ready!
(kfs) our proc: 0xffffffede84f0680
(kfs) launchd proc: 0xffffffede67dd400
(kfs) scanning launchd proc for p_textvp...
(kfs) check textvp=0xffffffede67e3308 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffede8739400 name_ptr=0xffffffed00e1c420 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffede61ee990 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffede0bc2aa8 name_ptr=0x50550008ad27 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffefff7e5880 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffed00910000 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffec199a4230 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffec19b1a300 name_ptr=0xffffffeeccc9ac60 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffed0033ce00 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffed0033cc00 name_ptr=0xffffffec18f35600 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffecffe50800 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffede67dd9d0 name_ptr=0x28 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffede67dd9e0 name_ptr=0x3c0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffec1999c700 name_ptr=0x10b (OV_NAME=0xb8)
(kfs) check textvp=0xffffffede4574300 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffec19ccb480 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffed0032e600 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffede67b68c0 name_ptr=0xa022000000 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffede67e39c0 name_ptr=0xa022000000 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffede61ee4c0 name_ptr=0xffffffede4882d50 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffede0bc24c0 name_ptr=0xffffffede0b8ba90 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffec199be500 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffeae5e45580 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffede67f8220 name_ptr=0x92f155 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffed004400e0 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) rootvnode not found via launchd (scanned proc+0x80 to proc+0x800)
(kfs) p_textvp offset may have changed on this iOS version
(kfs) rootvnode discovery failed; listdir will be unavailable
(kfs) kfs_init done

kfs ready!


(kfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf ← /private/var/containers/Bundle/Application/F639A37B-BE70-41BA-81BC-FB0A770784E0/lara.app/Comic Sans SFUI.ttf

(kfs) target mmap'd at 0x10fe00000 (size 8334556)

(kfs) task (from exploit): 0xffffffede84f0dc0

(kfs) vm_map: 0xffffffe9ff9c6120 (task+0x28)

0 : 33000000

8 : 420000

10 : ffffffe9ffc7ea20

18 : ffffffe9ffc85d50

20 : 10070c000

28 : 458000000

30 : 1000e000001d7

38 : ffffffe9ffcf2090

40 : 8d17fec1a219900

48 : 61f7834000

50 : 7fffffffffffffff

58 : 7fffffffffffffff

60 : 7fffffffffffffff

68 : 4000

70 : 19

78 : 0

(kfs) vm_map entries: 471, looking for 0x10fe00000

(kfs) found entry 0xffffffe9ffcdb920: 0x10fe00000-0x1105f4000

(kfs) patching entry flags: 0x20020880 → 0x20021980

(kfs) writing 1174536 bytes...

(kfs) zeroed 7160020 bytes of trailing garbage

(kfs) overwrite done!
(kfs) using font: /private/var/containers/Bundle/Application/F639A37B-BE70-41BA-81BC-FB0A770784E0/lara.app/Comic Sans SFUI.ttf
font changed to Comic Sans MS
(kfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf ← /private/var/containers/Bundle/Application/F639A37B-BE70-41BA-81BC-FB0A770784E0/lara.app/Comic Sans SFUI.ttf
(kfs) target mmap'd at 0x10fe00000 (size 8334556)
(kfs) task (from exploit): 0xffffffede84f0dc0
(kfs) vm_map: 0xffffffe9ff9c6120 (task+0x28)
(kfs) vm_map entries: 471, looking for 0x10fe00000
(kfs) found entry 0xffffffe9ffcdb920: 0x10fe00000-0x1105f4000
(kfs) patching entry flags: 0x20020880 → 0x20021980
(kfs) writing 1174536 bytes...
(kfs) zeroed 7160020 bytes of trailing garbage
(kfs) overwrite done!


---

## Timeline (Verbose)

- Mar 28, 2026: 🔴 Closed by @rooootdev


---

*Exported on 10/14/2026, 2:46:31 PM from [https://github.com/rooootdev/lara/issues/5](https://github.com/rooootdev/lara/issues/5)*
