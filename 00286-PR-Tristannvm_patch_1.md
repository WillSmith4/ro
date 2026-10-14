# Tristannvm patch 1

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#286](https://github.com/rooootdev/lara/pull/286)  
> **State:** 🔴 closed  
> **Branch:** `Tristannvm:Tristannvm-patch-1` → `rooootdev:main`  
> **Changes:** +25 / -38 across 3 files  
> **Created:** May 31, 2026  
> **Updated:** May 31, 2026  
> **Closed:** May 31, 2026  

---

## Description

*Opened by [@Tristannvm](https://github.com/Tristannvm)* *on May 31, 2026*


*No description provided.*


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (6)

- [`e85896c`](https://github.com/rooootdev/lara/commit/e85896ca9f2be9a6a6bea8a7047c0b0136412e7a) Update CreditsView to include new contributors — *Tristannvm* · May 31, 2026
- [`64f8575`](https://github.com/rooootdev/lara/commit/64f8575e1648a4c35770a76a6130fb37aba4aa98) Merge pull request #1 from Tristannvm/Tristannvm-patch-1 — *Tristannvm* · May 31, 2026
- [`3e00a10`](https://github.com/rooootdev/lara/commit/3e00a10a8688d6e878b40e66d119373095807a09) Add LinkCreditCell entries for contributors — *Tristannvm* · May 31, 2026
- [`402cda3`](https://github.com/rooootdev/lara/commit/402cda383185638778ac0a5d4485cc2f41d7302f) Define laraAccent color for UI elements — *Tristannvm* · May 31, 2026
- [`8f97c43`](https://github.com/rooootdev/lara/commit/8f97c43bc775d97a9041d03daa15d868a51efb9f) Merge pull request #2 from Tristannvm/Tristannvm-patch-1 — *Tristannvm* · May 31, 2026
- [`7727ae0`](https://github.com/rooootdev/lara/commit/7727ae0b722a29af73cd1bc272654f46e7cff4c6) Add share and copy log functionality to LogsView — *Tristannvm* · May 31, 2026


---

## Changed Files (3)

- 🟡 `lara/lara.swift` (modified) — +7 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -8,6 +8,12 @@
 import SwiftUI
 import UniformTypeIdentifiers
 
+let laraAccent = Color(
+    red: 175/255,
+    green: 151/255,
+    blue: 215/255
+)
+
 enum taboptions {
     case applying, tweaks, files, logs
 }
@@ -78,6 +84,7 @@ struct lara: App {
                         .tag(taboptions.logs)
                 }
             }
+            .tint(laraAccent)
             .environmentObject(mgr)
             .overlay {
                 if mgr.showrespring {
  ```

  </details>
- 🟡 `lara/views/app/LogsView.swift` (modified) — +2 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -45,6 +45,7 @@ struct LogsView: View {
                 ToolbarItemGroup(placement: .navigationBarTrailing) {
                     ShareLink(item: logsURL) {
                         Image(systemName: "square.and.arrow.up")
+                           .tint(Color("laraAccent"))
                     }
 
                     Button {
@@ -53,6 +54,7 @@ struct LogsView: View {
                         UIImpactFeedbackGenerator(style: .medium).impactOccurred()
                     } label: {
                         Image(systemName: "doc.on.doc")
+                            .tint(Color("laraAccent"))
                     }
                     
                     Button {
  ```

  </details>
- 🟡 `lara/views/app/settings/CreditsView.swift` (modified) — +16 / -38
  <details><summary>Diff</summary>

  ```diff
  @@ -1,48 +1,26 @@
-//
-//  CreditsView.swift
-//  lara
-//
-//  Created by lunginspector on 5/9/26.
-//
-
 import SwiftUI
 
 struct CreditsView: View {
     var body: some View {
         NavigationStack {
             List {
-                LinkCreditCell(name: "roooot", description: "Main Developer", url: "https://github.com/rooootdev") {
-                    LinkCreditIcon(url: "https://github.com/rooootdev.png")
-                }
-                LinkCreditCell(name: "wh1te4ever", description: "Made darksword-kexploit-fun", url: "https://github.com/wh1te4ever") {
-                    LinkCreditIcon(url: "https://github.com/wh1te4ever.png")
-                }
-                LinkCreditCell(name: "Duy Tran", description: "Various remotecall-related improvements and features", url: "https://github.com/khanhduytran0") {
-                    LinkCreditIcon(url: "https://github.com/khanhduytran0.png")
-                }
-                LinkCreditCell(name: "AppInstalleriOS", description: "Helped me with offsets and lots of other stuff", url: "https://github.com/AppInstalleriOSGH") {
-                    LinkCreditIcon(url: "https://github.com/AppInstalleriOSGH.png")
-                }
-                LinkCreditCell(name: "jailbreak.party", description: "dirtyZero Tweaks", url: "https://github.com/jailbreakdotparty") {
-                    LinkCreditIcon(url: "https://github.com/jailbreakdotparty.png")
-                }
-                LinkCreditCell(name: "lunginspector", description: "Frontend rewrite", url: "https://github.com/lunginspector") {
-                    LinkCreditIcon(url: "https://github.com/lunginspector.png")
-                }
-                LinkCreditCell(name: "Jurre", description: "EditorView, PocketPoster Helper, various improvements", url: "https://github.com/jurre111") {
-                    LinkCreditIcon(url: "https://github.com/jurre111.png")
-                }
-                LinkCreditCell(name: "neon", description: "Respring Script, zipmgr, fixing Passcode Themes, and adding App Decryption", url: "https://github.com/neonmodder123") {
-                    LinkCreditIcon(url: "https://github.com/neonmodder123.png")
-                }
-                LinkCreditCell(name: "Skadz", description: "Respring Method", url: "https://github.com/skadz108") {
-                    LinkCreditIcon(url: "https://github.com/skadz108.png")
-                }
-                LinkCreditCell(name: "hxhlb", description: "Various bug fixes", url: "https://github.com/hxhlb") {
-                    LinkCreditIcon(url: "https://github.com/hxhlb.png")
+
+                LinkCreditCell(
+                    name: "Tristannvm",
+                    description: "Developer",
+                    url: "https://github.com/Tristannvm"
+                ) {
+                    LinkCreditIcon(url: "https://github.com/Tristannvm.png")
                 }
-                LinkCreditCell(name: "leminlimez", description: "Various Cowabunga Tweaks", url: "https://github.com/leminlimez") {
-                    LinkCreditIcon(url: "https://github.com/leminlimez.png")
+
+                LinkCreditCell(
+                    name: "Capibarak",
+                    description: "Animal",
+                    url: "https://youtube.com/@Capibarakreal/"
+                ) {
+                    LinkCreditIcon(
+                        url: "https://cdn.discordapp.com/avatars/1069242431880560660/bfa8d4076b64f01f5f060086e627f98d.png?size=4096"
+                    )
                 }
             }
             .navigationTitle("Credits")
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit e85896c: Update CreditsView to include new contributors
- : 📝 Commit 64f8575: Merge pull request #1 from Tristannvm/Tristannvm-patch-1
- : 📝 Commit 3e00a10: Add LinkCreditCell entries for contributors
- : 📝 Commit 402cda3: Define laraAccent color for UI elements
- : 📝 Commit 8f97c43: Merge pull request #2 from Tristannvm/Tristannvm-patch-1
- : 📝 Commit 7727ae0: Add share and copy log functionality to LogsView
- May 31, 2026: 🔴 Closed by @Tristannvm
- May 31, 2026: 🗑️ @Tristannvm deleted head branch


---

*Exported on 10/14/2026, 3:10:25 PM from [https://github.com/rooootdev/lara/pull/286](https://github.com/rooootdev/lara/pull/286)*
