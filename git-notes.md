# GIT REPOSITORY WORKFLOW

## setting up the repository
We'll first open the terminal and navigate to the
desired parent directory. Then, run each of the following commands:

```bash
mkdir hello
cd hello
touch hello.sh
```

This will create a new directory called hello and a new file called
hello.sh within it. We then add some content to the hello.sh file:

```bash
echo "Hello, World" > hello.sh
```

This will write the text "Hello, World" to the hello.sh file.
Next, we need to initialize a new Git repository in the hello directory. Run
the following command:

```bash
git init
```

This will create a new Git repository in the hello directory, allowing you to
start tracking changes to your files.

## Commiting Changes

### types of changes

Change in git fall into two categories:

1. **Untracked** Changes: Files that have been added to your working directory but are not yet tracked by Git.

2. **Tracked Changes:** Files that Git is monitoring. Tracked files can be in one of the following states:

    - **Modified:** Files that have been changed but not yet staged.
    - **Staged:** Files that have been marked to be included in the next commit.
    - **Committed:** Files that have been saved to the repository's history.


#### Git Status

You can check the state of your repository with:

```bash
git status
```

It will show you:

- Files that are untracked, modified, or staged.
- Branch information and any pending changes.

#### Staging Changes

When you modify a file, Git knows it's changed but doesn’t yet include it in a commit. To stage the changes, you use the git add command:

```bash
git add <file>
```
#### Committing Changes

After staging, commit the changes to record them in the repository:

```bash
git commit -m "Message describing the change"
```

#### Viewing Changes

To view the changes made in your working directory compared to the last commit:

```bash
git diff
```
If you want to see changes between the staged files and the last commit:

```bash
git diff --staged
```

This will allow you to see what you've changed that still needs to be staged.

#### Undoing Changes

1.  **Unstage Changes:** If you staged a file by mistake, you can unstage it using:

```bash
git restore --staged <file>
```


2. **Discard Local Changes:** If you want to discard the changes in your working directory:

```bash
git restore <file>
```


# Viewing History
To view the commit history of your Git repository, you can use the git log
command. This command provides a detailed view of all the commits made in
the repository, including the commit hash, author, date, and commit
message.
To see the full commit history,

     run git log

If you prefer a more condensed view, you can use the --oneline option:

    git log --oneline

This will show each commit on a single line.

    git log --reverse

The --reverse option reverses the order of commits, showing the first commit at the top of the list.

    git reflog

`git reflog` is a command that shows a log of where the HEAD and branch references have moved over time in your repository. It tracks all changes to the HEAD, including actions that are not recorded in the regular Git log, such as resets, checkouts, rebases, and more.



### Controlled Entries

You can also customize the log output by specifying additional options. For
example, to view the last two commits:

    git log -n 2

Or to view commits made within the last 5 minutes:

    git log --since='5 minutes ago'


### Personalized Format: 

To show Git logs in a personalized format, you can use the git log command with a custom format string. Here's how you can achieve the format you described:


    git log --pretty=format:"* %h %ad | %s (%d) [%an]" --date=short

Let's break down the format string and options used:

    --pretty=format:"...": Specifies a custom format for the log output.
        %h: Abbreviated commit hash.
        %ad: Author date.
        %s: Commit message.
        %d: Reflog selector (branch and tag names).
        %an: Author name.

    --date=short: Displays the date in a short format (YYYY-MM-DD).

Further Customization


    %H: Full commit hash.
    %ae: Author email.
    %ar: Author date, relative.
    %cn: Committer name.
    %ce: Committer email.
    %cd: Committer date.
    %cr: Committer date, relative.




# Snapshorts
To revert to a previous state of your project, you can use the git checkout
command. This command allows you to switch between different commits,
effectively "checking out" a specific version of your files.

 * Restore First Snapshot: 

    git checkout HEAD~1 hello.sh
    cat hello.sh

this allows you to check the previous (1 commit ago) hello.sh file.


 * Return to Latest Version: 

