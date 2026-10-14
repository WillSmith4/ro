# iPhone 15 Pro A17 - 17.2.1 Possible font fix

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#15](https://github.com/rooootdev/lara/issues/15)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@rooootdev](https://github.com/rooootdev)  
> **Created:** Mar 30, 2026  
> **Updated:** Mar 31, 2026  
> **Closed:** Mar 31, 2026  

---

## Description

*Opened by [@ghost](https://github.com/ghost)* *on Mar 30, 2026*


It takes numerous tries to change the font however this might help

(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=77572)
(vfs) vm_map entries: 77572, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
font changed to DejaVuSansCondensed 
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x119034000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) task from exploit failed validation, trying proc_ro...
(vfs) task (via proc_ro): 0xffffffe459f6d170
(vfs) vm_map: 0xffffffdf90e97090 (task+0x28, nentries=662)
(vfs) vm_map entries: 662, looking for 0x119034000
(vfs) found entry 0xffffffdf929afba0: 0x119034000-0x119828000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) writing 827348 bytes...
(vfs) zeroed 7507208 bytes of trailing garbage
(vfs) overwrite done!

solution;
switch our_task to proc_ro before walking nmap


---

## Timeline (Verbose)

- Mar 31, 2026: 🔴 Closed by @rooootdev


---

## Comments (2)

### Comment #1 — [@ghost](https://github.com/ghost) · Mar 30, 2026

`ID: 4156401584`


full log:

