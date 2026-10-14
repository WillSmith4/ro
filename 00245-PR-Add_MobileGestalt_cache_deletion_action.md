# Add MobileGestalt cache deletion action

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#245](https://github.com/rooootdev/lara/pull/245)  
> **State:** 🔴 closed  
> **Branch:** `hxhlb:codex/mg-cache-reset` → `rooootdev:main`  
> **Changes:** +56 / -0 across 1 files  
> **Created:** May 14, 2026  
> **Updated:** May 15, 2026  
> **Closed:** May 15, 2026  

---

## Description

*Opened by [@hxhlb](https://github.com/hxhlb)* *on May 14, 2026*


## Summary

Add a dangerous recovery action for deleting the system MobileGestalt cache file and Lara's saved MobileGestalt backup.

This is intended for cases where MobileGestalt edits leave the device in a bad state and the user wants iOS to regenerate a clean MobileGestalt cache after a full reboot.

I am sending this as a separate PR because this is a destructive recovery action and I only have two test environments available. It has not been broadly tested across devices or iOS versions, so it should be reviewed independently from smaller MobileGestalt fixes.

## Changes

- Add a `Danger Zone` section to `GestaltFileView`.
- Add a `Delete MobileGestalt Cache` destructive action.
- Delete both:
  - the system MobileGestalt cache file
  - Lara's `SavedGestalt.plist` backup
- Require SBX before enabling the action.
- Show a destructive confirmation alert before deleting.
- Automatically exit Lara after successful deletion to avoid recreating a bad saved backup before reboot.
- The in-app warning tells the user to fully power off and turn the device back on; a respring is not enough.

## Testing

Tested manually on the two devices/environments I have available:

- iOS 17.3.1
- iOS 18.6

Observed behavior:

- The action successfully deletes the system MobileGestalt cache file.
- The action successfully deletes Lara's saved MobileGestalt backup.
- Lara exits after deletion.
- After a full reboot, iOS regenerates the MobileGestalt cache.

Because I only have these two test environments, this has not been fully validated on other devices or iOS versions.


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/25842559677/job/75930815637) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/25842559677/job/75930716379) — **success** — *GitHub Actions*


---

## PR Commits (1)

- [`4d6c550`](https://github.com/rooootdev/lara/commit/4d6c5504ea148de0b8e8668af0380b73dddab1d9) Add MobileGestalt cache deletion action — *Hades* · May 14, 2026


---

## Changed Files (1)

- 🟡 `lara/views/tweaks/ui/mobilegestalt/GestaltFileView.swift` (modified) — +56 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -5,9 +5,11 @@
 //  Created by lunginspector on 5/11/26.
 //
 
+import Darwin
 import SwiftUI
 
 struct GestaltFileView: View {
+    @ObservedObject private var mgr = laramgr.shared
     @State private var mgCurrentDict: NSMutableDictionary = NSMutableDictionary()
     @State private var mgCacheExtra: [String : Any] = [:]
     @Environment(\.dismiss) var dismiss
@@ -28,6 +30,15 @@ struct GestaltFileView: View {
                         mgCurrentExport()
                     })
                 }
+
+                Section(header: HeaderLabel(text: "Danger Zone", icon: "exclamationmark.triangle"), footer: Text("Deletes the system MobileGestalt cache file and Lara's saved backup. A full device reboot is required so iOS can regenerate a clean MobileGestalt cache.")) {
+                    Button(role: .destructive) {
+                        confirmMobileGestaltCacheDeletion()
+                    } label: {
+                        Text("Delete MobileGestalt Cache")
+                    }
+                    .disabled(!mgr.sbxready)
+                }
                 
                 Section(header: HeaderLabel(text: "CacheExtra", icon: "doc")) {
                     ForEach(mgCacheExtra.keys.sorted(), id: \.self) { key in
@@ -55,6 +66,51 @@ struct GestaltFileView: View {
             }
         }
     }
+
+    func confirmMobileGestaltCacheDeletion() {
+        Alertinator.shared.alert(
+            title: "Delete MobileGestalt Cache?",
+            body: "This may cause system issues if used incorrectly. Lara will automatically exit after deleting the system MobileGestalt cache file to prevent creating a bad saved backup. Fully power off the device and turn it back on immediately after Lara exits. A respring is not enough.",
+            actionLabel: "Delete and Exit",
+            action: deleteMobileGestaltCache
+        )
+    }
+
+    func deleteMobileGestaltCache() {
+        guard let documentsURL = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask).first else {
+            Alertinator.shared.alert(title: "Failed to Delete MobileGestalt Cache", body: "Could not locate Lara's Documents directory.")
+            return
+        }
+
+        let savedURL = documentsURL.appendingPathComponent("SavedGestalt.plist")
+        let filesystemURL = URL(fileURLWithPath: mgCurrentPath)
+        let fm = FileManager.default
+
+        var deletedCount = 0
+        var failures: [String] = []
+
+        for url in [filesystemURL, savedURL] {
+            do {
+                if fm.fileExists(atPath: url.path) {
+                    try fm.removeItem(at: url)
+                    deletedCount += 1
+                }
+            } catch {
+                failures.append("\(url.path): \(error.localizedDescription)")
+            }
+        }
+
+        if failures.isEmpty {
+            mgCurrentDict = NSMutableDictionary()
+            mgCacheExtra = [:]
+            print("(mg) deleted MobileGestalt cache files: \(deletedCount). Exiting so the user can fully reboot.")
+            DispatchQueue.main.asyncAfter(deadline: .now() + 0.25) {
+                exit(0)
+            }
+        } else {
+            Alertinator.shared.alert(title: "Failed to Delete MobileGestalt Cache", body: failures.joined(separator: "\n"))
+        }
+    }
     
     func mgSavedExport() {
         let documentsURL = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask).first
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 4d6c550: Add MobileGestalt cache deletion action
- May 15, 2026: 🔴 Closed by @lunginspector


---

## Comments (1)

### Comment #1 — [@lunginspector](https://github.com/lunginspector) · May 15, 2026

`ID: 4455097850`


this seems incredibly risky to blindly pull, cause it has the possibility to brick the device if it doesn't work how it's intended. i'll have to close this one, but i do like the idea here.


---

*Exported on 10/14/2026, 3:06:51 PM from [https://github.com/rooootdev/lara/pull/245](https://github.com/rooootdev/lara/pull/245)*
