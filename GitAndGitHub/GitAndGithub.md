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

- ### Git behind the scenes

**The config file**

> For every git repo, we can find a hidden .git folder, that's what identifies such directory as a git repo

In the .git folder, we can find the config file, which holds the settings that are applied to the repository (we can also find a config file with global scope elsewhere in our computer), in this local config file we can for example modify the colors that are used to display the names of the local branches in the repository.

In the .git folder we can also find the "refs" folder (stands for "references"), as the name already implies, here is where git stores the references for the local branches, the remotes and the tags. If we navigate to the "heads" folder of refs, we'll find a file for every local branch that we have in the repo (having the same name as the branch), inside every branch head file we'll simply find the commit hash of where that particular branch reference is pointing, the "tip" of the branch.

<div style="page-break-after: always;"></div>

- ### Git behind the scenes

**The HEAD file in the .git directory**

This file contains the reference of where, I, as the user, am placed in the repository. If we are placed inside a certain branch as it is usual, this HEAD file will contain a reference to a branch.

In this case, we are placed normally in a local branch of the repository. If we consult the contents of the HEAD file:

```console
>> cat .git/HEAD
```

**Output**

```console
>> ref: refs/heads/master
```

If we have checked out to a particular commit, **it means we are in detached HEAD state** (where HEAD points to a singular commit rather than a branch reference). Then by consulting the contents of HEAD we can appreciate the difference:

```console
>> cat .git/HEAD
```

**Output**

```console
>> 3739879d657f9680cba96ba36f508839465873e3
```

<div style="page-break-after: always;"></div>

- ### Git behind the scenes

**The objects folder**

Inside the .git folder, we can find the "objects" folder, which contains **4** types of git objects: **commits**, **trees**, **blobs** and **annotated tags**. The contents in this "objects" folder are encrypted and compressed, so what's inside is not human-readable.

**Hashing functions**

Hashing functions are functions that map input data of some arbitrary size to fixed-size output values.

As a subset of hashing functions, we can find cryptographic hash functions, which have the following qualities:

1. One-way function which is infeasible to invert.

2. Small change in input yields large change in the output.

3. Deterministic - same input yields same output.

4. Unlikely to find two outputs with the same value.

At the moment of writing this, git uses a cryptographic hashing function called SHA-1 (this could change in the future). SHA-1 always generates 40-digit hexadecimal numbers, the commit hashes we see all the time in git are the output of this SHA-1 function.

**Git as a key-value datastore**

Git is a **key-value** data store. We can insert any kind of content into a git repository, and git will hand us back a unique key we can later use to retrieve that content. These keys that we get back are SHA-1 checksums.

**git hash-object \<file-name\> -w** &rarr; This command takes the data we pass it, it stores that data in our .git/objects directory and gives us back the unique SHA-1 hash that refers to that data object. The **-w** flag stands for "write", if we do not provide this flag, git will simply return the corresponding SHA-1 hash according to the input data, without actually storing the information in the objects folder.

**git cat-file -p \<object-hash\>** &rarr; After we have stored data in our git object database using **git hash-object -w**, we can retrieve that data by its object hash (like the key in a dictionary). The **-p** flag stands for "pretty", it tells git to pretty print the contents of the object based on its type.

**Blobs**: Git blobs (binary large objects) are the object type Git uses to store the contents of files in a given repository. Blobs don't even include the filenames of each file or any other data. They just store the contents of a file.

**Trees**: Trees are git objects used to store the contents of a directory. Each tree contains pointers that can refer to blobs and to other trees. Each entry in a tree contains the SHA-1 hash of a blob or tree, as well as the mode, type, and filename.

<div style="page-break-after: always;"></div>

- ### Git behind the scenes

**Viewing a tree object in git**

We can use the same **git cat-file -p** command to view the tree object that is pointed to by the tip of a certain branch, with the following syntax:

```console
>> git cat-file -p master^{tree}
```

**Output**

