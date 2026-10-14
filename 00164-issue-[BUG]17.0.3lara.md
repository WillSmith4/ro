# [BUG]在17.0.3上无法使用lara

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#164](https://github.com/rooootdev/lara/issues/164)  
> **State:** 🟣 closed (completed)  
> **Closed by:** [@rooootdev](https://github.com/rooootdev)  
> **Labels:** `bug`  
> **Assignees:** [@Pro42good](https://github.com/Pro42good)  
> **Created:** Apr 22, 2026  
> **Updated:** May 10, 2026  
> **Closed:** May 10, 2026  

---

## Description

*Opened by [@bd5g2bp2xk-lgtm](https://github.com/bd5g2bp2xk-lgtm)* *on Apr 22, 2026*


### 描述错误：
<!--对错误是什么进行清晰简洁的描述
###重现：
重现行为的步骤：
1.转到“...”
2.点击“...”
3.向下滚动到“...”
4.看到错误

### 预期行为：
<!--你期待发生的事情。-->
``[卡进度%30一直在重复失败]``

### 截屏：
<!--如果适用，请添加屏幕截图以帮助解释您的问题。-->

### 设备信息：
- **设备：** ``[iPhone15ProMax 17.0.3]`` <!-- 例如iPhone 13 Pro、iPad Air 5 -->
- **芯片：** ``[A17]`` <!-- 例如A15、A14、M1 -->
- **iOS版本：** ``[17.0.3]`` <!-- 例如16.5（20F66）—包括構建號-->
- **以前越狱过吗？** ``[是]`` <!-- 是/否/首次在此设备上尝试-->
- **Lara版本/提交：** ``[1.2]`` <!-- 例如v0.3或提交哈希 -->

### 日志：
<!--粘贴崩溃日志、控制台输出或内核崩溃文本。使用```栅栏。-->
````

````lara started: 2026-04-22 06:28:51

(utils) TASK_TNEXT_OFFSET: 0x58

(utils) THREAD_MUPCB_OFFSET: 0x100

(utils) PROC_PID_OFFSET: 0x28

(offs) loaded saved t1sz_boot: 0x19

(offs) t1sz_boot: 0x19

(offs) saved t1sz_boot: 0x19

initialized offsets

(offs) loaded saved t1sz_boot: 0x19

(offs) t1sz_boot: 0x19

(offs) saved t1sz_boot: 0x19

initialized offsets

(ds) starting darksword
(ds) device: iPhone16,2
(ds) ispac: yes
(ds) running on non-a18 device
(ds) read_fd: 0x9
(ds) write_fd: 0xa
(ds) executable_path: /private/var/containers/Bundle/Application/7CAE203E-4BC7-480E-ACD1-C93D5E360A6E/lara.app/lara
(ds) host_executable_path: /private/var/containers/Bundle/Application/7CAE203E-4BC7-480E-ACD1-C93D5E360A6E/lara.app/lara
(ds) guest_executable_name: lara
(ds) host_executable_name: lara
(ds) kernel_process_name: lara
(ds) livecontainer_bundle: no
(ds) livecontainer_guest: no
(ds) rehosted_process: no
(ds) process_marker[0]: lara
(ds) executable_name: lara
(ds) free_thread_arg: 0x10f4f0000
(ds) physical_mapping_address: 0x110aac000
(ds) pc_object: 0x6503
(ds) pc_address: 0x329e28000
(ds) spraying 22528 sockets...
(ds) socket_ports_count: 0x5800
(ds) start_pcb_id: 0x420e0
(ds) end_pcb_id: 0x4d0de
(ds) looking in search mapping: 0
### 其他上下文：
<!--其他任何内容 — 最近的iOS更新、已安装的调整、之前的越狱状态等。-->
``[在此插入]``

### 提交前核对清单：
<!--请提交前确认以下所有内容。缺乏这些问题可能会被关闭。-->
- [ ] 我搜索了现有问题（已打开和已关闭），并确认这不是重复的
- [ ]我可以在Lara的最新版本/提交上重现此
- [ ]我已经提供了足够的细节（设备信息、日志、复制步骤）供维护人员调查
- [ ] 我同意与开发人员相互尊重地沟通，并理解骚扰可能会导致支持受限
<!--要标记一个检查，在括号内放一个“x”，像这样[x] -->


---

## Timeline (Verbose)

- Apr 22, 2026: 👤 @bd5g2bp2xk-lgtm assigned @Pro42good
- Apr 22, 2026: 🏷️ @bd5g2bp2xk-lgtm added label `bug`
- May 10, 2026: 🔴 Closed by @rooootdev


---

## Comments (15)

### Comment #1 — [@bd5g2bp2xk-lgtm](https://github.com/bd5g2bp2xk-lgtm) · Apr 22, 2026

`ID: 4292272645`


<img width="1290" height="2796" alt="Image" src="https://github.com/user-attachments/assets/cee31ea4-c276-4d3c-9cbc-5c828ba3192d" />
<img width="1290" height="2796" alt="Image" src="https://github.com/user-attachments/assets/f9b72e51-a389-4dd7-9648-662adeadab6b" />


---

### Comment #2 — [@ghost](https://github.com/ghost) · Apr 22, 2026

`ID: 4292719299`


Did you use 0x11 instead of 0x19 in the t1sz?


---

### Comment #3 — [@bd5g2bp2xk-lgtm](https://github.com/bd5g2bp2xk-lgtm) · Apr 22, 2026

`ID: 4293435355`


> 你在t1sz中使用了0x11而不是0x19吗？

是0x19，系统默认的我并没有去更改


---

### Comment #4 — [@bd5g2bp2xk-lgtm](https://github.com/bd5g2bp2xk-lgtm) · Apr 22, 2026

`ID: 4293440291`


> 你在t1sz中使用了0x11而不是0x19吗？

<img width="1290" height="2796" alt="Image" src="https://github.com/user-attachments/assets/1258dbde-7aa9-4626-bcdd-3dd1e2322f49" />


---

### Comment #5 — [@tiziodied](https://github.com/tiziodied) · Apr 22, 2026

`ID: 4293955264`


you should change it to 0x11, then it should work


---

### Comment #6 — [@bd5g2bp2xk-lgtm](https://github.com/bd5g2bp2xk-lgtm) · Apr 22, 2026

`ID: 4294177607`


> you should change it to 0x11, then it should work

我尝试切换成0x11依旧不行，很纳闷


---

### Comment #7 — [@iamshaoyu](https://github.com/iamshaoyu) · Apr 22, 2026

`ID: 4295099093`


一样卡30，就没成功过


---

### Comment #8 — [@bd5g2bp2xk-lgtm](https://github.com/bd5g2bp2xk-lgtm) · Apr 22, 2026

`ID: 4295107903`


> 一样卡30，就没成功过
> 

看来不是我个人的问题，你也是 17.0.3？


---

### Comment #9 — [@iamshaoyu](https://github.com/iamshaoyu) · Apr 22, 2026

`ID: 4295337400`


> > 一样卡30，就没成功过
> 
> 看来不是我个人的问题，你也是 17.0.3？

17.0


---

### Comment #10 — [@rooootdev](https://github.com/rooootdev) · Apr 22, 2026

`ID: 4297984547`


did you click "Save Offsets"?


---

### Comment #11 — [@bd5g2bp2xk-lgtm](https://github.com/bd5g2bp2xk-lgtm) · Apr 22, 2026

`ID: 4299237121`


> did you click "Save Offsets"?点击了，依旧是这个样子


---

### Comment #12 — [@iamshaoyu](https://github.com/iamshaoyu) · Apr 23, 2026

`ID: 4301395525`


Lara.log发给AI查询说是偏移量错误，等作者添加吧


---

### Comment #13 — [@Pro42good](https://github.com/Pro42good) · May 4, 2026

`ID: 4373314882`


Hello, This is a copy paste response, Im just trying to close issues.

But could you try on the latest official version and or on the latest nightly version of lara?

Please let me know if its fixed it's self or you can close the issue.


Oh and please issues in English only


---

### Comment #14 — [@bd5g2bp2xk-lgtm](https://github.com/bd5g2bp2xk-lgtm) · May 4, 2026

`ID: 4373524320`


> 你好，这是一个复制粘贴的回复，我只是想关闭问题。
> 
> 但你能试试最新的官方版本或最新的夜间版lara吗？
> 
> 请告诉我它是否自行修复，或者您可以关闭该问题。
> 
> 哦，請只用英文提出問題

I just tried it, and even though the progress has reached 8/8, I still can't use Lara! Do you need me to provide screenshots or running logs?


---

### Comment #15 — [@bd5g2bp2xk-lgtm](https://github.com/bd5g2bp2xk-lgtm) · May 4, 2026

`ID: 4373592983`


> Hello, This is a copy paste response, Im just trying to close issues.
> 
> But could you try on the latest official version and or on the latest nightly version of lara?
> 
> Please let me know if its fixed it's self or you can close the issue.
> 
> Oh and please issues in English only

(ds) starting darksword
(ds) device: iPhone16,2
(ds) ispac: yes
(ds) running on non-a18 device
(ds) read_fd: 0x9
(ds) write_fd: 0xa
(ds) executable_path: /private/var/containers/Bundle/Application/05500722-C4B7-4C15-980C-0F4E350D8062/lara.app/lara
(ds) host_executable_path: /private/var/containers/Bundle/Application/05500722-C4B7-4C15-980C-0F4E350D8062/lara.app/lara
(ds) guest_executable_name: lara
(ds) host_executable_name: lara
(ds) kernel_process_name: lara
(ds) livecontainer_bundle: no
(ds) livecontainer_guest: no
(ds) rehosted_process: no
(ds) process_marker[0]: lara
(ds) executable_name: lara
(ds) free_thread_arg: 0x108fe0000
(ds) physical_mapping_address: 0x108ff0000
(ds) pc_object: 0x8903
(ds) pc_address: 0xac0b58000
(ds) spraying 22528 sockets...
(ds) socket_ports_count: 0x5800
(ds) start_pcb_id: 0x8
(ds) end_pcb_id: 0xb006
(ds) looking in search mapping: 0 / 8 (0.00%)
(ds) looking in search mapping: 1 / 8 (12.50%)
(ds) looking in search mapping: 2 / 8 (25.00%)
(ds) looking in search mapping: 3 / 8 (37.50%)
(ds) looking in search mapping: 4 / 8 (50.00%)
(ds) looking in search mapping: 5 / 8 (62.50%)
(ds) looking in search mapping: 6 / 8 (75.00%)
(ds) looking in search mapping: 7 / 8 (87.50%)
(ds) retrying ds...
(ds) spraying 22528 sockets...
(ds) socket_ports_count: 0x5800
(ds) start_pcb_id: 0x16008
(ds) end_pcb_id: 0x21006
(ds) looking in search mapping: 0 / 8 (0.00%)


---

*Exported on 10/14/2026, 2:59:33 PM from [https://github.com/rooootdev/lara/issues/164](https://github.com/rooootdev/lara/issues/164)*
