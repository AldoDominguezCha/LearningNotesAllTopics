# Git and GitHub

> Git is the world's most popular VCS (Version Control System), a version control system is software that tracks and manage changes to a set of files over time. Linus Torvald is the creator of Git.

Version control systems such as Git generally allow:

- Revisiting earlier versions of the files

- Compare changes between versions (compare commits in the branch)

- Undo changes and a whole lot more

## What is Git?

Is the version control software that runs locally on your machine. You don't need to register for an account, you don't need internet to use it. You can use Git without ever touching GitHub. We can use the command line to interact with Git, as an additional option we can use graphical interfaces (GitKraken for example) that in the background just execute the same commands.

## What is GitHub?

Is a service that hosts Git repositories in the cloud and makes it easier to collaborate with other people. You do need to sign up for an account to use GitHub. It's an online place to share work that is done using Git.

## Some useful UNIX-based shell (like Git Bash) commands

- pwd : Print current directory. As the name implies, it prints the current directory or location in the folder system.

```console
>> pwd
```

- cd : Change directory. It allows us to change directories (to move in the folder system).

```console
>> cd ../skinet/API
```

- start : Open specified directory in the file explorer. The ".." are used to go back a level. Most of these commands receive a relative path such as in the following example (from the current directory we are at, go backwards one level, then enter "skinet", then enter "Core", and finally enter "Interfaces", that's the directory that will be shown in the file explorer):

```console
>> start ../skinet/Core/Interfaces
```

- touch : Create a new file (yes, it has a weird name). You can pass multiple paths to the command in a single execution (following example creates 3 files in the previous directory because of the ".."):

```console
>> touch ../appsettings.json ../BaseEntity.cs ../.gitignore
```

- mkdir : Make directory. As the name implies, it creates a single or multiple directories.

```console
>> mkdir ../API/wwwroot ../Core/Entities ../Infrastructure/Data
```

- rm : Remove. Delete files or folders, to delete folders we must provide the "-rf" (recursive and force) flag.

Deleting files:

```console
>> rm ../TestFile1.json ../TestFile2.json
```

Deleting folders (a single one in this case):

```console
>> rm -rf ./MyFolder1
```

<div style="page-break-after: always;"></div>

## Git concepts and usage

- ### Git repository

A git repository or "repo" is a workspace which files are being tracked and managed by Git. Every repository has its own history (series of commits).

NOTE: **Do not instantiate a new repo when you are already in an active repo** (check with "git status")

Git tracks the base folder that we initialized as a repo (where we executed "git init") and also all subsequent nested folders inside that base folder.

> "Dirty the working tree" &rarr; To add some modification, git notices there was a change in the tracked working directory.

- ### Git commits

We can think of a commit in Git as a "checkpoint". Once we have made changes in the set of files of the repository, we can then group some or all of them together into a commit. We could also say that a commit is a "version" or state of the set of files in the branch (timeline).

<p align="center">
    Work on stuff &rarr; Add changes to staging area (git add) &rarr; Commit (git commit)
</p>

**Write your commit messages in imperative mode ("make foo do frotz")**

Write commit messages as if you were giving orders to the code base to change its behavior, in present-tense imperative mode.

```console
>> git commit -m "inject and use product repository in products controller"
```

> Atomic: Something irreducible, a base unit.

When possible, a commit should encompass a single feature, change or fix. In other words, try to keep each commit focused on a single thing, **make atomic commits**.

It's a good idea to follow the pattern of having the first line of the commit message as a summary for all the changes introduced.

- ### .gitignore

We can tell Git which files and directories to ignore in a given repository, such as dependency and packages folders, secrets, API keys and so on (information that should not be public in case we want to host a copy of our repo in GitHub) using a .gitignore file. This .gitignore file is normally placed at the root of the repo.

<div style="page-break-after: always;"></div>

- ### Git branches

We can think of branches as alternative timelines for a project, alternative pathways or versions. They can diverge at a certain commit and represent "parallel realities".

They enable us to create separate contexts where we can try new things, or even work on multiple ideas in parallel.

> Whatever we do on one branch, will not impact other branches because they are different "timelines", unless we combine the branches, this is known in Git as the **merge** operation.

In Git we are always working on a branch, the default branch with wich we start a repo after **git init** is known as the "master" branch, this branch is not special, doesn't do anything unique, it's just the git naming convention for the beginning branch.

Usually, the master branch is designated as the "source of truth", the official working version of the project.

> "Feature branching" is when we create a new branch based off the offical codebase, and work on a new feature in this new "feature branch" alone, if we are successful implementing the feature, then we can merge the feature branch into the official branch. This way we will not break the functioning version of the project. **It's a terrible idea to work directly in the main branch for a large project**.

- ### Git HEAD

> **HEAD** represents a reference to a branch pointer, and a branch pointer is simply where a branch currently is, the last commit of the branch. **HEAD** can also be understood as the current branch.

- ### Git remote

Before we can push anything to Github, we need to tell Git about our remote repository on Github. We need to set up a "destination" to push the changes up to.

In Git, we refer to these "destinations" as remotes.

> Each remote is simply a URL where a hosted repository lives

- ### Remote Tracking Branches

We can think of them as "At the time you last communicated with this remote repository, here is where the x branch was pointing"

They follow the **\<remote\>/\<branch\>** pattern. Examples:

* **origin/master** references the state of the master branch on the remote repo named origin

* **upstream/logoRedesign** references the state of the logoRedesign branch on the remote repo named upstream (a common remote name)

<div style="page-break-after: always;"></div>

- ### Git repository visibility

> **Public**: Public repos are accessible to everyone in the internet. Anyone can acces, view and clone a public repository. **It doesn't mean anyone is able to modify the contents of your Github repo**. If someone wanted to push changes into a Github repo you own, you need to grant them collaborator permissions first.

> **Private**: Private repos are not discoverable by default, they can only be viewed and accessed by the owner and the people that have been granted access.

- ### Git README files

README files are used to communicate important information about a repository, including:

- What the project does
- How to run the project
- Why it's noteworthy (the perks and innovations of that package, framework, etc)
- Who maintains the project

READMEs are markdown files (**they have an \'.md\' extension**). Markdown is a lightweight markup language that provides a simple and convenient syntax to generate formatted text. It's a text-to-HTML conversion tool.

Github will actually look in the root of the repository for the **README.md** file, and it will render it in the main page of the respository. It's always a good idea adding a README file to every public repo, since recruiters and other people will be able to obtain a summary of our projects, giving it a professional look.

- ### Git gists

Github gists are a simple way to share code snippets and useful fragments with others.


<div style="page-break-after: always;"></div>

## Git commands

- ### **git init**

Initializes a new git repo, it created a new .git hidden folder in the location where the command is executed, which includes the files and metadata Git needs to work and track changes.

```console
>> git init
```

Output:

```console
Initialized empty Git repository in C:/Users/6112638/ECommerceAppWith.NETCoreAndAngular/StudentAssets/.git/
```
- ### **git status**

Displays the state of the repository and staging area.

```console
>> git status
```

Output:

```console
On branch errorHandling
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   API/Controllers/ErrorController.cs
        modified:   API/Startup.cs

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        API/Errors/ApiException.cs
        API/Middleware/

no changes added to commit (use "git add" and/or "git commit -a")
```

<div style="page-break-after: always;"></div>

- ### **git add**

Adds files to the staging area, where they are ready to be commited.

Add specific files by relative path to the staging area:

```console
>> git add ./API/Startup.cs ./Core/Interfaces/IGenericRespository.cs
```

Add all files, both tracked and untracked (newly added files Git is just finding about) to the staging area:

```console
>> git add .
```

<div style="page-break-after: always;"></div>

- ### **git commit**

Commit the changes present in the staging area.

Commit changes without providing the commit message at the same time, the default text editor will be opened, prompting us to provide the commit message there:

```console
>> git commit
```

Commit changes providing the commit message with the "-m" flag:

```console
>> git commit -m "add exception handling middleware to the pipeline"
```

Staging all available changes and then comitting said changes, like using **git add** and **git commit** all at once, it doesn't stage untracked files:

```console
>> git commit -am "add endpoints to retrieve product types and product brands"
```

Amend **last** commit, you can stage additional files before running the command to include them in the commit, modify the commit message, or both:

```console
>> git commit --amend "add exception handling middleware to the pipeline to manage null reference exception"
```

Commit changes copying the authorship information (user and email) and commit message from a different commit, we need to provide the commit hash as a parameter after the "-C" flag:

```console
>> git commit -a -C e85a9ff
```

<div style="page-break-after: always;"></div>

- ### **git log**

List all commits in the branch, with their complete commit hash, message, authorship information and timestamp:

```console
>> git log
```

List all commits, but only the abbreviated commit hash and the first line of the commit message, it doesn't matter how long this message is:

```console
>> git log --oneline
```

<div style="page-break-after: always;"></div>

- ### **git branch**

List all available branches in the git repo. In the output, the '*' signals the one we are currently at:

```console
>> git branch
```

List all available remote branches (all branches in all remotes registered for the local repo, we must remember that a "remote" is a destination, a hosting URL, both a target and a source):

```console
>> git branch --remote
```

Create a new branch based on the current HEAD reference. **Do not use spaces in the branch name**:

```console
>> git branch <branch-name>
```

Delete a branch that has been merged. We won't be able to complete the operation if the branch hasn't been merged:

```console
>> git branch --delete <branch-name>
```

Force delete a branch irrespective of its merge status. **Be careful with this one**:

```console
>> git branch --delete --force <branch-name>
```

Rename the branch we are currently at, the "-m" flag stands for "move":

```console
>> git branch -m <new-branch-name>
```

<div style="page-break-after: always;"></div>

- ### **git merge**

Merge the changes from the branch which name we are providing, into the current branch. **The receiving or target branch is always the one we are placed at**:

```console
>> git merge <new-branch-name>
```

We have 3 types of merges

- **Fast-forward merge**: We are just updating the HEAD reference of the original branch to "catch up" with the new one. The new or alternative branch hasn't diverged from the original branch, the original branch is only some commits behind this alternative or new branch, in this case, by merging the new branch, we are, in reality, just updating the HEAD of the original branch to point to the latest commit of the new or alternative branch, hence the name.

- **Merge with commit**: The new or alternative branch has diverged from the original one, this means, we added new commits to the original branch after we created the alternative branch (where we most likely added new commits as well), but there are no conflicts to solve in any of the files between the two branches, so we'll just need to provide a merge commit message in the default text editor.

- **Merge with conflicts**: The new or alternative branch has diverged from the original one, this means, we added new commits to the original branch after we created the alternative branch (where we most likely added new commits as well), and we have changes targetting the same line in one or multiple files between the branches (conflicts), we'll need to manually solve the conflicts, stage the files where we've solved a conflict and finally create a new commit to complete the merge.

Resolving merge conflicts

- Open the files with the merge conflicts.
- Edit the files to remove the conflicts. Decide which branch's content you want to keep in each conflict, or keep the content from both if needed.
- Remove the conflict markers in the document.
- Add your changes and then make a commit to complete the merge.

<div style="page-break-after: always;"></div>

- ### **git diff**

Show all unstaged changes only (from currently tracked files only), we can think of it as "what we could be adding into the staging area":

```console
>> git diff
```

Show all staged changes only, both the "--staged" and the "--cached" options work for this purpose, they are synonyms:

```console
>> git diff --staged
```

```console
>> git diff --cached
```

Show all staged and unstaged changes (from currently tracked files only):

```console
>> git diff HEAD
```

Show the changes (unstaged, staged or both according to the option provided) for a specific file or folder:

```console
>> git diff --staged  <filepath/folderPath>
```

List the differences between commits using the HEAD synonym, the following example lists the difference between the third commit in the list (ordering from newest to oldest) and the most recent one, assuming that the HEAD reference is currently pointing at the latest commit of the branch. We can sort of translate it to "the difference between the parent of the parent of HEAD and HEAD". **Order of the references matters in git diff**:

```console
>> git diff HEAD~2 HEAD
```

List the differences between the latest commits of two different branches. The two dots between the branch names are just a convention, they are not necessary. **Order of the references matters in git diff**:

```console
>> git diff <branchName1>..<branchName2>
```

<div style="page-break-after: always;"></div>

- ### **git stash**

Take all uncommited changes (unstaged and staged) and stash them away in the stash stack, reverting said chages in the working directory, kind of "putting them away in a pouch", to be applied later, at a different branch even if we want to:

```console
>> git stash save
```

Remove the most recently stashed changes (top of the stash stack, "last in, first out") and reapply them to the working directory, in the current branch. **Remember that you can stash changes from a certain branch, switch to a different branch and then apply the changes there, may be helpful if you made some work in an unintended branch by accident**:

```console
>> git stash pop
```

List the entries present in the stash stack (all stashed changes, this is global for all branches):

```console
>> git stash list
```

Show the changes for a specific entry in the stash stack. Where **n** is the index in the stash:

```console
>> git stash show -p stash@{n}
```

Apply the changes for a specific entry in the stash stack. **Remember that you can stash changes from a certain branch, switch to a different branch and then apply the changes there, may be helpful if you made some work in an unintended branch by accident**:

```console
>> git stash apply stash@{n}
```

Drop the entire stash stack. **Remove all entries from the stack, make sure to apply the desired stashed changes before doing this**:

```console
>> git stash drop
```

<div style="page-break-after: always;"></div>

- ### **git stash**

Drop a specific entry in the stash stack. Where **n** is the index in the stash:

```console
>> git stash drop stash@{n}
```

<div style="page-break-after: always;"></div>

- ### **git switch**

Switch to the specified branch:

```console
>> git switch <branch-name>
```

Create a new branch with the specified name and then switch to that new branch. The "-c" flag stands for "create":

```console
>> git switch -c <branch-name>
```

Create a new local branch (if not present in your local repository already) from the remote branch of the same name. **Basically "grabbing" a copy of a branch in the remote that we don't have local access to yet, of course this new local copy of the branch gets paired with its remote equivalent**:

```console
>> git switch <remote-branch-name>
```

<div style="page-break-after: always;"></div>

- ### **git checkout**

> Just as a reminder, **git checkout** is kind of a legacy git command, it does too much, a wide variety of operations and this can be confusing to grasp, that's why **git switch** was added.

Switch to the specified branch:

```console
>> git checkout <branch-name>
```

Create a new branch with the specified name and then switch to that new branch. The "-b" flag stands for "branch":

```console
>> git checkout -b <branch-name>
```

> Usually HEAD points to a specific branch reference, rather than a particular commit, and the branch reference itself points to the tip of the branch, the most recent commit on the branch.

When we make a new commit, the branch reference is updated to reflect the new commit, to move the branch reference to the this new latest commit, the new tip of the branch. However, HEAD stays the same, cause it's pointing at the branch reference itself, HEAD is still just pointing at that branch.

**This is all to say that HEAD usually refers to a branch, not a specific commit**

Change HEAD to point to a particular commit rather than at the branch reference. **We are not officially on a branch, we are floating around in a detached HEAD state**. Kind of "revisiting the past" to examine the contents of the old commit:

```console
>> git checkout <commit-hash>
```

**Detached HEAD is not a bad thing despite the name**

In detached HEAD state we have the following options:

- Stay in detached HEAD to examine the contents of the old commit. Poke around, view files, etc.

- Reattach the HEAD, which is to change HEAD to point again to a branch reference as it is usual, by changing back to any branch with **git switch <branch-name>** for example.

- Branch off from that old commit by creating a new branch and switching to it. You can now add and commit changes because we've reattached HEAD by making it point to this new branch reference.

<div style="page-break-after: always;"></div>

- ### **git checkout**

Create a new local branch (if not present in your local repository already) from the remote branch of the same name. **Basically "grabbing" a copy of a branch in the remote that we don't have local access to yet, of course this new local copy of the branch gets paired with its remote equivalent**. This does exactly the same as "git switch \<remote-branch-name\>", it's just the "older" way, remebering that **get checkout** used to do so many things:

```console
>> git checkout --track <remote-branch-name>
```

Change HEAD to point to the tip of the remote tracking branch, entering detached HEAD state, this is most useful after using **git fetch \<remote-name\>**, to take a peek into the latest changes integrated in that remote branch, remembering that **git fetch** will get the latest information about the remote, without actually pulling those changes into our working local copy of the repo, so we are "taking a peek" into the most recent state of the remote branch:

```console
>> git checkout <remote-name>/<remote-branch-name>
```

Check out to an old commit in the current branch using HEAD as a reference. The next example checks out to the commit that is 2 places prior to HEAD, which at this point must be pointing to the branch reference, the last commit of the current branch (grand parent of the last commit, two commits before this last commit):

```console
>> git checkout HEAD~2
```

If you are in detached HEAD state (HEAD is pointing to a particular commit rather than a branch reference), reattach HEAD to the last branch reference before HEAD was detached (the same as switching back to the last branch you were in before detaching):

```console
>> git switch -
```

Chekout to a specific commit in the branch using the tag for that commit as an alias, should the target commit have a tag. Doing this will take us to detached HEAD state, since we are making HEAD point to a particular commit instead of a branch reference as it is the normal state:

```console
>> git checkout <tag-for-commit>
```

<div style="page-break-after: always;"></div>

- ### **git checkout**

**Discard both staged and unstaged changes from one or multiple files**. We can think of it as checking out to the HEAD reference, but at that moment HEAD will be pointing to the branch refrence, which in turn will be poiting to the tip of the branch, the last commit in the branch, it's like **take me back to the last commit, and those unstaged and staged changes that get discarded are not part of that last commit**. As a note, the HEAD will bot be detached since we are checking out to HEAD itself, which is poitnting at the branch reference, not to a particular commit:

```console
>> git checkout HEAD <file1> <file2> <file3>
```

It's the exact same thing, but we can use **--** instead of **HEAD**:

```console
>> git checkout -- <file1> <file2> <file3>
```

Discard **both staged and unstaged changes** from all tracked files, **it doesn't remove untracked files**:

```console
>> git checkout HEAD .
```

Again, we can replace **HEAD** with **--**:

```console
>> git checkout -- .
```

<div style="page-break-after: always;"></div>

- ### **git restore**

Discard **only unstaged** changes from one or multiple files:

```console
>> git restore <file1> <file2> <file3>
```

Discard **only unstaged** changes from all files, **it doesn't remove untracked files**:

```console
>> git restore .
```

Restore the contents of a file to its state from the specified commit (we can use the **HEAD~n** syntax or provide the commit hash):

```console
>> git restore --source HEAD~1 <file>
```

Unstage one or multiple files:

```console
>> git restore --staged <file1> <file2>
```

<div style="page-break-after: always;"></div>

- ### **git reset**

Reset the branch back to a specific commit, **the subsequent commits are gone, but the changes belonging to those discarded commits are kept as unstaged changes**, in case you want to keep something from the removed commits, you can also use this to unstage all currently staged changes:

```console
>> git reset HEAD~<n>
```

You can use the abbreviated commit hash instead of the HEAD n parent syntax:

```console
>> git reset <commit-hash>
```

Reset the branch back to a specific commit, **also discarding the changes associated to those commits**:

```console
>> git reset --hard HEAD~<n>
```

You can use the abbreviated commit hash instead of the HEAD n parent syntax:

```console
>> git reset --hard <commit-hash>
```

<div style="page-break-after: always;"></div>

- ### **git revert**

Create a new commit which undoes the changes starting from a specified commit, **we use this as opposed to git reset to keep a full history of the changes, which is extremely important when collaborating**:

```console
>> git revert HEAD~<n>
```

You can use the abbreviated commit hash instead of the HEAD n parent syntax:

```console
>> git revert <commit-hash>
```

<div style="page-break-after: always;"></div>

- ### **git clone**

Clone (get a local copy of) a remote repository hosted on Github or similar hosting service. Git will retrieve all files associated with the repository and will copy them to your local machine, Git also initializes a new repository on your machine, giving you access to the full Git history of the cloned project. **Make sure you are not already inside a git repo before cloning**. This command is not tied to Github exclusively, it will clone a repo hosted in GitLab or any other hosting service just fine.

As an additional note, when cloning a remote repository, the remote added to the local copy is named as **\"origin\"**:

```console
>> git clone <url>
```

<div style="page-break-after: always;"></div>

- ### **git remote**

> Remembering that a remote is a "destination" where we want to push our code to. Each remote is simply a URL where the hosted repository lives, each remote also has a name

List any existing remotes for your repository, including the URL for the remote (the URL where the hosted repo lives). The "-v" flag stands for "verbose":

```console
>> git remote -v
```

A remote is really two things: a URL (where the hosted repo is located) and a label

Add new remote to our local repository:

```console
>> git remote add <remote-name> <url>
```

Rename an existing remote:

```console
>> git remote rename <old-name> <new-name>
```

Remove an existing remote:

```console
>> git remote remove <remote-name>
```

<div style="page-break-after: always;"></div>

- ### **git push**

Push the specified local branch up to the specified remote, to its corresponding branch in the remote, **it will create a new corresponding branch in the remote if not present already**: 

```console
>> git push <remote> <local-branch>
```

Push the specified local branch up to the specified remote, but to the specified branch in the remote, as opposed to its corresponding one. This can be used to completely overwrite an old feature branch with the latest contents of the main branch, just to provide an example:

```console
>> git push <remote> <local-branch>:<remote-branch>
```

Push the specified local branch up to the specified remote, to its corresponding branch in the remote, but also setting that remote branch as the upstream (default push to ranch in the remote) for the local branch, so in the future we can simply do **git push**, and by having configured this upstream for the local branch, git will know we actually want to push to the remote branch inside the remote we specified earlier. The "-u" flag stands for "upstream" (something like a default target and source):

```console
>> git push -u <remote> <local-branch>:<remote-branch>
```

<div style="page-break-after: always;"></div>

- ### **git fetch**

> It can be summarized as, go fetch the latest information on the state of the remote repository (we can fetch the latest state of the whole remote or only one branch of said remote), but **DON'T** actually pull those changes into my local copy. Just go see if any additional commits or branches have been added in the remote history that we don't know of yet, most likely by a collaborator. **This will only update the known state of the remote and its branches, fetching the latest version of its history (set of commits and available branches), it will not touch our local repo** 

Get the latest information about a remote repository (**new commits for the branches and new branches that may have been added**), without actually integrating the changes into your working directory. **It only updates remote tracking branches, it will not affect our actual local branches**. For example, this command will update the history in a remote tracking branch so that we know that our local branch is some commits behind its corresponding remote branch, possibly due to the work of a collaborator:

```console
>> git fetch <remote>
```

Get the latest information about a specific branch in the remote repository (**new commits for said remote branch that may have been added by a collaborator**), without actually integrating the changes into your local copy of the branch:

```console
>> git fetch <remote> <remote-branch-name>
```

<div style="page-break-after: always;"></div>

- ### **git pull**

> It's not recommended to execute **git pull** having uncommited work. It's a better idea to work on our changes, commit them, then pulling all that's available in the remote branch that may not be on our local corresponding branch (some collaborator's work), solving the merge conflicts if any, adding the merge commit and then finally push the branch to the remote. So basically, work on your stuff, commit those changes when ready, pull your teammates changes if any for the branch and solve any possible merge conflict, then push this final version of the branch  

Pull the latest state of the remote branch and **merge** it into the current branch, unlike **git fetch**, this one will actually alter our local branch, since it will integrate the latest commits from the remote branch into the branch we are currently at. If there's any conflict in the files, we'll just have to solve it the same way when there's a conflict merging local branches. **REMEMBER : The target branch for the merge is the one we are currently at, be careful with that**:

```console
>> git pull <remote> <remote-branch>
```

<div style="page-break-after: always;"></div>

- ### **git rebase**

To combine two branches together, as an alternative to merging, we can "rebase" the original branch, this moves the entire feature branch so that it begins at the tip of the target branch, **WE'LL REBASE THE BRANCH WHICH NAME WE PROVIDE IN THE COMMAND, ADDING AT THE TIP THE COMMITS OF THE BRANCH WE ARE CURRENTLY AT**, so we basically want to be placed in our feature branch where we've added the changes.

Instead of using a merge commit, rebasing rewrites history by creating new commits for each of the original feature branch commits. This command will ignore the merge commits we have in our feature branch caused by trying to keep up with the updates of the master branch, for example, and will only recreate at the end of the target branch the commits of the feature branch in which we actually introduced changes.

**WARNING**: never rebase with commits that have been shared with others, you only want to rebase with commits that you have in your local repo and other people don't, it's safe to do with the commits in your local feature branches for example. **THIS BASICALLY MEANS, DO NOT ALTER OTHER PEOPLE'S PAST HISTORY, DO NOT RECREATE THE COMMITS IN THE MAIN BRANCH THAT EVERYBODY HAS**:

```console
>> git switch <feature-branch>
>> git rebase <target-branch> (most likely the main branch)
```

Clean up the history with interactive rebase, we pick a certain commit in the branch from which we want to remake the commits that follow after by applying an operation to one or more commits in the range we have specified. We can rename commits, merge commits together (keep changes for all commits but combining said changes into a single commit, reducing the length of the history in the branch) and even **drop commits, thus discarding the changes related with that commit, so be careful with that option**:

**HEAD~(n)** &rarr; From which commit we are going to rebase our own branch using the commits encompassed in that same range, something like "reconstruct starting from this commit"

```console
>> git rebase -i HEAD~(n)
```

<div style="page-break-after: always;"></div>

- ### **git tag**

> As a note, a tag is always pointing to a particular commit, it's tied only to that single commit

Show the list of all tags present for the current repository without any additional information:

```console
>> git tag
```

List all tags that match the provided wildcard pattern. For example, we can provide **\"\*beta\*\"** to get all the tags containing the substring \"beta\" at any point. The **'l'** flag stands for "list", it's a bit redundant, but we do have to use it if we want to provide a matching pattern:

```console
>> git tag -l <matching-pattern> ("*beta*" for example, to get all tags that contain the substring "beta" at any point)
```

<div style="page-break-after: always;"></div>

## Git collaboration workflows

- Centralized workflow: Everybody is working in the exact same branch (master, main, whatever name it may have). **This is a terrible idea for complex projects and fairly large teams**

  - No one can work on anything without disturbing the main codebase. How do you experiment, adding risky commits if you are working on the official branch directly, which is the only one available?
  - The only way to collaborate on a feature together with a teammate is to push incomplete code to the master or main branch. Everybody else working on the project now has broken code...
  - Lots of time spent resolving conflicts and merging code, especially as team size scales up

- Using feature branches. **This one is the way to go for a serious project, NOBODY WORKS DIRECTLY IN THE MAIN BRANCH, THAT'S THE SOURCE OF TRUTH**

  - Treat the main branch as the official project history
  - Multiple teammates can collaborate on a single feature and share code back and forth without polluting the main branch of the project
  - The main branch will not contain broken code unless someone messes up
  - A feature branch is only merged into the main branch after a code review process and after it has passed integration tests, this means all the work in the feature branch is completed and the changes are functional



### Working with feature branches

**Before creating a new feature branch off of the main branch, we always have to do git pull \<remote-name\> \<remote-main-branch-name\> to make sure we have the latest version of the main branch**

> Pull Request: For short known as \"PR\", is a feature built into remote repository hosting services like Github and Azure. **It's not native to git itself**. A PR allows developers to alert team-members about new work that needs to be reviewed. It provides a mechanism to approve or reject the work on a given branch, facilitating discussion and feedback on the specified commits.

A PR is basically "I have this new stuff I want to merge into the master branch...what do you all think about it?"

The workflow to work using a PR is:

1. From the main branch, **having pulled the latest version**, create your feature branch
2. Add your changes in your local feature branch
3. Push the feature branch to Github
4. Open a pull request using the feature branch you pushed to Github
5. Wait for the PR to be approved and merged, pay attention to the discussion related to the changes

<div style="page-break-after: always;"></div>

### The fork and clone workflow

Github and other repository hosting services allow us to create personal copies of other people's public repositories or even a private one provided we have access to it. We call those copies a "fork" of the original.

When we fork a repo, we're basically asking Github "make me my own copy of this repo, please". As with pull requests, forking is not a git feature, the ability to fork is implemented by the hosting service, e.g., Github. When getting a fork of someone else's repo, the history of the original repo is kept, we can see the entire list of commits.

The "fork and clone" workflow is most used in open-source projects with hundreds or even thousands of contributors. In this scenario, we don't have direct contributor status for the repository, a basic overview of this workflow is as follows:

1. We create a fork (our own copy to which we can do anything we want) of the original repo to which we want to contribute but we can't do so directly

2. We clone **OUR FORK** of the project, we'll be pushing our changes to our forked copy, since we can't do that to the original repo where we want to contribute

3. We add a remote for the original repo from which we created the fork, we do this to use **git pull** and obtain the latest changes for the original project easily, **SINCE OUR FORKED COPY WILL NOT BE AUTOMATICALLY UPDATED WITH THE LATESTS COMMITS OF THE ORIGINAL REPO**, our forked repo and the original repo are not in sync, after the fork operation they are completely separated

4. We create a feature branch in our local repository, the one we obtained by cloning our fork of the project **and we pull the latest changes FROM THE ORIGINAL REPOSITORY**

5. After we have added an tested our changes in the feature branch of the local repo, we push the changes to our fork

6. We create a pull request from the feature branch of our fork, targeting the main branch of the project in the original repo

