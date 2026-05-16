# Lab 03: Resolving Merge Conflicts

## Objective
Simulate a merge conflict (where two branches edit the same line) and manually resolve it.

## Steps

### 1. Setup
```bash
mkdir conflict-demo && cd conflict-demo
git init
echo -e "line1\nline2\nline3" > file.txt
git add . && git commit -m "Initial: file.txt with 3 lines"
```

### 2. Create Two Branches that Edit the Same Line
```bash
# Branch A: changes line 2
git checkout -b branch-a
sed -i 's/line2/line2 - FROM BRANCH A/' file.txt    # Linux/WSL
# On Windows PowerShell: (Get-Content file.txt) -replace 'line2','line2 - FROM BRANCH A' | Set-Content file.txt
git add . && git commit -m "Branch A: updated line 2"

# Branch B: also changes line 2
git checkout main
git checkout -b branch-b
sed -i 's/line2/line2 - FROM BRANCH B/' file.txt
git add . && git commit -m "Branch B: updated line 2"
```

### 3. Attempt the Merge
```bash
git checkout main
git merge branch-a        # Fast-forward merge — succeeds
git merge branch-b        # CONFLICT! Both changed line 2
# Output: CONFLICT (content): Merge conflict in file.txt
```

### 4. Inspect the Conflict
```bash
git status                # Shows file.txt as "both modified"
cat file.txt
```
You'll see:
```
line1
<<<<<<< HEAD
line2 - FROM BRANCH A
=======
line2 - FROM BRANCH B
>>>>>>> branch-b
line3
```
Git shows both versions. `HEAD` is your current branch (main, which has branch-a's change). Below `=======` is the incoming change from branch-b.

### 5. Resolve Manually
Open `file.txt` in your editor and decide what the final content should be. For example, keep both:
```
line1
line2 - FROM BRANCH A and BRANCH B
line3
```
Remove ALL conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`).

### 6. Stage and Complete the Merge
```bash
git add file.txt
git status    # "All conflicts fixed but you are still merging"
git commit -m "resolve: merge branch-b, combined line-2 edits"
git log --oneline --graph
```

## Key Takeaways
- Conflicts occur when two branches edit the **same lines** of the same file.
- Git marks conflicts in the file with `<<<<<<<`, `=======`, `>>>>>>>`.
- You must manually edit the file, remove markers, `git add`, then `git commit`.
