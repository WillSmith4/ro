# Fix version date format and update size

> **Repository:** [rooootdev/lara](https://github.com/rooootdev/lara)  
> **Pull Request:** [#44](https://github.com/rooootdev/lara/pull/44)  
> **State:** 🟣 merged  
> **Branch:** `Pro42good:patch-2` → `rooootdev:main`  
> **Merged by:** [@rooootdev](https://github.com/rooootdev) on Apr 6, 2026  
> **Changes:** +2 / -2 across 1 files  
> **Created:** Apr 6, 2026  
> **Updated:** Apr 6, 2026  
> **Closed:** Apr 6, 2026  

---

## Description

*Opened by [@Pro42good](https://github.com/Pro42good)* *on Apr 6, 2026*


Fixes source.json to be AltStore/SideStore compliant.

- date: changed "19.5.2025" → "2025-05-19" (ISO 8601, required by AltStore spec)
- size: changed 2000 → 3450675 (actual IPA size in bytes, 3.29 MB)


---

## Checks & Status

- Overall: 🟡 **pending**

### Check Runs (5)

- 🔴 [Cleanup artifacts](https://github.com/rooootdev/lara/actions/runs/24036351290/job/70096927795) — **failure** — *GitHub Actions*
- 🔴 [build](https://github.com/rooootdev/lara/actions/runs/24036351135/job/70096810403) — **failure** — *GitHub Actions*
- ✅ [Upload results](https://github.com/rooootdev/lara/actions/runs/24036351290/job/70096896534) — **success** — *GitHub Actions*
- ✅ [Agent](https://github.com/rooootdev/lara/actions/runs/24036351290/job/70096828129) — **success** — *GitHub Actions*
- ✅ [Prepare](https://github.com/rooootdev/lara/actions/runs/24036351290/job/70096813778) — **success** — *GitHub Actions*


---

## Reviews (1)

### 💬 COMMENTED — [@copilot-pull-request-reviewer[bot]](https://github.com/apps/copilot-pull-request-reviewer) · Apr 6, 2026

## Pull request overview

Updates the AltStore/SideStore source manifest to match the expected metadata formats for app versions.

**Changes:**
- Convert the app version `date` field to ISO 8601 (`YYYY-MM-DD`).
- Update the app version `size` to the actual IPA byte size.





---

💡 <a href="/rooootdev/lara/new/main?filename=.github/instructions/*.instructions.md" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Add Copilot custom instructions</a> for smarter, more guided reviews. <a href="https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot" class="Link--inTextBlock" target="_blank" rel="noopener noreferrer">Learn how to get started</a>.


---

## PR Commits (1)

- [`d5cb2ef`](https://github.com/rooootdev/lara/commit/d5cb2ef58279814203a3a1dcecde262806c20fe8) Fix version date format and update size — *Pro42good DQ* · Apr 6, 2026


---

## Changed Files (1)

- 🟡 `source.json` (modified) — +2 / -2
  <details><summary>Diff</summary>

  ```diff
  @@ -21,8 +21,8 @@
       "versions": [
         {
           "version": "1.1",
-          "date": "19.5.2025",
-          "size": 2000,
+          "date": "2025-05-19",
+          "size": 3450675,
           "downloadURL": "https://github.com/rooootdev/lara/releases/download/latest/lara.ipa",
           "localizedDescription": "",
           "minOSVersion": "15.0"
  ```

  </details>


---

## References

### Commits (1)

- [a95bd61](https://github.com/danosito/lara-m-chips-fix/commit/a95bd61ed1ad6bcaf1fb5e607b46e342e92ff53c) — Merge pull request #44 from Pro42good/patch-2


---

## Timeline (Verbose)

- : 📝 Commit d5cb2ef: Fix version date format and update size
- Apr 6, 2026: 👀 @Pro42good requested review from @Copilot
- : 💬 someone reviewed: commented
- Apr 6, 2026: 🟣 @rooootdev merged
- Apr 6, 2026: 🔴 Closed by @rooootdev
- Apr 19, 2026: 🔗 Commit a95bd61 — Merge pull request #44 from Pro42good/patch-2


---

## Comments (1)

### Comment #1 — [@Pro42good](https://github.com/Pro42good) · Apr 6, 2026

`ID: 4192912934`


how in the world is my PR build even failing 💀


---

*Exported on 10/14/2026, 2:49:37 PM from [https://github.com/rooootdev/lara/pull/44](https://github.com/rooootdev/lara/pull/44)*