Once you've finished checking out the previous versions, you can return to
the latest version of the file by running:

    git checkout HEAD hello.sh



# Tagging

Tagging is a feature in Git that allows you to create named snapshots of your repository at specific points in time.

This is useful for marking important milestones or releases of your project, and can make it easier to track and manage the history of your codebase

To tag the current version of your repository, you can use the following
command:

    git tag v1


If you want to tag a previous version of your repository, you can use the
following command:

    git reflog
    git tag v1-beta HEAD@{1}

This will create a new tag named v1-beta that points to the commit that
was the previous HEAD (the commit before the most recent one).



n/b: After creating the tag, push it to the remote repository:

    git push origin v1-beta


To switch between different tagged versions of your repository, you can use
the git checkout command:

    git checkout v1
    git checkout v1-beta

This will update your working directory to the state of the repository at the time the v1 or v1-beta tag was created.

You can list all tags with:

    git tag

To see the tags along with their associated commit messages, use:

    git show-ref --tags


### Taging using commit hash

Identify the commit hash and create the tag:

    git log
    git tag v1.0 e4e3645

Push the tag to the remote repository:

    git push origin v1.0

### Additional Options

* Annotated Tags:

You can create an annotated tag, which allows you to add a message and stores additional metadata.

    git tag -a <tag-name> <commit-hash> -m "Tag message"

    git tag -a v1.0 e4e3645 -m "Release version 1.0"

- And then push it.



# Reverting Changes

If you've made changes to a file that you want to discard, you can use the
git checkout command to revert the file to its previous state.

For example, to revert the hello.sh file to its last committed state, you can run:

    git checkout -- hello.sh

This will discard any changes you've made to the hello.sh file and restore
it to the last committed version.




### Clean the Staging Area:

To discard the changes in the staging area, use the git restore --staged command. This will unstage the changes and move them back to the working directory.

    git restore --staged hello.sh

Then Discard Changes in the Working Directory:
To completely discard the unwanted changes, you can use the git restore command:

    git restore hello.sh


### Commit and Reverting

Steps:

- Make Changes: Edit the file to include the unwanted changes.

- Stage the File: Use the git add command to stage the changes:

    git add <file-path>

- Commit the Changes: Commit the staged changes:

    git commit -m "Add unwanted changes"

- Revert the Changes: To revert the file back to its original state (the state before the last commit), you can use:

    git checkout HEAD^ -- <file-path>

- Stage and Commit the Reversion (if needed): If you want to commit the revert:

    git add <file-path>
    git commit -m "Revert unwanted changes"


#### To Undo the Last Commit and Discard Changes

    Method 1: git reset --soft HEAD~1 to undo the commit and keep changes in the working directory.
    Method 2: git reset --hard HEAD~1 to undo the commit and discard changes.
    Method 3: git revert HEAD to create a new commit that undoes the last commit.




## Removing Commits


The `git reset` command is often used to remove commits by moving the HEAD pointer backward.

**a. Soft Reset**

This option will remove commits but keep the changes in the staging area.

    git reset --soft <commit-hash>

- Effect: The specified commit and any commits after it will be removed from the branch, but the changes will remain staged.

**b. Mixed Reset (default)**

This will remove commits and unstage the changes, keeping them in your working directory.

    git reset <commit-hash>

- Effect: The specified commit and later commits are removed. Changes are kept in the working directory but not staged.

**c. Hard Reset**

This will remove commits and discard all changes in the working directory and staging area.

    git reset --hard <commit-hash>

- Effect: The specified commit and later commits are removed, and all changes are lost.


## Displaying Logs with Deleted Commits: 

Use the `--reflog` Option in git log

You can use the `git log --reflog` option to include reflog entries (which may include deleted commits) in the log output.



## Cleaning Unreferenced (deleted) Commits:

- Expire reflog:

    git reflog expire --expire=now --all

This marks old references for garbage collection.

- Run garbage collection:

    git gc --prune=now

This permanently removes unreachable commits.

