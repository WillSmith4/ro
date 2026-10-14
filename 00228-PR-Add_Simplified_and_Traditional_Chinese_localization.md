# Add Simplified and Traditional Chinese localization

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#228](https://github.com/rooootdev/lara/pull/228)  
> **State:** 🔴 closed  
> **Branch:** `hxhlb:i18n-zh` → `rooootdev:main`  
> **Changes:** +6105 / -34 across 12 files  
> **Created:** May 6, 2026  
> **Updated:** May 23, 2026  
> **Closed:** May 7, 2026  

---

## Description

*Opened by [@hxhlb](https://github.com/hxhlb)* *on May 6, 2026*


*No description provided.*


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/25415517816/job/74546165051) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/25415517816/job/74546087608) — **success** — *GitHub Actions*


---

## PR Commits (1)

- [`b6727a1`](https://github.com/rooootdev/lara/commit/b6727a1fddd3f351f29ed5adaed238fa6aeb8248) Add Simplified and Traditional Chinese localization — *Hades* · May 6, 2026


---

## Changed Files (12)

- 🟡 `lara.xcodeproj/project.pbxproj` (modified) — +2 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -161,6 +161,8 @@
 			knownRegions = (
 				en,
 				Base,
+				"zh-Hans",
+				"zh-Hant-TW",
 			);
 			mainGroup = CC1C8B302F71DF9C00206982;
 			minimizedProjectReferenceProxies = 1;
  ```

  </details>
- 🟢 `lara/Localizable.xcstrings` (added) — +6067 / -0
- 🟡 `lara/PartyUI/App/Functions/Alertinator.swift` (modified) — +13 / -9
  <details><summary>Diff</summary>

  ```diff
  @@ -13,12 +13,16 @@ public class Alertinator {
     public static let shared = Alertinator()
     
     var alertController: UIAlertController?
+
+    private func localized(_ value: String) -> String {
+        NSLocalizedString(value, comment: "")
+    }
     
     public func alert(title: String, body: String, showCancel: Bool = true) {
         Task { @MainActor in
-            alertController = UIAlertController(title: title, message: body, preferredStyle: .alert)
+            alertController = UIAlertController(title: localized(title), message: localized(body), preferredStyle: .alert)
             if showCancel {
-                alertController?.addAction(.init(title: "OK", style: .cancel))
+                alertController?.addAction(.init(title: localized("OK"), style: .cancel))
             }
             alertController?.view.tintColor = UIColor(named: "AccentColor")
             self.present(alertController!)
@@ -27,12 +31,12 @@ public class Alertinator {
     
     public func alert(title: String, body: String, showCancel: Bool = true, actionLabel: String = "OK", action: @escaping () -> Void) {
         Task { @MainActor in
-            alertController = UIAlertController(title: title, message: body, preferredStyle: .alert)
-            alertController?.addAction(.init(title: actionLabel, style: .default) { _ in
+            alertController = UIAlertController(title: localized(title), message: localized(body), preferredStyle: .alert)
+            alertController?.addAction(.init(title: localized(actionLabel), style: .default) { _ in
                 action()
             })
             if showCancel {
-                alertController?.addAction(.init(title: "Cancel", style: .cancel))
+                alertController?.addAction(.init(title: localized("Cancel"), style: .cancel))
             }
             alertController?.view.tintColor = UIColor(named: "AccentColor")
             self.present(alertController!)
@@ -41,18 +45,18 @@ public class Alertinator {
     
     public func prompt(title: String, placeholder: String, showCancel: Bool = true, completion: @escaping (String?) async -> Void) {
         Task { @MainActor in
-            alertController = UIAlertController(title: title, message: nil, preferredStyle: .alert)
+            alertController = UIAlertController(title: localized(title), message: nil, preferredStyle: .alert)
             alertController?.addTextField { field in
-                field.placeholder = placeholder
+                field.placeholder = self.localized(placeholder)
             }
             if showCancel {
-                alertController?.addAction(UIAlertAction(title: "Cancel", style: .cancel) { _ in
+                alertController?.addAction(UIAlertAction(title: self.localized("Cancel"), style: .cancel) { _ in
                     Task {
                         await completion(nil)
                     }
                 })
             }
-            alertController?.addAction(UIAlertAction(title: "OK", style: .default) { _ in
+            alertController?.addAction(UIAlertAction(title: self.localized("OK"), style: .default) { _ in
                 let field = self.alertController?.textFields?.first
                 Task {
                     await completion(field?.text)
  ```

  </details>
- 🟡 `lara/PartyUI/UI/Buttons/ButtonLabel.swift` (modified) — +2 / -2
  <details><summary>Diff</summary>

  ```diff
  @@ -8,12 +8,12 @@
 import SwiftUI
 
 public struct ButtonLabel: View {
-    var text: String
+    var text: LocalizedStringKey
     var icon: String
     var useImage: Bool
     
     public init(text: String, icon: String, useImage: Bool = false) {
-        self.text = text
+        self.text = LocalizedStringKey(text)
         self.icon = icon
         self.useImage = useImage
     }
  ```

  </details>
- 🟡 `lara/PartyUI/UI/Buttons/NavigationLabel.swift` (modified) — +2 / -2
  <details><summary>Diff</summary>

  ```diff
  @@ -8,11 +8,11 @@
 import SwiftUI
 
 public struct NavigationLabel: View {
-    var text: String
+    var text: LocalizedStringKey
     var icon: String
     
     public init(text: String, icon: String = "") {
-        self.text = text
+        self.text = LocalizedStringKey(text)
         self.icon = icon
     }
     
  ```

  </details>
- 🟡 `lara/PartyUI/UI/Inputs/PlainToggle.swift` (modified) — +2 / -3
  <details><summary>Diff</summary>

  ```diff
  @@ -8,7 +8,7 @@
 import SwiftUI
 
 public struct PlainToggle: View {
-    var text: String
+    var text: LocalizedStringKey
     var icon: String
     var infoType: ToggleInfoType
     var infoTitle: String
@@ -18,7 +18,7 @@ public struct PlainToggle: View {
     @Binding var isOn: Bool
     
     public init(text: String, icon: String = "", infoType: ToggleInfoType = .none, infoTitle: String = "Information", infoMessage: String = "", minSupportedVersion: Double = 0.0, maxSupportedVersion: Double = 100.0, isOn: Binding<Bool>) {
-        self.text = text
+        self.text = LocalizedStringKey(text)
         self.icon = icon
         self.infoType = infoType
         self.infoTitle = infoTitle
@@ -52,4 +52,3 @@ public struct PlainToggle: View {
         }
     }
 }
-
  ```

  </details>
- 🟡 `lara/PartyUI/UI/Inputs/PlatterToggle.swift` (modified) — +2 / -3
  <details><summary>Diff</summary>

  ```diff
  @@ -12,7 +12,7 @@ public enum ToggleInfoType: Codable {
 }
 
 public struct PlatterToggle: View {
-    var text: String
+    var text: LocalizedStringKey
     var icon: String
     var color: Color
     var infoType: ToggleInfoType
@@ -23,7 +23,7 @@ public struct PlatterToggle: View {
     @Binding var isOn: Bool
     
     public init(text: String, icon: String = "", color: Color = Color.accentColor, infoType: ToggleInfoType = .none, infoTitle: String = "Information", infoMessage: String = "", minSupportedVersion: Double = 0.0, maxSupportedVersion: Double = 100.0, isOn: Binding<Bool>) {
-        self.text = text
+        self.text = LocalizedStringKey(text)
         self.icon = icon
         self.color = color
         self.infoType = infoType
@@ -61,4 +61,3 @@ public struct PlatterToggle: View {
         }
     }
 }
-
  ```

  </details>
- 🟡 `lara/PartyUI/UI/Lists/HeaderLabel.swift` (modified) — +2 / -2
  <details><summary>Diff</summary>

  ```diff
  @@ -8,11 +8,11 @@
 import SwiftUI
 
 public struct HeaderLabel: View {
-    var text: String
+    var text: LocalizedStringKey
     var icon: String
     
     public init(text: String, icon: String) {
-        self.text = text
+        self.text = LocalizedStringKey(text)
         self.icon = icon
     }
     
  ```

  </details>
- 🟡 `lara/PartyUI/UI/Lists/ThemedHeaderLabel.swift` (modified) — +2 / -2
  <details><summary>Diff</summary>

  ```diff
  @@ -8,11 +8,11 @@
 import SwiftUI
 
 public struct ThemedHeaderLabel: View {
-    var text: String
+    var text: LocalizedStringKey
     var icon: String
     
     public init(text: String, icon: String) {
-        self.text = text
+        self.text = LocalizedStringKey(text)
         self.icon = icon
     }
     
  ```

  </details>
- 🟡 `lara/PartyUI/UI/Terminal/TerminalHeader.swift` (modified) — +5 / -5
  <details><summary>Diff</summary>

  ```diff
  @@ -8,16 +8,16 @@
 import SwiftUI
 
 public struct TerminalHeader: View {
-    var text: String
+    var text: LocalizedStringKey
     var icon: String
     var color: Color
-    var context: String
+    var context: LocalizedStringKey?
     
     public init(text: String, icon: String, color: Color = Color(.label), context: String = "") {
-        self.text = text
+        self.text = LocalizedStringKey(text)
         self.icon = icon
         self.color = color
-        self.context = context
+        self.context = context.isEmpty ? nil : LocalizedStringKey(context)
     }
     
     public var body: some View {
@@ -36,7 +36,7 @@ public struct TerminalHeader: View {
                     .fontWeight(.medium)
                     .lineLimit(1)
             }
-            if !context.isEmpty {
+            if let context {
                 Text(context)
                     .foregroundStyle(.secondary)
                     .font(.subheadline)
  ```

  </details>
- 🟡 `lara/views/app/ContentView.swift` (modified) — +3 / -3
  <details><summary>Diff</summary>

  ```diff
  @@ -79,7 +79,7 @@ struct ContentView: View {
     }
     
     private var KRWSection: some View {
-        Section(header: HeaderLabel(text: "Kernel Read Write", icon: "wrench.and.screwdriver"), footer: Text(isdebugged() ? "Not available while a debugger is attached." : "Depending on your device configuration, this may not function properly.")) {
+        Section(header: HeaderLabel(text: "Kernel Read Write", icon: "wrench.and.screwdriver"), footer: Text(LocalizedStringKey(isdebugged() ? "Not available while a debugger is attached." : "Depending on your device configuration, this may not function properly."))) {
             if hasOffsets {
                 PlainAlert(title: "Kernelcache offsets missing!", icon: "exclamationmark.triangle.fill", text: "lara needs to download kernelcache offsets in order for the exploit to function properly. Download them in settings.")
             } else {
@@ -91,7 +91,7 @@ struct ContentView: View {
                         if mgr.dsready {
                             ButtonLabel(text: "Exploit Successful", icon: "checkmark")
                         } else if mgr.dsrunning {
-                            ButtonLabel(text: "Running Exploit... (\(Int(mgr.dsprogress * 100))%)", icon: "showMeProgressPlease")
+                            ButtonLabel(text: String(localized: "Running Exploit...") + " (\(Int(mgr.dsprogress * 100))%)", icon: "showMeProgressPlease")
                         } else if mgr.dsattempted && mgr.dsfailed {
                             ButtonLabel(text: "Exploit Failed!", icon: "xmark")
                         } else {
@@ -127,7 +127,7 @@ struct ContentView: View {
                             if mgr.vfsready {
                                 ButtonLabel(text: "VFS Initialized", icon: "checkmark")
                             } else if mgr.vfsrunning {
-                                ButtonLabel(text: "Initializing VFS... (\(Int(mgr.vfsprogress * 100))%)", icon: "showMeProgressPlease")
+                                ButtonLabel(text: String(localized: "Initializing VFS...") + " (\(Int(mgr.vfsprogress * 100))%)", icon: "showMeProgressPlease")
                             } else if mgr.vfsattempted && mgr.vfsfailed {
                                 ButtonLabel(text: "VFS Failed!", icon: "xmark")
                             } else {
  ```

  </details>
- 🟡 `lara/views/tweaks/ToolsView.swift` (modified) — +3 / -3
  <details><summary>Diff</summary>

  ```diff
  @@ -51,7 +51,7 @@ struct ToolsView: View {
                     
                     Spacer()
                     
-                    Text(isaslr ? "enabled" : "disabled")
+                    Text(LocalizedStringKey(isaslr ? "enabled" : "disabled"))
                         .foregroundColor(isaslr ? Color.red : Color.green)
                         .monospaced()
                     
@@ -145,9 +145,9 @@ struct ToolsView: View {
             Section {
                 Button {
                     if mgr.PPHelper() {
-                        status = "Succeeded. Open the Pocket Poster app, open settings and tap Detect."
+                        status = String(localized: "Succeeded. Open the Pocket Poster app, open settings and tap Detect.")
                     } else {
-                        status = "Failed. Check logs."
+                        status = String(localized: "Failed. Check logs.")
                     }
                 } label: {
                     Text("Pocket Poster Helper")
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit b6727a1: Add Simplified and Traditional Chinese localization
- May 7, 2026: 🔴 Closed by @hxhlb
- May 23, 2026: 🗑️ @hxhlb deleted head branch


---

## Comments (4)

### Comment #1 — [@tryh4rd3rr](https://github.com/tryh4rd3rr) · May 6, 2026

`ID: 4386396448`


"small PR" ahh PR :skull:


---

### Comment #2 — [@hxhlb](https://github.com/hxhlb) · May 6, 2026

`ID: 4389100546`


> "small PR" ahh PR 💀

What do you mean by that? 😭


---

### Comment #3 — [@Pro42good](https://github.com/Pro42good) · May 6, 2026

`ID: 4390629082`


Pretty Sure PR's aren't being apcepted right now due to the main developer (rooootdev) programming a major ui overhaul, I'll let them close this issue


---

### Comment #4 — [@hxhlb](https://github.com/hxhlb) · May 7, 2026

`ID: 4393165262`


> Pretty Sure PR's aren't being apcepted right now due to the main developer (rooootdev) programming a major ui overhaul, I'll let them close this issue

Oh, Got it, I'll close this for now and make a new PR once the UI overhaul is done.


---

*Exported on 10/14/2026, 3:05:26 PM from [https://github.com/rooootdev/lara/pull/228](https://github.com/rooootdev/lara/pull/228)*
