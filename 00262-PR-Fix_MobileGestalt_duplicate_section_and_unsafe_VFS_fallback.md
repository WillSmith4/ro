# Fix MobileGestalt duplicate section and unsafe VFS fallback

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#262](https://github.com/rooootdev/lara/pull/262)  
> **State:** 🟣 merged  
> **Branch:** `hxhlb:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on May 23, 2026  
> **Changes:** +11 / -52 across 2 files  
> **Created:** May 23, 2026  
> **Updated:** May 25, 2026  
> **Closed:** May 23, 2026  

---

## Description

*Opened by [@hxhlb](https://github.com/hxhlb)* *on May 23, 2026*


## Summary
- Remove a duplicated MobileGestalt section labeled `Internal`.
- Add a `fallback_vfs` option to `lara_overwritefile`, defaulting to `true` so existing callers keep the same behavior.
- Disable VFS fallback specifically for MobileGestalt writes.

## Why
The duplicated `Internal` section contained the same SpringBoard/Pasteboard toggles already shown under `SpringBoard`.

MobileGestalt writes are sensitive to file size changes. The sandbox overwrite path opens the file with `O_TRUNC`, so it can safely replace the plist with a different-sized serialized plist. The VFS overwrite fallback cannot safely resize the target file: it rejects larger sources and zero-fills trailing bytes for smaller sources. If sandbox overwrite fails and MobileGestalt falls back to VFS, the plist can become corrupted.

This keeps VFS fallback enabled by default for other overwrite users, but prevents it for MobileGestalt.

## Testing
- Tested on iPhone 13 Pro Max running iOS 17.3.1.
- Verified the MobileGestalt page no longer shows the duplicated `Internal` section.
- Verified MobileGestalt apply succeeds through the sandbox overwrite path without using VFS fallback.


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/26320827793/job/77489444980) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/26320827793/job/77489382578) — **success** — *GitHub Actions*


---

## PR Commits (2)

- [`6afd43c`](https://github.com/rooootdev/lara/commit/6afd43ccd66962937875a631c6cefac2486f5455) Remove duplicate MobileGestalt internal section — *Hades* · May 23, 2026
- [`758fa7e`](https://github.com/rooootdev/lara/commit/758fa7e755212075373b883c4291c3bfd5e9c310) Prevent MobileGestalt VFS fallback writes — *Hades* · May 23, 2026


---

## Changed Files (2)

- 🟡 `lara/classes/laramgr.swift` (modified) — +10 / -2
  <details><summary>Diff</summary>

  ```diff
  @@ -351,7 +351,7 @@ final class laramgr: ObservableObject {
     }
     
     @discardableResult