```console
100644 blob 93d3f9d923e2e296fdaabf45ce1b9f491c15a9d1    .gitignore
040000 tree 08e279c09d02a194b35b83a532a919a207901468    ASPNETCoreMiscellaneous
040000 tree b5867aad455c3f2972d88356cbb0fff4f0221289    Angular
040000 tree bd982ed6a33bf57bf5586be478ea27e6fefde92f    Examples
040000 tree 11c505e5f13e9f8996bb8ed6d6bb6c076f07c6a0    GitAndGitHub
040000 tree 34bad7ad5e6d6f0b057351a2684112b4ec115f79    TypeScript
```

We can see every reference entry in the tree, having the access mode, the type of reference: **blob (contents of a file)** or **tree (a nested folder representation)**, and the name of the reference (name of file when it's a blob reference or name of subfolder when it's a tree reference). This is the way a blob object having the contents of a certain file is related to its corresponding file name in the tree, and how the different subfolders are represented for every commit of every branch in the repository, thinking of a commit as a "snapshot", a different tree structure, where we may find more blob objects or nested trees compared to the prior commit.

**Commits**: Git commit objects combine a tree object along with information about the context that led to the current tree. Commits store a reference to a parent commit(s), the author, the commiter, and of course the commit message.

<p align="center">
  <img width="40%" height="40%" src="./img/CommitObject.png" />
</p>

<div style="page-break-after: always;"></div>

- ### Git behind the scenes

**Viewing a commit object in git**

Once again, using the **git cat-file -p**, we can visualize a commit object in git. And also by switching the **-p** flag for **-t** (which stands for "type"), we can see the type of git object which reference we are providing to the command:

```console
>> git cat-file -t 6f2b566eb5337571a5dd8f7b7e353381bf62c7f7 (a commit hash)
```
**Output**

```console
commit
```

Now visualizing the git commit object:

```console
>> git cat-file -p 6f2b566eb5337571a5dd8f7b7e353381bf62c7f7 (a commit hash)
```

**Output**

```console
tree fdc08f2765a174774c27742406e8df20a68a93f6
parent ba611aa3fe628697cf38fbcf5555450955fd5c9c
author Aldo Dominguez <aldodominguezchz@gmail.com> 1650927219 -0500
committer Aldo Dominguez <aldodominguezchz@gmail.com> 1650927219 -0500
```

As we can see, the commit object has its tree reference (the snapshot for the branch for that commit), the hash of its parent commit, the author and the commiter. 

**In summary, a commit object stores the hash corresponding to a tree, that tree for the commit stores the hash references to other trees (nested folders in the repository) and the hash references to blob objects, which are the final node, having only the contents of a file, the name for that file is stored in the tree itself preceeding the blob**.

<p align="center">
  <img width="25%" height="25%" src="./img/GitTreeModel.png" />
</p>

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

- ### **git diff**

List the differences between two commits referring to said commits using their related tag if exists:

```console
>> git diff <tag-for-first-commit> <tag-for-second-commit> (git diff v16.14.0 v17.0.0 as an example)
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

> As a note, a tag is always pointing to a particular commit, it's tied only to that single commit. A tag is simply a "sticky note" in a commit, most likely used to mark every commit as a release (major, minor and patch release, according to the semantic versioning convention)

There are two types of git tags: lightweight and annotated tags.

**Lightweight tags:** They are...lightweight. They are just a name/label that points to a particular commit.

**Annotated tags:** They store some additional meta data, including the author's name and email, the date, and a tagging message (like a commit message).

Show the list of all tags present for the current repository without any additional information:

```console
>> git tag
```

List all tags that match the provided wildcard pattern. For example, we can provide **\"\*beta\*\"** to get all the tags containing the substring \"beta\" at any point. The **'l'** flag stands for "list", it's a bit redundant, but we do have to use it if we want to provide a matching pattern:

```console
>> git tag -l <matching-pattern> ("*beta*" for example, to get all tags that contain the substring "beta" at any point)
```

Create a **lightweight** tag referring to the commit that HEAD is referencing:

```console
>> git tag <tagname>
```

Create an **annotated** tag referring to the commit that HEAD is referencing. We'll be prompted to provide the tag message in the default text editor, similar to the commit message when we don't provide the message as a parameter to the command. The **-a** flag stands for "annotated":

```console
>> git tag -a <tagname>
```

Create an **annotated** tag referring to the commit that HEAD is referencing. We can provide the tag message directly to the command, similar to a commit message, using the **\'-m\'** flag (which stands for "message"):

```console
>> git tag -a <tagname> -m <tag-message>
```

<div style="page-break-after: always;"></div>

- ### **git tag**

Create a tag for a previous commit, **it can be either lightweight or annotated**. We just have to target that commit using the commit hash:

```console
>> git tag -a <tagname> <commit-hash> -m <tag-message> (this example is for an annotated tag)
```

Delete a specific tag, **it can be either lightweight or annotated**:

```console
>> git tag -d <tagname>
```

**By default, git push does not transfer tags in our local repo to the remote server, we need to explicitly ask for this**. Push all tags in our local branch to the remote branch, by using the **--tags** option:

```console
>> git push <remote-name> <branch-name> --tags
```

View the details about a tag. If the tag is an annotated tag, it will show the tagger's info (username and email), tag creation date, tag message, and the info for the commit related to the tag:

```console
>> git show <tagname>
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

