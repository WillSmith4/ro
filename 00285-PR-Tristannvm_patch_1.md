# Tristannvm patch 1

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#285](https://github.com/rooootdev/lara/pull/285)  
> **State:** 🔴 closed  
> **Branch:** `Tristannvm:Tristannvm-patch-1` → `rooootdev:main`  
> **Changes:** +67 / -28 across 2 files  
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

## PR Commits (5)

- [`415568e`](https://github.com/rooootdev/lara/commit/415568ef863b9bbfbf4c1b8cba6cad292e99b09b) Delete lara/lara.swift — *Tristannvm* · May 31, 2026
- [`5038a23`](https://github.com/rooootdev/lara/commit/5038a23cacb094f4630978a30924c5639eb53ace) Create lara.swift with main app structure and features — *Tristannvm* · May 31, 2026
- [`8672522`](https://github.com/rooootdev/lara/commit/8672522dfa5436b3f00008bd0e4e3b24808be561) Rename lara.swift to lara/lara.swift — *Tristannvm* · May 31, 2026
- [`d1e6fb0`](https://github.com/rooootdev/lara/commit/d1e6fb0c18a71e52aa45e257ab3996cb908e4012) Add hex initializer and update accent color — *Tristannvm* · May 31, 2026
- [`1c3590a`](https://github.com/rooootdev/lara/commit/1c3590a3818c133396b836b55f7310aef6bac136) Refactor PrimaryButtonStyle to include loading state — *Tristannvm* · May 31, 2026


---

## Changed Files (2)

- 🟡 `lara/PartyUI/UI/Buttons/PrimaryButtonStyle.swift` (modified) — +41 / -21
  <details><summary>Diff</summary>

  ```diff
  @@ -1,33 +1,53 @@
-//
-//  PrimaryButtonStyle.swift
-//  PartyUI
-//
-//  Created by lunginspector on 3/3/26.
-//
-
 import SwiftUI
 
 public struct PrimaryButtonStyle: PrimitiveButtonStyle {
-    var color: Color = .accentColor
-    var shape: Shape
-    var useFullWidth: Bool
+
+    private var color: Color
+    private var useFullWidth: Bool
+    private var isLoading: Bool
+
     @Environment(\.isEnabled) private var isEnabled
-    
-    public init(color: Color = .accentColor, foregroundStyle: Color = .accentColor, shape: Shape = .rect(cornerRadius: cornerRad.component), useFullWidth: Bool = true) {
+
+    public init(
+        color: Color = .accentColor,
+        useFullWidth: Bool = true,
+        isLoading: Bool = false
+    ) {
         self.color = color
-        self.shape = shape
         self.useFullWidth = useFullWidth
+        self.isLoading = isLoading
     }
-    
+
     public func makeBody(configuration: Configuration) -> some View {
         configuration.label
-            .buttonStyle(.plain)
-            .foregroundStyle(isEnabled ? Color(.label) : .gray)
+            .font(.system(size: 16, weight: .semibold))
+            .foregroundStyle(isEnabled ? .white : .white.opacity(0.6))
             .frame(maxWidth: useFullWidth ? .infinity : nil)
-            .padding()
-            .background(isEnabled ? color : Color(.systemGray).opacity(0.2), in: AnyShape(shape))
-            .onTapGesture(perform: configuration.trigger)
-            .modifier(FadeAnimation())
+            .padding(.vertical, 14)
+            .padding(.horizontal, 20)
+            .background(
+                Capsule(style: .continuous)
+                    .fill(backgroundColor(isPressed: configuration.isPressed))
+            )
+            .scaleEffect(configuration.isPressed ? 0.96 : 1.0)
+            .opacity(configuration.isPressed ? 0.85 : 1.0)
+            .animation(.spring(response: 0.25, dampingFraction: 0.7), value: configuration.isPressed)
+            .onTapGesture {
+                guard isEnabled, !isLoading else { return }
+                configuration.trigger()
+
+                UIImpactFeedbackGenerator(style: .light).impactOccurred()
+            }
+            .overlay {
+                if isLoading {
+                    ProgressView()
+                        .tint(.white)
+                }
+            }
     }
-}
 
+    private func backgroundColor(isPressed: Bool) -> Color {
+        if !isEnabled { return .gray.opacity(0.3) }
+        return isPressed ? color.opacity(0.85) : color
+    }
+}
  ```

  </details>
- 🟡 `lara/lara.swift` (modified) — +26 / -7
  <details><summary>Diff</summary>

  ```diff
  @@ -15,6 +15,25 @@ enum taboptions {
 let g_isunsupported: Bool = isunsupported()
 var weonadebugbuild_pjbweouttahereexclamationmark: Bool = false
 
+// MARK: - Accent Color
+extension Color {
+    static let lightLavender = Color(red: 0.78, green: 0.70, blue: 0.95)
+
+    static let accent = Color(hex: "#848EF9")
+
+    init(hex: String) {
+        let hex = hex.trimmingCharacters(in: CharacterSet.alphanumerics.inverted)
+        var int: UInt64 = 0
+        Scanner(string: hex).scanHexInt64(&int)
+
+        let r = Double((int >> 16) & 0xFF) / 255.0
+        let g = Double((int >> 8) & 0xFF) / 255.0
+        let b = Double(int & 0xFF) / 255.0
+
+        self.init(red: r, green: g, blue: b)
+    }
+}
+
 @main
 struct lara: App {
     @StateObject private var mgr = laramgr.shared
@@ -52,15 +71,12 @@ struct lara: App {
                     }
                     .tag(taboptions.applying)
                 
-                // this has gotta fucking go
                 TweaksView(mgr: mgr)
                     .tabItem {
                         Image(systemName: "ant.fill")
                     }
                     .tag(taboptions.tweaks)
                 
-                
-                // i'm gonna strangle you root (the weight of your actions will crush you)
                 if showfmintabs {
                     SantanderView(startPath: "/")
                         .tabItem {
@@ -69,7 +85,6 @@ struct lara: App {
                         .tag(taboptions.files)
                 }
                 
-                // this too
                 if logsdisplaymode == .tabs {
                     LogsView(logger: globallogger)
                         .tabItem {
@@ -78,6 +93,7 @@ struct lara: App {
                         .tag(taboptions.logs)
                 }
             }
+            .tint(.accent)   // ✅ UPDATED ACCENT COLOR
             .environmentObject(mgr)
             .overlay {
                 if mgr.showrespring {
@@ -100,11 +116,14 @@ struct lara: App {
                     init_offsets()
                     offsets_init()
                     iconthememgr.startPendingFixupIfPossible()
-                    // beautiful name root
-                    // thanks
                     mgr.hasOffsets = emergencyfixfunctiontobereplacedlateronquestionmark()
                 } else {
-                    Alertinator.shared.alert(title: "This device is not supported!", body: "We apologize, but this device is currently not supported by Lara. Possible reasons: \n- You are on an unsupported iOS version (Supported: iOS 16.0 - iOS 18.7.1, iOS 26.0 - iOS 26.0.1) \n- Your device has MIE (A19+ or M5+) \n- A debugger is attached.", actionLabel: "Exit App", action: { exitinator() })
+                    Alertinator.shared.alert(
+                        title: "This device is not supported!",
+                        body: "We apologize, but this device is currently not supported by Lara. Possible reasons: \n- You are on an unsupported iOS version (Supported: iOS 16.0 - iOS 18.7.1, iOS 26.0 - iOS 26.0.1) \n- Your device has MIE (A19+ or M5+) \n- A debugger is attached.",
+                        actionLabel: "Exit App",
+                        action: { exitinator() }
+                    )
                 }
             }
             .onChange(of: scenephase, perform: handleScenePhase)
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 415568e: Delete lara/lara.swift
- : 📝 Commit 5038a23: Create lara.swift with main app structure and features
- : 📝 Commit 8672522: Rename lara.swift to lara/lara.swift
- : 📝 Commit d1e6fb0: Add hex initializer and update accent color
- : 📝 Commit 1c3590a: Refactor PrimaryButtonStyle to include loading state
- May 31, 2026: 🔴 Closed by @Tristannvm
- May 31, 2026: 🗑️ @Tristannvm deleted head branch


---

*Exported on 10/14/2026, 3:10:20 PM from [https://github.com/rooootdev/lara/pull/285](https://github.com/rooootdev/lara/pull/285)*
