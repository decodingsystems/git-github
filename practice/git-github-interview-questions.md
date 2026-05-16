# Git & GitHub Interview Questions

## Beginner Level

**1. What is the difference between Git and GitHub?**
Git is a distributed version control system (a command-line tool) that tracks changes to files locally. GitHub is a cloud-based hosting platform for Git repositories that adds collaboration features like Pull Requests, Issues, and CI/CD via GitHub Actions.

**2. What is a commit?**
A commit is a snapshot of all staged changes saved permanently to the repository's history. It includes a unique SHA hash, author, timestamp, message, and a pointer to its parent commit(s).

**3. Explain the three areas of a Git repository.**
- **Working Directory:** Where you edit files.
- **Staging Area (Index):** A preparation zone where you place changes before committing with `git add`.
- **Repository (.git):** The permanent object database where commits are stored.

**4. What is `git status`?**
Shows the current state of the working directory and staging area — listing untracked, modified, and staged files.

**5. How do you undo an unstaged change to a file?**
`git restore <file>` discards changes in the working directory back to the last committed state.

**6. What does `git clone` do?**
Downloads a remote repository to your local machine, creating a copy of the entire history, and automatically configures `origin` as the remote.

**7. What is a branch?**
A lightweight, movable pointer to a specific commit. Creating a branch allows isolated development without affecting other branches.

**8. How do you create and switch to a new branch in one command?**
`git checkout -b <branch-name>` or the modern equivalent `git switch -c <branch-name>`.

**9. What is `git pull` vs `git fetch`?**
- `git fetch origin`: Downloads new commits from the remote but does NOT merge them.
- `git pull`: Does `fetch` + `merge` in one step. Can cause unexpected merges; `fetch` first is safer.

**10. How do you push a local branch to GitHub for the first time?**
`git push -u origin <branch-name>`. The `-u` flag sets the upstream tracking reference.

---

## Intermediate Level

**11. What is a merge conflict and how do you resolve it?**
A conflict occurs when two branches modify the same lines of the same file. Git marks the conflicting sections with `<<<<<<<`, `=======`, `>>>>>>>`. You manually edit the file to the desired state, remove the markers, `git add` the resolved file, and `git commit` to complete the merge.

**12. What is the difference between `git merge` and `git rebase`?**
- `git merge`: Integrates two branches by creating a **merge commit**. Preserves full branch history.
- `git rebase`: Replays commits from one branch on top of another, creating a **linear history**. Rewrites SHAs. Cleaner but should never be used on shared/pushed commits.

**13. What is a fast-forward merge?**
When the target branch has not diverged from the source branch (no new commits on target since the branch point), Git simply moves the pointer forward without creating a merge commit.

**14. Explain `git stash`.**
Saves your current uncommitted changes (staged and unstaged) onto a stack, leaving you with a clean working directory. Useful when you need to switch context urgently. Restore with `git stash pop`.

**15. What does `git revert` do vs `git reset`?**
- `git revert <sha>`: Creates a **new commit** that undoes the changes of a specified commit. Safe for shared history.
- `git reset HEAD~1`: Moves the branch pointer back, effectively removing the commit. Dangerous on shared branches.

**16. What is a detached HEAD state?**
Occurs when HEAD points directly to a commit SHA instead of a branch name. Any commits made in this state are "floating" and can be lost if you switch away. Fix with `git checkout -b <new-branch>`.

**17. How do forks differ from branches?**
A **branch** is a parallel line of development within the same repository. A **fork** is a server-side copy of the entire repository under a different account, used in the open-source contributions workflow.

**18. How do you squash multiple commits into one?**
`git rebase -i HEAD~N` and change `pick` to `squash` (or `s`) for all commits you want merged into the previous one.

**19. What is `git cherry-pick`?**
Applies the changes introduced by a specific commit from another branch onto your current branch. Useful for applying a bug fix from `develop` to a `hotfix` branch.

**20. What are Git tags and how do they differ from branches?**
Tags are immutable references to specific commits, typically used for release versions (e.g., `v1.0.0`). Unlike branches, they never move when new commits are made.

---

## Advanced Level

**21. Explain Git's internal object model.**
Git stores 4 types of objects identified by SHA-1 hashes: **blob** (file content), **tree** (directory), **commit** (snapshot + metadata + parent pointer), **tag** (named commit pointer). All are stored in `.git/objects/`.

**22. What is the difference between `--soft`, `--mixed`, and `--hard` reset?**
All move HEAD backward N commits. `--soft` keeps changes staged. `--mixed` keeps changes unstaged (default). `--hard` destroys all changes permanently.

**23. How does a Pull Request work internally?**
A PR is a GitHub-level construct (not a Git concept). It references two branches and presents their diff. It's a request to merge one branch into another, with a review, discussion, and CI status tracking interface attached.

**24. What is `git reflog`?**
A safety net that records every time HEAD or a branch pointer moved, including after `reset --hard`. You can recover "lost" commits by finding their SHA in the reflog and checking out or creating a branch from it.

**25. What is `git bisect` used for?**
Performs a binary search through commit history to find the specific commit that introduced a bug. You mark commits as `good` or `bad`, and Git narrows down to the culprit efficiently.