<div style="page-break-after: always;"></div>

## Git reflogs

Git reflogs (git reference logs) are records of when the tips of branches and other references (our HEAD, for example) are updated in the repository. To better illustrate this, let's provide an example, we have at our disposal the **git reflog** command, with the **show** subcommand, if we want to list the movements, that's to say, the updates for a reference, a branch for example, we can go:

```console
>> git reflog show master
```

With the following output for a certain repository:

```console
171d96c (HEAD -> master, origin/master, origin/HEAD) master@{0}: commit: add content on git aliases to the git guide
c934e12 master@{1}: commit: make correction in title in git guide
b8de8c6 master@{2}: commit: complete content about git in the background for git guide
bd99722 master@{3}: commit: add content about git behind the scenes to the git guide
8b0bc79 master@{4}: commit: complete section about tags in git guide
e66ae76 master@{5}: commit: add content on git tags to the git guide
3d30605 master@{6}: commit: add content on Typescript
7c21fe1 master@{7}: clone: from github.com:AldoDominguezCha/LearningNotesAllTopics.git
```

In this case, we have asked for the reference logs of the master branch, we can see how this branch reference gets updated every time we add a new command to it, since this branch reference must now point to that latest commit.

**We can sometimes use the git reflog entries to access commits that seem lost and are not appearing in git log anymore.**

<div style="page-break-after: always;"></div>

## Git reflogs

We have a more complex scenario, if we try to see the list of updates for the HEAD reference, since with the HEAD reference we can point to different branches and even specific commits (using a different sample repository from the previous example):

```console
>> git reflog show HEAD
```

Has an example output:

```console
6c8e3b6 (HEAD -> eagerLoading, origin/eagerLoading) HEAD@{0}: checkout: moving from master to eagerLoading
f011641 (origin/master, origin/HEAD, master) HEAD@{1}: checkout: moving from 3739879d657f9680cba96ba36f508839465873e3 to master
3739879 HEAD@{2}: checkout: moving from master to HEAD~1
f011641 (origin/master, origin/HEAD, master) HEAD@{3}: checkout: moving from 3739879d657f9680cba96ba36f508839465873e3 to master
3739879 HEAD@{4}: checkout: moving from master to HEAD~1
f011641 (origin/master, origin/HEAD, master) HEAD@{5}: checkout: moving from eagerLoading to master
6c8e3b6 (HEAD -> eagerLoading, origin/eagerLoading) HEAD@{6}: checkout: moving from master to eagerLoading
f011641 (origin/master, origin/HEAD, master) HEAD@{7}: checkout: moving from eagerLoading to master
6c8e3b6 (HEAD -> eagerLoading, origin/eagerLoading) HEAD@{8}: checkout: moving from master to eagerLoading
f011641 (origin/master, origin/HEAD, master) HEAD@{9}: clone: from https://github.com/AldoDominguezCha/SkinetECommerce.git
```

In this other scenario, we can see how the HEAD reference was updated to point to different branches and even specific commits (detached HEAD state).

Every entry in the reference logs has a timestamp associated with it, we can filter reflog entries by time/date by using time qualifiers. This could be useful if we wanted to travel back to a certain commit in a branch, knowing only the approximate time of creation for that commit, we could reference the corresponding reflog reference for that commit, instead of go looking for its exact hash, as follows:

