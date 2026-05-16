# Lab 04: Working with Remote Repositories

## Objective
Connect a local repository to GitHub, push code, pull changes, and understand the full remote workflow.

## Prerequisites
- A GitHub account.
- Git installed and configured (`user.name`, `user.email`).
- SSH key set up OR Personal Access Token (PAT) for HTTPS.

## Setting Up SSH (Recommended — One-time)
```bash
ssh-keygen -t ed25519 -C "your@email.com"   # Press Enter for defaults
cat ~/.ssh/id_ed25519.pub                     # Copy the output
# Paste this public key into GitHub → Settings → SSH and GPG Keys → New SSH Key
ssh -T git@github.com                         # Test connection — "Hi username!"
```

## Steps

### 1. Create a Repo on GitHub
- Go to github.com → New repository.
- Name: `remote-demo`. Public. **Do NOT** initialize with README (we have local files).
- Copy the SSH URL: `git@github.com:username/remote-demo.git`.

### 2. Connect Local Repo to GitHub
```bash
mkdir remote-demo && cd remote-demo
git init
echo "# Remote Demo" > README.md
git add . && git commit -m "Initial commit"
git remote add origin git@github.com:username/remote-demo.git
git remote -v    # Confirm origin is set correctly
```

### 3. Push to GitHub
```bash
git branch -M main                    # Rename branch to main if needed
git push -u origin main               # Push + set upstream tracking
# Visit github.com/username/remote-demo — your file is there!
```

### 4. Simulate a Change "on GitHub" (Another Contributor)
- On GitHub, click `README.md` → Edit (pencil icon) → Add a line → Commit directly to main.

### 5. Pull the Change Locally
```bash
git log --oneline    # Your local doesn't have that commit yet
git fetch origin     # Download without merging — see what changed
git log origin/main --oneline   # Shows the new commit on remote
git pull             # Fetch + merge. Your local is now up-to-date.
git log --oneline    # New commit is now in your local history
```

### 6. Push a New Branch
```bash
git checkout -b feature/about-page
echo "About page content" > about.md
git add . && git commit -m "feat: add about page"
git push -u origin feature/about-page
# The branch now exists on GitHub too!
```

### 7. Delete the Remote Branch
```bash
git push origin --delete feature/about-page
git branch -d feature/about-page         # Delete local branch too
```

## Key Takeaways
- `git remote add origin <url>` links local to remote. Done once per repo.
- `git fetch` is safer than `git pull` — inspect before merging.
- `-u` in `git push -u` sets tracking so future `git push`/`git pull` know which remote branch to use.
