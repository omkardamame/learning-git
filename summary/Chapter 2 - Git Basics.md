# 2.1 [Getting a Git Repository](https://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository)

You typically obtain a Git repository in one of two ways:

1. You can take a local directory that is currently not under version control, and turn it into a Git repository, or
2. You can _clone_ an existing Git repository from elsewhere.

In either case, you end up with a Git repository on your local machine, ready for work.

## Initializing a Repository in an Existing Directory

If you have a project directory that is currently not under version control and you want to start controlling it with Git, you first need to go to that project’s directory. If you’ve never done this, it looks a little different depending on which system you’re running:

for Linux:

```bash
cd /home/user/my_project
```

for macOS:

```zsh
cd /Users/user/my_project
```

for Windows:

```powershell
cd C:/Users/user/my_project
```

and type:

```bash
git init
```

This creates a new sub-directory named `.git` that contains all of your necessary repository files — a Git repository skeleton. At this point, nothing in your project is tracked yet

## Cloning an Existing Repository

If you wish to get a copy of an existing Git repository (for example, to contribute to an open-source project), the command you need is `git clone <url>`.

A crucial distinction between Git and other version control systems (like Subversion) is that Git **receives a full copy of nearly all data** the server has. Instead of just getting a working copy of the latest version, every version of every file in the project's history is pulled down by default. Because every clone is a **full backup** of all the data, you can often use a client clone to restore a server if its disk becomes corrupted.

**Transfer Protocols and Customization**

• **Target Directories:** When you clone, Git usually creates a directory named after the repository. However, you can specify a different name by adding it as an additional argument: `` git clone <url> <my_new_name>``. For example, 

```bash
git clone https://github.com/omkardamame/learning-git.git
```
 
This will create a folder `learning-git` and save all the files from the repo into it.

If you do this,

```bash
git clone https://github.com/omkardamame/learning-git.git new-git
```

This will create a new folder called `new-git` and save all the files from the repo into it.

• **Protocols:** Git supports several transfer protocols. While many examples use **HTTPS** (`https://`), you may also see the **Git protocol** (`git://`) or **SSH** (`user@server:path/to/repo.git`).

# 2.2 [Recording Changes to the Repository](https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository)

Once you have a local Git repository, you begin the cycle of modifying files and **committing snapshots** of those changes into your repository whenever the project reaches a desired state.

**File States: Tracked vs. Untracked**

Every file in your working directory exists in one of two states: **tracked** or **untracked**.

• **Tracked files** are those that were in the last snapshot or have been newly staged; they can be **unmodified**, **modified**, or **staged**.

• **Untracked files** are everything else — files in your working directory that were not in your last snapshot and are not yet in your staging area. Git will not include untracked files in commits until you explicitly tell it to do so.

**Checking and Staging Changes**

• **git status**: This is the main tool used to determine which files are in which state. For a more compact view, `git status -s` or `git status --short` shows status symbols like `??` for untracked files, `A` for new staged files, and `M` for modified files. For example,

```console
omkar@black-box:~/study$ git status -s
 M README.md
?? lul_file
```

• **git add**: This is a multipurpose command used to **begin tracking new files**, **stage modified files**, and mark merge-conflicted files as resolved. It effectively adds "precisely this content" to the next commit. For example,

```bash
git add README.md
```

• **Crucial Note on Staging**: Git stages a file **exactly as it is** when you run `git add`. If you modify a file _after_ running `git add`, you must run the command again to stage the newest version of those changes.

If you run your status command again, you can see that your `README` file is now tracked and staged to be committed:

```console
$ git status
On branch main
Your branch is up-to-date with 'study/main'.
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)

    new file:   README.md
```

**Ignoring Files**

To prevent Git from automatically adding or showing untracked files—such as log files or build artifacts—you can create a **.gitignore** file. Rules for this file include:

