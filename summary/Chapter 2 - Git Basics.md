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