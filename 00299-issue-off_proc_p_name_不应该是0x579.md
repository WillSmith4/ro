# off_proc_p_name 不应该是0x579

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Issue:** [#299](https://github.com/rooootdev/lara/issues/299)  
> **State:** 🟢 open  
> **Labels:** `bug`  
> **Created:** Jun 18, 2026  
> **Updated:** Jun 18, 2026  

---

## Description

*Opened by [@QuiYou](https://github.com/QuiYou)* *on Jun 18, 2026*


off_proc_p_name = 0x381; 才是对的因为iPhone11,6_16.2_20C65_Restore.ipsw
在IDA 看到的是
char *__cdecl proc_best_name(proc_t p)
{
  if ( *((_BYTE *)p + 0x381) )
    return (char *)p + 0x381;
  else
    return (char *)p + 0x370;
}


---

## Timeline (Verbose)

- Jun 18, 2026: 🏷️ @QuiYou added label `bug`


---

## Comments (1)

### Comment #1 — [@neonmodder123](https://github.com/neonmodder123) · Jun 18, 2026

`ID: 4737701380`


“Lara”在自动获取“kernelcache”后应该使用它，这个漏洞利用对你有效吗？


---

*Exported on 10/14/2026, 3:11:35 PM from [https://github.com/rooootdev/lara/issues/299](https://github.com/rooootdev/lara/issues/299)*