lara started: 2026-03-30 12:11:56
(utils) PROC_PID_OFFSET: 0x28
downloading kernelcache to: /var/mobile/Containers/Data/Application/D07A8F56-C3D8-4EAF-8A2F-8C8F8FF3A973/Documents/kernelcache
libgrabkernel2: Found firmware URL: https://updates.cdn-apple.com/2023FallFCS/fullrestores/052-18109/661DD240-3D1B-4CB3-BB47-168916048AC1/iPhone16,1_17.2.1_21C66_Restore.ipsw (OTA: no)
libgrabkernel2: Downloading BuildManifest.plist...
libgrabkernel2: Downloading kernelcache.release.iphone16 to /var/mobile/Containers/Data/Application/D07A8F56-C3D8-4EAF-8A2F-8C8F8FF3A973/Documents/kernelcache...
libgrabkernel2: Downloaded kernelcache!
kernel base: 0xfffffff027004000
kernel entry: 0xfffffff02a354000
kernproc symbol: 0xfffffff027980668
rootvnode symbol: 0xfffffff02a5f4e80
kernproc offset: 0x97c668
rootvnode offset: 0x35f0e80
(ds) starting darksword
(ds) device: iPhone16,1
(ds) running on non-a18 device
(ds) read_fd: 0x14
(ds) write_fd: 0x15
(ds) executable_name: lara
(ds) free_thread_arg: 0x124420000
(ds) physical_mapping_address: 0x1123fc000
(ds) pc_object: 0x8f07
(ds) pc_address: 0x344988000
(ds) spraying 22528 sockets...
(ds) socket_ports_count: 0x5800
(ds) start_pcb_id: 0xf2042
(ds) end_pcb_id: 0xfd040
(ds) looking in search mapping: 0
(ds) pcb_start_offset: 0x0
(ds) target_inp_gencnt: 0xf264a
(ds) inp_list_next_pointer: 0xffffffe0c6960400
(ds) icmp6filter: 0xffffffe5905c2ae0
(ds) Corrupting icmp6filter pointer...
(ds) target corrupted: 0xffffffe0c6960548
(ds) found control_socket at idx: 0x304
(utils) kernel proc: 0xffffffe45e1f1480
(utils) looking for pid: 1664
(utils) found proc:  (pid=1664 uid=501 gid=501) @ 0xffffffe459f6ca40
(ds) highest_success_idx: 500
(ds) success_read_count: 65
exploit success!
kernel_base:  0xfffffff050740000
kernel_slide: 0x4973c000
(ds) Walking kernel structures...
(ds) control_socket_pcb: 0xffffffe0c6960000
(ds) pcbinfo_pointer: 0xfffffff053d30948
(ds) ipi_zone: 0xfffffff0510c52f0
(ds) zv_name: 0xfffffff0507ac65d
(ds) searching for kernel Mach-O header from 0xfffffff0507ac000...
(ds) candidate Mach-O at 0xfffffff050748000: filetype=2 cpuinfo=0x2c0000002 (iter=25)
(ds) candidate Mach-O at 0xfffffff050740000: filetype=12 cpuinfo=0xc00000002 (iter=27)
(ds) found MH_FILESET header at 0xfffffff050740000
(ds) kernel_base:  0xfffffff050740000
(ds) kernel_slide: 0x4973c000
(ds) iOS 17: using so_count offset 0x24c
(ds) kernel r/w is ready!
(ds) our_proc: 0xffffffe459f6ca40
(ds) our_task: 0xffffffe459f6d180
exploit success!
kernel_base:  0xfffffff050740000
kernel_slide: 0x4973c000
(vfs) vfs_init starting...
(vfs) Extracted heap PAC prefix: 0xffffff0000000000
(vfs) proc=0xffffffe459f6ca40 task=0xffffffe459f6d180 (from exploit)
(vfs) file overwrite ready!
(vfs) rootvnode offset probe: 0xffffffe3785f9700 (sym=0xfffffff053d30e80)
(vfs) rootvnode via offset: 0xffffffe3785f9700
(vfs) g_rootvnode = 0xffffffe3785f9700
(vfs) first_nc = 0xffffffe295fc08c0
(vfs) namecache candidate dump:
(vfs)   off 0x00 -> 0xffffffe295a29b08
(vfs)   off 0x08 -> 0xffffffe293ea30f8
(vfs)   off 0x10 -> 0xffffffe293ea30f8
(vfs)   off 0x18 -> 0xffffffe3785f9730
(vfs)   off 0x20 -> 0xffffffe29536a258
(vfs)   off 0x28 -> 0xffffffe293ea3118
(vfs)   off 0x30 -> 0xffffffe293114f70
(vfs)   off 0x38 -> 0xffffffe70f3ca6b8
(vfs)   off 0x40 -> 0x100000000
(vfs)   off 0x48 -> 0xffffffe3785f9700
(vfs)   off 0x50 -> 0x0
(vfs)   off 0x58 -> 0x74ead294
(vfs)   off 0x60 -> 0xffffffe45efe01b0
(vfs)   off 0x68 -> 0xffffffe293128720
(vfs)   off 0x70 -> 0xffffffe295371148
(vfs)   off 0x78 -> 0xffffffe296da87f0
(vfs) nc_vp = 0xffffffe3785f9700
(vfs) nc_name = 0x74ead294
(vfs) nc_name not a kernel pointer, skipping
(vfs) ncache verification failed
(vfs) vfs_init done
vfs ready!
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=42010)
(vfs) vm_map entries: 42010, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=47244)
(vfs) vm_map entries: 47244, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=47918)
(vfs) vm_map entries: 47918, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=48580)
(vfs) vm_map entries: 48580, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=48844)
(vfs) vm_map entries: 48844, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=49388)
(vfs) vm_map entries: 49388, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe077490000 (task+0x60, nentries=256)
(vfs) vm_map entries: 256, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=50486)
(vfs) vm_map entries: 50486, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=50524)
(vfs) vm_map entries: 50524, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=50532)
(vfs) vm_map entries: 50532, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=50538)
(vfs) vm_map entries: 50538, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=50538)
(vfs) vm_map entries: 50538, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=50572)
(vfs) vm_map entries: 50572, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=50682)
(vfs) vm_map entries: 50682, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=51388)
(vfs) vm_map entries: 51388, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=51824)
(vfs) vm_map entries: 51824, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=52362)
(vfs) vm_map entries: 52362, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=52878)
(vfs) vm_map entries: 52878, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=53470)
(vfs) vm_map entries: 53470, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=54030)
(vfs) vm_map entries: 54030, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=54518)
(vfs) vm_map entries: 54518, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=54800)
(vfs) vm_map entries: 54800, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSerif.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSerif.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=54820)
(vfs) vm_map entries: 54820, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSerif.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSerif.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=54864)
(vfs) vm_map entries: 54864, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSerif.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSerif.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=54936)
(vfs) vm_map entries: 54936, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSerif.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSerif.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=55196)
(vfs) vm_map entries: 55212, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSerif.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSerif.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=55798)
(vfs) vm_map entries: 55798, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSerif.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSerif.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=56444)
(vfs) vm_map entries: 56444, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=56576)
(vfs) vm_map entries: 56576, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSerif.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSerif.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=56618)
(vfs) vm_map entries: 56618, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=56716)
(vfs) vm_map entries: 56716, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSerif.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSerif.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=56736)
(vfs) vm_map entries: 56736, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=56736)
(vfs) vm_map entries: 56736, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSerif.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSerif.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=56754)
(vfs) vm_map entries: 56754, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=56754)
(vfs) vm_map entries: 56754, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSerif.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSerif.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=56760)
(vfs) vm_map entries: 56760, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=56782)
(vfs) vm_map entries: 56782, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSerif.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSerif.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=56834)
(vfs) vm_map entries: 56834, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=56834)
(vfs) vm_map entries: 56834, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=56868)
(vfs) vm_map entries: 56868, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=56868)
(vfs) vm_map entries: 56868, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=56874)
(vfs) vm_map entries: 56874, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=56874)
(vfs) vm_map entries: 56874, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=57404)
(vfs) vm_map entries: 57420, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=57650)
(vfs) vm_map entries: 57650, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=57650)
(vfs) vm_map entries: 57650, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=57650)
(vfs) vm_map entries: 57650, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=57650)
(vfs) vm_map entries: 57650, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=57660)
(vfs) vm_map entries: 57660, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=57672)
(vfs) vm_map entries: 57672, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=57688)
(vfs) vm_map entries: 57688, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=57688)
(vfs) vm_map entries: 57688, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=57826)
(vfs) vm_map entries: 57826, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe077490000 (task+0x60, nentries=256)
(vfs) vm_map entries: 256, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=58302)
(vfs) vm_map entries: 58302, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=59036)
(vfs) vm_map entries: 59036, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=59350)
(vfs) vm_map entries: 59350, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=59646)
(vfs) vm_map entries: 59646, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=59868)
(vfs) vm_map entries: 59868, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=60456)
(vfs) vm_map entries: 60472, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=60746)
(vfs) vm_map entries: 60746, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=61266)
(vfs) vm_map entries: 61282, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=61548)
(vfs) vm_map entries: 61548, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=62166)
(vfs) vm_map entries: 62182, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=62392)
(vfs) vm_map entries: 62392, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=62810)
(vfs) vm_map entries: 62810, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=63072)
(vfs) vm_map entries: 63072, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=63164)
(vfs) vm_map entries: 63164, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=63680)
(vfs) vm_map entries: 63696, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=64258)
(vfs) vm_map entries: 64274, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=64450)
(vfs) vm_map entries: 64450, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=64612)
(vfs) vm_map entries: 64612, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=65416)
(vfs) vm_map entries: 65416, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=66134)
(vfs) vm_map entries: 66134, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=66582)
(vfs) vm_map entries: 66582, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=66766)
(vfs) vm_map entries: 66766, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=67182)
(vfs) vm_map entries: 67182, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=67736)
(vfs) vm_map entries: 67756, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=68168)
(vfs) vm_map entries: 68168, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=68910)
(vfs) vm_map entries: 68910, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=69032)
(vfs) vm_map entries: 69032, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=69374)
(vfs) vm_map entries: 69374, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=69842)
(vfs) vm_map entries: 69842, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=70134)
(vfs) vm_map entries: 70134, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=70510)
(vfs) vm_map entries: 70510, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=70916)
(vfs) vm_map entries: 70916, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=70998)
(vfs) vm_map entries: 70998, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=71566)
(vfs) vm_map entries: 71582, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=72310)
(vfs) vm_map entries: 72310, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=72444)
(vfs) vm_map entries: 72444, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=72444)
(vfs) vm_map entries: 72444, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=72828)
(vfs) vm_map entries: 72828, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=73134)
(vfs) vm_map entries: 73134, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=73234)
(vfs) vm_map entries: 73234, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=73360)
(vfs) vm_map entries: 73360, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=73366)
(vfs) vm_map entries: 73366, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=73564)
(vfs) vm_map entries: 73564, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=73938)
(vfs) vm_map entries: 73938, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=74020)
(vfs) vm_map entries: 74020, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=74064)
(vfs) vm_map entries: 74064, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=74210)
(vfs) vm_map entries: 74210, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=74316)
(vfs) vm_map entries: 74316, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=74576)
(vfs) vm_map entries: 74576, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=74784)
(vfs) vm_map entries: 74784, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=74926)
(vfs) vm_map entries: 74926, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe077490000 (task+0x60, nentries=256)
(vfs) vm_map entries: 256, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=75914)
(vfs) vm_map entries: 75914, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=76914)
(vfs) vm_map entries: 76914, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
failed to change font
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x112414000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) vm_map: 0xffffffe45f5d3f80 (task+0x98, nentries=77572)
(vfs) vm_map entries: 77572, looking for 0x112414000
(vfs) vm_map_entry not found for 0x112414000
(vfs) using font: /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
font changed to DejaVuSansCondensed
(vfs) overwrite: /System/Library/Fonts/Core/SFUI.ttf <- /private/var/containers/Bundle/Application/C73C0BA3-FAAE-45E6-AF9F-18D26839C7C9/lara.app/DejaVuSansCondensed.ttf
(vfs) target mmap'd at 0x119034000 (size 8334556)
(vfs) task (from exploit): 0xffffffe459f6d180
(vfs) task from exploit failed validation, trying proc_ro...
(vfs) task (via proc_ro): 0xffffffe459f6d170
(vfs) vm_map: 0xffffffdf90e97090 (task+0x28, nentries=662)
(vfs) vm_map entries: 662, looking for 0x119034000
(vfs) found entry 0xffffffdf929afba0: 0x119034000-0x119828000
(vfs) patching entry flags: 0x20020880 -> 0x20021980
(vfs) writing 827348 bytes...
(vfs) zeroed 7507208 bytes of trailing garbage
(vfs) overwrite done!


---

### Comment #2 — [@rooootdev](https://github.com/rooootdev) · Mar 31, 2026

`ID: 4162067455`


fixed.


---

*Exported on 10/14/2026, 2:47:24 PM from [https://github.com/rooootdev/lara/issues/15](https://github.com/rooootdev/lara/issues/15)*
