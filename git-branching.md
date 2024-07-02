## Git Branching

https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell



---



### Branches in a Nutshell

Git stores data as a series of **snapshots**.

> When you make a commit, Git stores a commit object that contains a pointer to the snapshot of the content you staged. This object also contains the author’s name and email address, the message that you typed, and pointers to the commit or commits that directly came before this commit (its parent or parents): zero parents for the initial commit, one parent for a normal commit, and multiple parents for a commit that results from a merge of two or more branches.

Todo:

- [ ] Show blob, tree and commit objects in `.git` folder.

![branch-and-history](./git-branching.assets/branch-and-history.png) 

> [!NOTE]
>
> In Git, HEAD refers to the currently checked-out branch's latest commit. 

![main-head-v1.0](./git-branching.assets/main-head-v1.0.png) 


> A branch in Git is simply a lightweight movable pointer to one of these commits. The default branch name in Git is `master`. As you start making commits, you’re given a `master` branch that points to the last commit you made. Every time you commit, the `master` branch pointer moves forward automatically.

> [!NOTE]
>
> The `master` or `main` branch in Git is not a special branch. They are exactly like any other branch. The only reason nearly every repository has one is that the `git init` command creates it by default and most people don't bother to change it.
>
> All new GitHub repositories create a default branch named `main`, and GitHub no longer creates a `master` branch. Run `git config --global init.defaultBranch main` to set default git's default branch to `main`.
>
> https://github.blog/changelog/2020-10-01-the-default-branch-for-newly-created-repositories-is-now-main/

#### Creating a New Branch

What happens when you create a new branch? Well, doing so creates a new pointer for you to move around. Let’s say you want to create a new branch called `testing`. You do this with the `git branch` command:

```console
$ git branch testing
```

This creates a new pointer to the same commit you’re currently on.

![Two branches pointing into the same series of commits](./git-branching.assets/two-branches.png) 

How does Git know what branch you’re currently on? It keeps a special pointer called `HEAD`. In Git, this is a pointer to the local branch you’re currently on. In this case, you’re still on `main`. The `git branch` command only *created* a new branch — it didn’t switch to that branch.

![HEAD pointing to a branch](./git-branching.assets/head-to-master.png) 



You can easily see this by running a simple `git log` command that shows you where the branch pointers are pointing. This option is called `--decorate`.

![create-testing-branch](./git-branching.assets/create-testing-branch.png) 

#### Switching Branches

To switch to an existing branch, you run the `git checkout` command. Let’s switch to the new `testing` branch:

```console
$ git checkout testing
```

This moves `HEAD` to point to the `testing` branch.

![head-to-testing](./git-branching.assets/head-to-testing.png) 

![check-out-testing-branch](./git-branching.assets/check-out-testing-branch.png) 

What is the significance of that? Well, let’s do another commit:

```
$ echo 'a new line from testing branch' >> file1.txt
$ git add file1.txt
$ git commit -m 'docs: add a line to file1'
```

![advance-testing](./git-branching.assets/advance-testing.png) 

![commit-to-testing-branch](./git-branching.assets/commit-to-testing-branch.jpg) 

This is interesting, because now your `testing` branch has moved forward, but your `main` branch still points to the commit you were on when you ran `git checkout` to switch branches. Let’s switch back to the `main` branch:

```console
$ git checkout main
```

 ![checkout-master](./git-branching.assets/checkout-master.png) 

![testing-branch-missing](./git-branching.assets/testing-branch-missing.png) 

> [!NOTE]
>
> `git log` doesn’t show *all* the branches *all* the time
>
> If you were to run `git log` right now, you might wonder where the "testing" branch you just created went, as it would not appear in the output.
>
> The branch hasn’t disappeared; Git just doesn’t know that you’re interested in that branch and it is trying to show you what it thinks you’re interested in. In other words, by default, `git log` will only show commit history below the branch you’ve checked out.
>
> To show commit history for the desired branch you have to explicitly specify it: `git log testing`. To show all of the branches, add `--all` to your `git log` command.

![checkout-main-from-testing](./git-branching.assets/checkout-main-from-testing.png) 

> That command did two things. It moved the HEAD pointer back to point to the `main` branch, and it reverted the files in your working directory back to the snapshot that `main` points to. This also means the changes you make from this point forward will diverge from an older version of the project. It essentially rewinds the work you’ve done in your `testing` branch so you can go in a different direction.