• Standard **glob patterns** (like `*.[oa]` to ignore all `.o` or `.a` files).
• A leading slash (`/`) to avoid recursivity.
• A trailing slash (`/`) to specify a directory.
• An exclamation point (`!`) to negate a pattern.

Glob patterns are like simplified regular expressions that shells use. An asterisk (`*`) matches zero or more characters; `[abc]` matches any character inside the brackets (in this case a, b, or c); a question mark (`?`) matches a single character; and brackets enclosing characters separated by a hyphen (`[0-9]`) matches any character between them (in this case 0 through 9). You can also use two asterisks to match nested directories; `a/**/z` would match `a/z`, `a/b/z`, `a/b/c/z`, and so on.

Here is another example `.gitignore` file:

```console
# ignore all .a files
*.a

# but do track lib.a, even though you're ignoring .a files above
!lib.a

# only ignore the TODO file in the current directory, not subdir/TODO
/TODO

# ignore all files in any directory named build
build/

# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt

# ignore all .pdf files in the doc/ directory and any of its subdirectories
doc/**/*.pdf
```

**Viewing Changes**

While `git status` lists filenames, **git diff** shows the exact lines added or removed.

• **git diff**: Shows what you have changed but **not yet staged**.

• **git diff --staged** (or **--cached**): Shows what you have staged that will go into your **next commit**.

**Committing and Removing Files**

• **git commit**: This command records the snapshot set up in your staging area. You can use the **-m** flag to provide a message inline or the **-a** flag to **automatically stage every tracked file** and skip the `git add` step. For example,

```bash
git commit -m "YOUR COMMIT HERE"
```

• **git rm**: Removes a file from your tracked files and the working directory. If you want to **keep the file on your hard drive** but stop Git from tracking it, use **git rm --cached**.

The "Manual" Way (2 steps):

1. `rm old_script.py` (Delete the file from your hard drive)
2. `git add old_script.py` (Tell Git to stage this deletion)

The `git rm` Way (1 step):

`git rm old_script.py` This single command deletes the file from your folder **and** tells Git "I want to delete this in my next commit." When you run `git status`, you’ll immediately see the file listed under "Changes to be committed."

• **git mv**: A convenience command for renaming files; it is functionally equivalent to running `mv`, `git rm`, and `git add` in sequence.

If you didn't use `git mv`, you would have to do this:

1. `mv old_name.txt new_name.txt` (Rename the file on your disk)
2. `git rm old_name.txt` (Tell Git the old name is gone)
3. `git add new_name.txt` (Tell Git the new name exists)

The `git mv` Way (1 step):

`git mv old_name.txt new_name.txt` performs all three steps above instantly. Because the content is the same, Git is smart enough to show this in your `git status` as `renamed: old_name.txt -> new_name.txt`.

# 2.3 [Viewing the Commit History](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History)

The most basic and powerful tool for reviewing a repository's history is the **git log** **command**. By default, this command lists commits in **reverse chronological order**, showing the SHA-1 checksum, the author’s name and email, the date, and the commit message

**Formatting and Visualization Options**

You can customize the amount of detail displayed using several formatting flags:

• **-p** **or** **--patch**: Displays the **specific differences (the patch)** introduced in each commit. You can also limit the number of log entries displayed, such as using `-1` or `-2` to show only the last one or two entries.

```console
omkar@black-box:~/study$ git log -p -1
commit 967675effe0d2fc6d142eb9d95e4b6038dc9e50d (HEAD -> main, study/main)
Author: Omkar Damame <omkardamame.work@gmail.com>
Date:   Thu Dec 25 17:47:22 2025 +0530

    Updated README.md to latest progress

diff --git a/README.md b/README.md
index 8678d0a..61c29d9 100644
--- a/README.md
+++ b/README.md
@@ -23,7 +23,7 @@ Each item requires **reading + hands-on practice** before being marked complete.
 ### 📖 Chapter 2 – Git Basics (CRITICAL)
 
 - ✅ Initializing a repository (`git init`)
-- [ ] File lifecycle (untracked → staged → committed)
+- ✅ File lifecycle (untracked → staged → committed)
 - [ ] `git status` and `git diff`
 - [ ] Staging files (`git add`)
 - [ ] Committing correctly (`git commit`)
```

