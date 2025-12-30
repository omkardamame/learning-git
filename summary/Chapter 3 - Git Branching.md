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

![[basic-merging-1.png]](basic-merging-1.png)

2. The snapshot at the tip of the second branch (`iss53`).

3. The **common ancestor** of the two branches.

![[basic-merging-2.png]]((basic-merging-2.png))

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
