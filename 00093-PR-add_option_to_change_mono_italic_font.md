# add option to change mono/italic font

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#93](https://github.com/rooootdev/lara/pull/93)  
> **State:** 🟣 merged  
> **Branch:** `tryh4rd3rr:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 13, 2026  
> **Changes:** +26 / -1 across 2 files  
> **Created:** Apr 13, 2026  
> **Updated:** Apr 13, 2026  
> **Closed:** Apr 13, 2026  

---

## Description

*Opened by [@tryh4rd3rr](https://github.com/tryh4rd3rr)* *on Apr 13, 2026*


idk if this PR is good or not but it's good ig, tested and works on iPhone 16e with 26.0.1


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (1)

- [`5cb2f2a`](https://github.com/rooootdev/lara/commit/5cb2f2a1b45e142dd0ba9f4ea01e912c459d0210) cool — *Shin Morisawa* · Apr 13, 2026


---

## Changed Files (2)

- 🟡 `lara/classes/laramgr.swift` (modified) — +2 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -40,6 +40,8 @@ final class laramgr: ObservableObject {
     
     static let shared = laramgr()
     static let fontpath = "/System/Library/Fonts/Core/SFUI.ttf"
+    static let italicfontpath = "/System/Library/Fonts/Core/SFUIItalic.ttf"
+    static let monofontpath = "/System/Library/Fonts/Core/SFUIMono.ttf"
     private init() {}
     
     func run(completion: ((Bool) -> Void)? = nil) {
  ```

  </details>
- 🟡 `lara/views/FontPicker.swift` (modified) — +24 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -18,12 +18,27 @@ struct importedfont: Identifiable, Codable {
     let path: String
 }
 
+enum styletarget: String, CaseIterable {
+    case standard = "Standard"
+    case mono = "Mono"
+    case italic = "Italic"
+
+    var path: String {
+        switch self {
+            case .standard: return laramgr.fontpath
+            case .mono: return laramgr.monofontpath
+            case .italic: return laramgr.italicfontpath
+        }
+    }
+}
+
 struct FontPicker: View {
     @ObservedObject var mgr: laramgr
     @State private var showimporter = false
     @State private var customfonts: [importedfont] = load()
     @StateObject private var repostore = fontrepostore()
     @State private var showrepomgr = false
+    @State private var selectedTarget: styletarget = .standard
 
     var body: some View {
         NavigationStack {
@@ -60,6 +75,14 @@ struct FontPicker: View {
                 }
                 
                 Section {
+                    Picker("Target Style", selection: $selectedTarget) {
+                        ForEach(styletarget.allCases, id: \.self) { target in
+                            Text(target.rawValue).tag(target)
+                        }
+                    }
+                    .pickerStyle(.segmented)
+                    .padding(.vertical, 5)
+
                     if !customfonts.isEmpty {
                         ForEach(customfonts) { font in
                             Button {
@@ -69,7 +92,7 @@ struct FontPicker: View {
                                     save(customfonts)
                                     return
                                 }
-                                let success = mgr.vfsoverwritefromlocalpath(target: laramgr.fontpath, source: font.path)
+                                let success = mgr.vfsoverwritefromlocalpath(target: selectedTarget.path, source: font.path)
                                 success ? mgr.logmsg("font changed to \(font.name)") : mgr.logmsg("failed to change font")
                             } label: {
                                 Text(font.name)
  ```

  </details>


---

## References

### Commits (1)

- [6cb28a8](https://github.com/danosito/lara-m-chips-fix/commit/6cb28a83c0fb6ccddf43e58944b1220efcf09eeb) — Merge pull request #93 from shinmorisawa/main


---

## Timeline (Verbose)

- : 📝 Commit 5cb2f2a: cool
- Apr 13, 2026: 🟣 @rooootdev merged
- Apr 13, 2026: 🔴 Closed by @rooootdev
- Apr 19, 2026: 🔗 Commit 6cb28a8 — Merge pull request #93 from shinmorisawa/main


---

*Exported on 10/14/2026, 2:53:42 PM from [https://github.com/rooootdev/lara/pull/93](https://github.com/rooootdev/lara/pull/93)*