• **--stat**: Provides abbreviated statistics for each commit, listing **which files were modified** and how many lines were added or removed.

```console
omkar@black-box:~/study$ git log --stat
commit 967675effe0d2fc6d142eb9d95e4b6038dc9e50d (HEAD -> main, study/main)
Author: Omkar Damame <omkardamame.work@gmail.com>
Date:   Thu Dec 25 17:47:22 2025 +0530

    Updated README.md to latest progress

 README.md | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

commit f6fbd6daa87bf4f7d090a02c3d761350f5e0941c
Author: Omkar Damame <omkardamame.work@gmail.com>
Date:   Thu Dec 25 17:46:03 2025 +0530

    Completed File lifecycle from Chapter 2

 summary/Chapter 2 - Git Basics.md | 116 +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++-
 1 file changed, 115 insertions(+), 1 deletion(-)
```

• **--pretty**: Changes the output to prebuilt formats like `oneline` (one commit per line), `short`, `full`, or `fuller`.

```console
omkar@black-box:~/study$ git log --pretty=oneline
967675effe0d2fc6d142eb9d95e4b6038dc9e50d (HEAD -> main, study/main) Updated README.md to latest progress
f6fbd6daa87bf4f7d090a02c3d761350f5e0941c Completed File lifecycle from Chapter 2
d071d42dbfa4c1e1ddb6330623bc364b860d4d23 Added summary of Chapter 2: Getting a Git Repository
dfbeaecc46c0abb64a584b73590ad55a5f3a0ace Made a minor change in Chapter 1
96936be15348b085c240f115eb5bc51065e94679 Added summary for Chapter 1: Getting Started
aa8ff53be0c2ce10af8d005b1127e194635af004 Added emoji instead of generic tick-box for completion
9c8fde78a2d55308ca582322ce4c57f642e03c1b Started Chapter 2
5120a02a93add6a2cad5591e66733d27f78f3461 Completed Chapter 1
ea57ef858125ad74817aeba9fc7f919d56d32c25 chore: Added Pro Git Book link to CONTRIBUTING.md
ee2c85973d11935138b2fdfa91a8bce83b5d81c5 Changed content of CONTRIBUTING.md
92a72ab2e66250a5b3fd3f3f54c427a4efcd0c19 Changed few things
02ccf8d6e5cdad98e3d3b3b9dc5b7aa2fcbd1106 Initial commit for studying
```

```console
omkar@black-box:~/study$ git log --pretty=short
commit 967675effe0d2fc6d142eb9d95e4b6038dc9e50d (HEAD -> main, study/main)
Author: Omkar Damame <omkardamame.work@gmail.com>

    Updated README.md to latest progress

commit f6fbd6daa87bf4f7d090a02c3d761350f5e0941c
Author: Omkar Damame <omkardamame.work@gmail.com>

    Completed File lifecycle from Chapter 2

commit d071d42dbfa4c1e1ddb6330623bc364b860d4d23
Author: Omkar Damame <omkardamame.work@gmail.com>

    Added summary of Chapter 2: Getting a Git Repository
```

• **format**: Allows for highly customized layouts using specifiers, such as `%h` for the abbreviated hash, `%an` for the author name, `%ar` for the relative date, and `%s` for the subject.

```console
omkar@black-box:~/study$ git log --pretty=format:"%h - %an, %ar : %s"
967675e - Omkar Damame, 2 days ago : Updated README.md to latest progress
f6fbd6d - Omkar Damame, 2 days ago : Completed File lifecycle from Chapter 2
d071d42 - Omkar Damame, 2 days ago : Added summary of Chapter 2: Getting a Git Repository
```

Table 1. Useful specifiers for `git log --pretty=format`

