# 3.1 [Branches in a Nutshell](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell)

Branching is often described as Git’s **“killer feature”** because it is **incredibly lightweight**, making branching operations and switching between branches nearly instantaneous. While many other version control systems require copying all project files into a new directory—an expensive process for large projects—Git encourages a workflow that involves branching and merging many times a day.

**The Nature of Git Commits**

To understand branching, you must first understand how Git stores data. Git does not store data as a series of changes or deltas, but as a **series of snapshots**.

• **Objects**: When you commit, Git stores a **commit object** that contains a pointer to the snapshot of your staged content (a **tree object**), metadata (author, message), and pointers to the commit’s **parents**.

• **Initial vs. Normal Commits**: An initial commit has zero parents, a normal commit has one, and a merge commit has multiple parents.

• **Example**: If you stage three files (`README`, `test.rb`, `LICENSE`) and commit, Git creates **five objects**: three **blobs** (file contents), one **tree** (listing names and blobs), and one **commit** (pointing to that tree and metadata).

**What is a Branch?**

A branch in Git is simply a **lightweight, movable pointer** to one of these commit objects.

• **Default Branch**: The default branch name is **master** (though many now use `main`); it is not a special branch, but simply the name Git gives to the initial branch by default when you run `git init`.

• **Movement**: As you make new commits, the branch pointer you are currently on **automatically moves forward** to the latest commit.

**Listing, Creating and Switching Branches**

• **Listing**: List your local and remote branches by `git branch` and `git branch -r` respectively.

```console
omkar@black-box:~/study$ git branch
  learning-checkout-via-new-branch
* main
omkar@black-box:~/study$ git branch -r
  origin/learning-checkout-via-new-branch
  origin/main
```

• **Creating**: The **git branch** command creates a new pointer at the same commit you are currently on. `git branch testing` creates a new pointer named "testing" (only locally)

```console
omkar@black-box:~/study$ git branch test
omkar@black-box:~/study$ git branch
  learning-checkout-via-new-branch
  learning-to-create-new-branch
* main
  test
omkar@black-box:~/study$ git branch -r
  origin/learning-checkout-via-new-branch
  origin/learning-to-create-new-branch
  origin/main
```

Create a new branch using `git switch -c`

```console
git switch -c learning-to-create-new-branch
```

• **The HEAD Pointer**: Git knows which branch you are currently on by using a special pointer called **HEAD**, which points to your local branch.

• **Switching**: To move to an existing branch, use `git checkout` (or `git switch` in newer versions). `git checkout testing` moves `HEAD` to point to the "testing" branch.
    
```console
omkar@black-box:~/study$ git switch main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
```

• **Pushing**: To push to new branch, you'll have to push the local branch to remote first

```console
omkar@black-box:~/study$ git switch test
Switched to branch 'test'

omkar@black-box:~/study$ touch test

omkar@black-box:~/study$ git add .

omkar@black-box:~/study$ git commit -m "Added a test file to a new test branch"
[test 23dd69b] Added a test file to a new test branch
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 test
 
omkar@black-box:~/study$ git push
fatal: The current branch test has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin test

To have this happen automatically for branches without a tracking
upstream, see 'push.autoSetupRemote' in 'git help config'.

omkar@black-box:~/study$ git push origin -u test
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 12 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 291 bytes | 291.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
remote: 
remote: Create a pull request for 'test' on GitHub by visiting:
remote:      https://github.com/omkardamame/learning-git/pull/new/test
remote: 
To https://github.com/omkardamame/learning-git.git
 * [new branch]      test -> test
branch 'test' set up to track 'origin/test'.
```

• **Deleting**: To delete a branch, use `git branch -d <branch>` but this only deletes local branch. 

```console
omkar@black-box:~/study$ git branch -d list
Deleted branch list (was a26ebf9).
omkar@black-box:~/study$ git branch
  learning-checkout-via-new-branch
* main
```

To delete remotely, use

```console
omkar@black-box:~/study$ git branch -r
  origin/learning-checkout-via-new-branch
  origin/learning-to-create-new-branch
  origin/main
  origin/test
omkar@black-box:~/study$ git branch
  learning-checkout-via-new-branch
  learning-to-create-new-branch
* main
omkar@black-box:~/study$ 
omkar@black-box:~/study$ git push origin --delete test
To https://github.com/omkardamame/learning-git.git
 - [deleted]         test
```

