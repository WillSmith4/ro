# Fix Cowabunga Tweaks UI

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#257](https://github.com/rooootdev/lara/pull/257)  
> **State:** 🟣 merged  
> **Branch:** `jurre111:fix-sb-ui` → `rooootdev:main`  
> **Merged by:** [@lunginspector](https://github.com/lunginspector) on May 22, 2026  
> **Changes:** +213 / -433 across 2 files  
> **Created:** May 21, 2026  
> **Updated:** May 22, 2026  
> **Closed:** May 22, 2026  

---

## Description

*Opened by [@jurre111](https://github.com/jurre111)* *on May 21, 2026*


Fixes the buggy leminware.


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/26255019253/job/77275686204) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/26255019253/job/77275506509) — **success** — *GitHub Actions*


---

## Reviews (1)

### 💬 COMMENTED — [@copilot-pull-request-reviewer[bot]](https://github.com/apps/copilot-pull-request-reviewer) · May 21, 2026

## Pull request overview

This PR updates the “SB Customizer” (SpringBoard Tools) UI/flow under Tweaks, primarily by refactoring the SpringBoard customizer view and wiring it to the shared `laramgr` so it can trigger actions like respring.

**Changes:**
- Pass `mgr` into `SpringBoardView` from `TweaksView`.
- Rebuild `SpringBoardView` from a custom grid/card UI into a `NavigationStack` + `List` UI with per-option segmented selection and (when applicable) color/blur controls.
- Update apply/reset behavior and show an alert with an optional respring action after applying.

### Reviewed changes

Copilot reviewed 2 out of 2 changed files in this pull request and generated 3 comments.

| File | Description |
| ---- | ----------- |
| `lara/views/tweaks/TweaksView.swift` | Routes “SB Customizer” to `SpringBoardView(mgr: mgr)` so the view can use shared manager actions. |
| `lara/views/tweaks/sbcustomizer/SpringBoardView.swift` | Refactors the SpringBoard Tools UI and refactors tweak application/overwrite logic into the view. |



<details>
<summary>Comments suppressed due to low confidence (3)</summary>

**lara/views/tweaks/sbcustomizer/SpringBoardView.swift:162**
* `load()` updates `tweakOptions[i].value`, but the subsequent conditional checks `option.value` from the pre-mutation copy in the loop. This can cause color/blur state not to load when the stored value is "Color". Use `tweakOptions[i].value` / `tweakOptions[i].selectedOption` for the check (or compute the updated value once and reuse it).
```
        for (i, option) in tweakOptions.enumerated() {
            tweakOptions[i].value = getDefaultStr(forKey: option.key)
            tweakOptions[i].selectedOption = tweakOptions[i].value
            if option.sbType != nil {
                if option.value == "Color" {
                    tweakOptions[i].color = SpringboardColorManager.getColor(forType: option.sbType!)
                    tweakOptions[i].blur = SpringboardColorManager.getBlur(forType: option.sbType!)
                }
```
**lara/views/tweaks/sbcustomizer/SpringBoardView.swift:286**
* `overwriteFile` returns `true` from inside the `for path in ...` loop, so only the first replacement path is processed and the `succeeded` aggregation is effectively bypassed. Remove the early return and instead iterate all paths, updating `succeeded` based on each overwrite result, and return only after the loop completes.
```
            if replacementPaths[fileIdentifier] != nil {
                var succeeded = true
                for path in replacementPaths[fileIdentifier]! {
                    if fileIdentifier == "HomeBar" && value as? Bool == false {
                        if let url: URL = Bundle.main.url(forResource: "HomeBarAssets", withExtension: "car") {
                            do {
                                let replacementCar = try Data(contentsOf: url)
                                //try MDC.overwriteFile(at: "/System/Library/PrivateFrameworks/" + path, with: replacementCar)
                            } catch {
                                print(error.localizedDescription)
                                succeeded = false
                            }
                        } else {
                            print("Home bar file not found!")
                            return false
                        }
                    } else {
                        let randomGarbage = Data("###".utf8)
                        
                        let result = laramgr.shared.lara_overwritefile(target: "/System/Library/PrivateFrameworks/" + path, data: randomGarbage)
                        
                        if result.ok {
                            print("i hope it worked")
                        } else {
                            print("it didn't")
                        }
                        
                        return true
                    }
```
**lara/views/tweaks/sbcustomizer/SpringBoardView.swift:286**
* The overwrite result is not used to determine success: even when `result.ok` is false, the function still returns `true` and does not set `succeeded = false`. Please propagate `result.ok` into `succeeded` (and avoid reporting success on failure).
```
                        let result = laramgr.shared.lara_overwritefile(target: "/System/Library/PrivateFrameworks/" + path, data: randomGarbage)
                        
                        if result.ok {
                            print("i hope it worked")
                        } else {
                            print("it didn't")
                        }
                        
                        return true
                    }
```
</details>



---

💡 <a href="/rooootdev/lara/new/main?filename=.github/instructions/*.instructions.md" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Add Copilot custom instructions</a> for smarter, more guided reviews. <a href="https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Learn how to get started</a>.


---

## Review Comments (3)

*Inline comments on code changes*

### `lara/views/tweaks/sbcustomizer/SpringBoardView.swift`

**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · May 21, 2026 (line 104) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -44,67 +62,106 @@ struct SpringBoardView: View {
         .init(value: getDefaultStr(forKey: "NotifShadow"), key: "NotifShadow", sbType: .notifShadow, title: NSLocalizedString("Notification Banner Shadow", comment: "Springboard tool"), shortTitle: "Notification Shadow", imageName: "platter.filled.top.iphone", fileType: OverwritingFileTypes.springboard, options: ["Visible", "Color", "Disabled"]),
 //        .init(value: getDefaultStr(forKey: "ShortcutBanner"), key: "ShortcutBanner", title: NSLocalizedString("Shortcut Notification Banner", comment: "Springboard tool"), imageName: "pencil.slash", fileType: .springboard, options: ["Visible", "Disabled"], minimumOS: 16)
     ]
