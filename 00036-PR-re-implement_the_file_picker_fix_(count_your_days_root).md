# re-implement the file picker fix (count your days root)

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#36](https://github.com/rooootdev/lara/pull/36)  
> **State:** 🟣 merged  
> **Branch:** `lunginspector:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 6, 2026  
> **Changes:** +12 / -5 across 3 files  
> **Created:** Apr 6, 2026  
> **Updated:** Apr 6, 2026  
> **Closed:** Apr 6, 2026  

---

## Description

*Opened by [@lunginspector](https://github.com/lunginspector)* *on Apr 6, 2026*


*No description provided.*


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (1)

- [`505a3ac`](https://github.com/rooootdev/lara/commit/505a3ac66ed57b7d5a28e5cad11d8a98f3e6fd6a) re-implement the file picker fix — *lunginspector* · Apr 6, 2026


---

## Changed Files (3)

- 🟡 `lara/lara.swift` (modified) — +12 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -6,9 +6,16 @@
 //
 
 import SwiftUI
+import UniformTypeIdentifiers
 
 let g_isunsupported: Bool = isunsupported()
 
+extension UIDocumentPickerViewController {
+    @objc func fix_init(forOpeningContentTypes contentTypes: [UTType], asCopy: Bool) -> UIDocumentPickerViewController {
+        return fix_init(forOpeningContentTypes: contentTypes, asCopy: true)
+    }
+}
+
 @main
 struct lara: App {
     @ObservedObject private var mgr = laramgr.shared
@@ -20,6 +27,11 @@ struct lara: App {
     @AppStorage("selectedmethod") private var selectedmethod: method = .sbx
 
     init() {
+        // fix file picker
+        let fixMethod = class_getInstanceMethod(UIDocumentPickerViewController.self, #selector(UIDocumentPickerViewController.fix_init(forOpeningContentTypes:asCopy:)))!
+        let origMethod = class_getInstanceMethod(UIDocumentPickerViewController.self, #selector(UIDocumentPickerViewController.init(forOpeningContentTypes:asCopy:)))!
+        method_exchangeImplementations(origMethod, fixMethod)
+        
         if UserDefaults.standard.string(forKey: "selectedmethod") == nil {
             UserDefaults.standard.set(method.sbx.rawValue, forKey: "selectedmethod")
         }
  ```

  </details>
- 🟡 `lara/views/CustomView.swift` (modified) — +0 / -3
  <details><summary>Diff</summary>

  ```diff
  @@ -70,9 +70,6 @@ struct CustomView: View {
 
     private func importSource(_ url: URL) {
         let fm = FileManager.default
-        guard url.startAccessingSecurityScopedResource() else { return }
-        defer { url.stopAccessingSecurityScopedResource() }
-
         let tmpDir = URL(fileURLWithPath: NSTemporaryDirectory(), isDirectory: true)
         let dest = tmpDir.appendingPathComponent("vfs_custom_\(UUID().uuidString)")
 
  ```

  </details>
- 🟡 `lara/views/FontPicker.swift` (modified) — +0 / -2
  <details><summary>Diff</summary>

  ```diff
  @@ -128,8 +128,6 @@ struct FontPicker: View {
     
     func importfont(_ url: URL) {
         let fm = FileManager.default
-        guard url.startAccessingSecurityScopedResource() else { return }
-        defer { url.stopAccessingSecurityScopedResource() }
         let dir = fm.urls(for: .documentDirectory, in: .userDomainMask)[0]
             .appendingPathComponent("Custom")
         try? fm.createDirectory(at: dir, withIntermediateDirectories: true)
  ```

  </details>


---

## References

### Commits (1)

- [f2388a1](https://github.com/danosito/lara-m-chips-fix/commit/f2388a1dc84c9d22efd83bd1a44abbf9d3e66d48) — Merge pull request #36 from lunginspector/main


---

## Timeline (Verbose)

- : 📝 Commit 505a3ac: re-implement the file picker fix
- Apr 6, 2026: 🟣 @rooootdev merged
- Apr 6, 2026: 🔴 Closed by @rooootdev
- Apr 19, 2026: 🔗 Commit f2388a1 — Merge pull request #36 from lunginspector/main


---

*Exported on 10/14/2026, 2:49:00 PM from [https://github.com/rooootdev/lara/pull/36](https://github.com/rooootdev/lara/pull/36)*