```console
>> git checkout master@{2.days.ago}
```

In this case, the **master@{2.days.ago}** reflog entry referenced will be the one closest to that two weeks period. So it's like asking "where were you, the reference, pointing at, this amount of time ago?".

<div style="page-break-after: always;"></div>

## Git reflogs

**Undoing a rebase on a branch with git reflog**

Imagine we have squashed together several commits we did on a branch, but then we decide we'd actually like to have access to all the original single commits. If we consulted the reference logs for such a branch, we'd see something like this:

```console
485dc8b (HEAD -> flowers) flowers@{0}: rebase (finish): refs/heads/flowers onto 18fa4e84a22278cb88fe2a2eea699bd2464bd7e3
2d1720b flowers@{1}: rebase (finish): refs/heads/flowers onto cc423e61e5c81ea6c5702901e72f599483c4021b
da2ad69 flowers@{2}: commit: add celosia
aa18b2d flowers@{3}: commit: add amaranth to flowers
112b8a9 flowers@{4}: commit: add dahlias to flowers list
73a1229 flowers@{5}: commit: add some zinnias
cc423e6 flowers@{6}: commit: add flowers file
18fa4e8 (master) flowers@{7}: branch: Created from HEAD
```

We can see we created the branch, added some commits to it, and then we performed two different rebase operations on the branch, squashing some of the commits together. By consulting the list of commits for the branch (**git log --oneline**) we now have the following:

```console
485dc8b (HEAD -> flowers) add list of flower seeds
18fa4e8 (master) add summer seeds
46a5da8 add more greens
97f08b7 plant winter veggies
191e903 add veggies file
```

In that latest commit of the branch is where we have squashed together different individual commits, we'd like to get our separated original commits back in the branch's history. Then by looking in the reference logs for the branch, as we have done already, we can get "from the trash" sort of speak, the latest original commit before rebasing, it's the **flowers@{2}** git reference log entry for the branch, then we can apply a reset using that reference for the commit (we could also use the abbreviated hash we can also see in the reflog list, "da2ad69" specifically):

```console
git reset --hard flowers@{2} (the same as git reset --hard da2ad69, this would be using the commit's hash instead of its reflog reference) 
```

<div style="page-break-after: always;"></div>

## Git reflogs

**Undoing a rebase on a branch with git reflog**

Now, if we consulted again the list of commits for the branch (**git log --oneline**), we'd see that we have "saved" those individual commits we previously squashed together:

```console
da2ad69 (HEAD -> flowers) add celosia
aa18b2d add amaranth to flowers
112b8a9 add dahlias to flowers list
73a1229 add some zinnias
cc423e6 add flowers file
18fa4e8 (master) add summer seeds
46a5da8 add more greens
97f08b7 plant winter veggies
191e903 add veggies file
```

**We have effectively undone the rebase operation on the branch with the aid of the reference logs, by "retrieving" a "discarded" commit.**

<div style="page-break-after: always;"></div>

## Git aliases

> Git aliases are basically shortcuts, little custom commands we can define

We can create git aliases in two different ways: **Through the config files, wheter it is local or global, and in the command line**.

**Adding the alias directly in the config file** 

If we want to add the alias in the config file, we'll need to to do it in the "alias" section. As an example:

```console
[core]
	repositoryformatversion = 0
	filemode = false
	bare = false
	logallrefupdates = true
	symlinks = false
	ignorecase = true

[alias]
	lg = log --oneline
```

Given this **\"lg\"** alias we have added. If we run **git lg**, we get as the output:

```console
4f40a14 (HEAD -> master) add new dog to the list
6f2b566 add greeting
ba611aa initial commit, add dogs file
```

**Adding the alias through the command line**

This will add the alias in the repository scope only due to the **--local** flag:

```console
git config --local alias.remotebranches "branch --remote"
```

Given this **\"remotebranches\"** alias we have added. If we run **git remotebranches**, we get as the output:

```console
origin/HEAD -> origin/master
origin/master
```

Both ways of adding the alias are basically the same thing, appending this new definition under the "alias" section of the config file, so they are not exactly "different" methods, by declaring the alias through the command line we are just editing the config file all the same, so it just becomes a question of which method is more comfortable to use.