• **Working Directory Changes**: Switching branches **reverts the files in your working directory** to look like they did the last time you committed on that branch.

**Divergent History**

If you make a commit on a new branch and then switch back to your original branch to make different changes, your history **diverges**. Because a branch is just a tiny file containing the 40-character SHA-1 checksum of its commit, creating and destroying them is nearly free.

• **Viewing History**: To see where your branch pointers are and how your history has diverged, you can use specific log flags.

    ◦ **Example**: `git log --oneline --decorate --graph --all` provides a visual representation of these pointers and the divergent paths.

```console
omkar@black-box:~/study$ git log --oneline --decorate --graph --all
* 6a06f23 (HEAD -> main, origin/main) Learned how to delete, check, push remote branch
* 536fd36 Learned how to use git branch, switch and create.
* ca17669 Did PR merge for the first time😎!
*   b98e14e Merge branch 'learning-checkout-via-new-branch'
|\  
| * 47791bd (origin/learning-checkout-via-new-branch, learning-checkout-via-new-branch) Learning how to checkout after creating a new branch for it.
* | acf1341 Learning how to checkout after creating a new branch for it.
* |   343fced Merge pull request #2 from omkardamame/learning-to-create-new-branch
|\ \  
| * | 5600bbe (tag: random-tag, origin/learning-to-create-new-branch, learning-to-create-new-branch) Created another random file
| * | fe584c6 Created a random file
| * | 7a792f0 Checking if everything is good with new branch...
| * | 3112505 Created a new branch: learning-to-create-new-branch
|/ /  
* | a26ebf9 LOL! Forgot to add summary for Chapter 2 😅
* | 89dc206 Completed Chapter 2 and now I know about git alias 😬
* | 8b6196d Forgot to add updated notes of git tag 😅
* | 567d25f Now I can do git tag properly.
* | 498df35 (tag: testing-tags) Updated README.md and updated Chapter 2 summary.
* | 59c4a26 Rewrote correctly the README.md for accurate progress marking
* | 3777eb2 Added git revert in README.md
* | 91b745f Remove file test1 after testing git restore and checkout
* | b93288b Adding file test1 for testing git restore and checkout
* | 0df1f63 Minor change to README.md file
* | 60809f5 Updated README.md and completed till Chapter 2.3
* | 7b1e2dd Add MIT License to the project
* | 8c40729 Add Contributor Covenant Code of Conduct
* | 967675e Updated README.md to latest progress
* | f6fbd6d Completed File lifecycle from Chapter 2
* | d071d42 Added summary of Chapter 2: Getting a Git Repository
* | dfbeaec Made a minor change in Chapter 1
* | 96936be Added summary for Chapter 1: Getting Started
* | aa8ff53 Added emoji instead of generic tick-box for completion
* | 9c8fde7 Started Chapter 2
* | 5120a02 Completed Chapter 1
* | ea57ef8 chore: Added Pro Git Book link to CONTRIBUTING.md
* | ee2c859 Changed content of CONTRIBUTING.md
* | 92a72ab Changed few things
|/  
* 02ccf8d (tag: v1.0) Initial commit for studying
```

• **Shorthand Creation**: You can create and switch to a branch in a single step using the **-b** flag.

    ◦ **Example**: `$ git checkout -b featureA`.

*Note*
*From Git version 2.23 onwards you can use git switch instead of git checkout to:*
*Switch to an existing branch: `git switch testing-branch`.*
*Create a new branch and switch to it: `git switch -c new-branch`. The `-c` flag stands for create, you can also use the full flag: `--create`
*Return to your previously checked out branch: `git switch -`

# 3.2 [Basic Branching and Merging](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)

Basic branching and merging in Git follows a cycle of diverging from a main line of development to work on specific tasks and then integrating that work back once it is stable and then moving on with your previous/new work.

**The Basic Branching Workflow**

