# Update CardView.swift with fix

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#124](https://github.com/rooootdev/lara/pull/124)  
> **State:** 🟣 merged  
> **Branch:** `TheDiamondSquidy:TheDiamondSquidy-card-overwrite-patch-1` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 19, 2026  
> **Changes:** +101 / -61 across 1 files  
> **Created:** Apr 18, 2026  
> **Updated:** Apr 19, 2026  
> **Closed:** Apr 19, 2026  

---

## Description

*Opened by [@TheDiamondSquidy](https://github.com/TheDiamondSquidy)* *on Apr 18, 2026*


Attempt to fix the long standing card-overwrite bug with claude. Images should now change to each respective card correctly

Removed @State private var selectedOption from CardView entirely — it no longer lives at the parent level.

Added CardRowView as a private nested struct. It owns its own @State private var selectedOption, which is completely isolated per card instance. The row communicates back up to CardView via three callbacks: onReplace, onRestore, and onEditNumber.

Replaced the inline ForEach body with a clean CardRowView(...) call passing the callbacks through.

Also added // MARK: - section comments throughout to make the file a bit easier to navigate.

PS: first PR, neon edit or do your one if unsatisfied.


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (1)

- [`18a4eec`](https://github.com/rooootdev/lara/commit/18a4eec9d162fcdb25f161d8c7bc4686f0c6e705) Update CardView.swift with fix — *TheDiamondSquidy* · Apr 18, 2026


---

## Changed Files (1)

- 🟡 `lara/views/CardView.swift` (modified) — +101 / -61
  <details><summary>Diff</summary>

  ```diff
  @@ -3,6 +3,7 @@
 //  lara
 //
 //  Created by ruter on 29.03.26.
+//  Modified and fixed by claud, implemented by TheDiamondSqidy on 18.04.26
 //
 
 import SwiftUI
@@ -30,7 +31,6 @@ struct CardView: View {
     @State private var currentCardNumber = ""
     @State private var pendingNumberCard: CardItem?
     @State private var pendingRestoreCard: CardItem?
-    @State private var selectedOption: ReplaceOption? = nil
 
     private struct CardItem: Identifiable {
         let id: String
@@ -40,6 +40,81 @@ struct CardView: View {
         let backgroundFileName: String
     }
 
+    // MARK: - Card Row Subview
+    //
+    // Each CardRowView owns its own @State selectedOption, so changing the
+    // picker on one card cannot affect any other card's state. Previously,
+    // a single shared selectedOption on CardView caused every card's
+    // onChange handler to fire simultaneously, always resolving to the
+    // wrong (first) card.
+    private struct CardRowView: View {
+        let card: CardItem
+        let onReplace: (CardItem, ReplaceOption) -> Void
+        let onRestore: (CardItem) -> Void
+        let onEditNumber: (CardItem) -> Void
+        let previewImage: (CardItem) -> UIImage?
+
+        @State private var selectedOption: ReplaceOption? = nil
+
+        var body: some View {
+            Section(header: Text(card.backgroundFileName)) {
+                HStack(spacing: 12) {
+                    if let img = previewImage(card) {
+                        Image(uiImage: img)
+                            .resizable()
+                            .scaledToFit()
+                            .frame(width: 140, height: 90)
+                            .cornerRadius(8)
+                    } else {
+                        RoundedRectangle(cornerRadius: 8)
+                            .fill(Color.gray.opacity(0.2))
+                            .frame(width: 140, height: 90)
+                            .overlay(
+                                Image(systemName: "creditcard.fill")
+                                    .foregroundColor(.secondary)
+                            )
+                    }
+
+                    VStack(alignment: .leading, spacing: 6) {
+                        Text(card.bundleName)
+                            .font(.headline)
+
+                        Text(card.imagePath)
+                            .font(.caption)
+                            .foregroundColor(.secondary)
+                            .lineLimit(2)
+                    }
+
+                    Spacer()
+                }
+
+                Picker("Replace", selection: $selectedOption) {
+                    Text("Select…").tag(ReplaceOption?.none)
+                    ForEach(ReplaceOption.allCases) { option in
+                        Text(option.rawValue).tag(Optional(option))
+                    }
+                }
+                .pickerStyle(MenuPickerStyle())
+                .onChange(of: selectedOption) { option in
+                    guard let option = option else { return }
+                    onReplace(card, option)
+                    selectedOption = nil
+                }
+
+                Button("Restore") {
+                    onRestore(card)
+                }
+                .foregroundColor(.red)
+
+                Button("Edit Card Number") {
+                    onEditNumber(card)
+                }
+            }
+        }
+    }
+
+    // MARK: - Body
+
     var body: some View {
         List {
             Section {
@@ -69,74 +144,31 @@ struct CardView: View {
                 }
             } else {
                 ForEach(cards) { card in
-                    Section {
-                        HStack(spacing: 12) {
-                            if let img = previewImage(for: card) {
-                                Image(uiImage: img)
-                                    .resizable()
-                                    .scaledToFit()
-                                    .frame(width: 140, height: 90)
-                                    .cornerRadius(8)
-                            } else {
-                                RoundedRectangle(cornerRadius: 8)
-                                    .fill(Color.gray.opacity(0.2))
-                                    .frame(width: 140, height: 90)
-                                    .overlay(
-                                        Image(systemName: "creditcard.fill")
-                                            .foregroundColor(.secondary)
-                                    )
-                            }
-                            
-                            VStack(alignment: .leading, spacing: 6) {
-                                Text(card.bundleName)
-                                    .font(.headline)
-                                
-                                Text(card.imagePath)
-                                    .font(.caption)
-                                    .foregroundColor(.secondary)
-                                    .lineLimit(2)
-                            }
-                            
-                            Spacer()
-                        }
-                        
-                        Picker("Replace", selection: $selectedOption) {
-                            Text("Select…").tag(ReplaceOption?.none)
-                            ForEach(ReplaceOption.allCases) { option in
-                                Text(option.rawValue).tag(Optional(option))
-                            }
-                        }
-                        .pickerStyle(MenuPickerStyle())
-                        .onChange(of: selectedOption) { option in
-                            guard let option = option else { return }
-                            pendingCard = card 
+                    CardRowView(
+                        card: card,
+                        onReplace: { card, option in
+                            pendingCard = card
                             switch option {
                             case .photos:
                                 showimgpicker = true
                             case .files:
                                 showDocPicker = true
                             }
-                            
-                            selectedOption = nil
-                        }
-                            
-                            Button("Restore") {
-                                pendingRestoreCard = card
-                                restoreImage(card: card)
-                            }
-                            .foregroundColor(.red)
-                        
-                        Button("Edit Card Number") {
+                        },
+                        onRestore: { card in
+                            pendingRestoreCard = card
+                            restoreImage(card: card)
+                        },
+                        onEditNumber: { card in
                             pendingNumberCard = card
                             currentCardNumber = readCardNumber(for: card) ?? ""
                             cardNumberInput = currentCardNumber
                             showCardNumberEditor = true
-                        }
-                    } header: {
-                        Text(card.backgroundFileName)
-                    }
+                        },
+                        previewImage: previewImage
+                    )
                 }
-                
+
                 Section {
                     HStack(alignment: .top) {
                         AsyncImage(url: URL(string: "https://github.com/drkm9743.png")) { image in
@@ -148,16 +180,16 @@ struct CardView: View {
                         }
                         .frame(width: 40, height: 40)
                         .clipShape(Circle())
-                        
+
                         VStack(alignment: .leading) {
                             Text("drkm9743")
                                 .font(.headline)
-                            
+
                             Text("Inspiration.")
                                 .font(.subheadline)
                                 .foregroundColor(Color.secondary)
                         }
-                        
+
                         Spacer()
                     }
                     .onTapGesture {
@@ -210,6 +242,8 @@ struct CardView: View {
         }
     }
 
+    // MARK: - Card Scanning
+
     private func refreshCards() {
         guard !working else { return }
         working = true
@@ -379,6 +413,8 @@ struct CardView: View {
         return nil
     }
 
+    // MARK: - Image Replacement
+
     private func applyReplacement(card: CardItem, imageData: Data) {
         guard let image = UIImage(data: imageData) else {
             status = "Invalid image data"
@@ -440,6 +476,8 @@ struct CardView: View {
         }
     }
 
+    // MARK: - Card Number
+
     private func passJsonPath(for card: CardItem) -> String {
         card.directoryPath + "/pass.json"
     }
@@ -520,6 +558,8 @@ struct CardView: View {
         }
     }
 
+    // MARK: - Cache & I/O
+
     private func clearCache(for card: CardItem) {
         let fm = FileManager.default
         let dir = card.directoryPath
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 18a4eec: Update CardView.swift with fix
- Apr 19, 2026: 🟣 @rooootdev merged
- Apr 19, 2026: 🔴 Closed by @rooootdev


---

## Comments (1)

### Comment #1 — [@rooootdev](https://github.com/rooootdev) · Apr 19, 2026

`ID: 4275566072`


Hmm. Nice fix but I’ll clean up the code later.


👍 1


---

*Exported on 10/14/2026, 2:56:21 PM from [https://github.com/rooootdev/lara/pull/124](https://github.com/rooootdev/lara/pull/124)*