> [!NOTE]
>
> Switching branches changes files in your working directory
>
> It’s important to note that when you switch branches in Git, files in your working directory will change. If you switch to an older branch, your working directory will be reverted to look like it did the last time you committed on that branch. If Git cannot do it cleanly, it will not let you switch at all.

Let’s make a few changes and commit again:

```
$ echo 'a new line from main branch' >> file1.txt
$ git add file1.txt
$ git commit -m 'docs: add a line to file1'
```

> Now your project history has diverged (see [Divergent history](https://git-scm.com/book/en/v2/ch00/divergent_history)). You created and switched to a branch, did some work on it, and then switched back to your main branch and did other work. Both of those changes are isolated in separate branches: you can switch back and forth between the branches and merge them together when you’re ready. And you did all that with simple `branch`, `checkout`, and `commit` commands.

![advance-master](./git-branching.assets/advance-master.png) 

You can also see this easily with the `git log` command. If you run `git log --oneline --decorate --graph --all` it will print out the history of your commits, showing where your branch pointers are and how your history has diverged.

![advance-master-log](./git-branching.assets/advance-master-log.png) 

![divergent-view-fork](./git-branching.assets/divergent-view-fork.png) 

> Because a branch in Git is actually a simple file that contains the 40 character SHA-1 checksum of the commit it points to, branches are cheap to create and destroy. Creating a new branch is as quick and simple as writing 41 bytes to a file (40 characters and a newline).
>
> This is in sharp contrast to the way most older VCS tools branch, which involves copying all of the project’s files into a second directory. This can take several seconds or even minutes, depending on the size of the project, whereas in Git the process is always instantaneous. Also, because we’re recording the parents when we commit, finding a proper merge base for merging is automatically done for us and is generally very easy to do. These features help encourage developers to create and use branches often.

> [!NOTE]
>
> Creating a new branch and switching to it at the same time
>
> It’s typical to create a new branch and want to switch to that new branch at the same time — this can be done in one operation with `git checkout -b <newbranchname>`.

> [!NOTE]
>
> From Git version 2.23 onwards you can use `git switch` instead of `git checkout` to:
>
> - Switch to an existing branch: `git switch testing-branch`.
> - Create a new branch and switch to it: `git switch -c new-branch`. The `-c` flag stands for create, you can also use the full flag: `--create`.
> - Return to your previously checked out branch: `git switch -`.



---



### Basic Branching and Merging

https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging

Let’s go through a simple example of branching and merging with a workflow that you might use in the real world. You’ll follow these steps:

1. Do some work on a website.
2. Create a branch for a new user story you’re working on.
3. Do some work in that branch.

At this stage, you’ll receive a call that another issue is critical and you need a hotfix. You’ll do the following:

1. Switch to your production branch.
2. Create a branch to add the hotfix.
3. After it’s tested, merge the hotfix branch, and push to production.
4. Switch back to your original user story and continue working.

#### Basic Branching

First, let’s say you’re working on your project and have a couple of commits already on the `main` branch.

```
echo 'basic branching' > index.html
git add index.html
git commit -m 'C0: create index.html'

echo 'create and use branches often' >> index.html
git add index.html
git commit -m 'C1: add a line to webpage'

echo 'branches are lightweight movable pointer to commits' >> index.html
git add index.html
git commit -m 'C2: add branch definition to webpage'
```

![basic-branching-1](./git-branching.assets/basic-branching-1.png) 

![basic-branching-1-log](./git-branching.assets/basic-branching-1-log.png) 

You’ve decided that you’re going to work on issue #53 in whatever issue-tracking system your company uses. To create a new branch and switch to it at the same time, you can run the `git checkout -b iss53` or `git switch -c iss53`:

```
$ git switch -c iss53
Switched to a new branch 'iss53'
```

> [!NOTE]
>
> This is shorthand for:
>
> ```console
> $ git branch iss53
> $ git checkout iss53
> ```

![basic-branching-2](./git-branching.assets/basic-branching-2.png) 

You work on your website and do some commits. Doing so moves the `iss53` branch forward, because you have it checked out (that is, your `HEAD` is pointing to it):

```console
echo '-- branching in git --' >> index.html
git commit -a -m 'Create new footer [issue 53]'
```

![add-footer-c3](./git-branching.assets/add-footer-c3.png) 

![basic-branching-3](./git-branching.assets/basic-branching-3.png) 

Now you get the call that there is an issue with the website, and you need to fix it immediately. With Git, you don’t have to deploy your fix along with the `iss53` changes you’ve made, and you don’t have to put a lot of effort into reverting those changes before you can work on applying your fix to what is in production. All you have to do is switch back to your `main` branch.

However, before you do that, note that if your working directory or staging area has uncommitted changes that conflict with the branch you’re checking out, Git won’t let you switch branches. It’s best to have a clean working state when you switch branches. There are ways to get around this (namely, stashing and commit amending) that we’ll cover later on, in [Stashing and Cleaning](https://git-scm.com/book/en/v2/ch00/_git_stashing). For now, let’s assume you’ve committed all your changes, so you can switch back to your `main` branch:

```
$ git switch main
Switched to branch 'main'
```

At this point, your project working directory is exactly the way it was before you started working on issue #53, and you can concentrate on your hotfix. 

> [!IMPORTANT]
>
> This is an important point to remember: **when you switch branches, Git resets your working directory to look like it did the last time you committed on that branch**. It adds, removes, and modifies files automatically to make sure your working copy is what the branch looked like on your last commit to it.

Next, you have a hotfix to make. Let’s create a `hotfix` branch on which to work until it’s completed:

```
git switch -c hotfix
echo 'contact john.doe@example.com' >> index.html
git commit -a -m 'C4: Fix broken email address'
```

![basic-branching-4](./git-branching.assets/basic-branching-4.png) 

![basic-branching-4-log](./git-branching.assets/basic-branching-4-log.png) 

You can run your tests, make sure the hotfix is what you want, and finally merge the `hotfix` branch back into your `main` branch to deploy to production. You do this with the `git merge` command:

```
$ git switch main
$ git merge hotfix

Updating 401e4c2..93b0def
Fast-forward
 index.html | 1 +
 1 file changed, 1 insertion(+)
```

You’ll notice the phrase “fast-forward” in that merge. Because the commit `C4` pointed to by the branch `hotfix` you merged in was directly ahead of the commit `C2` you’re on, Git simply moves the pointer forward. To phrase that another way, when you try to merge one commit with a commit that can be reached by following the first commit’s history, Git simplifies things by moving the pointer forward because there is no divergent work to merge together — this is called a “fast-forward.”

Your change is now in the snapshot of the commit pointed to by the `main` branch, and you can deploy the fix.

![basic-branching-5](./git-branching.assets/basic-branching-5.png) 

![basic-branching-5-log](./git-branching.assets/basic-branching-5-log.png) 

After your super-important fix is deployed, you’re ready to switch back to the work you were doing before you were interrupted. However, first you’ll delete the `hotfix` branch, because you no longer need it — the `main` branch points at the same place. You can delete it with the `-d` option to `git branch`:

```
$ git branch -d hotfix
Deleted branch hotfix (was 93b0def).
```

Now you can switch back to your work-in-progress branch on issue #53 and continue working on it.

```
git switch iss53
echo '-= new footer =-' >> index.html
git commit -a -m 'C5: Finish the new footer [issue 53]'
```

![basic-branching-6](./git-branching.assets/basic-branching-6.png) 

![basic-branching-6-log](./git-branching.assets/basic-branching-6-log.png) 

It’s worth noting here that the work you did in your `hotfix` branch is not contained in the files in your `iss53` branch. If you need to pull it in, you can merge your `main` branch into your `iss53` branch by running `git merge main`, or you can wait to integrate those changes until you decide to pull the `iss53` branch back into `main` later.



#### Basic Merging

Suppose you’ve decided that your issue #53 work is complete and ready to be merged into your `main` branch. In order to do that, you’ll merge your `iss53` branch into `main`, much like you merged your `hotfix` branch earlier. All you have to do is check out the branch you wish to merge into and then run the `git merge` command:

```
$ git checkout main
# Or
$ git switch main

$ git merge iss53
```

This looks a bit different than the `hotfix` merge you did earlier. In this case, your development history has diverged from some older point. Because the commit on the branch you’re on isn’t a direct ancestor of the branch you’re merging in, Git has to do some work. In this case, Git does a simple three-way merge, using the two snapshots pointed to by the branch tips and the common ancestor of the two.

![basic-merging-1](./git-branching.assets/basic-merging-1.png) 

Instead of just moving the branch pointer forward, Git creates a new snapshot that results from this three-way merge and automatically creates a new commit that points to it. This is referred to as a merge commit, and is special in that it has more than one parent.

![basic-merging-2](./git-branching.assets/basic-merging-2.png) 

Now that your work is merged in, you have no further need for the `iss53` branch. You can close the issue in your issue-tracking system, and delete the branch:

```console
$ git branch -d iss53
```



#### Basic Merge Conflicts

Occasionally, this process doesn’t go smoothly. If you changed the same part of the same file differently in the two branches you’re merging, Git won’t be able to merge them cleanly. If your fix for issue #53 modified the same part of a file as the `hotfix` branch, you’ll get a merge conflict that looks something like this:

```
$ git switch main
$ git merge iss53
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```

![merge-conflict-view](./git-branching.assets/merge-conflict-view.png) 

Git hasn’t automatically created a new merge commit. It has paused the process while you resolve the conflict. If you want to see which files are unmerged at any point after a merge conflict, you can run `git status`:

```
$ git status
On branch main
......
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   index.html

no changes added to commit (use "git add" and/or "git commit -a")
```

Anything that has merge conflicts and hasn’t been resolved is listed as unmerged. Git adds standard conflict-resolution markers to the files that have conflicts, so you can open them manually and resolve those conflicts. Your file contains a section that looks something like this:

![conflict-markers](./git-branching.assets/conflict-markers.png) 

This means the version in `HEAD` (your `main` branch, because that was what you had checked out when you ran your merge command) is the top part of that block (everything above the `=======`), while the version in your `iss53` branch looks like everything in the bottom part. In order to resolve the conflict, you have to either choose one side or the other or merge the contents yourself. For instance, you might resolve this conflict by replacing the entire block with this:

```
-= merged footer =-
```

This resolution has a little of each section, and the `<<<<<<<`, `=======`, and `>>>>>>>` lines have been completely removed. After you’ve resolved each of these sections in each conflicted file, run `git add` on each file to mark it as resolved. Staging the file marks it as resolved in Git.

If you want to use a graphical tool to resolve these issues, you can run `git mergetool`, which fires up an appropriate visual merge tool and walks you through the conflicts:

```
git mergetool
```

![ext-merge-tools-cmd](./git-branching.assets/ext-merge-tools-cmd.png) 

The screenshot below shows using [Neovim](https://neovim.io/) as merge tool.

![merge-tool-view](./git-branching.assets/merge-tool-view.png) 

The screenshot below shows using [Kaleidoscope](https://kaleidoscope.app/) as merge tool.

![ext-conflict-resolver](./git-branching.assets/ext-conflict-resolver.png) 

If you want to use a merge tool other than the default (Git chose `opendiff` in this case because the command was run on macOS), you can see all the supported tools listed at the top after “one of the following tools.” Just type the name of the tool you’d rather use.

After you exit the merge tool, Git asks you if the merge was successful. If you tell the script that it was, it stages the file to mark it as resolved for you. You can run `git status` again to verify that all conflicts have been resolved:

```
$ git status
On branch main
Your branch and 'origin/main' have diverged,
and have 8 and 1 different commits each, respectively.
  (use "git pull" if you want to integrate the remote branch with yours)

All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:
        modified:   index.html
```

If you’re happy with that, and you verify that everything that had conflicts has been staged, you can type `git commit` to finalize the merge commit. The commit message by default looks something like this:

```
$ # git commit will launch the default editor
$ git commit
```

![merge-commit-editor](./git-branching.assets/merge-commit-editor.png) 

If you think it would be helpful to others looking at this merge in the future, you can modify this commit message with details about how you resolved the merge and explain why you did the changes you made if these are not obvious.

![merge-commit-log-view](./git-branching.assets/merge-commit-log-view.png) 

The screenshot below shows using [Fork](https://git-fork.com/) to check merged branches.

![merge-commit-fork-gui-view](./git-branching.assets/merge-commit-fork-gui-view.png) 



---



### Branch Management

https://git-scm.com/book/en/v2/Git-Branching-Branch-Management

Now that you’ve created, merged, and deleted some branches, let’s look at some branch-management tools that will come in handy when you begin using branches all the time.

The `git branch` command does more than just create and delete branches. If you run it with no arguments, you get a simple listing of your current branches:

```console
$ git branch
  iss53
* main
  testing
```

Notice the `*` character that prefixes the `main` branch: it indicates the branch that you currently have checked out (i.e., the branch that `HEAD` points to). This means that if you commit at this point, the `main` branch will be moved forward with your new work. To see the last commit on each branch, you can run `git branch -v`:

```console
$ git branch -v
  iss53   7bd33cb C5: Finish the new footer [issue 53]
* main    8b0e02a [......] Merge branch 'iss53'
  testing d1ce380 docs: add a line to file1
```

The useful `--merged` and `--no-merged` options can filter this list to branches that you have or have not yet merged into the branch you’re currently on. To see which branches are already merged into the branch you’re on, you can run `git branch --merged`:

```console
$ git branch --merged
 iss53
* main
```

Because you already merged in `iss53` earlier, you see it in your list. Branches on this list without the `*` in front of them are generally fine to delete with `git branch -d`; you’ve already incorporated their work into another branch, so you’re not going to lose anything.

To see all the branches that contain work you haven’t yet merged in, you can run `git branch --no-merged`:

```
$ git branch --no-merged
 testing
```

This shows your other branch. Because it contains work that isn’t merged in yet, trying to delete it with `git branch -d` will fail:

```console
$ git branch -d testing
error: the branch 'testing' is not fully merged.
If you are sure you want to delete it, run 'git branch -D testing'
```

If you really do want to delete the branch and lose that work, you can force it with `-D`, as the helpful message points out.



#### Changing a branch name

> [!CAUTION]
>
> **Do not rename branches that are still in use by other collaborators**. Do not rename a branch like master/main/mainline without having read the section [Changing the master branch name](https://git-scm.com/book/en/v2/ch00/_changing_master).

Suppose you have a branch that is called `bad-branch-name` and you want to change it to `corrected-branch-name`, while keeping all history. You also want to change the branch name on the remote (GitHub, GitLab, other server). How do you do this?

Rename the branch locally with the `git branch --move` command:

```console
$ git branch --move bad-branch-name corrected-branch-name
```

This replaces your `bad-branch-name` with `corrected-branch-name`, but this change is only local for now. To let others see the corrected branch on the remote, push it:

```console
$ git push --set-upstream origin corrected-branch-name
```

Now we’ll take a brief look at where we are now:

```console
$ git branch --all
* corrected-branch-name
  main
  remotes/origin/bad-branch-name
  remotes/origin/corrected-branch-name
  remotes/origin/main
```

Notice that you’re on the branch `corrected-branch-name` and it’s available on the remote. However, the branch with the bad name is also still present there but you can delete it by executing the following command:

```console
$ git push origin --delete bad-branch-name
```

Now the bad branch name is fully replaced with the corrected branch name.



#### Changing the master branch name

> [!WARNING]  
>
> Changing the name of a branch like master/main/mainline/default will break the integrations, services, helper utilities and build/release scripts that your repository uses. Before you do this, make sure you consult with your collaborators. Also, make sure you do a thorough search through your repo and update any references to the old branch name in your code and scripts.

Rename your local `master` branch into `main` with the following command:

```console
$ git branch --move master main
```

There’s no local `master` branch anymore, because it’s renamed to the `main` branch.

To let others see the new `main` branch, you need to push it to the remote. This makes the renamed branch available on the remote.

```console
$ git push --set-upstream origin main
```

Now we end up with the following state:

```console
$ git branch --all
* main
  remotes/origin/HEAD -> origin/master
  remotes/origin/main
  remotes/origin/master
```

Your local `master` branch is gone, as it’s replaced with the `main` branch. The `main` branch is present on the remote. However, the old `master` branch is still present on the remote. Other collaborators will continue to use the `master` branch as the base of their work, until you make some further changes.

Now you have a few more tasks in front of you to complete the transition:

- Any projects that depend on this one will need to update their code and/or configuration.
- Update any test-runner configuration files.
- Adjust build and release scripts.
- Redirect settings on your repo host for things like the repo’s default branch, merge rules, and other things that match branch names.
- Update references to the old branch in documentation.
- Close or merge any pull requests that target the old branch.

After you’ve done all these tasks, and are certain the `main` branch performs just as the `master` branch, you can delete the `master` branch:

```console
$ git push origin --delete master
```



---



### Branching Workflows

https://git-scm.com/book/en/v2/Git-Branching-Branching-Workflows

#### Long-Running Branches

Because Git uses a simple three-way merge, merging from one branch into another multiple times over a long period is generally easy to do. This means you can have several branches that are always open and that you use for different stages of your development cycle; you can merge regularly from some of them into others.

Many Git developers have a workflow that embraces this approach, such as having only code that is entirely stable in their `main` branch — possibly only code that has been or will be released. They have another parallel branch named `develop` or `next` that they work from or use to test stability — it isn’t necessarily always stable, but whenever it gets to a stable state, it can be merged into `main`. It’s used to pull in topic branches (short-lived branches, like your earlier `iss53` branch) when they’re ready, to make sure they pass all the tests and don’t introduce bugs.

In reality, we’re talking about pointers moving up the line of commits you’re making. The stable branches are farther down the line in your commit history, and the bleeding-edge branches are farther up the history.

![lr-branches-1](./git-branching.assets/lr-branches-1.png) 

It’s generally easier to think about them as work silos, where sets of commits graduate to a more stable silo when they’re fully tested.

![lr-branches-2](./git-branching.assets/lr-branches-2.png) 

You can keep doing this for several levels of stability. Some larger projects also have a `proposed` or `pu` (proposed updates) branch that has integrated branches that may not be ready to go into the `next` or `main` branch. The idea is that your branches are at various levels of stability; when they reach a more stable level, they’re merged into the branch above them. Again, having multiple long-running branches isn’t necessary, but it’s often helpful, especially when you’re dealing with very large or complex projects.

#### Topic Branches

Topic branches, however, are useful in projects of any size. A topic branch is a short-lived branch that you create and use for a single particular feature or related work. This is something you’ve likely never done with a VCS before because it’s generally too expensive to create and merge branches. But in Git it’s common to create, work on, merge, and delete branches several times a day.

You saw this in the last section with the `iss53` and `hotfix` branches you created. You did a few commits on them and deleted them directly after merging them into your main branch. This technique allows you to context-switch quickly and completely — because your work is separated into silos where all the changes in that branch have to do with that topic, it’s easier to see what has happened during code review and such. You can keep the changes there for minutes, days, or months, and merge them in when they’re ready, regardless of the order in which they were created or worked on.

Consider an example of doing some work (on `main`), branching off for an issue (`iss91`), working on it for a bit, branching off the second branch to try another way of handling the same thing (`iss91v2`), going back to your `master` branch and working there for a while, and then branching off there to do some work that you’re not sure is a good idea (`dumbidea` branch). Your commit history will look something like this:

![topic-branches-1](./git-branching.assets/topic-branches-1.png) 

Now, let’s say you decide you like the second solution to your issue best (`iss91v2`); and you showed the `dumbidea` branch to your coworkers, and it turns out to be genius. You can throw away the original `iss91` branch (losing commits `C5` and `C6`) and merge in the other two. Your history then looks like this:

![topic-branches-2](./git-branching.assets/topic-branches-2.png) 

It’s important to remember when you’re doing all this that these branches are completely local. When you’re branching and merging, everything is being done only in your Git repository — there is no communication with the server.

https://www.gitkraken.com/learn/git/best-practices/git-branch-strategy

[GitHub flow](https://docs.github.com/en/get-started/using-github/github-flow)

The diagram below shows vertical view of GitHub flow.

<img src="./git-branching.assets/github-flow.png" alt="github-flow" style="zoom: 50%;" /> 

>The GitHub flow branching strategy is a relatively simple workflow that allows smaller teams, or web applications/products that don’t require supporting multiple versions, to expedite their work.
>
>In [GitHub flow](https://githubflow.github.io/), the main branch contains your production-ready code.
>
>The other branches, feature branches, should contain work on new features and bug fixes and will be merged back into the main branch when the work is finished and properly reviewed.

The diagram below shows horizontal view of GitHub flow.

![github-flow-hotfix](./git-branching.assets/github-flow-hotfix.png) 

![github-flow-release](./git-branching.assets/github-flow-release.png) 

Credit: [Gitflow workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)

[GitLab Flow](https://about.gitlab.com/topics/version-control/what-is-gitlab-flow/)

<img src="./git-branching.assets/gitlab-flow.png" alt="gitlab-flow" style="zoom:50%;" /> 

>At its core, the GitLab flow branching strategy is a clearly-defined workflow. While similar to the GitHub flow branch strategy, the main differentiator is the addition of environment branches—ie production and pre-production—or release branches, depending on the situation.
>
>Just as in the other two Git branch strategies, [GitLab flow](https://docs.gitlab.com/ee/topics/gitlab_flow.html) has a main branch that contains code that is ready to be deployed. However, this code is not the source of truth for releases.
>
>In GitLab flow, the feature branch contains work for new features and bug fixes which will be merged back into the main branch when they’re finished, reviewed, and approved.
>
>The GitLab flow branching strategy works with two different types of release cycles:
>
>1. Versioned Release: each release has an associated release branch that is based off the main branch. Bug fixes should be merged into the main branch first, before being cherry-picked into the release branch.
>2. Continuous Release: production branches are utilized to contain deployment-ready code, so code is merged into the production branch when it’s ready to be released.



---



### Remote Branches

Remote references are references (pointers) in your remote repositories, including branches, tags, and so on. You can get a full list of remote references explicitly with `git ls-remote <remote>`, or `git remote show <remote>` for remote branches as well as more information. Nevertheless, a more common way is to take advantage of remote-tracking branches.

Remote-tracking branches are references to the state of remote branches. They’re local references that you can’t move; Git moves them for you whenever you do any network communication, to make sure they accurately represent the state of the remote repository. Think of them as bookmarks, to remind you where the branches in your remote repositories were the last time you connected to them.

Remote-tracking branch names take the form `<remote>/<branch>`. For instance, if you wanted to see what the `main` branch on your `origin` remote looked like as of the last time you communicated with it, you would check the `origin/main` branch. If you were working on an issue with a partner and they pushed up an `iss53` branch, you might have your own local `iss53` branch, but the branch on the server would be represented by the remote-tracking branch `origin/iss53`.

This may be a bit confusing, so let’s look at an example. Let’s say you have a Git server on your network at `git.ourcompany.com`. If you clone from this, Git’s `clone` command automatically names it `origin` for you, pulls down all its data, creates a pointer to where its `main` branch is, and names it `origin/main` locally. Git also gives you your own local `main` branch starting at the same place as origin’s `main` branch, so you have something to work from.

> [!NOTE]
>
> “origin” is not special
>
> Just like the branch name “master” does not have any special meaning in Git, neither does “origin”. While “master” is the default name for a starting branch when you run `git init` which is the only reason it’s widely used, “origin” is the default name for a remote when you run `git clone`. If you run `git clone -o booyah` instead, then you will have `booyah/master` as your default remote branch.

Create a fork of https://github.com/sait-lab/git-demo-repo and clone your fork to your local machine.![create-fork-of-demo-repo](./git-branching.assets/create-fork-of-demo-repo.png) 

```
git clone git@github.com:YOUR_GITHUB_USERNAME/git-demo-repo ~/git-demo-remote
cd ~/git-demo-remote
```

![remote-branches-1-log](./git-branching.assets/remote-branches-1-log.png) 

![remote-branches-1](./git-branching.assets/remote-branches-1.png) 

If you do some work on your local `main` branch, and, in the meantime, someone else pushes to `github.com` and updates its `main` branch, then your histories move forward differently. Also, as long as you stay out of contact with your `origin` server, your `origin/main` pointer doesn’t move.

 ![remote-and-local-commits](./git-branching.assets/remote-and-local-commits.png)  

To synchronize your work with a given remote, you run a `git fetch <remote>` command (in our case, `git fetch origin`). This command looks up which server “origin” is (in this case, it’s `github.com`), fetches any data from it that you don’t yet have, and updates your local database, moving your `origin/main` pointer to its new, more up-to-date position.

![remote-and-local-commits-fork](./git-branching.assets/remote-and-local-commits-fork.png) 

![remote-branch-3-fetch](./git-branching.assets/remote-branch-3-fetch.png) 



#### Pushing

When you want to share a branch with the world, you need to push it up to a remote to which you have write access. Your local branches aren’t automatically synchronized to the remotes you write to — you have to explicitly push the branches you want to share. That way, you can use private branches for work you don’t want to share, and push up only the topic branches you want to collaborate on.

If you have a branch named `serverfix` that you want to work on with others, you can push it up the same way you pushed your first branch. Run `git push <remote> <branch>`:

```
$ git switch -c serverfix
Switched to a new branch 'serverfix'

$ git push origin serverfix
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 4 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (6/6), 509 bytes | 509.00 KiB/s, done.
Total 6 (delta 2), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (2/2), completed with 1 local object.
remote:
remote: Create a pull request for 'serverfix' on GitHub by visiting:
remote:      https://github.com/hongyanca/git-demo-repo/pull/new/serverfix
remote:
To github.com:hongyanca/git-demo-repo.git
 * [new branch]      serverfix -> serverfix
```

![push-serverfix-branch](./git-branching.assets/push-serverfix-branch.png) 

> The next time one of your collaborators fetches from the server, they will get a reference to where the server’s version of `serverfix` is under the remote branch `origin/serverfix`:
>
>```console
>$ git fetch origin
>```
>
> It’s important to note that when you do a fetch that brings down new remote-tracking branches, you don’t automatically have local, editable copies of them. In other words, in this case, you don’t have a new `serverfix` branch — you have only an `origin/serverfix` pointer that you can’t modify.
>
> To merge this work into your current working branch, you can run `git merge origin/serverfix`. If you want your own `serverfix` branch that you can work on, you can base it off your remote-tracking branch:
>
>```console
>$ git checkout -b serverfix origin/serverfix
>Branch serverfix set up to track remote branch serverfix from origin.
>Switched to a new branch 'serverfix'
>```
>
> This gives you a local branch that you can work on that starts where `origin/serverfix` is.

Let's simulate this situation by creating a new `clientfix` branch on GitHub's web interface.

![create-clientfix-branch-on-gh](./git-branching.assets/create-clientfix-branch-on-gh.png) 

```
$ git fetch origin
From github.com:hongyanca/git-demo-repo
 * [new branch]      clientfix  -> origin/clientfix

$ git checkout -b clientfix origin/clientfix
branch 'clientfix' set up to track 'origin/clientfix'.
Switched to a new branch 'clientfix'
```

![fetch-origin-new-local-branch](./git-branching.assets/fetch-origin-new-local-branch.png) 



#### Tracking Branches

Checking out a local branch from a remote-tracking branch automatically creates what is called a “tracking branch” (and the branch it tracks is called an “upstream branch”). Tracking branches are local branches that have a direct relationship to a remote branch. If you’re on a tracking branch and type `git pull`, Git automatically knows which server to fetch from and which branch to merge in.

When you clone a repository, it generally automatically creates a `main` branch that tracks `origin/main`. However, you can set up other tracking branches if you wish — ones that track branches on other remotes, or don’t track the `main` branch. The simple case is the example you just saw, running `git checkout -b <branch> <remote>/<branch>`. This is a common enough operation that Git provides the `--track` shorthand.

In fact, this is so common that there’s even a shortcut for that shortcut. If the branch name you’re trying to checkout (a) doesn’t exist and (b) exactly matches a name on only one remote, Git will create a tracking branch for you:

```console
$ git checkout clientfix
```

To set up a local branch with a different name than the remote branch, you can easily use the first version with a different local branch name:

```console
$ git checkout -b sf origin/serverfix
Branch sf set up to track remote branch serverfix from origin.
Switched to a new branch 'sf'
```

![checkout-b-own-branch](./git-branching.assets/checkout-b-own-branch.png) 

Now, your local branch `sf` will automatically pull from `origin/serverfix`.

If you already have a local branch and want to set it to a remote branch you just pulled down, or want to change the upstream branch you’re tracking, you can use the `-u` or `--set-upstream-to` option to `git branch` to explicitly set it at any time.

```console
$ git switch -c cf
$ git branch -u origin/clientfix
branch 'cf' set up to track 'origin/clientfix'.
```

![set-upstream](./git-branching.assets/set-upstream.png) 

If you want to see what tracking branches you have set up, you can use the `-vv` option to `git branch`. This will list out your local branches with more information including what each branch is tracking and if your local branch is ahead, behind or both.

```console
$ git branch -vv
* cf        d2d1e83 [origin/clientfix: ahead 2, behind 2] local update 2
  clientfix 11e02f8 [origin/clientfix] remote update 2
  main      d2d1e83 [origin/main: ahead 2, behind 2] local update 2
  serverfix d2d1e83 local update 2
  sf        d2d1e83 [origin/serverfix] local update 2
```

It’s important to note that these numbers are only since the last time you fetched from each server. This command does not reach out to the servers, it’s telling you about what it has cached from these servers locally. If you want totally up to date ahead and behind numbers, you’ll need to fetch from all your remotes right before running this. You could do that like this:

```console
$ git fetch --all; git branch -vv
```



#### Pulling

While the **`git fetch`** command will fetch all the changes on the server that you don’t have yet, it **will not modify your working directory at all**. It will simply get the data for you and let you merge it yourself. However, there is a command called `git pull` which is essentially a `git fetch` immediately followed by a `git merge` in most cases. If you have a tracking branch set up as demonstrated in the last section, either by explicitly setting it or by having it created for you by the `clone` or `checkout` commands, `git pull` will look up what server and branch your current branch is tracking, fetch from that server and then try to merge in that remote branch.

Generally it’s better to simply use the `fetch` and `merge` commands explicitly as the magic of `git pull` can often be confusing.



#### Deleting Remote Branches

Suppose you’re done with a remote branch — say you and your collaborators are finished with a feature and have merged it into your remote’s `master` branch (or whatever branch your stable codeline is in). You can delete a remote branch using the `--delete` option to `git push`. If you want to delete your `serverfix` branch from the server, you run the following:

```console
$ git push origin --delete serverfix
To github.com:hongyanca/git-demo-repo.git
 - [deleted]         serverfix
```

Basically all this does is to remove the pointer from the server. The Git server will generally keep the data there for a while until a garbage collection runs, so if it was accidentally deleted, it’s often easy to recover.



---



### Rebasing

