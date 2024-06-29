## Git Basics

![git-logo](./README.assets/git-logo.png) 

This repository serves as a demonstration of Git, based on the book [Pro Git 2nd Edition](https://github.com/progit/progit2/) available at https://git-scm.com/book/en/v2. It is recommended to read Chapters 1, 2, 3, 5, and 6 to grasp the fundamentals of Git.

### About Versions Control

https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control

#### Local Version Control Systems

![Local version control diagram](./README.assets/local.png) 

#### Centralized Version Control Systems

![Centralized version control diagram](./README.assets/centralized.png) 

#### Distributed Version Control Systems

![Distributed version control diagram](./README.assets/distributed.png) 



### What is Git?

https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F

> Other systems (CVS, Subversion, Perforce, and so on) think of the information they store as a set of files and the changes made to each file over time (this is commonly described as *delta-based* version control).

![Storing data as changes to a base version of each file](./README.assets/deltas.png) 

> Git thinks about its data more like a **stream of snapshots**.
>
> This makes Git more like a mini filesystem with some incredibly powerful tools built on top of it, rather than simply a VCS.

![Git stores data as snapshots of the project over time](./README.assets/snapshots.png)

#### Git Advantages

- Nearly Every Operation Is Local
- Git Has Integrity (SHA1, transition to SHA-256)
- Git Generally Only Adds Data

#### The Three States

> Pay attention now — here is the main thing to remember about Git if you want the rest of your learning process to go smoothly. Git has three main states that your files can reside in: *modified*, *staged*, and *committed*:
>
> - Modified means that you have changed the file but have not committed it to your database yet.
> - Staged means that you have marked a modified file in its current version to go into your next commit snapshot.
> - Committed means that the data is safely stored in your local database.

#### Three main sections of a Git project

The working tree or working directory, the staging area, and the Git directory.

![Working tree, staging area, and Git directory](./README.assets/areas.png)

> The working tree is a single checkout of one version of the project. These files are pulled out of the compressed database in the Git directory and placed on disk for you to use or modify.
>
> The staging area is a file, generally contained in your Git directory, that stores information about what will go into your next commit. Its technical name in Git parlance is the “index”, but the phrase “staging area” works just as well.
>
> The Git directory is where Git stores the metadata and object database for your project. This is the most important part of Git, and it is what is copied when you *clone* a repository from another computer.

#### Basic Git Workflow

1. You modify files in your working tree.
2. You selectively stage just those changes you want to be part of your next commit, which adds *only* those changes to the staging area.
3. You do a commit, which takes the files as they are in the staging area and stores that snapshot permanently to your Git directory.

### The Git Command Line

> There are a lot of different ways to use Git. There are the original command-line tools, and there are many graphical user interfaces of varying capabilities. [...] The command line is the only place you can run *all* Git commands — most of the GUIs implement only a partial subset of Git functionality for simplicity. If you know how to run the command-line version, you can probably also figure out how to run the GUI version, while the opposite is not necessarily true.

#### Installing Git

https://github.com/git-guides/install-git

#### First-Time Git Setup

https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup

Check git version:

```shell
git --version
```

Global configuration

```shell
# Mandatory: Set your user name and email address.
git config --global user.name "John Doe"
git config --global user.email "johndoe@example.com"

# Optional but recommended: Set default branch to main.
git config --global init.defaultBranch main

# Optional. If not configured, Git uses your system’s default editor.
git config --global core.editor vim
# Or
git config --global core.editor nvim
```

> [!TIP]  
> https://jvns.ca/blog/2024/02/16/popular-git-config-options/

Use the `git config --list` command to list all the settings Git can find at that point:

```shell
git config --list
```

```
filter.lfs.required=true
filter.lfs.clean=git-lfs clean -- %f
filter.lfs.smudge=git-lfs smudge -- %f
filter.lfs.process=git-lfs filter-process
user.email=johndoe@example.com
user.name=John Doe
core.editor=nvim
init.defaultbranch=main
...
```

#### Need Help?

```shell
git help <verb>
git <verb> --help
man git-<verb>
```

### Getting a Git Repository

https://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository

> You typically obtain a Git repository in one of two ways:
>
> 1. You can take a local directory that is currently not under version control, and turn it into a Git repository, or
> 2. You can *clone* an existing Git repository from elsewhere.

#### Initializing a Repository in an Existing Directory

> If you have a project directory that is currently not under version control and you want to start controlling it with Git, you first need to go to that project’s directory. [...] and type:

```shell
git init
```

> This creates a new subdirectory named `.git` that contains all of your necessary repository files — a Git repository skeleton. At this point, nothing in your project is tracked yet.
>
> If you want to start version-controlling existing files (as opposed to an empty directory), you should probably begin tracking those files and do an initial commit. You can accomplish that with a few `git add` commands that specify the files you want to track, followed by a `git commit`:

```shell
git add FILE1
git add FILE2
......
# Or
git add .

git commit -m 'Initial project version'
```

#### Cloning an Existing Repository

> If you want to get a copy of an existing Git repository — for example, a project you’d like to contribute to — the command you need is `git clone`. 
>
> You clone a repository with `git clone <url>`. 

For example, if you want to clone this repository, you can do so like this:

```shell
git clone https://github.com/sait-lab/git-basics
```

That creates a directory named `git-basics`, initializes a `.git` directory inside it, pulls down all the data for that repository, and checks out a working copy of the latest version. If you go into the new `git-basics` directory that was just created, you’ll see the project files in there, ready to be worked on or used.

If you want to clone the repository into a directory named something other than `git-basics`, you can specify the new directory name as an additional argument:

```
git clone https://github.com/sait-lab/git-basics ~/my-git-notes
```

That command does the same thing as the previous one, but the target directory is called `my-git-notes`.

Git has a number of different transfer protocols you can use. The previous example uses the `https://` protocol, but you may also see `git://` or `user@server:path/to/repo.git`, which uses the SSH transfer protocol.

> [!TIP]  
> [GitHub Key-Based SSH Authentication](https://github.com/sait-lab/devops/blob/main/GitHub%20Key-Based%20SSH%20Authentication.md)

### Recording Changes to the Repository

https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository

#### States of files in your working directory

>each file in your working directory can be in one of two states: *tracked* or *untracked*. Tracked files are files that were in the last snapshot, as well as any newly staged files; they can be unmodified, modified, or staged. In short, tracked files are files that Git knows about.

> Untracked files are everything else — any files in your working directory that were not in your last snapshot and are not in your staging area. When you first clone a repository, all of your files will be tracked and unmodified because Git just checked them out and you haven’t edited anything.

> As you edit files, Git sees them as modified, because you’ve changed them since your last commit. As you work, you selectively stage these modified files and then commit all those staged changes, and the cycle repeats.

![lifecycle](./README.assets/lifecycle.png) 

#### Checking the Status of Your Files

The main tool you use to determine which files are in which state is the `git status` command. If you run this command directly after a clone, you should see something like this:

```
$ git status                                                               ubuntu@kind
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

Add a new file to this project, a simple `MY-NOTES.md` file. If the file didn’t exist before, and you run `git status`, you see your untracked file like so:

```
$ echo 'This file contains my notes' > MY-NOTES.md
$ git status

On branch main
Your branch is up to date with 'origin/main'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        MY-NOTES.md

nothing added to commit but untracked files present (use "git add" to track)
```

> Untracked basically means that Git sees a file you didn’t have in the previous snapshot (commit), and which hasn’t yet been staged; Git won’t start including it in your commit snapshots until you explicitly tell it to do so. It does this so you don’t accidentally begin including generated binary files or other files that you did not mean to include.

![git_commands](./README.assets/git_commands.png) 
Credit: [Chapter 15 Git Command Line Interface (CLI) | The Shiny AWS Book (business-science.github.io)](https://business-science.github.io/shiny-production-with-aws-book/git-command-line-interface-cli.html)

#### Tracking New Files

Use the command `git add` to track a new file.

```
$ git add MY-NOTES.md
```

If you run your status command again, you can see that your `MY-NOTES` file is now tracked and staged to be committed:

```
git status                                                            ubuntu@kind
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   MY-NOTES.md
```

> [!TIP]  
> You can enhance the shell with `git` integration. The screenshot below shows [romkatv/powerlevel10k: A Zsh theme (github.com)](https://github.com/romkatv/powerlevel10k) adds git status to the `zsh` command line prompt.
>
> ![zsh-theme-git-prompt](./README.assets/zsh-theme-git-prompt.jpg) 

> You can tell that it’s staged because it’s under the “Changes to be committed” heading. If you commit at this point, the version of the file at the time you ran `git add` is what will be in the subsequent historical snapshot. You may recall that when you ran `git init` earlier, you then ran `git add <files>` — that was to begin tracking files in your directory. The `git add` command takes a path name for either a file or a directory; if it’s a directory, the command adds all the files in that directory recursively.

#### Staging Modified Files

> If you change a previously tracked file called `CONTRIBUTING.md` and then run your `git status` command again, you get something that looks like this:

![modify-contributing](./README.assets/modify-contributing.jpg) 

> The `CONTRIBUTING.md` file appears under a section named “Changes not staged for commit” — which means that a file that is tracked has been modified in the working directory but not yet staged. To stage it, you run the `git add` command. `git add` is a **multipurpose** command — you use it to begin tracking new files, to stage files, and to do other things like marking merge-conflicted files as resolved. It may be helpful to think of it more as “add precisely this content to the next commit” rather than “add this file to the project”. Let’s run `git add` now to stage the `CONTRIBUTING.md` file, and then run `git status` again:

```
$ git add CONTRIBUTING.md
$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   CONTRIBUTING.md
        new file:   MY-NOTES.md
```

> Both files are staged and will go into your next commit. At this point, suppose you remember one little change that you want to make in `CONTRIBUTING.md` before you commit it. You open it again and make that change, and you’re ready to commit. However, let’s run `git status` one more time:

![modify-contributing-after-add](./README.assets/modify-contributing-after-add.jpg) 

> Now `CONTRIBUTING.md` is listed as both staged *and* unstaged. How is that possible? It turns out that Git stages a file exactly as it is when you run the `git add` command. If you commit now, the version of `CONTRIBUTING.md` as it was when you last ran the `git add` command is how it will go into the commit, not the version of the file as it looks in your working directory when you run `git commit`. If you modify a file after you run `git add`, you have to run `git add` again to stage the latest version of the file:

```
$ git add CONTRIBUTING.md
$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   CONTRIBUTING.md
        new file:   MY-NOTES.md
```

#### Short Status

If you run `git status -s` or `git status --short` you get a far more simplified output from the command:

```
$ git status -s
M  CONTRIBUTING.md
A  MY-NOTES.md
```

#### Ignoring Files

>Often, you’ll have a class of files that you don’t want Git to automatically add or even show you as being untracked. These are generally automatically generated files such as log files or files produced by your build system. In such cases, you can create a file listing patterns to match them named `.gitignore`.


> [!TIP]  
>
> GitHub maintains a fairly comprehensive list of good `.gitignore` file examples for dozens of projects and languages at https://github.com/github/gitignore if you want a starting point for your project.

>The rules for the patterns you can put in the `.gitignore` file are as follows:
>- Blank lines or lines starting with `#` are ignored.
>- Standard glob patterns work, and will be applied recursively throughout the entire working tree.
>- You can start patterns with a forward slash (`/`) to avoid recursivity.
>- You can end patterns with a forward slash (`/`) to specify a directory.
>- You can negate a pattern by starting it with an exclamation point (`!`).

Here is an example `.gitignore` file:

```
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

#### Viewing Your Staged and Unstaged Changes

> If the `git status` command is too vague for you — you want to know exactly what you changed, not just which files were changed — you can use the `git diff` command. We’ll cover `git diff` in more detail later, but you’ll probably use it most often to answer these two questions: What have you changed but not yet staged? And what have you staged that you are about to commit? Although `git status` answers those questions very generally by listing the file names, `git diff` shows you the exact lines added and removed — the patch, as it were.

Let’s say you edit and stage the `MY-NOTES.md` file again and then edit the `CONTRIBUTING.md` file without staging it. If you run your `git status` command, you once again see something like this:

```
$ git status
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   MY-NOTES.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   CONTRIBUTING.md
```

> To see what you’ve changed but not yet staged, type `git diff` with no other arguments:

![git-diff](./README.assets/git-diff.jpg) 

> That command compares what is in your working directory with what is in your staging area. The result tells you the changes you’ve made that you haven’t yet staged.

![git-diff--staged](./README.assets/git-diff--staged.png) 

> It’s important to note that `git diff` by itself doesn’t show all changes made since your last commit — only changes that are still unstaged. If you’ve staged all of your changes, `git diff` will give you no output.
>
> For another example, if you stage the `CONTRIBUTING.md` file and then edit it, you can use `git diff` to see the changes in the file that are staged and the changes that are unstaged. If our environment looks like this:

```
$ git add CONTRIBUTING.md
$ echo '# test line' >> CONTRIBUTING.md
$ git status
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   CONTRIBUTING.md
        modified:   MY-NOTES.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   CONTRIBUTING.md
```

> Now you can use `git diff` to see what is still unstaged:

```
$ git diff
diff --git a/CONTRIBUTING.md b/CONTRIBUTING.md
index 112d9d7..a64cb95 100644
--- a/CONTRIBUTING.md
+++ b/CONTRIBUTING.md
@@ -3,3 +3,4 @@
 Feel free to fork this repository and submit pull requests with improvements to the documents or suggestions for additional guides. Your contributions are highly appreciated!

 Thanks.
+# test line
```

> and `git diff --cached` to see what you’ve staged so far (`--staged` and `--cached` are synonyms):

```
$ git diff --cached
diff --git a/CONTRIBUTING.md b/CONTRIBUTING.md
index 0c90661..112d9d7 100644
--- a/CONTRIBUTING.md
+++ b/CONTRIBUTING.md
@@ -1,3 +1,5 @@
 ## Contributing

-Feel free to fork this repository and submit pull requests with improvements to the documents or suggestions for additional guides. Your contributions are highly appreciated!Thanks
+Feel free to fork this repository and submit pull requests with improvements to the documents or suggestions for additional guides. Your contributions are highly appreciated!
+
+Thanks.
diff --git a/MY-NOTES.md b/MY-NOTES.md
index fa002f7..dfa8c4b 100644
--- a/MY-NOTES.md
+++ b/MY-NOTES.md
@@ -1 +1 @@
-This file contains my notes.
+This file contains my notes
```

> [!NOTE]  
>
> **Git Diff in an External Tool**
>
> We will continue to use the git diff command in various ways throughout the rest of the book. There is another way to look at these diffs if you prefer a graphical or external diff viewing program instead. If you run git difftool instead of git diff, you can view any of these diffs in software like emerge, vimdiff and many more (including commercial products). Run `git difftool --tool-help` to see what is available on your system.
>
> ![git-diff-ext-macos](./README.assets/git-diff-ext-macos.png) 
>
> ![git-diff-ext-nvimdiff](./README.assets/git-diff-ext-nvimdiff.png) 

#### Committing Your Changes

>Now that your staging area is set up the way you want it, you can commit your changes. Remember that anything that is still unstaged — any files you have created or modified that you haven’t run `git add` on since you edited them — won’t go into this commit. They will stay as modified files on your disk. In this case, let’s say that the last time you ran `git status`, you saw that everything was staged, so you’re ready to commit your changes. The simplest way to commit is to type `git commit`:

```console
$ git commit
```