To understand this in a real-world context, imagine you are working on a website and receive a request to fix a specific issue (#53).

1. **Create a New Branch**: You create a branch and switch to it in one step using **git checkout -b iss53**. This is shorthand for running `git branch iss53` followed by `git checkout iss53`.

2. **Work and Commit**: As you work, the `iss53` branch pointer moves forward with each commit because your **HEAD** (the pointer to your current location) is pointing to it.

3. **The Interruption**: You receive a call for a critical production hotfix. Before you switch back to your production branch, ensure your working directory is clean, as uncommitted changes that conflict with the target branch will prevent you from switching.

4. **Create a Hotfix**: You switch to `master` or `main` (whatever your production branch is) via `git checkout master`. Your working directory is automatically reset to look exactly like it did at the last commit on `master`. You then create a new branch for the fix: `git checkout -b hotfix`.

**Merging and Fast-Forwards**

Once the hotfix is tested and ready, you must merge it back into your production branch to deploy it.

• **Fast-Forward Merge**: You switch to the branch you want to merge _into_ (`master`) and run **git merge hotfix**. Because the `hotfix` branch points to a commit that is a direct descendant of the commit you are currently on, Git performs a **"fast-forward"**. This means Git simply moves the branch pointer forward because there is no divergent work to reconcile.

• **Cleanup**: After the fix is deployed, you no longer need the temporary branch and can delete it using **git branch -d hotfix**.

**Three-Way Merges**

After finishing the hotfix, you return to your original work on `iss53`. When that feature is finally complete, you merge it into `master` by running `git merge master`, or you can wait to integrate those changes until you decide to pull the `iss53` branch back into `master` later.

Unlike the hotfix merge, your development history has now diverged. Because the current commit on `master` is not a direct ancestor of `iss53`, Git performs a **three-way merge**. It uses:

1. The snapshot at the tip of the first branch (`master`).

![[basic-merging-1.png]]

2. The snapshot at the tip of the second branch (`iss53`).

3. The **common ancestor** of the two branches.

![[basic-merging-2.png]]

Instead of just moving a pointer, Git creates a new snapshot and a special **merge commit** that has more than one parent.

**Basic Merge Conflicts**

If you changed the exact same part of the same file in both branches, Git cannot merge them automatically and will report a **merge conflict**.

• **Checking Status**: Running **git status** will show you which files are "unmerged".

```console
omkar@black-box:~/study$ git merge learning-to-resolve-merge-issues 
Auto-merging merge-issue.txt
CONFLICT (content): Merge conflict in merge-issue.txt
Automatic merge failed; fix conflicts and then commit the result.

omkar@black-box:~/study$ git status
On branch main
Your branch is up to date with 'origin/main'.

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
	both modified:   merge-issue.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

• **Conflict Markers**: Git adds standard markers to the affected files. The content between `<<<<<<< HEAD` and `=======` is the version from your current branch, while the content between `=======` and `>>>>>>>` is from the branch being merged in. For example,

```console
omkar@black-box:~/study$ git merge learning-to-resolve-merge-issues 
error: Merging is not possible because you have unmerged files.
hint: Fix them up in the work tree, and then use 'git add/rm <file>'
hint: as appropriate to mark resolution and make a commit.
fatal: Exiting because of an unresolved conflict.

omkar@black-box:~/study$ cat merge-issue.txt 
This is some random text here which is gonna be the same across branches.

<<<<<<< HEAD
This is the text I am gonna change and made some changes from main.
=======
This is the text I am gonna change and I am in new branch now.
>>>>>>> learning-to-resolve-merge-issues
```

• **Resolution**: You must manually edit the file to resolve the differences. Once fixed, run **git add** on the file to mark it as resolved.

• **Finalizing**: After all conflicts are staged, run **git commit** to finalize the merge commit and do `git push`. If you find the manual process difficult, you can use **git mergetool** to launch a graphical resolution helper (You'll need to configure it).

```console
omkar@black-box:~/study$ git log --oneline --graph --all
*   b12cf96 (HEAD -> main, origin/main) Resolved merge issues.
|\  
| * 7d72ea5 (origin/learning-to-resolve-merge-issues, learning-to-resolve-merge-issues) Made minor change in merge-issue.txt file
* | 2362549 Made minor changes to produce merge issues.
|/  
* e2ab314 Prepared a file for merge-issue testing.
* d5179fb Started and added Chapter 3 summary
```

## `git merge --no-ff`

### What it does:

- Forces Git to create a **merge commit**, even if a fast-forward is possible.
- Preserves the **branch structure** in the commit history.
- Useful for keeping track of feature branches and their commits.

### Syntax:

```bash
git merge --no-ff <branch>
```

### Benefits:

- **Clear history:** Shows explicitly which commits came from a feature branch.
- **Easy reverts:** Reverting a feature branch can be done by reverting the merge commit.
- **Collaboration-friendly:** Makes branch development visible in logs and graphs.
- **Auditability:** Helpful for enterprise and long-term projects.

### Behavior with conflicts:

- Conflicts are handled **the same way as a normal merge**:
    - Git stops and marks conflicts in files.
    - You resolve conflicts manually.
    - You stage (`git add`) and commit the merge.
- Even if a fast-forward was possible, a merge commit is still created.

### Example:

```console
# Branch structure before merge
main:    A --- B
feature:     C --- D

# Merge with --no-ff
git checkout main
git merge --no-ff feature

# Branch structure after merge
main:    A --- B --- M
              \   /
feature:       C --- D
```

Here `M` is the merge commit created by `--no-ff`

Here is how you can find `--no-ff` is used

```console
* a0c6e91 (HEAD -> main) ...
|\  
| * 98055bd ... (Branch A)
* | 583ed4d ... (Branch B)
|/
```

# 3.3 [Branch Management](https://git-scm.com/book/en/v2/Git-Branching-Branch-Management)

The **git branch** **command** is the primary tool for managing branches, allowing you to list, create, delete, and rename them.

**Listing and Identifying Branches**

Running the command with no arguments provides a simple list of your current local branches.

```console
omkar@black-box:~/study$ git branch
  learning-checkout-via-new-branch
  learning-to-create-new-branch
  learning-to-resolve-merge-issues
* main
```

• **The Current Branch**: The branch prefixed with an `asterisk (*)` is the one you currently have checked out, meaning **HEAD** points to it. Any new commits you make will advance this specific branch pointer.

• **Detailed View**: To see the **last commit** made on each branch, you can run `git branch -v`.

```console
omkar@black-box:~/study$ git branch -v
  learning-checkout-via-new-branch 47791bd Learning how to checkout after creating a new branch for it.
  learning-to-create-new-branch    5600bbe Created another random file
  learning-to-resolve-merge-issues 98055bd Added a random text to introduce merge issue.
* main                             4d049c9 Updated README.md and learned how to resolve conflicts.
```

**Filtering by Merge Status**

You can filter the branch list to see which branches have or have not yet been integrated into your current work.

• **Merged Branches**: `git branch --merged` lists branches that have already been merged into the branch you are currently on. These are generally **safe to delete** using **git branch -d** because you have already incorporated their work elsewhere.

```console
omkar@black-box:~/study$ git branch --merged
  learning-checkout-via-new-branch
  learning-to-create-new-branch
  learning-to-resolve-merge-issues
* main
```

• **Unmerged Branches**: `git branch --no-merged` lists branches containing work that is not yet in your current branch.

```console
omkar@black-box:~/study$ git branch --no-merged
  test-branch
```

• **Safe Deletion**: If you attempt to delete an unmerged branch (made commit but didn't push) with `-d`, Git will block the operation with an error to prevent the **permanent loss of work**. To delete it anyway, you must use the force flag: `git branch -D <branchname>`.

```console
omkar@black-box:~/study$ git switch main
Already on 'main'
Your branch is up to date with 'origin/main'.

omkar@black-box:~/study$ git checkout -b unmerged-branch
Switched to a new branch 'unmerged-branch'

omkar@black-box:~/study$ echo "test" > unmerged.txt
omkar@black-box:~/study$ git add unmerged.txt 
omkar@black-box:~/study$ git commit -m "Unmerged commit"
[unmerged-branch a7b3832] Unmerged commit
 1 file changed, 1 insertion(+)
 create mode 100644 unmerged.txt
omkar@black-box:~/study$ git switch main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.

omkar@black-box:~/study$ git branch -d unmerged-branch 
error: the branch 'unmerged-branch' is not fully merged.
If you are sure you want to delete it, run 'git branch -D unmerged-branch'
omkar@black-box:~/study$ git branch -D unmerged-branch
Deleted branch unmerged-branch (was a7b3832).
```

• **Advanced Filtering**: You can check the merge status of branches relative to a branch other than your current one by adding that branch name as an argument.

    ◦ Example: To see what is not merged into `main` while you are on a different branch: `git branch --no-merged main`.

**Changing Branch Names**

You can rename a branch locally using the **--move** flag.

• **Example**: To rename `bad-branch-name` to `corrected-branch-name`: `git branch --move bad-branch-name corrected-branch-name`.

```console
omkar@black-box:~/study$ git branch
  learning-checkout-via-new-branch
  learning-to-create-new-branch
  learning-to-resolve-merge-issues
* main
  test
omkar@black-box:~/study$ git branch --move test test2
omkar@black-box:~/study$ git branch
  learning-checkout-via-new-branch
  learning-to-create-new-branch
  learning-to-resolve-merge-issues
* main
  test2
```

• **Remote Synchronization**: This change is only local; to update the server, you must push the new name (`git push --set-upstream origin corrected-branch-name`) and then delete the old name from the remote (`git push origin --delete bad-branch-name`).

**Renaming the Default Branch (Master to Main)**

Changing the name of a primary branch like `master` or `main` is a significant operation that can **break integrations, release scripts, and CI/CD pipelines**.

1. **Rename locally**: `git branch --move master main`.

2. **Push to remote**: `git push --set-upstream origin main`.

3. **Cleanup**: You must then update all project dependencies, test configurations, and documentation to reflect the new name before finally deleting the old `master` branch on the remote.

# 3.4 [Branching Workflows](https://git-scm.com/book/en/v2/Git-Branching-Branching-Workflows)

The lightweight nature of Git branching allows for several different types of **branching workflows** that can be incorporated into a development cycle. These workflows generally fall into two categories: **long-running branches** and **short-lived topic branches**.

**Long-Running Branches**

Because Git uses a simple three-way merge, it is easy to merge from one branch to another repeatedly over long periods. Many Git developers use a **progressive-stability** workflow:

• **The Master/Main Branch:** Only code that is **entirely stable**—often only code that has been or will be released—is kept in the `master` (or `main`) branch.

• **Parallel Branches:** A parallel branch, often named `develop` or `next`, is used for active work or stability testing. It is not always stable but is merged into `master` once it reaches a stable state.

• **Silos of Stability:** Larger projects may have even more levels, such as a `proposed` or `pu` (proposed updates) branch for integrated work that isn't quite ready for the `next` branch. Commits graduate to a **more stable silo** only when they are fully tested.

**Topic Branches**

A **topic branch** is a **short-lived branch** created for a single particular feature or piece of related work. In Git, it is common to create, work on, merge, and delete these branches multiple times a day.

• **Context Switching:** This technique allows you to **switch contexts quickly** and completely, as each branch acts as a silo where all changes are related to a specific topic.

• **Isolation:** You can keep these changes in their silo for minutes or months, merging them only when they are ready, regardless of the order in which they were created.

• **Local Nature:** It is important to remember that these branching and merging operations are **completely local**; no communication with a server is required.

**Examples from the Sources**

• **Stability Levels:** In a linear view, a `master` branch might point to commit `C1`, while `develop` points to `C5` and a `topic` branch points to `C7`, representing **increasing levels of bleeding-edge code**.

• **Iterative Features:** You might branch `iss91` from `master` to work on a feature. If you want to try a different approach, you can branch `iss91v2` off of `iss91`. Meanwhile, you can switch back to `master` and branch `dumbidea` to experiment with a risky fix.

• **Final Integration:** After review, you might decide the second solution (`iss91v2`) and the `dumbidea` were best. You would then **merge those two into** **master** and throw away the original `iss91` branch.

# 3.5 [Remote Branches](https://git-scm.com/book/en/v2/Git-Branching-Remote-Branches)

**Remote references** are pointers (references) in your remote repositories, including branches, tags, and more. The most common way to interact with these is through **remote-tracking branches**, which act as local references to the state of remote branches. These branches are non-movable bookmarks that Git moves for you whenever you perform network communication to ensure they accurately represent the state of the remote repository.

You can get a full list of remote references explicitly with `git ls-remote <remote_git_url>`, or `git remote show <remote_git_url>`, `git remote show` for remote branches as well as more information.

**Remote-Tracking Branch Naming and Defaults**

Remote-tracking branches take the form `<remote>/<branch>`. For example, to see what the `master` branch on your `origin` remote looked like the last time you connected, you would check the `origin/master` branch.

The name **origin** is not special; it is simply the default name Git gives to the server when you run the `git clone` command. Just as `master` is a default branch name that can be changed, you can use the `-o` flag during a clone (e.g., `git clone -o blayt`) to name your default remote branch `blayt/master` instead.

**Synchronizing with Remotes**

• **Fetching**: To synchronize your work, run `git fetch <remote>` (e.g., `origin`). This command fetches any data from that server that you don't yet have and moves your local `origin/master` pointer to its new, more up-to-date position.

```console
omkar@black-box:~/study$ git fetch blayt
From https://github.com/omkardamame/learning-git
 * [new branch]      learning-checkout-via-new-branch -> blayt/learning-checkout-via-new-branch
 * [new branch]      learning-to-create-new-branch    -> blayt/learning-to-create-new-branch
 * [new branch]      learning-to-resolve-merge-issues -> blayt/learning-to-resolve-merge-issues
 * [new branch]      main                             -> blayt/main
```

• **Adding New Remotes**: You can add additional internal or team-based servers using `git remote add <shortname> <url>`. For instance, adding a remote named `teamone` allows you to run `git fetch teamone` to see their work represented as `teamone/master`.

```console
omkar@black-box:~/study$ git remote add blayt https://github.com/omkardamame/learning-git.git

omkar@black-box:~/study$ git fetch

omkar@black-box:~/study$ git remote -v
blayt	https://github.com/omkardamame/learning-git.git (fetch)
blayt	https://github.com/omkardamame/learning-git.git (push)
origin	https://github.com/omkardamame/learning-git.git (fetch)
origin	https://github.com/omkardamame/learning-git.git (push)

omkar@black-box:~/study$ git branch -r
  origin/learning-checkout-via-new-branch
  origin/learning-to-create-new-branch
  origin/learning-to-resolve-merge-issues
  origin/main

omkar@black-box:~/study$ git fetch blayt
From https://github.com/omkardamame/learning-git
 * [new branch]      learning-checkout-via-new-branch -> blayt/learning-checkout-via-new-branch
 * [new branch]      learning-to-create-new-branch    -> blayt/learning-to-create-new-branch
 * [new branch]      learning-to-resolve-merge-issues -> blayt/learning-to-resolve-merge-issues
 * [new branch]      main                             -> blayt/main
omkar@black-box:~/study$ 
omkar@black-box:~/study$ git branch -r
  blayt/learning-checkout-via-new-branch
  blayt/learning-to-create-new-branch
  blayt/learning-to-resolve-merge-issues
  blayt/main
  origin/learning-checkout-via-new-branch
  origin/learning-to-create-new-branch
  origin/learning-to-resolve-merge-issues
  origin/main
```

• **Pushing**: To share a local branch named `serverfix`, you must explicitly push it using `git push origin serverfix`.

• **Rename on Push**: You can push a local branch to a remote branch with a different name using the format `git push origin <local_branch>:<remote_branch>` (e.g., `$ git push origin serverfix:awesomebranch`). You'll have to resolve merge conflicts if there are any. Also it doesn't matter if branch is not created already.

```console
omkar@black-box:~/study$ git push origin test:test-new
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/omkardamame/learning-git.git
   09f2aa5..486e065  test -> test-new
```

**Tracking Branches**

Checking out a local branch from a remote-tracking branch automatically creates a **tracking branch** (also called an **upstream branch**). These local branches have a direct relationship with a remote branch; if you run `git pull` while on one, Git automatically knows which server to fetch from and which branch to merge in.

**Common Examples for Creating Tracking Branches:**

• **Standard Method**: `git checkout -b serverfix origin/serverfix` | This creates and adds branch locally

• **Shorthand Flag**: `git checkout --track origin/serverfix` | This just adds the branch locally.

• **Automatic Shortcut**: If the branch you are trying to check out doesn't exist locally but matches a name on exactly one remote, Git creates a tracking branch for you: `git checkout serverfix` | Adds branch locally.

• **Setting Upstream Later**: To change the upstream branch for an existing local branch, use `git branch -u origin/serverfix`.

**Inspecting and Deleting Remote Branches**

• **Detailed Status**: Use `git branch -vv` to see which local branches are tracking remote branches and whether you are "ahead," "behind," or both. Note that this command uses cached data; to get up-to-date numbers, you should run `git fetch --all`  `git branch -vv`.

• **Deletion**: If you are finished with a remote branch, you can delete it from the server using the **--delete** option: `git push origin --delete serverfix`. This operation simply removes the pointer from the server; the underlying data is generally kept until a garbage collection runs.

# 3.6 [Rebasing](https://git-scm.com/book/en/v2/Git-Branching-Rebasing)

In Git, there are two primary ways to integrate changes from one branch into another: merging and **rebasing**. While merging performs a three-way merge between two latest branch snapshots and their common ancestor, rebasing takes the **patch of the change** introduced in one branch and **reapplies it on top** of another.

**The Basic Rebase Operation**

Rebasing works by following these internal steps:

1. Identifying the **common ancestor** of the two branches.

2. Getting the **diff** introduced by each commit of the branch you are currently on and saving those diffs to **temporary files**.

3. **Resetting** the current branch to the same commit as the branch you are rebasing onto.

4. Applying each of the saved changes in turn.

![[rebasing-1.png]](rebasing-1.png)

The primary benefit of this process is a **cleaner history**. If you examine the log of a rebased branch, it appears as a **linear history**, looking as though all work happened in a straight line even if it was originally developed in parallel. This is often used to ensure your commits apply cleanly to a remote branch before submitting them to a project you do not maintain, allowing the maintainer to perform a simple **fast-forward merge**.

• **Standard Rebase:** To rebase an "experiment" branch onto "master" | `git switch experiment && git rebase main`

• **Fast-Forwarding After Rebase:** Once the rebase is complete, you must move the master pointer forward:

```
- Ultra-short memory trick:
  - The branch **you are on** gets rewritten | `git checkout feature && git rebase main`
  - Do not rewrite branches that other people depend on
```

**Advanced Rebasing Options**

Git allows for more complex "transitive" rebases using the **--onto** flag. This is useful when you have a topic branch that was based on another topic branch and you want to move it to a third branch.

• **Rebasing onto a Different Base:** Imagine you have a `client` branch based on a `server` branch. To take the `client` changes and replay them directly onto `master` (skipping the `server` changes):

- Example: `git rebase --onto master server client`.

• **Direct Rebasing:** You can rebase a topic branch onto a base branch **without checking it out first** by specifying both in the command.

- Example: `git rebase master server` (This checks out `server` for you and replays its work onto `master`).

• **Interactive Rebasing:** `git rebase -i` means **interactive rebase**. It lets you _edit, reorder, squash, or drop commits_ instead of rebasing automatically. It is one of Git’s most powerful tools for cleaning up commit history before sharing it.

What interactive rebase is used for

Typical reasons to use:

- combine several small commits into one (squash)    
- rewrite or fix commit messages
- delete unwanted commits
- reorder commits
- split a commit into multiple commits

It **rewrites history**, so you normally use it **on local branches** that nobody else is using yet.

Basic usage:

You normally specify how far back you want to edit:

```console
git rebase -i HEAD~3
```

This means: open the last 3 commits for editing.

You can also rebase to a branch:

```console
git rebase -i main
```

This means: rewrite your branch commits on top of `main`, interactively.

Git opens your default editor showing something like:

```console
pick a1b2c3 First commit
pick d4e5f6 Second commit
pick 123abc Third commit

# Commands:
#  pick  = use commit
#  reword = use commit, but edit the commit message
#  edit  = pause to amend the commit
#  squash = combine with previous commit
#  fixup  = like squash, but discard commit message
#  drop   = delete commit
```


**The Golden Rule: The Perils of Rebasing**

Despite its advantages, rebasing has one major drawback: **Do not rebase commits that exist outside your repository and that people may have based work on**.

When you rebase, you are **abandoning existing commits** and creating new ones that are similar but technically different (new SHA-1 hashes). If you push these commits and a collaborator bases work on them, and you then rewrite them via rebase and push again, the collaborator will be forced to re-merge their work. This leads to a "pickle" where the history contains confusing duplicate commits.

If you find yourself in a situation where a partner has force-pushed a rebase over your work, you can mitigate the pain by running **git pull --rebase** instead of a normal pull.

**Rebase vs. Merge**

The choice between the two often comes down to philosophy:

• **Merging** views history as a **historical record** of what actually happened, preserving even the messy merge commits for posterity.

• **Rebasing** views history as the **story** of how the project was made, allowing you to edit and clean up your "first draft" to provide a coherent narrative for future readers.

# 3.7 [Summary](https://git-scm.com/book/en/v2/Git-Branching-Summary)

We’ve covered basic branching and merging in Git. You should feel comfortable creating and switching to new branches, switching between branches and merging local branches together. You should also be able to share your branches by pushing them to a shared server, working with others on shared branches and rebasing your branches before they are shared. Next, we’ll cover what you’ll need to run your own Git repository-hosting server.