| Specifier | Description of Output                            |
| --------- | ------------------------------------------------ |
| %H        | Commit hash                                      |
| %h        | Abbreviated commit hash                          |
| %T        | Tree hash                                        |
| %t        | Abbreviated tree hash                            |
| %P        | Parent hashes                                    |
| %p        | Abbreviated parent hashes                        |
| %an       | Author name                                      |
| %ae       | Author email                                     |
| %ad       | Author date (format respects the --date= option) |
| %ar       | Author date, relative                            |
| %cn       | Committer name                                   |
| %ce       | Committer email                                  |
| %cd       | Committer date                                   |
| %cr       | Committer date, relative                         |
| %s        | Subject                                          |

Note: You may be wondering what the difference is between _author_ and _committer_. The author is the person who originally wrote the work, whereas the committer is the person who last applied the work. So, if you send in a patch to a project and one of the core members applies the patch, **both of you get credit** — you as the author, and the core member as the committer.

• **--graph**: Adds a visual **ASCII graph** showing branch and merge history alongside the commit data.

```console
omkar@black-box:~/study$ git log --pretty=format:"%h %s" --graph
* 967675e Updated README.md to latest progress
* f6fbd6d Completed File lifecycle from Chapter 2
* d071d42 Added summary of Chapter 2: Getting a Git Repository
```

Table 2. Common options to `git log`

| Option          | Description                                                                                               |
| --------------- | --------------------------------------------------------------------------------------------------------- |
| -p              | Show the patch introduced with each commit.                                                               |
| --stat          | Show statistics for files modified in each commit.                                                        |
| --shortstat     | Display only the changed/insertions/deletions line from the --stat command.                               |
| --name-only     | Show the list of files modified after the commit information.                                             |
| --name-status   | Show the list of files affected with added/modified/deleted information as well.                          |
| --abbrev-commit | Show only the first few characters of the SHA-1 checksum instead of all 40.                               |
| --relative-date | Display the date in a relative format (for example, “2 weeks ago”) instead of using the full date format. |
| --graph         | Display an ASCII graph of the branch and merge history beside the log output.                             |
| --pretty        | Show commits in an alternate format. Option values include oneline, short, full, fuller, and format.      |
| --oneline       | Shorthand for --pretty=oneline --abbrev-commit used together.                                             |

**Limiting History and Filtering**

To find specific information in large projects, you can limit the output:

• -n: Shows only the last _n_ commits (e.g., `2`).

```console
omkar@black-box:~/study$ git log -n 2
commit 967675effe0d2fc6d142eb9d95e4b6038dc9e50d (HEAD -> main, study/main)
Author: Omkar Damame <omkardamame.work@gmail.com>
Date:   Thu Dec 25 17:47:22 2025 +0530

    Updated README.md to latest progress

commit f6fbd6daa87bf4f7d090a02c3d761350f5e0941c
Author: Omkar Damame <omkardamame.work@gmail.com>
Date:   Thu Dec 25 17:46:03 2025 +0530

    Completed File lifecycle from Chapter 2
```

• **Time-limiting**: Options like **--since** **and** **--until** allow you to filter by relative time (e.g., "2.weeks") or specific dates (e.g., "2008-01-15").

```bash
git log --since=2.weeks
```

• **Metadata Filtering**: Use **--author** to find commits by a specific person or **--grep** to search commit messages for keywords.

```bash
omkar@black-box:~/study$ git log --pretty="%h - %s" --author='Omkar' 
967675e - Updated README.md to latest progress
f6fbd6d - Completed File lifecycle from Chapter 2
d071d42 - Added summary of Chapter 2: Getting a Git Repository
```

• **-S** **(The "Pickaxe")**: Searches for commits that changed the number of occurrences of a **specific string** in the code.

```bash
git log -S function_name
```

• **Path Filtering**: Adding a path after double dashes (e.g., `-- path/to/file`) limits the log to commits that introduced changes to **specific files or directories**.