+
+    let mgr: laramgr
+    let replacementPaths: [String: [String]] = [
+        SpringBoardOptions.DockHidden.rawValue: ["CoreMaterial.framework/dockDark.materialrecipe", "CoreMaterial.framework/dockLight.materialrecipe"],
+        SpringBoardOptions.HomeBarHidden.rawValue: ["MaterialKit.framework/Assets.car"],
+        SpringBoardOptions.FolderBGHidden.rawValue: ["SpringBoardHome.framework/folderLight.materialrecipe", "SpringBoardHome.framework/folderDark.materialrecipe", "SpringBoardHome.framework/folderDarkSimplified.materialrecipe"],
+        SpringBoardOptions.FolderBlurDisabled.rawValue: ["SpringBoardHome.framework/folderExpandedBackgroundHome.materialrecipe", "SpringBoardHome.framework/folderExpandedBackgroundHomeSimplified.materialrecipe"],
+        SpringBoardOptions.SwitcherBlurDisabled.rawValue: ["SpringBoard.framework/homeScreenBackdrop-application.materialrecipe", "SpringBoard.framework/homeScreenBackdrop-switcher.materialrecipe"],
+        SpringBoardOptions.CCModuleBackgroundDisabled.rawValue: ["CoreMaterial.framework/modules.materialrecipe"],
+        SpringBoardOptions.PodBackgroundDisabled.rawValue: ["SpringBoardHome.framework/podBackgroundViewLight.visualstyleset", "SpringBoardHome.framework/podBackgroundViewDark.visualstyleset"],
+        SpringBoardOptions.NotifBackgroundDisabled.rawValue: ["CoreMaterial.framework/plattersDark.materialrecipe", "CoreMaterial.framework/platters.materialrecipe"],
+        SpringBoardOptions.ShortcutBanner.rawValue: ["SpringBoard.framework/BannersAuthorizedBundleIDs.plist"]
+    ]
     
     var body: some View {
-        GeometryReader { screenGeometry in
-            ZStack {
-                Rectangle()
-                    .opacity(0)
-                    .ignoresSafeArea(edges: .all)
-                    .blur(radius: flippedOption == nil ? 0 : 8, opaque: false)
-                    .frame(maxWidth: .infinity, maxHeight: .infinity)
-                ScrollView {
-                    VStack {
-                        Button {
-                            if flippedOption == nil {
-                                applyTweaks()
-                            } else {
-                                withAnimation(Animation.easeInOut(duration: CardAnimationSpeed)) {
-                                    for (i, opt) in tweakOptions.enumerated() {
-                                        if opt.title == flippedOption?.title {
-                                            tweakOptions[i].animate3d = false
-                                            break
+        NavigationStack {
+            List {
+                Section(header: HeaderLabel(text: "Applying", icon: "checkmark")) {
+                    Button("Apply") {
+                        applyTweaks()
+                    }
+                    Button("Reset all") {
+                        for ind in tweakOptions.indices {
+                            tweakOptions[ind].value = "Visible"
+                            tweakOptions[ind].selectedOption = "Visible"
+                            UserDefaults.standard.set("Visible", forKey: tweakOptions[ind].key)
+                        }
+                        applyTweaks()
+                    }
+                }
+                ForEach($tweakOptions) { $option in
+                    Section(header: HeaderLabel(text: option.title, icon: option.imageName)) {
+                        Picker("Option", selection: $option.selectedOption) {
+                            ForEach(0..<option.options.count) { ind in
+                                Text(option.options[ind]).tag(option.options[ind])
+                            }
+                        }
+                        .labelsHidden()
+                        .pickerStyle(.segmented)
+                        .onChange(of: option.selectedOption) { newvalue in
```

</details>

The Picker updates `option.selectedOption` (and UserDefaults), but `applyTweaks()` switches on `option.value`. Since `option.value` is never updated on selection changes, pressing Apply can use a stale value. Consider either (a) using `selectedOption` throughout `applyTweaks()`, or (b) keeping `value` in sync in the `.onChange` handler (and using a single source of truth).

This issue also appears on line 155 of the same file.


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · May 21, 2026 (line 226) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -121,408 +178,130 @@ struct SpringBoardView: View {
             return false
         }
     }
-    
+
     func applyTweaks() {
-        if !inProgress {
-            //UIApplication.shared.alert(title: NSLocalizedString("Applying Springboard Tweaks...", comment: ""), body: NSLocalizedString("Please wait...", comment: ""), animated: true, withButton: false)
-            var failed: Bool = false
-            for option in tweakOptions {
-                //  apply tweak
-                if option.value == "Disabled" {
-                    print("Applying tweak \"" + option.title + "\"")
-                    var succeeded = false
-                    if option.sbType != nil {
-                        succeeded = apply(option.sbType!, .gray.opacity(0), 0)
-                    } else {
-                        succeeded = overwriteFile(typeOfFile: option.fileType, fileIdentifier: option.key, true)
-                    }
-                    if succeeded {
-                        print("Successfully applied tweak \"" + option.title + "\"")
-                    } else {
-                        print("Failed to apply tweak \"" + option.title + "\"!!!")
-                        failed = true
-                    }
-                    
-                } else if option.value == "Visible" {
-                    print("Applying tweak \"" + option.title + "\"")
-                    if option.sbType != nil {
-                        if option.sbType! == .switcher {
-                            let succeeded = apply(option.sbType!, .gray.opacity(1), 20, save: false)
-                            if succeeded {
-                                print("Successfully applied tweak \"" + option.title + "\"")
-                            } else {
-                                print("Failed to apply tweak \"" + option.title + "\"!!!")
-                            }
-                        } else {
-                            do {
-                                try SpringboardColorManager.revertFiles(forType: option.sbType!)
-                                print("Successfully applied tweak \"" + option.title + "\"")
-                            } catch {
-                                print("Failed to apply tweak \"" + option.title + "\"!!!")
-                                print(error.localizedDescription)
-                            }
-                        }
-                    } else {
-                        let succeeded = overwriteFile(typeOfFile: option.fileType, fileIdentifier: option.key, false)
+        var failed: Bool = false
+        for option in tweakOptions {
+            //  apply tweak
+            if option.value == "Disabled" {
+                print("Applying tweak \"" + option.title + "\"")
+                var succeeded = false
+                if option.sbType != nil {
+                    succeeded = apply(option.sbType!, .gray.opacity(0), 0)
+                } else {
+                    succeeded = overwriteFile(typeOfFile: option.fileType, fileIdentifier: option.key, true)
+                }
+                if succeeded {
+                    print("Successfully applied tweak \"" + option.title + "\"")
+                } else {
+                    print("Failed to apply tweak \"" + option.title + "\"!!!")
+                    failed = true
+                }
+                
+            } else if option.value == "Visible" {
+                print("Applying tweak \"" + option.title + "\"")
+                if option.sbType != nil {
+                    if option.sbType! == .switcher {
+                        let succeeded = apply(option.sbType!, .gray.opacity(1), 20, save: false)
                         if succeeded {
                             print("Successfully applied tweak \"" + option.title + "\"")
                         } else {
                             print("Failed to apply tweak \"" + option.title + "\"!!!")
                         }
-                    }
-                    
-                } else if option.value == "Color" || option.value == "Blur" {
-                    if option.sbType != nil {
-                        print("Applying tweak \"" + option.title + "\"")
-                        let succeeded = apply(option.sbType!, option.color, Int(option.blur))
-                        if succeeded {
+                    } else {
+                        do {
+                            try SpringboardColorManager.revertFiles(forType: option.sbType!)
                             print("Successfully applied tweak \"" + option.title + "\"")
-                        } else {
+                        } catch {
                             print("Failed to apply tweak \"" + option.title + "\"!!!")
-                            failed = true
+                            print(error.localizedDescription)
                         }
-                    } else {
-                        print("\(option.title) does not have a springboard type!")
-                        failed = true
                     }
-                }
-            }
-            
-            DispatchQueue.main.asyncAfter(deadline: .now() + 0.3) {
-                //UIApplication.shared.dismissAlert(animated: true)
-                if failed {
-                    Alertinator.shared.alert(title: "useless ass alert", body: "something failed while applying tweaks")
-                    //UIApplication.shared.alert(body: "An error occurred when applying tweaks")
                 } else {
-                    Alertinator.shared.alert(title: "yayy", body: "it should've worked?")
-                    //UIApplication.shared.alert(title: NSLocalizedString("Successfully applied tweaks", comment: "Successfully applied tweaks"), body: NSLocalizedString("Respring to see changes", comment: "Respring to see changes"))
-                }
-            }
-        }
-    }
-    
-    struct GeneralOption: Identifiable {
-        var value: String
-        var id = UUID()
-        var key: String
-        var sbType: SpringboardColorManager.SpringboardType?
-        var title: String
-        var shortTitle: String?
-        var imageName: String
-        var fileType: OverwritingFileTypes
-        var options: [String] = []
-        var selectedOption: String = "Visible"
-        var flipped: Bool = false
-        var animate3d: Bool = false
-        
-        var minimumOS: Int = 14
-        
-        var color: Color = Color.gray
-        var blur: Double = 30
-    }
-    
-    struct SpringBoardOptionView: View {
-        @Binding var option: GeneralOption
-        var screenGeom: GeometryProxy
-        @Binding var flippedOption: GeneralOption?
-        @Binding var otherOptions: [GeneralOption]
-
-        var body: some View {
-            return GeometryReader { cardGeometry in
-                VStack {
-                    Spacer()
-                    ZStack {
-                        FrontSpringBoardView(option: $option, flippedOption: $flippedOption, otherOptions: $otherOptions).opacity(option.flipped ? 0.0 : 1.0)
-                            .frame(width: cardGeometry.size.width, height: cardGeometry.size.height)
-                            .scaleEffect(option.animate3d ? (min(screenGeom.size.width*0.95, screenGeom.size.height*0.95)) / cardGeometry.size.width: 1)
-                        
-                        BackSpringBoardView(option: $option, flippedOption: $flippedOption, screenGeom: screenGeom).opacity(option.flipped ? 1.0 : 0.0)
-                            .frame(width: min(screenGeom.size.width, screenGeom.size.height)*0.95, height: min(screenGeom.size.width, screenGeom.size.height)*0.95)
-                            .scaleEffect(option.animate3d ? 1 : cardGeometry.size.width / (min(screenGeom.size.width*0.95, screenGeom.size.height*0.95)))
+                    let succeeded = overwriteFile(typeOfFile: option.fileType, fileIdentifier: option.key, false)
+                    if succeeded {
+                        print("Successfully applied tweak \"" + option.title + "\"")
+                    } else {
+                        print("Failed to apply tweak \"" + option.title + "\"!!!")
```

</details>

When `overwriteFile(...)` fails in the "Visible" branch, the code logs the failure but doesn't set `failed = true`, so the final success alert can still be shown even though some tweaks weren't applied. Propagate the failure into `failed` (consistent with the other branches).


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · May 21, 2026 (lines 257-294) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -121,408 +178,130 @@ struct SpringBoardView: View {
             return false
         }
     }
-    
+
     func applyTweaks() {
-        if !inProgress {
-            //UIApplication.shared.alert(title: NSLocalizedString("Applying Springboard Tweaks...", comment: ""), body: NSLocalizedString("Please wait...", comment: ""), animated: true, withButton: false)
-            var failed: Bool = false
-            for option in tweakOptions {
-                //  apply tweak
-                if option.value == "Disabled" {
-                    print("Applying tweak \"" + option.title + "\"")
-                    var succeeded = false
-                    if option.sbType != nil {
-                        succeeded = apply(option.sbType!, .gray.opacity(0), 0)
-                    } else {
-                        succeeded = overwriteFile(typeOfFile: option.fileType, fileIdentifier: option.key, true)
-                    }
-                    if succeeded {
-                        print("Successfully applied tweak \"" + option.title + "\"")
-                    } else {
-                        print("Failed to apply tweak \"" + option.title + "\"!!!")
-                        failed = true
-                    }
-                    
-                } else if option.value == "Visible" {
-                    print("Applying tweak \"" + option.title + "\"")
-                    if option.sbType != nil {
-                        if option.sbType! == .switcher {
-                            let succeeded = apply(option.sbType!, .gray.opacity(1), 20, save: false)
-                            if succeeded {
-                                print("Successfully applied tweak \"" + option.title + "\"")
-                            } else {
-                                print("Failed to apply tweak \"" + option.title + "\"!!!")
-                            }
-                        } else {
-                            do {
-                                try SpringboardColorManager.revertFiles(forType: option.sbType!)
-                                print("Successfully applied tweak \"" + option.title + "\"")
-                            } catch {
-                                print("Failed to apply tweak \"" + option.title + "\"!!!")
-                                print(error.localizedDescription)
-                            }
-                        }
-                    } else {
-                        let succeeded = overwriteFile(typeOfFile: option.fileType, fileIdentifier: option.key, false)
+        var failed: Bool = false
+        for option in tweakOptions {
+            //  apply tweak
+            if option.value == "Disabled" {
+                print("Applying tweak \"" + option.title + "\"")
+                var succeeded = false
+                if option.sbType != nil {
+                    succeeded = apply(option.sbType!, .gray.opacity(0), 0)
+                } else {
+                    succeeded = overwriteFile(typeOfFile: option.fileType, fileIdentifier: option.key, true)
+                }
+                if succeeded {
+                    print("Successfully applied tweak \"" + option.title + "\"")
+                } else {
+                    print("Failed to apply tweak \"" + option.title + "\"!!!")
+                    failed = true
+                }
+                
+            } else if option.value == "Visible" {
+                print("Applying tweak \"" + option.title + "\"")
+                if option.sbType != nil {
+                    if option.sbType! == .switcher {
+                        let succeeded = apply(option.sbType!, .gray.opacity(1), 20, save: false)
                         if succeeded {
                             print("Successfully applied tweak \"" + option.title + "\"")
                         } else {
                             print("Failed to apply tweak \"" + option.title + "\"!!!")
                         }
-                    }
-                    
-                } else if option.value == "Color" || option.value == "Blur" {
-                    if option.sbType != nil {
-                        print("Applying tweak \"" + option.title + "\"")
-                        let succeeded = apply(option.sbType!, option.color, Int(option.blur))
-                        if succeeded {
+                    } else {
+                        do {
+                            try SpringboardColorManager.revertFiles(forType: option.sbType!)
                             print("Successfully applied tweak \"" + option.title + "\"")
-                        } else {
+                        } catch {
                             print("Failed to apply tweak \"" + option.title + "\"!!!")
-                            failed = true
+                            print(error.localizedDescription)
                         }
-                    } else {
-                        print("\(option.title) does not have a springboard type!")
-                        failed = true
                     }
-                }
-            }
-            
-            DispatchQueue.main.asyncAfter(deadline: .now() + 0.3) {
-                //UIApplication.shared.dismissAlert(animated: true)
-                if failed {
-                    Alertinator.shared.alert(title: "useless ass alert", body: "something failed while applying tweaks")
-                    //UIApplication.shared.alert(body: "An error occurred when applying tweaks")
                 } else {
-                    Alertinator.shared.alert(title: "yayy", body: "it should've worked?")
-                    //UIApplication.shared.alert(title: NSLocalizedString("Successfully applied tweaks", comment: "Successfully applied tweaks"), body: NSLocalizedString("Respring to see changes", comment: "Respring to see changes"))
-                }
-            }
-        }
-    }
-    
-    struct GeneralOption: Identifiable {
-        var value: String
-        var id = UUID()
-        var key: String
-        var sbType: SpringboardColorManager.SpringboardType?
-        var title: String
-        var shortTitle: String?
-        var imageName: String
-        var fileType: OverwritingFileTypes
-        var options: [String] = []
-        var selectedOption: String = "Visible"
-        var flipped: Bool = false
-        var animate3d: Bool = false
-        
-        var minimumOS: Int = 14
-        
-        var color: Color = Color.gray
-        var blur: Double = 30
-    }
-    
-    struct SpringBoardOptionView: View {
-        @Binding var option: GeneralOption
-        var screenGeom: GeometryProxy
-        @Binding var flippedOption: GeneralOption?
-        @Binding var otherOptions: [GeneralOption]
-
-        var body: some View {
-            return GeometryReader { cardGeometry in
-                VStack {
-                    Spacer()
-                    ZStack {
-                        FrontSpringBoardView(option: $option, flippedOption: $flippedOption, otherOptions: $otherOptions).opacity(option.flipped ? 0.0 : 1.0)
-                            .frame(width: cardGeometry.size.width, height: cardGeometry.size.height)
-                            .scaleEffect(option.animate3d ? (min(screenGeom.size.width*0.95, screenGeom.size.height*0.95)) / cardGeometry.size.width: 1)
-                        
-                        BackSpringBoardView(option: $option, flippedOption: $flippedOption, screenGeom: screenGeom).opacity(option.flipped ? 1.0 : 0.0)
-                            .frame(width: min(screenGeom.size.width, screenGeom.size.height)*0.95, height: min(screenGeom.size.width, screenGeom.size.height)*0.95)
-                            .scaleEffect(option.animate3d ? 1 : cardGeometry.size.width / (min(screenGeom.size.width*0.95, screenGeom.size.height*0.95)))
+                    let succeeded = overwriteFile(typeOfFile: option.fileType, fileIdentifier: option.key, false)
+                    if succeeded {
+                        print("Successfully applied tweak \"" + option.title + "\"")
+                    } else {
+                        print("Failed to apply tweak \"" + option.title + "\"!!!")
                     }
-                    .modifier(FlipEffect(flipped: $option.flipped, angle: option.animate3d ? 180 : 0, axis: (x: 0.25, y: 1)))
-                    .offset(x: option.animate3d ? -cardGeometry.frame(in: .named("mainFrame")).origin.x + screenGeom.size.width*0.025: -(screenGeom.size.width*0.95)/2 + cardGeometry.size.width/2, y: option.animate3d ? -cardGeometry.frame(in: .named("mainFrame")).origin.y + (screenGeom.size.height)/8: -screenGeom.size.width/2 + cardGeometry.size.height/2)
-                    
                 }
-            }
-            .aspectRatio(1, contentMode: .fit)
-        }
-    }
-
-    struct FrontSpringBoardView: View {
-        @Binding var option: GeneralOption
-        @Binding var flippedOption: GeneralOption?
-        @Binding var otherOptions: [GeneralOption]
-        
-        var body: some View {
-            return VStack {
-                VStack {
-                    Image(systemName: option.imageName)
-                        .resizable()
-                        .aspectRatio(contentMode: .fit)
-                        .frame(width: 24, height: 24)
-                        .foregroundColor(.blue)
-                        .opacity(0.5)
-                        .padding(.top, 28)
-                    
-                    Text(option.title)
-                        .foregroundStyle(Color(.label))
-                        .lineLimit(2)
-                        .padding(.horizontal)
-                        .minimumScaleFactor(0.5)
-                        .multilineTextAlignment(.center)
-                    
-                    Image(systemName: option.selectedOption == "Disabled" ? "x.circle" : "pencil.circle")
-                        .resizable()
-                        .aspectRatio(contentMode: .fit)
-                        .frame(width: 24, height: 24)
-                        .foregroundColor(option.selectedOption == "Color" ? .green : .red)
-                        .opacity(option.selectedOption == "Visible" ? 0 : 0.8)
-                }
-                .frame(maxWidth: .infinity, maxHeight: .infinity)
-                .padding(.vertical)
-                .background(Color(.secondarySystemBackground)
-                    .opacity(1)
-                )
-                .cornerRadius(10)
-                .onTapGesture {
-                    if flippedOption == nil {
-                        withAnimation(Animation.easeInOut(duration: CardAnimationSpeed)) {
-                            flippedOption = option
-                            option.animate3d.toggle()
-                        }
+                
+            } else if option.value == "Color" || option.value == "Blur" {
+                if option.sbType != nil {
+                    print("Applying tweak \"" + option.title + "\"")
+                    let succeeded = apply(option.sbType!, option.color, Int(option.blur))
+                    if succeeded {
+                        print("Successfully applied tweak \"" + option.title + "\"")
                     } else {
-                        withAnimation(Animation.easeInOut(duration: CardAnimationSpeed)) {
-                            for (i, opt) in otherOptions.enumerated() {
-                                if opt.title == flippedOption?.title {
-                                    otherOptions[i].animate3d = false
-                                    break
-                                }
-                            }
-                            flippedOption = nil
-                        }
+                        print("Failed to apply tweak \"" + option.title + "\"!!!")
+                        failed = true
                     }
+                } else {
+                    print("\(option.title) does not have a springboard type!")
+                    failed = true
                 }
             }
         }
+        if failed {
+            Alertinator.shared.alert(title: "useless ass alert", body: "something failed while applying tweaks")
+        } else {
+            Alertinator.shared.alert(title: "Success!", body: "Respring to see changes.", actionLabel: "Respring", action: { mgr.respring() })
+        }
     }
     
-    struct BackSpringBoardView: View {
-        @Binding var option: GeneralOption
-        @Binding var flippedOption: GeneralOption?
-        var screenGeom: GeometryProxy
-        
-        var body: some View {
-            return VStack (alignment: .center) {
-                HStack {
-                    Text(option.shortTitle ?? option.title)
-                        .font(.title)
-                        .bold()
-                        .padding(.horizontal, 20)
-                        .padding(.top, 20)
-                    Spacer()
-                }
-                
-                ZStack {
-                    Rectangle()
-                        .foregroundColor(Color(.secondarySystemFill))
-                        .cornerRadius(8)
-                        .frame(width: screenGeom.size.width - 25 - (10 * CGFloat(option.options.count)), height: screenGeom.size.width*0.075)
-                    HStack {
-                        ForEach(0..<option.options.count) { ind in
-                            Button(action: {
-                                option.selectedOption = option.options[ind]
-                            }) {
-                                ZStack {
-                                    Rectangle()
-                                        .foregroundStyle(Color(.systemBackground).opacity(option.selectedOption == option.options[ind] ? 1 : 0))
-                                    Text(option.options[ind])
-                                        .font(.system(size: 13))
-                                        .lineLimit(1)
-                                }
-                                .frame(width: (screenGeom.size.width/CGFloat(option.options.count)) - (10 * CGFloat(option.options.count)), height: screenGeom.size.width*0.075)
-                            }
-                            .buttonStyle(.plain)
-                        }
-                    }
-                }
-                .padding(.horizontal, 20)
-                .padding(.bottom, 15)
-                .pickerStyle(.segmented)
-                Spacer()
-                
-                // MARK: Visible Text
-                if option.selectedOption == "Visible" {
-                    Image(systemName: "checkmark.circle")
-                        .foregroundColor(.green)
-                        .font(.system(size: 40))
-                    
-                // MARK: Color and Blur selector
-                } else if option.selectedOption == "Color" || option.selectedOption == "Blur" {
-                    VStack{
-                        if option.selectedOption == "Color" {
-                            HStack {
-                                Text("Color:")
-                                    .font(.title)
-                                    .fontWeight(.medium)
-                                    .padding(.horizontal, 25)
-                                Spacer()
-                                ColorPicker("Set notification banner color", selection: $option.color)
-                                    .labelsHidden()
-                                    .scaleEffect(1.5)
-                                    .padding(.horizontal, 50)
+    // MARK: sigh     //UIApplication.shared.alert(title: NSLocalizedString("Successfully applied tweaks", comment: "Successfully applied tweaks"), body: NSLocalizedString("Respring to see changes", comment: "Respring to see changes"))
+    func overwriteFile<Value>(typeOfFile: OverwritingFileTypes, fileIdentifier: String, _ value: Value) -> Bool {
+        // find the path and replace the file
+        // springboard option
+        if typeOfFile == OverwritingFileTypes.springboard {
+            // springboard tweak being applied
+            if replacementPaths[fileIdentifier] != nil {
+                var succeeded = true
+                for path in replacementPaths[fileIdentifier]! {
+                    if fileIdentifier == "HomeBar" && value as? Bool == false {
+                        if let url: URL = Bundle.main.url(forResource: "HomeBarAssets", withExtension: "car") {
+                            do {
+                                let replacementCar = try Data(contentsOf: url)
+                                //try MDC.overwriteFile(at: "/System/Library/PrivateFrameworks/" + path, with: replacementCar)
+                            } catch {
+                                print(error.localizedDescription)
+                                succeeded = false
                             }
+                        } else {
+                            print("Home bar file not found!")
+                            return false
                         }
-                        HStack {
-                            Text("Blur:   \(Int(option.blur))")
-                                .frame(width: 125)
-                            Spacer()
-                            Slider(value: $option.blur, in: 0...150, step: 1.0)
-                                .padding(.horizontal)
-                        }
-                    }
-                    
-                // MARK: Disabled Text
-                } else if option.selectedOption == "Disabled" {
-                    Image(systemName: "x.circle")
-                        .foregroundColor(.red)
-                        .font(.system(size: 40))
-                }
-                
-                Spacer()
-                
-                Button(action: {
-                    // save
-                    UserDefaults.standard.set(option.selectedOption, forKey: option.key)
-                    option.value = option.selectedOption
-                    if option.selectedOption == "Color" || option.selectedOption == "Blur" {
-                        do {
-                            try SpringboardColorManager.createColor(forType: option.sbType!, color: CIColor(color: UIColor(option.color)), blur: Int(option.blur), asTemp: false)
-                            print("Success")
-                        } catch {
-                            print(error.localizedDescription)
-                        }
-                    }
-                    
-                    // close menu
-                    if flippedOption!.title == option.title {
-                        withAnimation(Animation.easeInOut(duration: CardAnimationSpeed)) {
-                            flippedOption = nil
-                            option.animate3d.toggle()
+                    } else {
+                        let randomGarbage = Data("###".utf8)
+                        
+                        let result = laramgr.shared.lara_overwritefile(target: "/System/Library/PrivateFrameworks/" + path, data: randomGarbage)
+                        
+                        if result.ok {
+                            print("i hope it worked")
+                        } else {
+                            print("it didn't")
                         }
+                        
+                        return true
                     }
-                }) {
-                    Text("Save")
                 }
-                //.buttonStyle(TintedButton(color: .blue, fullwidth: true))
-                .padding(25)
+                return succeeded
             }
-            .background(Color(.secondarySystemBackground))
-            .cornerRadius(10)
         }
+        
+        return true
     }
```

</details>

`overwriteFile(...)` returns `true` for unsupported `typeOfFile` values or when `replacementPaths[fileIdentifier]` is missing. That masks errors and makes the UI report success even when nothing happened. Return `false` for unsupported/missing cases (or surface an error) so callers can set `failed` appropriately.

This issue also appears in the following locations of the same file:
- line 258
- line 277



---

## PR Commits (2)

- [`bf3d257`](https://github.com/rooootdev/lara/commit/bf3d257bd8f0541981ab4829f1db4bfe2f1325a3) fix SBView UI — *jurre111* · May 21, 2026
- [`c453c98`](https://github.com/rooootdev/lara/commit/c453c98536917ceabea9e0af782f79bfadedc6fc) fix saving — *Jurre* · May 21, 2026


---

## Changed Files (2)

- 🟡 `lara/views/tweaks/TweaksView.swift` (modified) — +1 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -22,7 +22,7 @@ struct TweaksView: View {
                     NavigationLink("Liquid Glass", destination: LiquidGlassView())
                         .disabled(!mgr.vfsready)
                     if doubleSystemVersion() < 26.0 {
-                        NavigationLink("SB Customizer", destination: SpringBoardView())
+                        NavigationLink("SB Customizer", destination: SpringBoardView(mgr: mgr))
                             .disabled(!mgr.vfsready)
                     }
                 }
  ```

  </details>
- 🟡 `lara/views/tweaks/sbcustomizer/SpringBoardView.swift` (modified) — +212 / -432
  <details><summary>Diff</summary>

  ```diff
  @@ -6,14 +6,18 @@
 //
 
 import SwiftUI
+import UIKit
 
-var inProgress = false
-var CardAnimationSpeed = 0.3
-
-func getDefaultStr(forKey: String, defaultValue: String = "Visible") -> String {
-    let defaults = UserDefaults.standard
-    
-    return defaults.string(forKey: forKey) ?? defaultValue
+enum SpringBoardOptions: String, CaseIterable {
+    case DockHidden = "DockHidden"
+    case HomeBarHidden = "HomeBar"
+    case FolderBGHidden = "FolderBGHidden"
+    case FolderBlurDisabled = "FolderBlurDisabled"
+    case SwitcherBlurDisabled = "SwitcherBlurDisabled"
+    case CCModuleBackgroundDisabled = "CCModuleBackgroundDisabled"
+    case PodBackgroundDisabled = "PodBackgroundDisabled"
+    case NotifBackgroundDisabled = "NotifBackgroundDisabled"
+    case ShortcutBanner = "ShortcutBanner"
 }
 
 enum OverwritingFileTypes {
@@ -24,12 +28,26 @@ enum OverwritingFileTypes {
     case region
 }
 
+
+struct GeneralOption: Identifiable {
+        var value: String
+        var id = UUID()
+        var key: String
+        var sbType: SpringboardColorManager.SpringboardType?
+        var title: String
+        var shortTitle: String?
+        var imageName: String
+        var fileType: OverwritingFileTypes
+        var options: [String] = []
+        var selectedOption: String = "Visible"
+        
+        var minimumOS: Int = 14
+        
+        var color: Color = Color.gray
+        var blur: Double = 30
+    }
+
 struct SpringBoardView: View {
-    // lazyvgrid
-    private var gridItemLayout = [GridItem(.adaptive(minimum: 150))]
-    
-    @State var flippedOption: GeneralOption?
-    
     // list of options
     @State var tweakOptions: [GeneralOption] = [
         .init(value: getDefaultStr(forKey: "Dock"), key: "Dock", sbType: .dock, title: NSLocalizedString("Dock", comment: "Springboard tool"), imageName: "dock.rectangle", fileType: OverwritingFileTypes.springboard, options: ["Visible", "Color", "Disabled"]),
@@ -44,67 +62,107 @@ struct SpringBoardView: View {
         .init(value: getDefaultStr(forKey: "NotifShadow"), key: "NotifShadow", sbType: .notifShadow, title: NSLocalizedString("Notification Banner Shadow", comment: "Springboard tool"), shortTitle: "Notification Shadow", imageName: "platter.filled.top.iphone", fileType: OverwritingFileTypes.springboard, options: ["Visible", "Color", "Disabled"]),
 //        .init(value: getDefaultStr(forKey: "ShortcutBanner"), key: "ShortcutBanner", title: NSLocalizedString("Shortcut Notification Banner", comment: "Springboard tool"), imageName: "pencil.slash", fileType: .springboard, options: ["Visible", "Disabled"], minimumOS: 16)
     ]
+
+    let mgr: laramgr
+    let replacementPaths: [String: [String]] = [
+        SpringBoardOptions.DockHidden.rawValue: ["CoreMaterial.framework/dockDark.materialrecipe", "CoreMaterial.framework/dockLight.materialrecipe"],
+        SpringBoardOptions.HomeBarHidden.rawValue: ["MaterialKit.framework/Assets.car"],
+        SpringBoardOptions.FolderBGHidden.rawValue: ["SpringBoardHome.framework/folderLight.materialrecipe", "SpringBoardHome.framework/folderDark.materialrecipe", "SpringBoardHome.framework/folderDarkSimplified.materialrecipe"],
+        SpringBoardOptions.FolderBlurDisabled.rawValue: ["SpringBoardHome.framework/folderExpandedBackgroundHome.materialrecipe", "SpringBoardHome.framework/folderExpandedBackgroundHomeSimplified.materialrecipe"],
+        SpringBoardOptions.SwitcherBlurDisabled.rawValue: ["SpringBoard.framework/homeScreenBackdrop-application.materialrecipe", "SpringBoard.framework/homeScreenBackdrop-switcher.materialrecipe"],
+        SpringBoardOptions.CCModuleBackgroundDisabled.rawValue: ["CoreMaterial.framework/modules.materialrecipe"],
+        SpringBoardOptions.PodBackgroundDisabled.rawValue: ["SpringBoardHome.framework/podBackgroundViewLight.visualstyleset", "SpringBoardHome.framework/podBackgroundViewDark.visualstyleset"],
+        SpringBoardOptions.NotifBackgroundDisabled.rawValue: ["CoreMaterial.framework/plattersDark.materialrecipe", "CoreMaterial.framework/platters.materialrecipe"],
+        SpringBoardOptions.ShortcutBanner.rawValue: ["SpringBoard.framework/BannersAuthorizedBundleIDs.plist"]
+    ]
     
     var body: some View {
-        GeometryReader { screenGeometry in
-            ZStack {
-                Rectangle()
-                    .opacity(0)
-                    .ignoresSafeArea(edges: .all)
-                    .blur(radius: flippedOption == nil ? 0 : 8, opaque: false)
-                    .frame(maxWidth: .infinity, maxHeight: .infinity)
-                ScrollView {
-                    VStack {
-                        Button {
-                            if flippedOption == nil {
-                                applyTweaks()
-                            } else {
-                                withAnimation(Animation.easeInOut(duration: CardAnimationSpeed)) {
-                                    for (i, opt) in tweakOptions.enumerated() {
-                                        if opt.title == flippedOption?.title {
-                                            tweakOptions[i].animate3d = false
-                                            break
+        NavigationStack {
+            List {
+                Section(header: HeaderLabel(text: "Applying", icon: "checkmark")) {
+                    Button("Apply") {
+                        applyTweaks()
+                    }
+                    Button("Reset all") {
+                        for ind in tweakOptions.indices {
+                            tweakOptions[ind].value = "Visible"
+                            tweakOptions[ind].selectedOption = "Visible"
+                            UserDefaults.standard.set("Visible", forKey: tweakOptions[ind].key)
+                        }
+                        applyTweaks()
+                    }
+                }
+                ForEach($tweakOptions) { $option in
+                    Section(header: HeaderLabel(text: option.title, icon: option.imageName)) {
+                        Picker("Option", selection: $option.selectedOption) {
+                            ForEach(0..<option.options.count) { ind in
+                                Text(option.options[ind]).tag(option.options[ind])
+                            }
+                        }
+                        .labelsHidden()
+                        .pickerStyle(.segmented)
+                        .onChange(of: option.selectedOption) { newvalue in
+                            option.value = newvalue
+                            UserDefaults.standard.set(newvalue, forKey: option.key)
+                        }
+                        if option.selectedOption == "Color" || option.selectedOption == "Blur" {
+                            if option.selectedOption == "Color" {
+                                HStack(spacing: 12) {
+                                    Text("Color")
+                                    Spacer()
+                                    Text(colortohex(option.color))
+                                        .monospaced()
+                                        .foregroundColor(.secondary)
+                                    ColorPicker("Set notification banner color", selection: $option.color)
+                                        .labelsHidden()
+                                        .frame(width: 40)
+                                        .onChange(of: option.color) { newcolor in
+                                            do {
+                                                try SpringboardColorManager.createColor(forType: option.sbType!, color: CIColor(color: UIColor(newcolor)), blur: Int(option.blur), asTemp: false)
+                                                print("Success")
+                                            } catch {
+                                                print(error.localizedDescription)
+                                            }
                                         }
-                                    }
-                                    flippedOption = nil
                                 }
                             }
-                        } label: {
-                            ButtonLabel(text: "Apply", icon: "checkmark")
-                        }
-                        .buttonStyle(TranslucentButtonStyle(color: .green))
-                        
-                        Spacer()
-                    }
-                    //.blur(radius: flippedOption == nil ? 0 : 8, opaque: false)
-                    
-                    LazyVGrid(columns: gridItemLayout, alignment: .center) {
-                        ForEach($tweakOptions) { option in
-                            SpringBoardOptionView(option: option, screenGeom: screenGeometry, flippedOption: $flippedOption, otherOptions: $tweakOptions)
-                                .zIndex(option.flipped.wrappedValue ? 2 : 0)
-                                //.blur(radius: (flippedOption == nil || flippedOption!.title == option.title.wrappedValue) ? 0 : 8, opaque: false)
+                            HStack {
+                                Text("Blur:")
+                                Spacer()
+                                Text("\(Int(option.blur))")
+                                    .monospaced()
+                                    .foregroundColor(.secondary)
+                            }
+                            Slider(value: $option.blur, in: 0...150, step: 1.0, onEditingChanged: { _ in
+                                do {
+                                    try SpringboardColorManager.createColor(forType: option.sbType!, color: CIColor(color: UIColor(option.color)), blur: Int(option.blur), asTemp: false)
+                                    print("Success")
+                                } catch {
+                                    print(error.localizedDescription)
+                                }
+                            })
                         }
                     }
-                    .padding(.vertical)
                 }
             }
-            .coordinateSpace(name: "mainFrame")
+            .navigationTitle("SpringBoard Tools")
             .onAppear {
-                for (i, option) in tweakOptions.enumerated() {
-                    tweakOptions[i].value = getDefaultStr(forKey: option.key)
-                    tweakOptions[i].selectedOption = option.value
-                    if option.sbType != nil {
-                        if option.value == "Color" {
-                            tweakOptions[i].color = SpringboardColorManager.getColor(forType: option.sbType!)
-                            tweakOptions[i].blur = SpringboardColorManager.getBlur(forType: option.sbType!)
-                        }
-                    }
+                load()
+            }
+        }
+    }
+
+    func load() {
+        for (i, option) in tweakOptions.enumerated() {
+            tweakOptions[i].value = getDefaultStr(forKey: option.key)
+            tweakOptions[i].selectedOption = tweakOptions[i].value
+            if option.sbType != nil {
+                if option.value == "Color" {
+                    tweakOptions[i].color = SpringboardColorManager.getColor(forType: option.sbType!)
+                    tweakOptions[i].blur = SpringboardColorManager.getBlur(forType: option.sbType!)
                 }
             }
         }
-        .padding()
-        .navigationTitle("SpringBoard Tools")
-        .navigationViewStyle(.stack)
     }
     
     func apply(_ sbType: SpringboardColorManager.SpringboardType, _ color: Color, _ blur: Int, save: Bool = true) -> Bool {
@@ -121,408 +179,130 @@ struct SpringBoardView: View {
             return false
         }
     }
-    
+
     func applyTweaks() {
-        if !inProgress {
-            //UIApplication.shared.alert(title: NSLocalizedString("Applying Springboard Tweaks...", comment: ""), body: NSLocalizedString("Please wait...", comment: ""), animated: true, withButton: false)
-            var failed: Bool = false
-            for option in tweakOptions {
-                //  apply tweak
-                if option.value == "Disabled" {
-                    print("Applying tweak \"" + option.title + "\"")
-                    var succeeded = false
-                    if option.sbType != nil {
-                        succeeded = apply(option.sbType!, .gray.opacity(0), 0)
-                    } else {
-                        succeeded = overwriteFile(typeOfFile: option.fileType, fileIdentifier: option.key, true)
-                    }
-                    if succeeded {
-                        print("Successfully applied tweak \"" + option.title + "\"")
-                    } else {
-                        print("Failed to apply tweak \"" + option.title + "\"!!!")
-                        failed = true
-                    }
-                    
-                } else if option.value == "Visible" {
-                    print("Applying tweak \"" + option.title + "\"")
-                    if option.sbType != nil {
-                        if option.sbType! == .switcher {
-                            let succeeded = apply(option.sbType!, .gray.opacity(1), 20, save: false)
-                            if succeeded {
-                                print("Successfully applied tweak \"" + option.title + "\"")
-                            } else {
-                                print("Failed to apply tweak \"" + option.title + "\"!!!")
-                            }
-                        } else {
-                            do {
-                                try SpringboardColorManager.revertFiles(forType: option.sbType!)
-                                print("Successfully applied tweak \"" + option.title + "\"")
-                            } catch {
-                                print("Failed to apply tweak \"" + option.title + "\"!!!")
-                                print(error.localizedDescription)
-                            }
-                        }
-                    } else {
-                        let succeeded = overwriteFile(typeOfFile: option.fileType, fileIdentifier: option.key, false)
+        var failed: Bool = false
+        for option in tweakOptions {
+            //  apply tweak
+            if option.value == "Disabled" {
+                print("Applying tweak \"" + option.title + "\"")
+                var succeeded = false
+                if option.sbType != nil {
+                    succeeded = apply(option.sbType!, .gray.opacity(0), 0)
+                } else {
+                    succeeded = overwriteFile(typeOfFile: option.fileType, fileIdentifier: option.key, true)
+                }
+                if succeeded {
+                    print("Successfully applied tweak \"" + option.title + "\"")
+                } else {
+                    print("Failed to apply tweak \"" + option.title + "\"!!!")
+                    failed = true
+                }
+                
+            } else if option.value == "Visible" {
+                print("Applying tweak \"" + option.title + "\"")
+                if option.sbType != nil {
+                    if option.sbType! == .switcher {
+                        let succeeded = apply(option.sbType!, .gray.opacity(1), 20, save: false)
                         if succeeded {
                             print("Successfully applied tweak \"" + option.title + "\"")
                         } else {
                             print("Failed to apply tweak \"" + option.title + "\"!!!")
                         }
-                    }
-                    
-                } else if option.value == "Color" || option.value == "Blur" {
-                    if option.sbType != nil {
-                        print("Applying tweak \"" + option.title + "\"")
-                        let succeeded = apply(option.sbType!, option.color, Int(option.blur))
-                        if succeeded {
+                    } else {
+                        do {
+                            try SpringboardColorManager.revertFiles(forType: option.sbType!)
                             print("Successfully applied tweak \"" + option.title + "\"")
-                        } else {
+                        } catch {
                             print("Failed to apply tweak \"" + option.title + "\"!!!")
-                            failed = true
+                            print(error.localizedDescription)
                         }
-                    } else {
-                        print("\(option.title) does not have a springboard type!")
-                        failed = true
                     }
-                }
-            }
-            
-            DispatchQueue.main.asyncAfter(deadline: .now() + 0.3) {
-                //UIApplication.shared.dismissAlert(animated: true)
-                if failed {
-                    Alertinator.shared.alert(title: "useless ass alert", body: "something failed while applying tweaks")
-                    //UIApplication.shared.alert(body: "An error occurred when applying tweaks")
                 } else {
-                    Alertinator.shared.alert(title: "yayy", body: "it should've worked?")
-                    //UIApplication.shared.alert(title: NSLocalizedString("Successfully applied tweaks", comment: "Successfully applied tweaks"), body: NSLocalizedString("Respring to see changes", comment: "Respring to see changes"))
-                }
-            }
-        }
-    }
-    
-    struct GeneralOption: Identifiable {
-        var value: String
-        var id = UUID()
-        var key: String
-        var sbType: SpringboardColorManager.SpringboardType?
-        var title: String
-        var shortTitle: String?
-        var imageName: String
-        var fileType: OverwritingFileTypes
-        var options: [String] = []
-        var selectedOption: String = "Visible"
-        var flipped: Bool = false
-        var animate3d: Bool = false
-        
-        var minimumOS: Int = 14
-        
-        var color: Color = Color.gray
-        var blur: Double = 30
-    }
-    
-    struct SpringBoardOptionView: View {
-        @Binding var option: GeneralOption
-        var screenGeom: GeometryProxy
-        @Binding var flippedOption: GeneralOption?
-        @Binding var otherOptions: [GeneralOption]
-
-        var body: some View {
-            return GeometryReader { cardGeometry in
-                VStack {
-                    Spacer()
-                    ZStack {
-                        FrontSpringBoardView(option: $option, flippedOption: $flippedOption, otherOptions: $otherOptions).opacity(option.flipped ? 0.0 : 1.0)
-                            .frame(width: cardGeometry.size.width, height: cardGeometry.size.height)
-                            .scaleEffect(option.animate3d ? (min(screenGeom.size.width*0.95, screenGeom.size.height*0.95)) / cardGeometry.size.width: 1)
-                        
-                        BackSpringBoardView(option: $option, flippedOption: $flippedOption, screenGeom: screenGeom).opacity(option.flipped ? 1.0 : 0.0)
-                            .frame(width: min(screenGeom.size.width, screenGeom.size.height)*0.95, height: min(screenGeom.size.width, screenGeom.size.height)*0.95)
-                            .scaleEffect(option.animate3d ? 1 : cardGeometry.size.width / (min(screenGeom.size.width*0.95, screenGeom.size.height*0.95)))
+                    let succeeded = overwriteFile(typeOfFile: option.fileType, fileIdentifier: option.key, false)
+                    if succeeded {
+                        print("Successfully applied tweak \"" + option.title + "\"")
+                    } else {
+                        print("Failed to apply tweak \"" + option.title + "\"!!!")
                     }
-                    .modifier(FlipEffect(flipped: $option.flipped, angle: option.animate3d ? 180 : 0, axis: (x: 0.25, y: 1)))
-                    .offset(x: option.animate3d ? -cardGeometry.frame(in: .named("mainFrame")).origin.x + screenGeom.size.width*0.025: -(screenGeom.size.width*0.95)/2 + cardGeometry.size.width/2, y: option.animate3d ? -cardGeometry.frame(in: .named("mainFrame")).origin.y + (screenGeom.size.height)/8: -screenGeom.size.width/2 + cardGeometry.size.height/2)
-                    
                 }
-            }
-            .aspectRatio(1, contentMode: .fit)
-        }
-    }
-
-    struct FrontSpringBoardView: View {
-        @Binding var option: GeneralOption
-        @Binding var flippedOption: GeneralOption?
-        @Binding var otherOptions: [GeneralOption]
-        
-        var body: some View {
-            return VStack {
-                VStack {
-                    Image(systemName: option.imageName)
-                        .resizable()
-                        .aspectRatio(contentMode: .fit)
-                        .frame(width: 24, height: 24)
-                        .foregroundColor(.blue)
-                        .opacity(0.5)
-                        .padding(.top, 28)
-                    
-                    Text(option.title)
-                        .foregroundStyle(Color(.label))
-                        .lineLimit(2)
-                        .padding(.horizontal)
-                        .minimumScaleFactor(0.5)
-                        .multilineTextAlignment(.center)
-                    
-                    Image(systemName: option.selectedOption == "Disabled" ? "x.circle" : "pencil.circle")
-                        .resizable()
-                        .aspectRatio(contentMode: .fit)
-                        .frame(width: 24, height: 24)
-                        .foregroundColor(option.selectedOption == "Color" ? .green : .red)
-                        .opacity(option.selectedOption == "Visible" ? 0 : 0.8)
-                }
-                .frame(maxWidth: .infinity, maxHeight: .infinity)
-                .padding(.vertical)
-                .background(Color(.secondarySystemBackground)
-                    .opacity(1)
-                )
-                .cornerRadius(10)
-                .onTapGesture {
-                    if flippedOption == nil {
-                        withAnimation(Animation.easeInOut(duration: CardAnimationSpeed)) {
-                            flippedOption = option
-                            option.animate3d.toggle()
-                        }
+                
+            } else if option.value == "Color" || option.value == "Blur" {
+                if option.sbType != nil {
+                    print("Applying tweak \"" + option.title + "\"")
+                    let succeeded = apply(option.sbType!, option.color, Int(option.blur))
+                    if succeeded {
+                        print("Successfully applied tweak \"" + option.title + "\"")
                     } else {
-                        withAnimation(Animation.easeInOut(duration: CardAnimationSpeed)) {
-                            for (i, opt) in otherOptions.enumerated() {
-                                if opt.title == flippedOption?.title {
-                                    otherOptions[i].animate3d = false
-                                    break
-                                }
-                            }
-                            flippedOption = nil
-                        }
+                        print("Failed to apply tweak \"" + option.title + "\"!!!")
+                        failed = true
                     }
+                } else {
+                    print("\(option.title) does not have a springboard type!")
+                    failed = true
                 }
             }
         }
+        if failed {
+            Alertinator.shared.alert(title: "useless ass alert", body: "something failed while applying tweaks")
+        } else {
+            Alertinator.shared.alert(title: "Success!", body: "Respring to see changes.", actionLabel: "Respring", action: { mgr.respring() })
+        }
     }
     
-    struct BackSpringBoardView: View {
-        @Binding var option: GeneralOption
-        @Binding var flippedOption: GeneralOption?
-        var screenGeom: GeometryProxy
-        
-        var body: some View {
-            return VStack (alignment: .center) {
-                HStack {
-                    Text(option.shortTitle ?? option.title)
-                        .font(.title)
-                        .bold()
-                        .padding(.horizontal, 20)
-                        .padding(.top, 20)
-                    Spacer()
-                }
-                
-                ZStack {
-                    Rectangle()
-                        .foregroundColor(Color(.secondarySystemFill))
-                        .cornerRadius(8)
-                        .frame(width: screenGeom.size.width - 25 - (10 * CGFloat(option.options.count)), height: screenGeom.size.width*0.075)
-                    HStack {
-                        ForEach(0..<option.options.count) { ind in
-                            Button(action: {
-                                option.selectedOption = option.options[ind]
-                            }) {
-                                ZStack {
-                                    Rectangle()
-                                        .foregroundStyle(Color(.systemBackground).opacity(option.selectedOption == option.options[ind] ? 1 : 0))
-                                    Text(option.options[ind])
-                                        .font(.system(size: 13))
-                                        .lineLimit(1)
-                                }
-                                .frame(width: (screenGeom.size.width/CGFloat(option.options.count)) - (10 * CGFloat(option.options.count)), height: screenGeom.size.width*0.075)
-                            }
-                            .buttonStyle(.plain)
-                        }
-                    }
-                }
-                .padding(.horizontal, 20)
-                .padding(.bottom, 15)
-                .pickerStyle(.segmented)
-                Spacer()
-                
-                // MARK: Visible Text
-                if option.selectedOption == "Visible" {
-                    Image(systemName: "checkmark.circle")
-                        .foregroundColor(.green)
-                        .font(.system(size: 40))
-                    
-                // MARK: Color and Blur selector
-                } else if option.selectedOption == "Color" || option.selectedOption == "Blur" {
-                    VStack{
-                        if option.selectedOption == "Color" {
-                            HStack {
-                                Text("Color:")
-                                    .font(.title)
-                                    .fontWeight(.medium)
-                                    .padding(.horizontal, 25)
-                                Spacer()
-                                ColorPicker("Set notification banner color", selection: $option.color)
-                                    .labelsHidden()
-                                    .scaleEffect(1.5)
-                                    .padding(.horizontal, 50)
+    // MARK: sigh     //UIApplication.shared.alert(title: NSLocalizedString("Successfully applied tweaks", comment: "Successfully applied tweaks"), body: NSLocalizedString("Respring to see changes", comment: "Respring to see changes"))
+    func overwriteFile<Value>(typeOfFile: OverwritingFileTypes, fileIdentifier: String, _ value: Value) -> Bool {
+        // find the path and replace the file
+        // springboard option
+        if typeOfFile == OverwritingFileTypes.springboard {
+            // springboard tweak being applied
+            if replacementPaths[fileIdentifier] != nil {
+                var succeeded = true
+                for path in replacementPaths[fileIdentifier]! {
+                    if fileIdentifier == "HomeBar" && value as? Bool == false {
+                        if let url: URL = Bundle.main.url(forResource: "HomeBarAssets", withExtension: "car") {
+                            do {
+                                let replacementCar = try Data(contentsOf: url)
+                                //try MDC.overwriteFile(at: "/System/Library/PrivateFrameworks/" + path, with: replacementCar)
+                            } catch {
+                                print(error.localizedDescription)
+                                succeeded = false
                             }
+                        } else {
+                            print("Home bar file not found!")
+                            return false
                         }
-                        HStack {
-                            Text("Blur:   \(Int(option.blur))")
-                                .frame(width: 125)
-                            Spacer()
-                            Slider(value: $option.blur, in: 0...150, step: 1.0)
-                                .padding(.horizontal)
-                        }
-                    }
-                    
-                // MARK: Disabled Text
-                } else if option.selectedOption == "Disabled" {
-                    Image(systemName: "x.circle")
-                        .foregroundColor(.red)
-                        .font(.system(size: 40))
-                }
-                
-                Spacer()
-                
-                Button(action: {
-                    // save
-                    UserDefaults.standard.set(option.selectedOption, forKey: option.key)
-                    option.value = option.selectedOption
-                    if option.selectedOption == "Color" || option.selectedOption == "Blur" {
-                        do {
-                            try SpringboardColorManager.createColor(forType: option.sbType!, color: CIColor(color: UIColor(option.color)), blur: Int(option.blur), asTemp: false)
-                            print("Success")
-                        } catch {
-                            print(error.localizedDescription)
-                        }
-                    }
-                    
-                    // close menu
-                    if flippedOption!.title == option.title {
-                        withAnimation(Animation.easeInOut(duration: CardAnimationSpeed)) {
-                            flippedOption = nil
-                            option.animate3d.toggle()
+                    } else {
+                        let randomGarbage = Data("###".utf8)
+                        
+                        let result = laramgr.shared.lara_overwritefile(target: "/System/Library/PrivateFrameworks/" + path, data: randomGarbage)
+                        
+                        if result.ok {
+                            print("i hope it worked")
+                        } else {
+                            print("it didn't")
                         }
+                        
+                        return true
                     }
-                }) {
-                    Text("Save")
                 }
-                //.buttonStyle(TintedButton(color: .blue, fullwidth: true))
-                .padding(25)
+                return succeeded
             }
-            .background(Color(.secondarySystemBackground))
-            .cornerRadius(10)
         }
+        
+        return true
     }
 }
 
-// MARK: sigh
-func overwriteFile<Value>(typeOfFile: OverwritingFileTypes, fileIdentifier: String, _ value: Value) -> Bool {
-    // find the path and replace the file
-    // springboard option
-    if typeOfFile == OverwritingFileTypes.springboard {
-        // springboard tweak being applied
-        if replacementPaths[fileIdentifier] != nil {
-            var succeeded = true
-            for path in replacementPaths[fileIdentifier]! {
-                if fileIdentifier == "HomeBar" && value as? Bool == false {
-                    if let url: URL = Bundle.main.url(forResource: "HomeBarAssets", withExtension: "car") {
-                        do {
-                            let replacementCar = try Data(contentsOf: url)
-                            //try MDC.overwriteFile(at: "/System/Library/PrivateFrameworks/" + path, with: replacementCar)
-                        } catch {
-                            print(error.localizedDescription)
-                            succeeded = false
-                        }
-                    } else {
-                        print("Home bar file not found!")
-                        return false
-                    }
-                } else {
-                    let randomGarbage = Data("###".utf8)
-                    
-                    let result = laramgr.shared.lara_overwritefile(target: "/System/Library/PrivateFrameworks/" + path, data: randomGarbage)
-                    
-                    if result.ok {
-                        print("i hope it worked")
-                    } else {
-                        print("it didn't")
-                    }
-                    
-                    return true
-                }
-            }
-            return succeeded
-        }
-    }
+func getDefaultStr(forKey: String, defaultValue: String = "Visible") -> String {
+    let defaults = UserDefaults.standard
     
-    return true
-}
-
-struct FlipEffect: GeometryEffect {
-
-      var animatableData: Double {
-            get { angle }
-            set { angle = newValue }
-      }
-
-      @Binding var flipped: Bool
-      var angle: Double
-      let axis: (x: CGFloat, y: CGFloat)
-
-      func effectValue(size: CGSize) -> ProjectionTransform {
-
-            DispatchQueue.main.async {
-                  self.flipped = self.angle >= 90 && self.angle < 270
-            }
-
-            let tweakedAngle = flipped ? -180 + angle : angle
-            let a = CGFloat(Angle(degrees: tweakedAngle).radians)
-
-            var transform3d = CATransform3DIdentity;
-            transform3d.m34 = -1/max(size.width, size.height)
-
-            transform3d = CATransform3DRotate(transform3d, a, axis.x, axis.y, 0)
-            transform3d = CATransform3DTranslate(transform3d, -size.width/2.0, -size.height/2.0, 0)
-
-            let affineTransform = ProjectionTransform(CGAffineTransform(translationX: size.width/2.0, y: size.height / 2.0))
-
-            return ProjectionTransform(transform3d).concatenating(affineTransform)
-      }
-}
-
-let replacementPaths: [String: [String]] = [
-    SpringBoardOptions.DockHidden.rawValue: ["CoreMaterial.framework/dockDark.materialrecipe", "CoreMaterial.framework/dockLight.materialrecipe"],
-    SpringBoardOptions.HomeBarHidden.rawValue: ["MaterialKit.framework/Assets.car"],
-    SpringBoardOptions.FolderBGHidden.rawValue: ["SpringBoardHome.framework/folderLight.materialrecipe", "SpringBoardHome.framework/folderDark.materialrecipe", "SpringBoardHome.framework/folderDarkSimplified.materialrecipe"],
-    SpringBoardOptions.FolderBlurDisabled.rawValue: ["SpringBoardHome.framework/folderExpandedBackgroundHome.materialrecipe", "SpringBoardHome.framework/folderExpandedBackgroundHomeSimplified.materialrecipe"],
-    SpringBoardOptions.SwitcherBlurDisabled.rawValue: ["SpringBoard.framework/homeScreenBackdrop-application.materialrecipe", "SpringBoard.framework/homeScreenBackdrop-switcher.materialrecipe"],
-    SpringBoardOptions.CCModuleBackgroundDisabled.rawValue: ["CoreMaterial.framework/modules.materialrecipe"],
-    SpringBoardOptions.PodBackgroundDisabled.rawValue: ["SpringBoardHome.framework/podBackgroundViewLight.visualstyleset", "SpringBoardHome.framework/podBackgroundViewDark.visualstyleset"],
-    SpringBoardOptions.NotifBackgroundDisabled.rawValue: ["CoreMaterial.framework/plattersDark.materialrecipe", "CoreMaterial.framework/platters.materialrecipe"],
-    SpringBoardOptions.ShortcutBanner.rawValue: ["SpringBoard.framework/BannersAuthorizedBundleIDs.plist"]
-]
-
-
-enum SpringBoardOptions: String, CaseIterable {
-    case DockHidden = "DockHidden"
-    case HomeBarHidden = "HomeBar"
-    case FolderBGHidden = "FolderBGHidden"
-    case FolderBlurDisabled = "FolderBlurDisabled"
-    case SwitcherBlurDisabled = "SwitcherBlurDisabled"
-    case CCModuleBackgroundDisabled = "CCModuleBackgroundDisabled"
-    case PodBackgroundDisabled = "PodBackgroundDisabled"
-    case NotifBackgroundDisabled = "NotifBackgroundDisabled"
-    case ShortcutBanner = "ShortcutBanner"
+    return defaults.string(forKey: forKey) ?? defaultValue
 }
 
-#Preview {
-    SpringBoardView()
-}
+func colortohex(_ color: Color) -> String {
+    let ui = UIColor(color)
+    var r: CGFloat = 0, g: CGFloat = 0, b: CGFloat = 0, a: CGFloat = 0
+    ui.getRed(&r, green: &g, blue: &b, alpha: &a)
+    return String(format: "#%02X%02X%02X (%02X)", Int(r*255), Int(g*255), Int(b*255), Int(a*255))
+}
\ No newline at end of file
  ```

  </details>


---

## Timeline (Verbose)

- : 📝 Commit bf3d257: fix SBView UI
- May 21, 2026: 👀 @jurre111 requested review from @Copilot
- : 💬 someone reviewed: commented
- : 📝 Commit c453c98: fix saving
- May 22, 2026: 🟣 @lunginspector merged
- May 22, 2026: 🔴 Closed by @lunginspector


---

## Comments (1)

### Comment #1 — [@jurre111](https://github.com/jurre111) · May 21, 2026

`ID: 4513055871`


Yeah it's not perfect, but at least it's usable now. Kinda needs a rewrite...


---

*Exported on 10/14/2026, 3:07:55 PM from [https://github.com/rooootdev/lara/pull/257](https://github.com/rooootdev/lara/pull/257)*