-    func lara_overwritefile(target: String, source: String) -> (ok: Bool, message: String) {
+    func lara_overwritefile(target: String, source: String, fallback_vfs: Bool = true) -> (ok: Bool, message: String) {
         guard FileManager.default.fileExists(atPath: source) else {
             return (false, "source file not found: \(source)")
         }
@@ -371,6 +371,10 @@ final class laramgr: ObservableObject {
         if result.ok {
             return result
         }
+
+        guard fallback_vfs else {
+            return result
+        }
         
         guard vfsready else {
             return (false, result.message + " | vfs not ready")
@@ -381,11 +385,15 @@ final class laramgr: ObservableObject {
     }
     
     @discardableResult
-    func lara_overwritefile(target: String, data: Data) -> (ok: Bool, message: String) {
+    func lara_overwritefile(target: String, data: Data, fallback_vfs: Bool = true) -> (ok: Bool, message: String) {
         let result = sbxready ? sbxoverwrite(path: target, data: data) : (false, "sbx not ready")
         if result.0 {
             return result
         }
+
+        guard fallback_vfs else {
+            return result
+        }
         
         guard vfsready else {
             return (false, result.1 + ", vfs not ready")
  ```

  </details>
- 🟡 `lara/views/tweaks/mobilegestalt/GestaltView.swift` (modified) — +1 / -50
  <details><summary>Diff</summary>

  ```diff
  @@ -287,56 +287,7 @@ struct GestaltView: View {
                 } header: {
                     HeaderLabel(text: "SpringBoard", icon: "gear")
                 }
-                
-                Section {
-                    PlainToggle(
-                        text: "Disable Lock After Respring",
-                        icon: "lock.open",
-                        isOn: nuggetbinding(
-                            "SBDontLockAfterCrash",
-                            path: fileloc.springboard.rawValue
-                        )
-                    )
-
-                    PlainToggle(
-                        text: "Disable Low Battery Alerts",
-                        icon: "battery.25",
-                        isOn: nuggetbinding(
-                            "SBHideLowPowerAlerts",
-                            path: fileloc.springboard.rawValue
-                        )
-                    )
-
-                    PlainToggle(
-                        text: "Show Dynamic Island in Screenshots",
-                        icon: "camera",
-                        isOn: nuggetbinding(
-                            "SBAlwaysShowSystemApertureInSnapshots",
-                            path: fileloc.springboard.rawValue
-                        )
-                    )
-
-                    PlainToggle(
-                        text: "Play Sound on Paste",
-                        icon: "speaker.wave.2",
-                        isOn: nuggetbinding(
-                            "PlaySoundOnPaste",
-                            path: fileloc.pasteboard.rawValue
-                        )
-                    )
 
-                    PlainToggle(
-                        text: "System Paste Notifications",
-                        icon: "doc.on.clipboard",
-                        isOn: nuggetbinding(
-                            "AnnounceAllPastes",
-                            path: fileloc.pasteboard.rawValue
-                        )
-                    )
-                } header: {
-                    HeaderLabel(text: "Internal", icon: "ant")
-                }
-                
                 Section {
                     PlainToggle(
                         text: "Metal HUD Debug",
@@ -482,7 +433,7 @@ struct GestaltView: View {
             
             // bro please dont bootloop
             let mgData = try verifyPlist(mgCurrentDict, targetPath: mgCurrentPath)
-            let result = mgr.lara_overwritefile(target: mgCurrentPath, data: mgData)
+            let result = mgr.lara_overwritefile(target: mgCurrentPath, data: mgData, fallback_vfs: false)
             
             if result.ok {
                 Alertinator.shared.alert(title: "Successfully applied MobileGestalt!", body: "Respring to see any changes", actionLabel: "Respring", action: { mgr.respring() })
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 6afd43c: Remove duplicate MobileGestalt internal section
- : 📝 Commit 758fa7e: Prevent MobileGestalt VFS fallback writes
- May 23, 2026: ✏️ @hxhlb renamed: "Fix duplicate MobileGestalt Internal section" → "Fix MobileGestalt duplicate section and unsafe VFS fallback"
- May 23, 2026: 🟣 @rooootdev merged
- May 23, 2026: 🔴 Closed by @rooootdev


---

## Comments (3)

### Comment #1 — [@Lafu666](https://github.com/Lafu666) · May 23, 2026

`ID: 4524247828`


Can you add the function of cleaning system data?


---

### Comment #2 — [@hxhlb](https://github.com/hxhlb) · May 23, 2026

`ID: 4525646640`


> Can you add the function of cleaning system data?

Could you clarify what you mean by “system data”? Do you mean things like cache, logs, OTA files, temporary files, etc.?


---

### Comment #3 — [@Lafu666](https://github.com/Lafu666) · May 25, 2026

`ID: 4531621625`


> > Can you add the function of cleaning system data?
> 
> Could you clarify what you mean by “system data”? Do you mean things like cache, logs, OTA files, temporary files, etc.?

Yes


---

*Exported on 10/14/2026, 3:08:19 PM from [https://github.com/rooootdev/lara/pull/262](https://github.com/rooootdev/lara/pull/262)*
