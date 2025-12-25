# 1.1 [About Version Control](https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control)

What is “version control”, and why should you care? Version control is a system that records changes to a file or set of files over time so that you can recall specific versions later.

It allows you to revert selected files back to a previous state, revert the entire project back to a previous state, compare changes over time, see who last modified something that might be causing a problem, who introduced an issue and when, and more. 
Using a VCS also generally means that if you screw things up or lose files, you can easily recover. In addition, you get all this for very little overhead.

# 1.2 [A Short History of Git](https://git-scm.com/book/en/v2/Getting-Started-A-Short-History-of-Git)

As with many great things in life, Git began with a bit of creative destruction and fiery controversy.

Git originated in **2005** following a breakdown in the relationship between the Linux kernel community and the company behind **BitKeeper**, a proprietary tool they had used since 2002. Led by **Linus Torvalds**, the community developed Git to replace the practice of passing around patches and archived files.

The system was designed with several core goals in mind:

• **High speed** and a **simple design**.
• Strong support for **non-linear development**, allowing for thousands of parallel branches.
• A **fully distributed** architecture.
• The ability to handle **large-scale projects** efficiently.

Since its inception, Git has matured into an easy-to-use tool that maintains its initial qualities of speed and efficiency

# 1.3 [What is Git?](https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F)

• **Snapshots, Not Deltas:** Unlike other version control systems that store a base file and a list of incremental changes (deltas), Git views its data as a **stream of snapshots** of a miniature filesystem. Every time you commit, Git "takes a picture" of all your files; if a file has not changed, it simply links to the previous version to maintain efficiency.

• **Local Operations and Speed:** Git is designed to be **amazingly fast** because nearly all operations are performed using local files and resources. Since your entire project history is stored on your local disk, you do not need a network connection to browse history or compare file versions.

• **Data Integrity:** Reliability is a core pillar of Git's design. Every file and commit is identified by a **SHA-1 hash** (a 40-character checksum), making it impossible to change the contents of any file or directory without Git knowing about it.

• **Strong Support for Non-linear Development:** Git was built to handle **thousands of parallel branches**, allowing for complex, non-linear development workflows that are much more efficient than traditional systems.

• **Fully Distributed Nature:** Git is a **fully distributed system**, meaning every user has a complete backup of the project’s data and history, which provides significant redundancy and allows for various distributed workflows.

• **The Three States:** To understand Git's workflow, you must recognize the three states a file can reside in:

   ◦ **Modified**: You have changed the file but have not yet committed it to your database.

   ◦ **Staged:** You have marked a modified file in its current version to go into your next commit snapshot.

   ◦ **Committed:** The data is safely stored in your local Git directory.

# 1.4 [The Command Line](https://git-scm.com/book/en/v2/Getting-Started-The-Command-Line)

• **The Primary Interface:** It introduces the command line as the fundamental way to interact with Git, providing the most control and access to all features.

• **Bridge to Practice:** This section moves the learner from the "What" and "Why" (Chapters 1.2 and 1.3) into the practical phase of using the software.

• **Efficiency & Speed:** Using the command line aligns with Git's core design goals of being "amazingly fast" and having a "simple design".

• **Gateway to Advanced Tools:** Familiarity with the command line is necessary before moving on to recording changes, branching, and exploring Git's internal "plumbing".

# 1.5 [Installing Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

• **Linux:** Git is usually installed using the distribution’s native package manager. For example, on Fedora, one would use `sudo dnf install git-all`, while on Debian-based systems like Ubuntu, `sudo apt install git-all` is used.

• **macOS:** The easiest method is often running `git --version` in the terminal; if it isn't installed, the system will prompt the user to install the **Xcode Command Line Tools**. Alternatively, many users utilize the **Homebrew** package manager. If you want a more up to date version, you can also install it via a binary installer. A macOS Git installer is maintained and available for download at the Git website, at [https://git-scm.com/download/mac](https://git-scm.com/download/mac).