- Confirm deletion:

    git fsck --full --unreachable 
    
This checks for any remaining unreferenced objects.


## Amend the Last Commit

If your goal is to modify the last commit itself (including changing the file) without creating a new commit, you can amend it:

* Step 1: Make the desired changes to the file.
* Step 2: Stage the changes:

    git add <file-path>

* Step 3: Amend the last commit:

    git commit --amend --no-edit

Effect: This updates the last commit with your changes while keeping the original commit message.



# Moving files

To move a file to a folder in a Git repository, you can use the git mv command. This command not only moves the file but also stages the change for the next commit. Here’s how to do it:

    git mv <source-file> <destination-folder>



# Exploring .git/ Directory:

1. objects/

- Purpose: This directory stores all of Git’s internal data, including commits, trees (directory listings), blobs (file contents), and tags.
- Structure: It contains subdirectories named by the first two characters of object hashes, and within each, the remaining 38 characters are used as filenames. For example, the commit with SHA abc123... would be stored in a file under objects/ab/123....
- Data stored: This is where Git stores the actual content (files, commits, trees). Each version of a file is stored as a "blob" in this directory.

2. refs/

- Purpose: This directory contains references to commits, such as branches, tags, and other references.

**Subdirectories:**

- *refs/heads/:* Contains files for each branch. The file for each branch contains the commit hash that the branch points to.

- *refs/tags/:* Stores tag references, which point to specific commits.

- *refs/remotes/:* Contains references to remote-tracking branches (i.e., branches on remote repositories like origin).

Example: If you have a branch called main, there will be a file named main inside `refs/heads/` which contains the SHA of the latest commit on that branch.

3. config

- Purpose: This file stores the repository-specific Git configuration. This is where Git looks for settings specific to this repository (as opposed to the global configuration stored in ~/.gitconfig).
- Contents: You can configure options like the repository's default branch, user information (name and email), remotes, merge settings, and more.

Example: If you run git config user.name "Your Name", it may add an entry to the config file:

    [user]
      name = Your Name

4. HEAD

- Purpose: This file is a symbolic reference to the current branch or commit you're working on. It points to the branch or commit that is checked out.
- Contents: Typically, it contains a reference to the current branch, like:

    ref: refs/heads/main

This means HEAD is pointing to the main branch. If you're in a detached HEAD state (e.g., after checking out a specific commit), the file will contain the hash of the specific commit.

### Latest Object Hash

To find the latest commit hash: 

    git log -1 --format="%H"

printing the type of the hash:

    git cat-file -t <hash>

_Example_

```bash
git cat-file -t 9e88551f44362cf945c833a59fbfad9491e1e326
```

_Output_

```bash
commit
```

The output depends on the type of the file such as:
- `commit`
- `tree`
- `blob`

To display the content of the hash, use:

    git cat-file -p <hash>

1. **commit object**

A commit in Git represents a snapshot of the entire project at a specific point in time. It acts as a pointer to a tree object that holds the structure of your project’s files and directories.

*Content of commit object:*

A commit object stores metadata and references to other objects. It contains the following:
- `A reference to a tree object` (representing the directory structure at the time of the commit).
- `References to parent commits` (except for the first commit, which has no parent).
- `Author and committer information` (name, email, timestamp).
- `The commit message`, which describes the changes in that commit.

Example of a commit object content (obtained using `git cat-file -p <commit-hash>`):
```bash
tree 7b34a1f36c4b5db8a1df562c1d34859f8ec9c6a0
parent 9c5b6f78e5d4c9ecb47b3e2d03d6ed8cb6a0e26c
author Baraq23 <wolfgangkoppe18@gmail.com> 1726951888 +0300
committer Baraq23 <wolfgangkoppe18@gmail.com> 1726951888 +0300

This is the commit message.
```
The reference of the parent commits allow Git to form a history chain.



2. **blob object**
A blob (Binary Large Object) in Git represents the actual content of a file. It contains the raw data of a file at a specific point in time, without any metadata (like filename or directory structure). It’s just the contents of the file.

