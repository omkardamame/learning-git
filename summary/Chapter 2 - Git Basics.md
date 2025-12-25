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