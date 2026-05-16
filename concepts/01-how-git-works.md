# How Git Works — Theory and Internals

## What is Version Control?
A **Version Control System (VCS)** records changes to files over time so you can recall specific versions later. It answers questions like: *Who changed this line? When? Why?* Without VCS, teams resort to emailing zipped folders or overwriting each other's work.

**Centralized VCS (e.g., SVN):** One central server holds the full history. You check out files and check them back in. Single point of failure.

**Distributed VCS (Git):** Every contributor has a **full copy** of the entire repository and its history on their local machine. No central server is required to work. Speed, offline capability, and resilience are the key advantages.

---

## Git's Data Model — Not a Delta System
Most VCS tools store data as a set of files and the *changes* made to each (deltas). Git thinks differently. Git stores data as a series of **snapshots** of the entire project.

Each time you commit, Git takes a picture of what all your files look like at that moment and stores a reference to that snapshot. If files haven't changed, Git doesn't store them again—it links to the previous identical file. This makes Git extremely efficient.

---

## Git's Object Database
Every piece of data in Git is stored as one of 4 object types, all addressed by a **SHA-1 hash** of their content:

| Object | Description |
|--------|-------------|
| **Blob** | Stores the raw content of a single file. No filename or metadata. |
| **Tree** | A directory listing—maps filenames to blob or other tree objects. |
| **Commit** | Points to a root Tree, plus metadata: author, timestamp, commit message, and pointer(s) to parent commit(s). |
| **Tag** | A named, annotated pointer to a specific commit (for releases like `v1.0`). |

Every commit is a snapshot of the repository's root tree at a given moment, with a pointer to its parent commit, forming a chain back to the very first commit.

---

## The Directed Acyclic Graph (DAG)
Git's history is a **DAG (Directed Acyclic Graph)**. Each commit points back to its parent(s). A regular commit has one parent. A merge commit has two or more parents. This graph structure powers branching and merging: branches are simply lightweight, movable pointers to commit nodes in the graph.

---

## The Three Areas of Git
Understanding where your work lives is critical:

1. **Working Directory:** Your local file system. Where you edit files. Changes here are "untracked" or "unstaged".
2. **Staging Area (Index):** A preparation zone. `git add` moves changes here. The Index is exactly what your next commit will look like.
3. **Repository (.git dir):** The permanent object database. `git commit` writes the staged snapshot here permanently.

```
Working Dir  →  git add  →  Staging Area  →  git commit  →  Repository
     ←←←←←←←  git checkout / git restore  ←←←←←←←←←←←←←←
```

---

## HEAD, Branches, and Refs
- **Branch:** A lightweight pointer (just a 40-character SHA file in `.git/refs/heads/`) pointing to the latest commit on that branch. It moves forward automatically with every new commit.
- **HEAD:** A special pointer that tells Git which branch you are currently on. Most of the time HEAD → branch → commit. When you checkout a specific commit hash, HEAD detaches from any branch (Detached HEAD state).
- **Tags:** Like branches but immutable. They point to a commit and never move.

---

## The .git Directory
Run `ls -la .git` inside any repository:
```
HEAD          ← Which branch/commit is currently checked out
config        ← Repo-specific config (remotes, author, etc.)
objects/      ← The object database (all blobs, trees, commits)
refs/         ← Branches (heads/) and Tags (tags/) as pointer files
index         ← The staging area
logs/         ← History of where HEAD and branches have pointed
```
Everything Git knows about your project lives in this directory. Copy it anywhere and you have the full history.
