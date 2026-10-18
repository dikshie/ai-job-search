# How to Fetch and Merge Upstream Updates (Git CLI)

This guide explains how to manually pull the latest changes from the upstream repository (`MadsLorentzen/ai-job-search`), merge them into your personal fork (`dikshie/ai-job-search`), verify everything passes, and push the updated branch to GitHub.

---

## Step 1: Verify Your Git Remotes (One-Time Check)

Confirm that both `origin` (your fork) and `upstream` (the canonical repository) are configured:

```bash
git remote -v
```

Expected output:
```text
origin    git@github.com:dikshie/ai-job-search.git (fetch)
origin    git@github.com:dikshie/ai-job-search.git (push)
upstream  https://github.com/MadsLorentzen/ai-job-search (fetch)
upstream  https://github.com/MadsLorentzen/ai-job-search (push)
```

*(If `upstream` is missing, add it once with: `git remote add upstream https://github.com/MadsLorentzen/ai-job-search.git`)*

---

## Step 2: Ensure Your Working Tree Is Clean

Switch to `master` and verify there are no uncommitted local changes:

```bash
git checkout master
git status
```

If you have work in progress, commit it or stash it (`git stash`) before merging.

---

## Step 3: Fetch the Latest Changes from `upstream`

Download the latest commits and refs from `upstream` without modifying your local files yet:

```bash
git fetch upstream
```

---

## Step 4: Inspect Incoming Commits (Optional)

Preview what commits have been added to `upstream/master` since your last merge:

```bash
git log --oneline HEAD..upstream/master
```

To see which files changed in `upstream/master` since the common ancestor:

```bash
git diff --stat $(git merge-base HEAD upstream/master)..upstream/master
```

*(You can also run the repo's built-in upstream update checker: `.venv/bin/python tools/check_upstream_updates.py`)*

---

## Step 5: Merge `upstream/master` into Local `master`

Merge the fetched upstream changes into your current `master` branch:

```bash
git merge upstream/master --no-edit
```

### Handling Merge Conflicts (If Any Occur)
If both your fork and `upstream` modified the same lines of a file (e.g., `CLAUDE.md` or `SETUP.md`), Git will pause and list the conflicted files:
1. Check which files have conflicts:
   ```bash
   git status
   ```
2. Open the conflicted file(s) and resolve the conflict markers (`<<<<<<< HEAD`, `=======`, `>>>>>>> upstream/master`), keeping your personal profile data intact while adopting upstream structural updates.
3. Stage the resolved file(s) and complete the merge commit:
   ```bash
   git add <resolved-file>
   git commit --no-edit
   ```

---

## Step 6: Refresh Portal CLIs & Run Verification Checks

After merging, rebuild the Bun portal CLIs (in case upstream updated `.agents/skills/*/cli`) and run the test suite and framework version check:

```bash
# 1. Reinstall / update portal search CLIs
.venv/bin/python tools/install_portal_clis.py

# 2. Run the Python test suite
.venv/bin/pytest

# 3. Verify framework version alignment across agent files
.venv/bin/python tools/check_framework_version.py
```

---

## Step 7: Push the Merged Branch to Your GitHub Fork (`origin`)

Once all checks pass, push your updated `master` branch to your remote repository on GitHub:

```bash
git push origin master
```

Verify everything is in sync:

```bash
git status
```

---

## Quick One-Liner (When Working Tree Is Clean)

For routine updates when no conflicts are expected:

```bash
git fetch upstream && git merge upstream/master --no-edit && .venv/bin/python tools/install_portal_clis.py && .venv/bin/pytest && git push origin master
```
