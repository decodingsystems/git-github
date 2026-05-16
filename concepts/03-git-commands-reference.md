# Git Commands — Complete Reference by Category

## 1. Setup & Configuration
```bash
git config --global user.name "Your Name"        # Set your identity
git config --global user.email "you@example.com"
git config --global core.editor "code --wait"    # Use VS Code as default editor
git config --list                                 # View all settings
```

## 2. Repository Initialization
```bash
git init                     # Initialize a new local repo in current folder
git clone <url>              # Clone a remote repo locally
git clone <url> my-folder    # Clone into a specific folder name
```

## 3. Staging & Committing
```bash
git status                   # See untracked/modified/staged files
git add <file>               # Stage a specific file
git add .                    # Stage all changes in current directory
git add -p                   # Interactively stage chunks (patch mode)
git commit -m "message"      # Commit staged changes with a message
git commit --amend           # Modify the most recent commit (message or files)
```

## 4. Branching
```bash
git branch                   # List local branches
git branch -a                # List local + remote branches
git branch <name>            # Create a new branch (don't switch)
git checkout <name>          # Switch to a branch
git checkout -b <name>       # Create and switch in one step
git switch <name>            # Modern equivalent of checkout (branches only)
git switch -c <name>         # Modern equivalent of checkout -b
git branch -d <name>         # Delete a branch (safe—merged only)
git branch -D <name>         # Force delete (even if unmerged)
git branch -m <old> <new>    # Rename a branch
```

## 5. Merging & Rebasing
```bash
git merge <branch>           # Merge a branch into the current branch
git merge --no-ff <branch>   # Force a merge commit even for fast-forwards
git rebase <branch>          # Reapply commits on top of another base
git rebase -i HEAD~3         # Interactive rebase: squash/edit last 3 commits
git cherry-pick <sha>        # Apply a single commit from another branch
```

## 6. Remote Operations
```bash
git remote -v                # Show remotes
git remote add origin <url>  # Add a remote named 'origin'
git fetch origin             # Download objects from remote (no merge)
git pull origin main         # Fetch + merge
git push origin main         # Push local commits to remote
git push -u origin <branch>  # Push and set upstream tracking
git push origin --delete <branch>  # Delete a remote branch
```

## 7. Inspection & History
```bash
git log                      # Full commit history
git log --oneline            # Compact one-line view
git log --graph --oneline    # Visual branch graph in terminal
git diff                     # Unstaged changes vs last commit
git diff --staged            # Staged changes vs last commit
git diff main..feature       # Diff between two branches
git show <sha>               # Show details of a specific commit
git blame <file>             # Show who last changed each line
```

## 8. Undoing Changes
```bash
git restore <file>           # Discard unstaged changes in a file
git restore --staged <file>  # Unstage a file (keep changes in working dir)
git revert <sha>             # Create a new commit that undoes a commit
git reset --soft HEAD~1      # Undo last commit, keep changes staged
git reset --mixed HEAD~1     # Undo last commit, keep changes unstaged (default)
git reset --hard HEAD~1      # Undo last commit AND discard all changes (DANGER!)
```

## 9. Stashing
```bash
git stash                    # Save current uncommitted work to a stash
git stash push -m "WIP: fix"  # Stash with a descriptive message
git stash list               # List all stashes
git stash pop                # Apply most recent stash and remove it
git stash apply stash@{2}    # Apply a specific stash (keep it)
git stash drop stash@{0}     # Delete a specific stash
git stash clear              # Delete all stashes
```

## 10. Tags
```bash
git tag                      # List all tags
git tag v1.0                 # Create a lightweight tag
git tag -a v1.0 -m "Release" # Annotated tag with message
git push origin v1.0         # Push a tag to remote
git push origin --tags       # Push all tags
```
