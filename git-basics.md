### Table of Contents

   * [Git Basics](#git-basics)
      * [About Versions Control](#about-versions-control)
         * [Local Version Control Systems](#local-version-control-systems)
         * [Centralized Version Control Systems](#centralized-version-control-systems)
         * [Distributed Version Control Systems](#distributed-version-control-systems)
      * [What is Git?](#what-is-git)
         * [Git Advantages](#git-advantages)
         * [The Three States](#the-three-states)
         * [Three main sections of a Git project](#three-main-sections-of-a-git-project)
         * [Basic Git Workflow](#basic-git-workflow)
      * [The Git Command Line](#the-git-command-line)
         * [Installing Git](#installing-git)
         * [First-Time Git Setup](#first-time-git-setup)
         * [Need Help?](#need-help)
      * [Getting a Git Repository](#getting-a-git-repository)
         * [Initializing a Repository in an Existing Directory](#initializing-a-repository-in-an-existing-directory)
         * [Cloning an Existing Repository](#cloning-an-existing-repository)
      * [Recording Changes to the Repository](#recording-changes-to-the-repository)
         * [States of files in your working directory](#states-of-files-in-your-working-directory)
         * [Checking the Status of Your Files](#checking-the-status-of-your-files)
         * [Tracking New Files](#tracking-new-files)
         * [Staging Modified Files](#staging-modified-files)
         * [Short Status](#short-status)
         * [Ignoring Files](#ignoring-files)
         * [Viewing Your Staged and Unstaged Changes](#viewing-your-staged-and-unstaged-changes)
         * [Committing Your Changes](#committing-your-changes)
         * [Skipping the Staging Area](#skipping-the-staging-area)
         * [Removing Files](#removing-files)
         * [Moving Files](#moving-files)
      * [Viewing the Commit History](#viewing-the-commit-history)
      * [Undoing Things](#undoing-things)
         * [Redo the commit](#redo-the-commit)
         * [Unstaging a Staged File](#unstaging-a-staged-file)
         * [Unmodifying a Modified File](#unmodifying-a-modified-file)
      * [Working with Remotes](#working-with-remotes)
         * [Showing Your Remotes](#showing-your-remotes)
         * [Adding Remote Repositories](#adding-remote-repositories)
         * [Fetching and Pulling from Your Remotes](#fetching-and-pulling-from-your-remotes)
         * [Pushing to Your Remotes](#pushing-to-your-remotes)
         * [Inspecting a Remote](#inspecting-a-remote)
         * [Renaming and Removing Remotes](#renaming-and-removing-remotes)
      * [Tagging](#tagging)
         * [Creating Tags](#creating-tags)
         * [Annotated Tags](#annotated-tags)
         * [Lightweight Tags](#lightweight-tags)
         * [Tagging Later](#tagging-later)
         * [Sharing Tags](#sharing-tags)
         * [Deleting Tags](#deleting-tags)
         * [Checking out Tags](#checking-out-tags)
---
> The staging area is a file, generally contained in your Git directory, that stores information about what will go into your next commit. Its technical name in Git parlance is the "index", but the phrase "staging area" works just as well.

> You can tell that it’s staged because it’s under the "Changes to be committed" heading. If you commit at this point, the version of the file at the time you ran `git add` is what will be in the subsequent historical snapshot. You may recall that when you ran `git init` earlier, you then ran `git add <files>` — that was to begin tracking files in your directory. The `git add` command takes a path name for either a file or a directory; if it’s a directory, the command adds all the files in that directory recursively.
> The `README` file appears under a section named "Changes not staged for commit" — which means that a file that is tracked has been modified in the working directory but not yet staged. To stage it, you run the `git add` command. `git add` is a **multipurpose** command — you use it to begin tracking new files, to stage files, and to do other things like marking merge-conflicted files as resolved. It may be helpful to think of it more as "add precisely this content to the next commit" rather than "add this file to the project". Let’s run `git add` now to stage the `README` file, and then run `git status` again:
> The obvious value to amending commits is to make minor improvements to your last commit, without cluttering your repository history with commit messages of the form, "Oops, forgot to add a file" or "Darn, fixing a typo in last commit".
> What if you realize that you don’t want to keep your changes to the `file2.txt` file? How can you easily unmodify it — revert it back to what it looked like when you last committed?
> If you clone a repository, the command automatically adds that remote repository under the name "origin". So, `git fetch origin` fetches any new work that has been pushed to that server since you cloned (or last fetched from) it. It’s important to note that the `git fetch` command only downloads the data to your local repository — it doesn’t automatically merge it with any of your work or modify what you’re currently working on. You have to merge it manually into your work when you’re ready.

#### Creating Tags

> Git supports two types of tags: *lightweight* and *annotated*.
>
> A lightweight tag is very much like a branch that doesn’t change — it’s just a pointer to a specific commit.
>
> Annotated tags, however, are stored as full objects in the Git database. They’re checksummed; contain the tagger name, email, and date; have a tagging message; and can be signed and verified with GNU Privacy Guard (GPG). It’s generally recommended that you create annotated tags so you can have all this information; but if you want a temporary tag or for some reason don’t want to keep the other information, lightweight tags are available too.

#### Annotated Tags

Creating an annotated tag in Git is simple. The easiest way is to specify `-a` when you run the `tag` command:

```
$ git tag -a v1.0 -m "my version 1.0"
$ git tag
v1.0
```

> The `-m` specifies a tagging message, which is stored with the tag. If you don’t specify a message for an annotated tag, Git launches your editor so you can type it in.

You can see the tag data along with the commit that was tagged by using the `git show` command:

![git-show-tag](./git-basics.assets/git-show-tag.jpg) 

That shows the tagger information, the date the commit was tagged, and the annotation message before showing the commit information.

#### Lightweight Tags

Another way to tag commits is with a lightweight tag. This is basically the commit checksum stored in a file — no other information is kept. To create a lightweight tag, don’t supply any of the `-a`, `-s`, or `-m` options, just provide a tag name:

```console
$ git tag v1.0-lw
$ git tag
v1.0
v1.0-lw
```

This time, if you run `git show` on the tag, you don’t see the extra tag information. The command just shows the commit (not Tagger, Date and Tagging message):

```
commit c88f81e4d0f3db6c869fea8c0a5f77e9e964fdfa (HEAD -> main, tag: v1.0-lw, tag: v1.0)
Author: Hong Yan <hong.yan@sait.ca>
Date:   Mon Jul 1 01:05:36 2024 -0600

    docs: add second file

diff --git a/file2.txt b/file2.txt
new file mode 100644
index 0000000..1c59427
--- /dev/null
+++ b/file2.txt
@@ -0,0 +1 @@
+second file
```

#### Tagging Later

You can also tag commits after you’ve moved past them. Suppose your commit history looks like this:

```
$ git log --pretty=oneline
c88f81e4d0f3db6c869fea8c0a5f77e9e964fdfa (HEAD -> main, tag: v1.0-lw, tag: v1.0) docs: add second file
05fa8735d4b12cb07c08c1e6c4a3edb60b515256 add youtube as a resource line
112baf88345293f41f8ade0de15ead40ffc68bf6 docs: add first note
10d0b1e2ecac34e24ca0fb5ab3c40e120c7dc880 docs: add personal notes and modify README
f3a85ef6962e008c20a770785907284ffbd9b43d (origin/main, origin/HEAD) docs: add learn-git link
5ec89905ee345ab0ffe1e73fdc1f1dbfb8b1ac9a docs: add learngitbranching
93a4b4e8056292c08a46e921c042996dc973bcd9 docs: add README
dd8e2a99b997ca5fa3a722c5785b78ab5496515f docs: add file1.txt
8d5a743ae486d8f4bbc179b02ce25c8134847e5c docs: add README
bee491d94feebaeb2bdb21b978aa6d03fa5e995a Initial commit
```

Now, suppose you forgot to tag the project at v0.9, which was at the `05fa8735d...` commit. You can add it after the fact. To tag that commit, you specify the commit checksum (or part of it) at the end of the command:

```
$ git tag -a v0.9 05fa8735d -m "ver 0.9 before ga"
$ git tag
v0.9
v1.0
v1.0-lw

$ git show v0.9
tag v0.9
Tagger: Hong Yan <hong.yan@sait.ca>
Date:   Mon Jul 1 02:11:42 2024 -0600

ver 0.9 before ga

commit 05fa8735d4b12cb07c08c1e6c4a3edb60b515256 (tag: v0.9)
Author: Hong Yan <hong.yan@sait.ca>
Date:   Mon Jul 1 00:41:33 2024 -0600

    add youtube as a resource line

diff --git a/README b/README
index 1bca90c..769e57c 100644
--- a/README
+++ b/README
@@ -2,3 +2,4 @@ welcome to git demo repo
 learn git
 git is a powerful tool
 keep working on labs
+youtube is a great educational resource
```

#### Sharing Tags

By default, the `git push` command doesn’t transfer tags to remote servers. You will have to explicitly push tags to a shared server after you have created them. This process is just like sharing remote branches — you can run `git push origin <tagname>`.

```
$ git push origin v1.0
```

If you have a lot of tags that you want to push up at once, you can also use the `--tags` option to the `git push` command. This will transfer all of your tags to the remote server that are not already there.

```
$ git push origin --tags
```

> [!NOTE]
>
> `git push` pushes both types of tags
>
> `git push <remote> --tags` will push both lightweight and annotated tags. There is currently no option to push only lightweight tags, but if you use `git push <remote> --follow-tags` only annotated tags will be pushed to the remote.

#### Deleting Tags

To delete a tag on your local repository, you can use `git tag -d <tagname>`. For example, we could remove our lightweight tag above as follows:

```console
$ git tag -d v1.0-lw
Deleted tag 'v1.0-lw' (was c88f81e)

$ git tag
v0.9
v1.0
```

Note that this does not remove the tag from any remote servers. You can delete a remote tag is with:

```
$ git push origin --delete <tagname>
```

#### Checking out Tags

If you want to view the versions of files a tag is pointing to, you can do a `git checkout` of that tag, although this puts your repository in "detached HEAD" state, which has some ill side effects:

```console
$ git checkout v1.1
```

In "detached HEAD" state, if you make changes and then create a commit, the tag will stay the same, but your new commit won’t belong to any branch and will be unreachable, except by the exact commit hash. Thus, if you need to make changes — say you’re fixing a bug on an older version, for instance — you will generally want to create a branch:

```console
$ git checkout -b version2 v2.0.0
Switched to a new branch 'version2'
```

If you do this and make a commit, your `version2` branch will be slightly different than your `v2.0.0` tag since it will move forward with your new changes, so do be careful.