```bash
git log -- path/to/file
```

# 2.4 [Undoing Things](https://git-scm.com/book/en/v2/Git-Basics-Undoing-Things)

At any stage of development, you may need to **undo a change**, though you must be careful as some operations can result in the **permanent loss of work**. Committed snapshots are generally very difficult to lose, but changes that have never been committed may be gone forever once undone.

- **Amending the Last Commit**

If you commit too early, forget to add a file, or make a typo in your commit message, you can use the **--amend** **option**.

• **Command:** `$ git commit --amend`

```bash
git commit -m 'Initial commit'
git add forgotten_file
git commit --amend -m "git tFixed commit"
```

• This command takes your current staging area and uses it for the commit, **replacing the previous commit entirely**.

• **Warning:** You should **only amend commits that are still local** and have not been pushed to a shared repository, as rewriting pushed history causes major problems for collaborators.

- **Unstaging a Staged File**

If you accidentally stage a file (using `git add *`, for example) and want to move it back to being just a modified file, you can use the following commands depending on your Git version:

• **For versions 2.23.0+:** Use **$ git restore --staged filename**.

```bash
omkar@black-box:~/study$ git add *
omkar@black-box:~/study$ git status
On branch main
Your branch is up to date with 'study/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   test1
	new file:   test2

omkar@black-box:~/study$ git restore --staged test2
omkar@black-box:~/study$ git status
On branch main
Your branch is up to date with 'study/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   test1

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	test2
```

• **For older versions:** Use **$ git reset HEAD filename**. These commands change your index to match the last commit but **do not touch the changes in your working directory**.

```bash
omkar@black-box:~/study$ git add *
omkar@black-box:~/study$ git status
On branch main
Your branch is up to date with 'study/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   test1
	new file:   test2

omkar@black-box:~/study$ git reset HEAD test2
omkar@black-box:~/study$ git status
On branch main
Your branch is up to date with 'study/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   test1

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	test2
```

- **Unmodifying a Modified File**

If you realize you do not want to keep changes made to a file and want to **revert it to exactly how it looked in the last commit**, you have two options:

• **For versions 2.23.0+:** Use `git restore filename`.

• **For older versions:** Use `git checkout -- filename`.

• **Warning:** Both of these are **dangerous commands**. Git replaces your local file with the last committed version, and **any unsaved local changes are permanently deleted**.

**Reverting Commits**

For cases where you need to undo a change that has already been recorded in history, the **git revert** command can be used. Unlike reset, which moves pointers, a revert creates a **new commit** that applies the **exact opposite changes** of the commit you are targeting.

Simple example

1. You have these commits:

```bash
Commit 3 – Added footer 
Commit 2 – Added header 
Commit 1 – Initial project files
```

2. You realize “Added header” (Commit 2) was a mistake.
3. Run:

```bash
git revert <hash-of-commit-2>
```

4. Git automatically creates a new commit:

```bash
Commit 4 – Revert "Added header" 
Commit 3 – Added footer 
Commit 2 – Added header 
Commit 1 – Initial project files
```

The “header” changes are undone, but the original history remains.

## One-line example (revert last commit)

```bash
git revert HEAD
```

That is the simplest and most common use.

# 2.5 [Working with Remotes](https://git-scm.com/book/en/v2/Git-Basics-Working-with-Remotes)

**Remote repositories** are versions of your project hosted on the Internet, a network, or even elsewhere on your **local machine**. Collaborating with others requires managing these remotes by pushing and pulling data to share work.

**Showing Your Remotes**

To see which remote servers you have configured, run the **git remote** command, which lists the shortnames of your remote handles. If you have cloned a repository, you will likely see **origin**, the default name Git gives to the server you cloned from. Using **git remote -v** provides a more detailed view, showing the specific URLs stored for each shortname used for fetching and pushing.

