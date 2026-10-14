# Fix OTA Update & Screen Time status persistence after app reinstall

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#289](https://github.com/rooootdev/lara/pull/289)  
> **State:** 🟣 merged  
> **Branch:** `A11pwnX:Fix` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Jun 6, 2026  
> **Changes:** +52 / -10 across 2 files  
> **Created:** Jun 3, 2026  
> **Updated:** Jun 6, 2026  
> **Closed:** Jun 6, 2026  

---

## Description

*Opened by [@A11pwnX](https://github.com/A11pwnX)* *on Jun 3, 2026*


This PR fixes an issue where the OTA Update and Screen Time status was incorrectly shown as Enabled after reinstalling the application.

Instead of relying on the app’s local storage, the app now directly checks the real system state from:

/private/var/db/com.apple.xpc.launchd/disabled.plist

This ensures the correct status is preserved even after the app is deleted and reinstalled.

Changes

- Read OTA Update status from disabled.plist
- Read Screen Time status from disabled.plist
- Fixed incorrect default Enabled state after reinstall
- Removed the unnecessary respring button from:
    - OTAView.swift
    - ScreenTimeView.swift

Result

The UI now always reflects the actual system configuration instead of temporary app storage values.


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/26889376866/job/79311027900) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/26889376866/job/79310681641) — **success** — *GitHub Actions*


---

## PR Commits (2)

- [`4f25602`](https://github.com/rooootdev/lara/commit/4f2560276fd2f134043e8725041ec4f35f104e1e) Implement syncStateFromPlist to verify OTA status — *A11pwnX* · Jun 3, 2026
- [`028243a`](https://github.com/rooootdev/lara/commit/028243afbe8cdf0780d0b9f9334f66a3ebde2571) Implement syncStateFromPlist to manage Screen Time state — *A11pwnX* · Jun 3, 2026


---

## Changed Files (2)

- 🟡 `lara/views/tweaks/OTAView.swift` (modified) — +27 / -5
  <details><summary>Diff</summary>

  ```diff
  @@ -51,21 +51,43 @@ struct OTAView: View {
                     }
                 }
                 .disabled(isWorking || !otaDisabled)
-
-                Button("Respring") {
-                    mgr.respring()
-                }
-                .disabled(isWorking)
             }
         }
         .navigationTitle("OTA Updates")
+        .onAppear {
+            if !otaDisabled {
+                syncStateFromPlist()
+            }
+        }
         .alert("Result", isPresented: .constant(lastResult != nil)) {
             Button("OK") { lastResult = nil }
         } message: {
             Text(lastResult ?? "")
         }
     }
 
+    private func syncStateFromPlist() {
+        DispatchQueue.global(qos: .userInitiated).async {
+            let plistPath = "/private/var/db/com.apple.xpc.launchd/disabled.plist"
+            let daemonLabels = [
+                "com.apple.mobile.softwareupdated",
+                "com.apple.OTATaskingAgent",
+                "com.apple.softwareupdateservicesd",
+                "com.apple.mobile.NRDUpdated",
+            ]
+            guard let data = NSData(contentsOfFile: plistPath) as Data?,
+                  let plist = try? PropertyListSerialization.propertyList(from: data, options: [], format: nil) as? [String: Any] else {
+                return
+            }
+            let allDisabled = daemonLabels.allSatisfy { (plist[$0] as? Bool) == true }
+            if allDisabled {
+                DispatchQueue.main.async {
+                    otaDisabled = true
+                }
+            }
+        }
+    }
+
     private func apply(disabled: Bool) {
         isWorking = true
         DispatchQueue.global(qos: .userInitiated).async {
  ```

  </details>
- 🟡 `lara/views/tweaks/ScreenTimeView.swift` (modified) — +25 / -5
  <details><summary>Diff</summary>

  ```diff
  @@ -80,21 +80,41 @@ struct ScreenTimeView: View {
                     }
                 }
                 .disabled(isWorking || !screenTimeDisabled)
-
-                Button("Respring") {
-                    mgr.respring()
-                }
-                .disabled(isWorking)
             }
         }
         .navigationTitle("Screen Time")
+        .onAppear {
+            if !screenTimeDisabled {
+                syncStateFromPlist()
+            }
+        }
         .alert("Result", isPresented: .constant(lastResult != nil)) {
             Button("OK") { lastResult = nil }
         } message: {
             Text(lastResult ?? "")
         }
     }
 
+    private func syncStateFromPlist() {
+        DispatchQueue.global(qos: .userInitiated).async {
+            let plistPath = "/private/var/db/com.apple.xpc.launchd/disabled.plist"
+            let daemonLabels = [
+                "com.apple.ScreenTimeAgent",
+                "com.apple.UsageTrackingAgent",
+            ]
+            guard let data = NSData(contentsOfFile: plistPath) as Data?,
+                  let plist = try? PropertyListSerialization.propertyList(from: data, options: [], format: nil) as? [String: Any] else {
+                return
+            }
+            let allDisabled = daemonLabels.allSatisfy { (plist[$0] as? Bool) == true }
+            if allDisabled {
+                DispatchQueue.main.async {
+                    screenTimeDisabled = true
+                }
+            }
+        }
+    }
+
     private func applyDisable() {
         isWorking = true
         let agent = killScreenTimeAgent
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 4f25602: Implement syncStateFromPlist to verify OTA status
- : 📝 Commit 028243a: Implement syncStateFromPlist to manage Screen Time state
- Jun 6, 2026: 🟣 @rooootdev merged
- Jun 6, 2026: 🔴 Closed by @rooootdev


---

*Exported on 10/14/2026, 3:10:42 PM from [https://github.com/rooootdev/lara/pull/289](https://github.com/rooootdev/lara/pull/289)*