*Content of the blob object*

It’s just the contents of the file.

Example of a blob object content: If you run git `cat-file -p <blob-hash>`, it might return something like this for a text file:

```bash
This is the content of the file.
```
Each version of a file is stored as a unique blob based on its contents.

3. **tree object**

A tree object in Git represents the structure of a directory. It records the filenames, paths, and permissions, and references blobs for file contents or other trees for subdirectories.

*Content of a tree object:*

A tree object lists the directory contents, associating filenames with either blobs (for files) or other trees (for subdirectories).

Example of a tree object content: If you run `git cat-file -p <tree-hash>`, you’ll see something like:

```bash
100644 blob b2a48c0bce6151b2e5d1d6c9a5d4d9f3ea314563    file1.txt
100755 blob 87f1f629f8e0c5b8f6b5e3f1c5e3f8d6ec344323    script.sh
040000 tree 7d1d67d2a5f8d4b8f6e8a7f6b5c3a9f7d9e6d434    subdirectory
```
In this example:
- `100644` and `100755`: File permissions.
- `blob`: The file type (*blob objects* for files).
- `b2a48c0bce6151b2e5d1d6c9a5d4d9f3ea314563`: The SHA-1 hash of the file contents (blob).
- `file1.txt` and `script.sh`: The filenames.
- *subdirectory*: A reference to another `tree` object (which represents a subdirectory).

Tree objects therefore records the directory structure, including filenames, paths, and permissions.


### Dumping Directory Tree: 

Dumping a directory simply means to output or display its content.


*Dump the contents of the lib/ directory and the hello.sh file using Git commands.*

to dump the content of lib directory, use:
    `git show <hash>`

**git show** is a Git command used to display various types of objects in Git, such as commits, trees, and blobs. By default, it shows the details of a single commit, including the commit message, author information, and the changes (diff) introduced in that commit.

*To view a specific file in the commit*

    git show <commit>:file

_Example_

```bash
git show  HEAD:lib
```

_Output_

```bash
tree HEAD:lib

hello.sh
```

To show the content of the `hello.sh` in `lib` directory, first cd to lib using the command `cd lib`, then `git show hello.sh`

Or, simply use the command `git show HEAD:lib/hello.sh`







# Branching
## Create and Switch to New Branch:

To create a branch in git, use `git branch <branch-name`
To switch to a branch in git, use `git checkout <branch-name>`
To create a new branch then switch to it `git checkout -b <branch-name>`

## compare and show the differences between the branches (main & greet branches)

*for Makefile*

1. **Command:**

```bash
git diff master greet -- Makefile
```

This shows the differences between the master and greet branches in the Makefile file.

![](/Makefile-master-branch.png)
- Makefile in `master` branch
![](/Makefile-greet-branch.png)
- Makefile in `greet` branch

2. **Header of the Diff:**

![](/git-diff.png)
- Makefile difference

```bash
diff --git a/Makefile b/Makefile
index 407082d..12d4ffb 100644
```

This indicates that the file being compared is Makefile. The index numbers (`407082d` and `12d4ffb`) represent the specific commit hashes or file states before and after the change.

3. **Changes:**

```bash
--- a/Makefile
+++ b/Makefile
```

`--- a/Makefile` is the file's original state (on the *master* branch), and `+++ b/Makefile` is the updated state (on the *greet* branch).

4. **Actual Diff:**

```bash
@@ -1,3 +1,4 @@
```

This shows that the comparison starts at line `1` and that `3` lines were originally there, but `4` lines are present in the updated version.

```bash
+# Ensure it runs the updated lib/hello.sh file
```

The `+` indicates a line added to the file in the greet branch. This comment ensures that the Makefile uses the updated `lib/hello.sh` file.

5. **No changes to the following lines:**

```bash
makefile

TARGET="lib/hello.sh"

run:
```
These lines are unchanged between the two branches.


## Draw a commit tree diagram