```bash
omkar@black-box:~/study$ git remote -v
origin	https://github.com/omkardamame/learning-git.git (fetch)
origin	https://github.com/omkardamame/learning-git.git (push)
```

**Adding Remote Repositories**

While `git clone` adds the `origin` remote automatically, you can add new remotes explicitly using the command **git remote add shortname url**. Once added, you can use the shortname (such as `study`) on the command line as a convenient alias for the full URL.

```bash
git remote add study https://github.com/omkardamame/learning-git.git
```

```bash
omkar@black-box:~/study$ git remote -v
origin	https://github.com/omkardamame/learning-git.git (fetch)
origin	https://github.com/omkardamame/learning-git.git (push)
study	https://github.com/omkardamame/learning-git.git (fetch)
study	https://github.com/omkardamame/learning-git.git (push)
```

**Fetching and Pulling from Remotes**

To get data from a remote project without automatically merging it into your current work, use `git fetch <remote>`. This command downloads all the data from that remote that you do not yet have, providing references to all its branches for inspection or manual merging.

```console
omkar@black-box:~/study$ git fetch origin 
From https://github.com/omkardamame/learning-git
 + 13b2f64...59c4a26 main       -> origin/main  (forced update)
```

Alternatively, if your local branch is set up to **track** a remote branch, you can use **git pull**. This command automatically fetches the data and then attempts to **merge** that remote branch into your current local branch. Starting with Git version 2.27, `git pull` will issue a warning if the `pull.rebase` variable is not explicitly configured to either "true" or "false".

**Pushing to Your Remotes**

When you are ready to share a specific branch upstream, use `git push <remote> <branch>` (e.g., `git push origin master`). This operation only succeeds if you have **write access** to the server and if no one else has pushed changes in the meantime. If the remote has been updated since your last fetch, Git will reject your push, and you must incorporate the new work into yours before being allowed to push again.

**Inspecting, Renaming, and Removing Remotes**

• **Inspecting:** Use `git remote show REMOTE_NAME` to see detailed information, including the fetch and push URLs, the HEAD branch, and which local branches are configured for automatic pulling and pushing.

```console
omkar@black-box:~/study$ git remote show origin
* remote origin
  Fetch URL: https://github.com/omkardamame/learning-git.git
  Push  URL: https://github.com/omkardamame/learning-git.git
  HEAD branch: main
  Remote branch:
    main tracked
  Local ref configured for 'git push':
    main pushes to main (up to date)
```

• **Renaming:** Use `git remote rename <old_name> <new_name>` to change a remote's shortname; this also updates all associated remote-tracking branch names.

```console
omkar@black-box:~/study$ git remote -v
origin	https://github.com/omkardamame/learning-git.git (fetch)
origin	https://github.com/omkardamame/learning-git.git (push)
study	https://github.com/omkardamame/learning-git.git (fetch)
study	https://github.com/omkardamame/learning-git.git (push)
omkar@black-box:~/study$ git remote rename study study2
omkar@black-box:~/study$ git remote -v
origin	https://github.com/omkardamame/learning-git.git (fetch)
origin	https://github.com/omkardamame/learning-git.git (push)
study2	https://github.com/omkardamame/learning-git.git (fetch)
study2	https://github.com/omkardamame/learning-git.git (push)
```

• **Removing:** If a contributor is no longer active or a server has moved, use `git remote remove` or `git remote rm` to delete the remote and its associated configuration and tracking branches.

```console
omkar@black-box:~/study$ git remote -v
origin	https://github.com/omkardamame/learning-git.git (fetch)
origin	https://github.com/omkardamame/learning-git.git (push)
study2	https://github.com/omkardamame/learning-git.git (fetch)
study2	https://github.com/omkardamame/learning-git.git (push)
omkar@black-box:~/study$ git remote remove study2
omkar@black-box:~/study$ git remote -v
origin	https://github.com/omkardamame/learning-git.git (fetch)
origin	https://github.com/omkardamame/learning-git.git (push)
```