• **Windows:** Users typically download the official [**Git for Windows**](https://git-scm.com/download/win) installer which provides both a command-line version (Git Bash) and a standard GUI. To get an automated installation you can use the [Git Chocolatey package](https://community.chocolatey.org/packages/git). Note that the Chocolatey package is community maintained.

• **Installing from Source:** This is a more advanced option that allows users to get the most up-to-date version by downloading the source code, installing necessary dependencies, and compiling the software manually (Refer the book).

# 1.6 [First-Time Git Setup](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup)

After installing Git, you should customize your environment using the **git config** **tool**, which controls how Git looks and operates. These configurations generally only need to be performed once on a given computer, as they persist between upgrades and can be changed at any time.

**Configuration Levels** Git stores configuration variables in three different levels, with each level overriding the previous one:

1. **System**: Located in `[path]/etc/gitconfig`, these settings apply to every user and all their repositories; they are accessed using the **--system** option.

2. **Global**: Found in `~/.gitconfig` or `~/.config/git/config`, these are specific to you as a user and affect all repositories on your system; they are accessed with the **--global** option.

3. **Local**: Stored in the `.git/config` file of a specific repository, these apply only to that project and are accessed using the **--local** option (the default).

**Setting Your Identity** The most critical first-time setup step is to **set your user name and email address**. This information is **immutably baked into every commit** you create. You can set these globally with the following commands:

```bash
git config --global user.name "John Doe"
```

```bash
git config --global user.email johndoe@example.com
```

**Configuring Your Editor and Branch Names** You can also configure the **default text editor** that Git launches when you need to type a message; otherwise, Git uses your system's default. For example, to use VSCode, you would run

```bash
git config --global core.editor code 
```

Additionally, while Git defaults to using `master` for the initial branch name in new repositories, from version 2.28 onwards, you can set a different default, such as **main**, using 

```bash
git config --global init.defaultBranch main`
```

**Checking Your Settings** To verify your configurations, you can use 

```bash
git config --list
```

OR

```bash
git config --list --show-origin
```

to see all settings Git finds at that moment. If you see duplicate keys because Git has read the same key from different files, it will use the last value it encountered. To see the specific value of a single key, you can run `git config <key>`, and to identify which configuration file is responsible for a specific setting, use `git config --show-origin <key>`. For example,

```bash
git config user.name
```

Output will be like: 
```console
John Doe
```

# 1.7 [Getting Help](https://git-scm.com/book/en/v2/Getting-Started-Getting-Help)

If you need assistance while using Git, there are **three equivalent ways** to access the comprehensive manual page (manpage) for any Git command:

```bash 
git help <verb>
```

```bash
git <verb> --help
```

For example, to access the help page for the configuration command, you would run 

```bash
git help config
```

These resources are particularly useful because they can be accessed **anywhere, even while offline.**

If you do not require the full manual but only need a **quick refresher** on the available options for a specific command, you can use the **-h** **option** to receive a more concise output:

```bash
git add -h
```

as in:

```console
git add -h
usage: git add [<options>] [--] <pathspec>...

    -n, --dry-run               dry run
    -v, --verbose               be verbose

    -i, --interactive           interactive picking
    -p, --patch                 select hunks interactively
    -e, --edit                  edit current diff and apply
    -f, --force                 allow adding otherwise ignored files
    -u, --update                update tracked files
    --renormalize               renormalize EOL of tracked files (implies -u)
    -N, --intent-to-add         record only the fact that the path will be added later
    -A, --all                   add changes from all tracked and untracked files
    --ignore-removal            ignore paths removed in the working tree (same as --no-all)
    --refresh                   don't add, only refresh the index
    --ignore-errors             just skip files which cannot be added because of errors
    --ignore-missing            check if - even missing - files are ignored in dry run
    --sparse                    allow updating entries outside of the sparse-checkout cone
    --chmod (+|-)x              override the executable bit of the listed files
    --pathspec-from-file <file> read pathspec from file
```

# 1.8 [Summary](https://git-scm.com/book/en/v2/Getting-Started-Summary)

By this point, you should possess a **basic understanding of what Git is** and recognize how it **differs from the centralized version control systems** you may have used in the past.

Additionally, you should now have a **working version of Git installed** on your system that has been **configured with your personal identity**. With these foundational setup steps completed, you are prepared to move on to learning **Git basics**, which include the primary commands used for the majority of daily tasks.
