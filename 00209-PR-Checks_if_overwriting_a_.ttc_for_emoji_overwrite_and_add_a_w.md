# Checks if overwriting a .ttc for emoji overwrite and add a warning if not.

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#209](https://github.com/rooootdev/lara/pull/209)  
> **State:** 🟣 merged  
> **Branch:** `neonmodder123:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 28, 2026  
> **Changes:** +14 / -0 across 1 files  
> **Created:** Apr 28, 2026  
> **Updated:** Apr 28, 2026  
> **Closed:** Apr 28, 2026  

---

## Description

*Opened by [@neonmodder123](https://github.com/neonmodder123)* *on Apr 28, 2026*


I will make a PR to larafonts to have updated .ttc files for emoji overwrite


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/25070816973/job/73450533023) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/25070816973/job/73450292958) — **success** — *GitHub Actions*


---

## PR Commits (1)

- [`e10c2c7`](https://github.com/rooootdev/lara/commit/e10c2c7043747a81db1b3c96751b202591e16722) add .ttc emoji checks checks — *neon* · Apr 28, 2026


---

## Changed Files (1)

- 🟡 `lara/views/FontPicker.swift` (modified) — +14 / -0
  <details><summary>Diff</summary>

  ```diff
  @@ -347,6 +347,10 @@ private struct repoemojirow: View {
 
         Button {
             if isdownloaded, let localurl {
+				guard localurl.pathExtension.lowercased() == "ttc" else {
+            		mgr.logmsg("emoji font must be .ttc, got .\(localurl.pathExtension)")
+            		return
+        		}
                 let success = mgr.vfsoverwritefromlocalpath(target: emojipath, source: localurl.path)
                 success ? mgr.logmsg("emoji changed to \(emoji.name)") : mgr.logmsg("failed to change emojis")
             } else {
@@ -355,6 +359,16 @@ private struct repoemojirow: View {
         } label: {
             HStack {
                 Text(emoji.name)
+				if let remoteurl = URL(string: emoji.url),
+           			remoteurl.pathExtension.lowercased() != "ttc" {
+            		Text("not .ttc")
+                	.font(.caption2)
+                	.foregroundColor(.red)
+                	.padding(.horizontal, 5)
+                	.padding(.vertical, 2)
+                	.background(Color.red.opacity(0.1))
+                	.clipShape(Capsule())
+        		}
                 Spacer()
                 if repostore.downloading.contains(emoji.url) {
                     ProgressView()
  ```

  </details>


---

## References

### Commits (1)

- [9fc6ffb](https://github.com/letienndat/lara/commit/9fc6ffb8e54049fe785acc3b6361633ea2578ce1) — Squashed commit of the following:


---

## Timeline (Verbose)

- : 📝 Commit e10c2c7: add .ttc emoji checks checks
- Apr 28, 2026: 🟣 @rooootdev merged
- Apr 28, 2026: 🔴 Closed by @rooootdev
- Apr 29, 2026: 🔗 Commit 9fc6ffb — Squashed commit of the following:


---

*Exported on 10/14/2026, 3:03:54 PM from [https://github.com/rooootdev/lara/pull/209](https://github.com/rooootdev/lara/pull/209)*