A `commit tree diagram` is a visual representation of the structure of commits in a Git repository. It shows the history of commits, how they are connected, and how different branches relate to each other.

To visualize a commit tree in a Git repository, use `git log --graph --oneline --all`

_Output_

```bash
* 3970737 (greet) Update: Makefile
* 0e95828 Update: hello.sh
* 2f8bfba Add: greeter.sh
* c113d77 (HEAD -> master) Add: Makefile
* 1a455c0 Chore: move hello.sh to lib dir
* 453c5b6 Update: author information comments added to hello.sh file
| * d69f528 (tag: oops) commiting unwanted change to be reverted
|/  
* c9e9f78 (tag: v1) Update: commiting changes in line 4 & 5
* 229280c (tag: v1-beta) Update: comment added on line 3 in hello.sh file
* 248f1cb Update: hello.sh changes made
* 0e7ce85 Add: hello.sh file (first commit)
```


# Conflicts, merging and rebasing

### Merging Notes

For example, to merge the `main branch` into the `greet branch`, you should be on the `greet branch`. This is because the branch you are currently on is the one that will receive the changes from the branch you are merging.

*Merging the `main` branch into `greet`:*

1. Switch to the `greet` branch (if you're not already on it):
```bash
git checkout greet
```

2. Merge the `main` branch into `greet`:
```bash
git merge main
```

This command will pull the changes from the main branch and merge them into greet. If there are no `conflicts`, Git will complete the merge and ask for a `merge commit message`, which you can edit or accept as default.


### Git Conflicts

A `Git conflict `occurs when Git is unable to automatically merge two sets of changes, `usually because the same file or the same line of code has been modified differently in separate branches or commits`. Git will then stop the merge process and require you to resolve the conflicting changes manually before proceeding.


### Rebasing notes

`Rebasing` in Git is a way to integrate changes from one branch into another by moving or replaying commits. Instead of merging the two branches, `rebasing rewrites the commit history` by taking a sequence of commits and applying them `on top of another branch`. This creates a linear, cleaner commit history.

**How Rebasing Works**

Let’s say you have the following history, where `main` has diverged from `feature`:
```bash
A---B---C main
     \
      D---E feature
```

- A, B, C: Commits on `main`.
- D, E: Commits on `feature`.

Now, you want to rebase `feature` onto main so that the commits from `feature` are applied on top of the latest changes in `main`.

1. Check out the `feature` branch:

```bash
git checkout feature
```

2. Rebase `feature` onto `main`:

```bash
    git rebase main
```

Git will then:

- Take the commits from `feature` (D and E).
- Replay them on top of the latest commit from `main` (C).

The new history after rebasing will look like this:

```bash
A---B---C---D'---E' feature
```

Now, the `feature` branch is up-to-date with `main`, and the commit history is linear.


**Commands for Rebasing:**

1. Check out the `feature` branch:`git checkout <branch>`

```bash
git checkout feature
```

2. Rebase `feature` onto `main`: `git rebase <branch>`

```bash
    git rebase main
```

3. Abort a rebase (if you run into too many conflicts): use `--abort` option

```bash
git rebase --abort
```

4. Continue a rebase (after resolving conflicts): use `--continue` option

```bash
git rebase --continue
```

5. Interactive rebase (to reorder, squash, or edit commits): use `<-i>` option with `<branch>`

```bash
git rebase -i <branch>
```

## Understanding Fast-Forwarding and Differences: 

`Fast-forwarding` in Git happens when you merge two branches, and no new commits were made on the branch you're merging into. In this case, Git doesn’t create a merge commit. Instead, it simply moves the branch pointer forward to include all the new changes from the other branch, as if everything happened in a straight line.

***Merging vs. Rebasing***

1. **Merging**

Merging is a way to combine the work from one branch into another. It keeps the commit history from both branches intact and creates a merge commit to indicate that the two branches have been combined.

*Pros of Merging:*

- Preserves branch history: Both branches’ commit histories remain unchanged and are merged into the new branch.
- Clear context: The merge commit shows when and why the branches were combined.

*Cons of Merging:*

- Non-linear history: The commit history can become cluttered with merge commits, making it harder to follow a linear flow of changes

2. **Rebasing**

Rebasing is a way to reapply commits from one branch on top of another branch, effectively moving the base of the feature branch to the tip of the target branch. This process creates a linear history without merge commits.

*Pros of Rebasing:*

- Clean, linear history: Rebasing creates a single, straightforward history, which is easier to understand and navigate.
- No merge commits: There’s no clutter from unnecessary merge commits.
- Ideal for feature branches: Before merging a feature branch into main, rebasing makes sure the feature branch is based on the latest changes in main.

*Cons of Rebasing:*

- Rewrites history: Rebasing changes the commit history by creating new commits with different hashes (`D' and E'` instead of `D and E`), which can cause issues if the branch is already shared with others.
- Can cause conflicts: Rebasing each commit on top of another branch may trigger conflicts that need to be resolved commit by commit.


# Local and remote repositories

**make a clone of repository**

Navigate to the folder where you want to clone the repository, then use the following command:

    git clone <path-repo-to-be-clone> <cloned-repo-name>

For remote repositories, use the following command:

    git clone <repo-url> <cloned-repo-name>

_Example_

```bash
git clone  hello cloned_hello
```

**To show remote information**

*Commands*

- `git remote`
- `git remote -v`
- `git remote show origin`

*Listing branches*

- `git branch`  lists local branches
- `git branch -r` lists remote branches
- `git branch -a`  lists all branches


1. **To fetch changes from the remote repository**

- `git fetch origin` will fetch all new commits, branches, and tags from the remote `origin` (the remote repository you cloned from) and update your local repository with this information.
 - *N/B* Fetching does not automatically merge or modify your working directory. It only updates the remote-tracking branches.



2. **Display the logs, including the fetched commits:**

```bash
git log --all --oneline --graph --decorate
```
* `--all`: This option includes all branches in the logs (local and remote).
* `--oneline`: Shows each commit on one line, making the log more readable.
* `--graph`: Displays an ASCII graph showing the branch and merge structure.
* `--decorate`: Shows references (like branch names and tags) alongside the commits.


3. **Merge the changes from the remote main branch into the local main branch.**

```bash
git checkout main
git fetch origin
git merge origin/main

```


4. **Create and track the `greet` branch:** (branch from origin)
```bash
git checkout --track origin/greet
```
- You can confirm the tracking relationship by running:
```bash
git branch -vv
```

This will show the tracking status for all branches.


5. **Add a remote and push the `main` and `greet` branches:**

- Add a new remote (assuming the new remote URL is https://example.com/user/repo.git):

```bash
git remote add new-remote https://example.com/user/repo.git
git push new-remote main
git push new-remote greet
```

- `git push new-remote main` This will push the local main branch to the new-remote repository.
- `git push new-remote greet` This will push the local greet branch to the new-remote repository.


# Bare repositories

A `bare repository` is a Git repository that does not contain a working directory (the actual files of the project). Instead, it only contains the version control data (the .git directory), which includes the commit history, branches, tags, and configuration files.

**Importance of Bare Repositories**

***Collaboration:*** Bare repositories act as central hubs where team members can push and pull code. They don't have a working directory, so no one can directly change the files.

***Remote Repositories:*** Most remote repositories, like those on GitHub or GitLab, are bare repositories.

***Avoid Conflicts:*** Since bare repositories don’t have files that can be changed, there’s no risk of conflicts when multiple people are working on the same project.


**Creating a Bare Repository**
* Navigate to the directory where you want to create the bare repository: This should be outside the hello directory.
* Clone the hello repository as a bare repository: Use the `--bare` option with the `git clone`
```bash
git clone --bare <repository-to-be-cloned> <bare-repo-name>
```
* Add the Bare Repository as a Remote to the Original Repository
```bash
cd /path/to/<original-repo>
git remote add bare-origin /path/to/<bare-repo>
```
