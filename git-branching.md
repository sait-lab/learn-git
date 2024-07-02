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
