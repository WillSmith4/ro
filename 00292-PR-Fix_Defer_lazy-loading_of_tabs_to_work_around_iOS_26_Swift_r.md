# Fix: Defer lazy-loading of tabs to work around iOS 26 Swift runtime m…

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#292](https://github.com/rooootdev/lara/pull/292)  
> **State:** 🔴 closed  
> **Branch:** `index3rr:fix/ios26-runtime-workaround` → `rooootdev:main`  
> **Changes:** +62 / -44 across 1 files  
> **Created:** Jun 6, 2026  
> **Updated:** Jun 6, 2026  
> **Closed:** Jun 6, 2026  

---

## Description

*Opened by [@index3rr](https://github.com/index3rr)* *on Jun 6, 2026*


x


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (1)

- [`9c789ab`](https://github.com/rooootdev/lara/commit/9c789abb813c48eb8cbc1012cb396a5570cd9939) Fix: Defer lazy-loading of tabs to work around iOS 26 Swift runtime metadata resolution bug — *index3rr* · Jun 6, 2026


---

## Changed Files (1)

- 🟡 `lara/lara.swift` (modified) — +62 / -44
  <details><summary>Diff</summary>

  ```diff
  @@ -25,6 +25,7 @@ struct lara: App {
     @AppStorage("showFMInTabs") private var showfmintabs: Bool = true
     @AppStorage("logsdisplaymode") private var logsdisplaymode: logsdisplaymode = .toolbar
     @State private var selectedtab: taboptions = .applying
+    @State private var appInitialized: Bool = false
     
     init() {
         #if DEBUG
@@ -45,56 +46,67 @@ struct lara: App {
     
     var body: some Scene {
         WindowGroup {
-            TabView(selection: $selectedtab) {
-                ContentView()
-                    .tabItem {
-                        Image(systemName: "wrench.and.screwdriver.fill")
-                    }
-                    .tag(taboptions.applying)
-                
-                // this has gotta fucking go
-                TweaksView(mgr: mgr)
-                    .tabItem {
-                        Image(systemName: "ant.fill")
-                    }
-                    .tag(taboptions.tweaks)
-                
-                
-                // i'm gonna strangle you root (the weight of your actions will crush you)
-                if showfmintabs {
-                    SantanderView(startPath: "/")
+            ZStack {
+                // Lazy load tabs to reduce metadata resolution at startup
+                // Workaround for iOS 26 beta Swift runtime bug in type metadata resolution
+                TabView(selection: $selectedtab) {
+                    ContentView()
                         .tabItem {
-                            Image(systemName: "folder.fill")
+                            Image(systemName: "wrench.and.screwdriver.fill")
                         }
-                        .tag(taboptions.files)
+                        .tag(taboptions.applying)
+                    
+                    // Defer TweaksView construction until needed or after init completes
+                    if selectedtab == .tweaks || appInitialized {
+                        TweaksView(mgr: mgr)
+                            .tabItem {
+                                Image(systemName: "ant.fill")
+                            }
+                            .tag(taboptions.tweaks)
+                    } else {
+                        EmptyView()
+                            .tabItem {
+                                Image(systemName: "ant.fill")
+                            }
+                            .tag(taboptions.tweaks)
+                    }
+                    
+                    // Defer file manager if enabled
+                    if showfmintabs && (selectedtab == .files || appInitialized) {
+                        SantanderView(startPath: "/")
+                            .tabItem {
+                                Image(systemName: "folder.fill")
+                            }
+                            .tag(taboptions.files)
+                    }
+                    
+                    // Defer logs view
+                    if logsdisplaymode == .tabs && (selectedtab == .logs || appInitialized) {
+                        LogsView(logger: globallogger)
+                            .tabItem {
+                                Image(systemName: "terminal")
+                            }
+                            .tag(taboptions.logs)
+                    }
+                }
+                .environmentObject(mgr)
+                .overlay {
+                    if mgr.showrespring {
+                        respringview()
+                            .brightness(-1.0)
+                            .ignoresSafeArea()
+                    }
                 }
-                
-                // this too
-                if logsdisplaymode == .tabs {
+                .sheet(isPresented: Binding(
+                    get: { logsdisplaymode == .toolbar && mgr.showLogs },
+                    set: { mgr.showLogs = $0 }
+                )) {
                     LogsView(logger: globallogger)
-                        .tabItem {
-                            Image(systemName: "terminal")
-                        }
-                        .tag(taboptions.logs)
                 }
-            }
-            .environmentObject(mgr)
-            .overlay {
-                if mgr.showrespring {
-                    respringview()
-                        .brightness(-1.0)
-                        .ignoresSafeArea()
+                .sheet(isPresented: $iconthememgr.showFixupSheet) {
+                    IconThemeFixupView()
                 }
             }
-            .sheet(isPresented: Binding(
-                get: { logsdisplaymode == .toolbar && mgr.showLogs },
-                set: { mgr.showLogs = $0 }
-            )) {
-                LogsView(logger: globallogger)
-            }
-            .sheet(isPresented: $iconthememgr.showFixupSheet) {
-                IconThemeFixupView()
-            }
             .onAppear {
                 if !isunsupported() {
                     init_offsets()
@@ -103,8 +115,13 @@ struct lara: App {
                     // beautiful name root
                     // thanks
                     mgr.hasOffsets = emergencyfixfunctiontobereplacedlateronquestionmark()
+                    
+                    // Mark initialization complete to allow lazy views to load
+                    DispatchQueue.main.asyncAfter(deadline: .now() + 0.1) {
+                        appInitialized = true
+                    }
                 } else {
-                    Alertinator.shared.alert(title: "This device is not supported!", body: "We apologize, but this device is currently not supported by Lara. Possible reasons: \n- You are on an unsupported iOS version (Supported: iOS 16.0 - iOS 18.7.1, iOS 26.0 - iOS 26.0.1) \n- Your device has MIE (A19+ or M5+) \n- A debugger is attached.", actionLabel: "Exit App", action: { exitinator() })
+                    Alertinator.shared.alert(title: "This device is not supported!", body: "We apologize, but this device is currently not supported by Lara. Possible reasons: \n- You are on an u[...]")
                 }
             }
             .onChange(of: scenephase, perform: handleScenePhase)
@@ -165,3 +182,4 @@ extension UIDocumentPickerViewController {
 
 // make strings compatiable with errors
 extension String: @retroactive Error {}
+
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit 9c789ab: Fix: Defer lazy-loading of tabs to work around iOS 26 Swift runtime metadata resolution bug
- Jun 6, 2026: 🔴 Closed by @index3rr
- Jun 6, 2026: 🟢 Reopened by @index3rr
- Jun 6, 2026: 🔴 Closed by @index3rr


---

*Exported on 10/14/2026, 3:11:00 PM from [https://github.com/rooootdev/lara/pull/292](https://github.com/rooootdev/lara/pull/292)*
