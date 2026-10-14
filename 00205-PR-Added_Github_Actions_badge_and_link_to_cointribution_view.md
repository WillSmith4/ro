# Added Github Actions badge and link to cointribution view

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#205](https://github.com/rooootdev/lara/pull/205)  
> **State:** 🟣 merged  
> **Branch:** `RenCupDotDev:main` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 27, 2026  
> **Changes:** +4 / -1 across 1 files  
> **Created:** Apr 27, 2026  
> **Updated:** Apr 27, 2026  
> **Closed:** Apr 27, 2026  

---

## Description

*Opened by [@RenCupDotDev](https://github.com/RenCupDotDev)* *on Apr 27, 2026*


second small update from me


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (2)

- ⚪ [release](https://github.com/rooootdev/lara/actions/runs/25009960004/job/73242995559) — **skipped** — *GitHub Actions*
- ✅ [build](https://github.com/rooootdev/lara/actions/runs/25009960004/job/73242794005) — **success** — *GitHub Actions*


---

## Reviews (1)

### 💬 COMMENTED — [@copilot-pull-request-reviewer[bot]](https://github.com/apps/copilot-pull-request-reviewer) · Apr 27, 2026

## Pull request overview

Updates the project README to surface CI status and make it easier for readers to find contributor information.

**Changes:**
- Added a GitHub Actions workflow status badge to the README header badges.
- Updated the credits section to link to the GitHub contributors graph.





---

💡 <a href="/rooootdev/lara/new/main?filename=.github/instructions/*.instructions.md" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Add Copilot custom instructions</a> for smarter, more guided reviews. <a href="https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Learn how to get started</a>.


---

## Review Comments (2)

*Inline comments on code changes*

### `README.md`

**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 27, 2026 (line 23) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -20,6 +20,9 @@
   <a href="https://github.com/rooootdev/lara/releases">
     <img src="https://img.shields.io/github/v/release/rooootdev/lara" alt="Release">
   </a>
+   <a href="https://github.com/rooootdev/lara/actions">
```

</details>

The new GitHub Actions badge link is indented differently than the other badges (one extra leading space before the <a>), which makes the badge block harder to keep consistent. Align the indentation with the surrounding <a> elements.
```suggestion
  <a href="https://github.com/rooootdev/lara/actions">
```


**[@Copilot](https://github.com/apps/copilot-pull-request-reviewer)** · Apr 27, 2026 (line 160) · review: COMMENTED

<details><summary>Diff context</summary>

```diff
@@ -154,7 +157,7 @@ if things get weird later, **Delete Kernelcache Data** in Settings wipes the cac
 - opa334 for the kernel exploit poc, ChOma and XPF
 - AppInstaller iOS for help with offsets
 - AlfieCG for libgrabkernel2
-- Everyone who contributed!
+- Everyone who contributed! (visible <a href="https://github.com/rooootdev/lara/graphs/contributors">Here</a> )
```

</details>

There’s an extra space before the closing parenthesis in the contributors credit line, and the link text/capitalization is a bit awkward. Consider removing the stray space and using more natural wording (e.g., lowercasing the link text) to match the surrounding README style.
```suggestion
- Everyone who contributed! (visible <a href="https://github.com/rooootdev/lara/graphs/contributors">here</a>)
```



---

## PR Commits (3)

- [`f499566`](https://github.com/rooootdev/lara/commit/f499566bccecb8f6997a5a901d49f4f89606b4ff) Update README.md — *RenCup* · Apr 27, 2026
- [`ad5c422`](https://github.com/rooootdev/lara/commit/ad5c42237dd0592dffed63b5defc68032e4bf93a) Enhance contributor section with a link — *RenCup* · Apr 27, 2026
- [`74d9f80`](https://github.com/rooootdev/lara/commit/74d9f80997b50901e9ffc081f10b852c60f99e59) Fix formatting and improve contributor acknowledgment — *Pro42good* · Apr 27, 2026


---

## Changed Files (1)

- 🟡 `README.md` (modified) — +4 / -1
  <details><summary>Diff</summary>

  ```diff
  @@ -20,6 +20,9 @@
   <a href="https://github.com/rooootdev/lara/releases">
     <img src="https://img.shields.io/github/v/release/rooootdev/lara" alt="Release">
   </a>
+  <a href="https://github.com/rooootdev/lara/actions">
+    <img src="https://img.shields.io/github/actions/workflow/status/rooootdev/lara/build.yml?branch=main&style=flat&logo=github" alt="GitHub Actions">
+  </a>
 </p>
 
 <p align="center">
@@ -154,7 +157,7 @@ if things get weird later, **Delete Kernelcache Data** in Settings wipes the cac
 - opa334 for the kernel exploit poc, ChOma and XPF
 - AppInstaller iOS for help with offsets
 - AlfieCG for libgrabkernel2
-- Everyone who contributed!
+- Everyone who contributed! (Visible <a href="https://github.com/rooootdev/lara/graphs/contributors">Here</a>)
 
 <br>
 <div align="center">a beautiful kexploit ♥️</div>
  ```

  </details>


---

## References

### Commits (1)

- [9fc6ffb](https://github.com/letienndat/lara/commit/9fc6ffb8e54049fe785acc3b6361633ea2578ce1) — Squashed commit of the following:


---

## Timeline (Verbose)

- : 📝 Commit f499566: Update README.md
- : 📝 Commit ad5c422: Enhance contributor section with a link
- Apr 27, 2026: 👀 @Pro42good requested review from @Copilot
- : 💬 someone reviewed: commented
- : 📝 Commit 74d9f80: Fix formatting and improve contributor acknowledgment
- Apr 27, 2026: 🟣 @rooootdev merged
- Apr 27, 2026: 🔴 Closed by @rooootdev
- Apr 29, 2026: 🔗 Commit 9fc6ffb — Squashed commit of the following:


---

*Exported on 10/14/2026, 3:03:33 PM from [https://github.com/rooootdev/lara/pull/205](https://github.com/rooootdev/lara/pull/205)*
