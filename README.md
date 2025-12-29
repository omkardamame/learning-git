---

## ✅ Learning Checklist (Pro Git – 2nd Edition)

This checklist tracks my progress through **Pro Git (2nd Edition)**.  
Each topic includes **explicit Git commands** to aid future revision.

---

### 📖 Chapter 1 – Getting Started

- ✅ What version control is and why it matters
- ✅ Centralized vs Distributed VCS
- ✅ Short history of Git
- ✅ What is Git (snapshots, not diffs)
- ✅ Command-line basics
- ✅ Installing Git
- ✅ First-time Git setup  
  (`git config --global user.name`, `git config --global user.email`)
- ✅ Getting help  
  (`git help`, `git <command> --help`)

🎯 **Outcome:** Understand what Git is and how to set it up correctly.

---

### 📖 Chapter 2 – Git Basics (CRITICAL)

- ✅ Getting a repository  
  (`git init`, `git clone`)
- ✅ Recording changes  
  (`git status`, `git add`, `git commit`)
- ✅ Viewing history  
  (`git log`, `git log --oneline --graph`)
- ✅ Undoing things  
  - ✅ Restore files: `git restore`
  - ✅ Unstage files: `git restore --staged`
  - ✅ Reset commits: `git reset`
  - ✅ Revert commits: `git revert`
- ✅ Working with remotes  
  (`git remote`, `git fetch`, `git pull`, `git push`)
- ✅ Tagging  
  (`git tag`, `git tag -a`, `git tag -d`, `git psuh origin --delete`)
- ✅ Git aliases  
  (`git config --global alias.*`)

🎯 **Outcome:** Confident daily Git usage without fear.

---

### 📖 Chapter 3 – Branching (MOST IMPORTANT)

- ✅ What branches really are  
  (`git branch`, `git show-branch`)
- ✅ Creating and switching branches  
  (`git branch`, `git switch`, `git checkout`)
- ✅ Merging branches  
  (`git merge`)
- ✅ Fast-forward vs three-way merge  
  (`git merge`, `git merge --no-ff`)
- ✅ Resolving merge conflicts  
  (`git status`, manual resolve, `git commit`)
- ✅ Branch management  
  (`git branch -d`, `git branch -D`)
- ✅ Remote branches  
  (`git branch -r`, `git push -u`)
- ✅ Detailed Branch management and renaming
  (`git branch --no-merged`, `git branch --merged`, `git branch --no-merged main`, `git branch --move`, `git push --set-upstream`)
- [ ] Rebasing  
  (`git rebase`, `git rebase -i`)
- [ ] Understanding `HEAD`  
  (`git symbolic-ref HEAD`)

🎯 **Outcome:** Full control over branching and merging.

---

### 📖 Chapter 4 – Git on the Server

- [ ] Git transport protocols  
  (SSH, HTTP, HTTPS)
- [ ] Setting up Git on a server  
  (`git init --bare`)
- [ ] SSH authentication  
  (`ssh-keygen`, `authorized_keys`)
- [ ] Git Daemon
- [ ] Smart HTTP
- [ ] GitWeb
- [ ] GitLab basics
- [ ] Hosted Git services (GitHub, GitLab, etc.)

🎯 **Outcome:** Understand how Git repositories are hosted and accessed.

---

### 📖 Chapter 5 – Distributed Git

- [ ] Centralized vs distributed workflows
- [ ] Feature-branch workflow
- [ ] Contributing to a project  
  (`git fetch`, `git rebase`, `git pull --rebase`)
- [ ] Maintaining a project  
  (`git merge`, `git tag`, release flow)

🎯 **Outcome:** Work effectively in team and open-source environments.

---

### 📖 Chapter 6 – GitHub

- [ ] Account setup and configuration
- [ ] Forking repositories
- [ ] Pull requests
- [ ] Code reviews
- [ ] Issues and discussions
- [ ] Syncing fork with upstream  
  (`git remote add upstream`, `git fetch upstream`)
- [ ] Releases and tags

🎯 **Outcome:** Professional usage of GitHub for collaboration.

---

### 📖 Chapter 7 – Git Tools (ADVANCED & IMPORTANT)

- [ ] Revision selection  
  (`git log`, `git show`, `git diff`)
- [ ] Interactive staging  
  (`git add -p`)
- [ ] Stashing work  
  (`git stash`, `git stash pop`, `git stash list`)
- [ ] Cleaning workspace  
  (`git clean -fd`)
- [ ] Searching history  
  (`git grep`, `git log -S`)
- [ ] Rewriting history  
  (`git commit --amend`, `git rebase -i`)
- [ ] Reset demystified  
  (`git reset --soft|--mixed|--hard`)
- [ ] Advanced merging
- [ ] Rerere  
  (`git rerere`)
- [ ] Debugging with Git  
  (`git bisect`)
- [ ] Submodules  
  (`git submodule`)
- [ ] Bundling  
  (`git bundle`)
- [ ] Credential storage

🎯 **Outcome:** Recover from mistakes and maintain clean history.

---

### 📖 Chapter 8 – Customizing Git

- [ ] Git configuration levels  
  (`--system`, `--global`, `--local`)
- [ ] Git attributes  
  (`.gitattributes`)
- [ ] Git hooks  
  (`.git/hooks`)
- [ ] Enforced policies (pre-commit, pre-push)

🎯 **Outcome:** Git tailored to personal and team workflows.

---

### 📖 Chapter 9 – Git and Other Systems

- [ ] Using Git as a client
- [ ] Migrating from other VCS to Git

🎯 **Outcome:** Understand Git interoperability.

---

### 📖 Chapter 10 – Git Internals (OPTIONAL BUT POWERFUL)

- [ ] Plumbing vs porcelain commands
- [ ] Git objects  
  (`git cat-file`)
- [ ] References and HEAD
- [ ] Packfiles
- [ ] Refspecs
- [ ] Transfer protocols
- [ ] Maintenance and data recovery  
  (`git fsck`, `git reflog`)
- [ ] Environment variables

🎯 **Outcome:** Deep mental model of Git internals.

---

## 🧭 Progress Rule

A topic is complete only if:
- ✔ Read from *Pro Git*
- ✔ Practiced in this repository
- ✔ Can recall the related commands later

---

## 🏁 End Goal

- Confident daily Git usage
- Clean, intentional commit history
- Ability to debug and recover repositories
- Professional Git workflows

---
