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

