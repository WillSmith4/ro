# Update compatibility

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#3](https://github.com/rooootdev/lara/pull/3)  
> **State:** 🟣 merged  
> **Branch:** `Ltsparten88:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Mar 27, 2026  
> **Changes:** +10 / -8 across 2 files  
> **Created:** Mar 27, 2026  
> **Updated:** Mar 27, 2026  
> **Closed:** Mar 27, 2026  

---

## Description

*Opened by [@Ltsparten88](https://github.com/Ltsparten88)* *on Mar 27, 2026*


Change tabs to support pre iOS 18.0.
Tested on a iPhone 15 pro max, iOS 17.5 font changing worked just fine.
See images below


<img width="562" height="1230" alt="IMG_0513" src="https://github.com/user-attachments/assets/85f475f8-d264-4095-9cb0-a07de4448b17" />
<img width="562" height="1230" alt="IMG_0513" src="https://github.com/user-attachments/assets/8386b8b8-679c-4c1f-ba46-6d29f6ef671a" />


---

## Checks & Status

- Overall: 🟡 **pending**


---

## PR Commits (1)

- [`6c1cc1e`](https://github.com/rooootdev/lara/commit/6c1cc1ee2ba4c880bc272a2e9b9cb0eb18016356) Update compatibility — *Ltsparten88* · Mar 27, 2026


---

## Changed Files (2)

- 🟡 `lara.xcodeproj/project.pbxproj` (modified) — +2 / -2
  <details><summary>Diff</summary>

  ```diff
  @@ -307,7 +307,7 @@
 				INFOPLIST_KEY_UILaunchScreen_Generation = YES;
 				INFOPLIST_KEY_UISupportedInterfaceOrientations_iPad = "UIInterfaceOrientationPortrait UIInterfaceOrientationPortraitUpsideDown UIInterfaceOrientationLandscapeLeft UIInterfaceOrientationLandscapeRight";
 				INFOPLIST_KEY_UISupportedInterfaceOrientations_iPhone = "UIInterfaceOrientationPortrait UIInterfaceOrientationLandscapeLeft UIInterfaceOrientationLandscapeRight";
-				IPHONEOS_DEPLOYMENT_TARGET = 18.0;
+				IPHONEOS_DEPLOYMENT_TARGET = 17.0;
 				LD_RUNPATH_SEARCH_PATHS = (
 					"$(inherited)",
 					"@executable_path/Frameworks",
@@ -351,7 +351,7 @@
 				INFOPLIST_KEY_UILaunchScreen_Generation = YES;
 				INFOPLIST_KEY_UISupportedInterfaceOrientations_iPad = "UIInterfaceOrientationPortrait UIInterfaceOrientationPortraitUpsideDown UIInterfaceOrientationLandscapeLeft UIInterfaceOrientationLandscapeRight";
 				INFOPLIST_KEY_UISupportedInterfaceOrientations_iPhone = "UIInterfaceOrientationPortrait UIInterfaceOrientationLandscapeLeft UIInterfaceOrientationLandscapeRight";
-				IPHONEOS_DEPLOYMENT_TARGET = 18.0;
+				IPHONEOS_DEPLOYMENT_TARGET = 17.0;
 				LD_RUNPATH_SEARCH_PATHS = (
 					"$(inherited)",
 					"@executable_path/Frameworks",
  ```

  </details>
- 🟡 `lara/lara.swift` (modified) — +8 / -6
  <details><summary>Diff</summary>

  ```diff
  @@ -16,13 +16,15 @@ struct lara: App {
     var body: some Scene {
         WindowGroup {
             TabView {
-                Tab("lara", systemImage: "ant.fill") {
-                    ContentView()
-                }
+                ContentView()
+                    .tabItem {
+                        Label("lara", systemImage: "ant.fill")
+                    }
 
-                Tab("Logs", systemImage: "text.document.fill") {
-                    LogsView(logger: globallogger)
-                }
+                LogsView(logger: globallogger)
+                    .tabItem {
+                        Label("Logs", systemImage: "text.document.fill")
+                    }
             }
             .onAppear {
                 init_offsets()
  ```

  </details>


---

## References

### Commits (1)

- [1041c5c](https://github.com/danosito/lara-m-chips-fix/commit/1041c5ca23f1eccb8b4abceafb7b53b7fe47f4a2) — Merge pull request #3 from Ltsparten88/main


---

## Timeline (Verbose)

- : 📝 Commit 6c1cc1e: Update compatibility
- Mar 27, 2026: 🟣 @rooootdev merged
- Mar 27, 2026: 🔴 Closed by @rooootdev
- Apr 19, 2026: 🔗 Commit 1041c5c — Merge pull request #3 from Ltsparten88/main


---

*Exported on 10/14/2026, 2:46:24 PM from [https://github.com/rooootdev/lara/pull/3](https://github.com/rooootdev/lara/pull/3)*
