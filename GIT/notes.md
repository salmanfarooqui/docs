# Git

### Step 1

Create a new directory, open it and perform `git init`

to create a new git repository. This will create a hidden .git folder inside your current folder.



### Step 2

Add your name and email, for first time.

Name - `git config --global user.name "Mona Lisa"`

For single repository, use `git config user.name "Mona Lisa"`

Email - `git config --global user.email "email@example.com"`



### Step 3

If you have not cloned an existing repository and want to connect your repository to a remote server, you need to add it with `git remote add origin <url_to_repo>`

Example -  https://github.com/salmanfarooqui/FirstRepo.git

Now you are able to push your changes to the selected remote server



## Workflow

your local repository consists of three \"trees\" maintained by git. the first one is your Working Directory which holds the actual files. the second one is the Index which acts as a staging area and finally the HEAD which points to the last commit you\'ve made.



![1588542059836](http://docs.salmanfarooqui.com/GIT/images/1588542059836.png)



## Git reset

(stage it --- means add it to the index --- using git add)

If your master branch (currently checked out) is like this:

\- A - B - C (HEAD, master)

and you realize you want master to point to B, not C, you will use git reset B to move it there:

\- A - B (HEAD, master)

​	# - C is still here, but there\'s no branch pointing to it anymore



> This is different from a checkout. If you\'d run git checkout B, you\'d get this:
>
> - A - B (HEAD) - C (master)
>
> You\'ve ended up in a detached HEAD state. HEAD, work tree, index all match B, but the master branch was left behind at C. If you make a new commit D at this point, you\'ll get this, which is probably not what you want:
>
> - A - B - C (master)
>
>   ​		\
>
>   ​	D (HEAD)



### --hard

--hard makes everything match the commit you\'ve reset to. One primary use is blowing away your work but not switching commits: `git reset --hard` means `git reset --hard HEAD`

In Layman\'s term, it removes the git commit and git add and also the modified file. So your file goes back to where it was at last commit or whatever you choose.



 NOTE - HEAD\~ is short for HEAD\~1 and means the commit\'s first parent. [HEAD\~2 means the commit\'s first parent\'s first parent]{.underline}. Think of HEAD\~n as \"n commits before HEAD\" or \"the nth generation ancestor of HEAD\".

 HEAD\^ (or HEAD\^1) also means the commit\'s first parent. [HEAD\^2 means the commit\'s second parent]{.underline}.

 \~2 means up two levels in the hierarchy, via the first parent if a commit has more than one parent

 \^2 means the second parent where a commit has more than one parent (i.e. because it\'s a merge)



![What_s_the_difference_between_HEAD_and_HEAD_in_Git_Stack_Overflow](http://docs.salmanfarooqui.com/GIT/images/What_s_the_difference_between_HEAD_and_HEAD_in_Git_Stack_Overflow.png)




### --mixed

--mixed is the default, i.e. `git reset` means `git reset --mixed`. It **resets the index**, but not the work tree. **This means all your files are intact**, but any differences between the original commit and the one you reset to will show up as local modifications (or untracked files) with git status. In Layman\'s term, it just removes the git commit and git add, but the modified file remains same.

( file changes will stay in your working tree. Also the changes will stay on your index)

Use this when you realize you made some bad commits, but you want to keep all the work you\'ve done so you can fix it up and recommit. In order to commit, you\'ll have to add files to the index again (git add \...)



### --soft

--soft doesn\'t touch the index or work tree. All your files are intact as with \--mixed, but all the changes show up as **changes to be committed** with git status. Resets the head to \<commit\>

`git reset --soft HEAD^`   or   `git reset --soft HEAD~`   or   `git reset --soft HEAD~1` (resets head to last commit)

( keep the changes in your working tree but not on the index; so if you want to \"redo\" the commit, you will have to add the changes (git add) before commiting )

The index is untouched, so you can commit immediately if you want. Also the changes will stay on your index, so following with a git commit will create a commit with the exact same changes as the commit you \"removed\" before.

It moves HEAD, and only the HEAD.

This differ from commit \--amend as:

- it doesn\'t create a new commit.

- it can actually move HEAD to any commit (as commit \--amend is only about not moving HEAD, while allowing to redo the current commit)

In Layman\'s term, it just removes git commit (not git add). Index remains the same. Moves the Head.

![1588542387776](C:\Users\Salman Farooqui\AppData\Roaming\Typora\typora-user-images\1588542387776.png)



**If you use a path, Git won\'t move HEAD**. Only the index and working directory play a role when you use a path. Let\'s say you\'ve been working on version 3 of a file and added it to the index. Run git reset some-file.ext to unstage that particular file. (git reset some-file.ext assumes that by default you meant `git reset --mixed HEAD some-file.ext`)

`$ git reset Head newtext.txt`



## Diff between origin master and origin/master

- master is your local branch.
- origin master is the branch master on the remote repository named origin.
- origin/master is your local copy of origin master.

When you do `git pull` (what I consider as evil, anyone else?), it automatically does :

- `git fetch`  it copies origin master into origin/master. (and all other origin xxx into origin/xxx).
- `git merge`  it merges origin/master into master.



## How to



### Undo files changes. Still not staged(git add)

`git checkout -- .`    (all unstaged files)

`git checkout -- foo.txt` for specific file.

Undo changes made to files. This is before you staged (git add) the files. If staged then use reset.

<br>

Better way (less dangerous) -

`git stash save --keep-index`

Include \--include-untracked if you\'d want to be thorough about it.

After that, you can drop that stash with a git stash drop command if you like.

<br>

### Undo Staged (git add) (unstage a file)

`$ git reset Head newtext.txt`

or `$ git reset Head .` (for all staged files)

<br>

### Remove all old commits but keep files. Fresh start with first commit.

```js
git checkout --orphan newBranch     // create a new orphan branch named newBranch

git add -A     # Add all files and commit them

git commit -m "commit message here"

git branch -D master    # Deletes the master branch

git branch -m master     # Rename the current branch to master

git push -f origin master    # Force push master branch to github

git gc --aggressive --prune=all    # remove the old files
```

<br>

### Deleting the Commited File

First stage the delete, `git rm filename.txt`

then commit the delete, `git commit -m "I have deleted filename"`

<br>

### Delete the Commited File Ouside git (using windows explorer)

First, delete the file. Click the file, press Delete button.



Now do, `git add -A`

which adds and updates any changes(addition, deleteion or modification) to working directory

Then, simply to do the commit, `git commit -m "deleted the file"`

<br>

### Undelete after Deleting the Commited File

First the delete, `git rm filename.txt` (deletion staged and file removed from working directory)

Now if want to undo the delete-

First unstage the delete, `git reset HEAD filename.txt`

Then get back the file removed from working directory, `git checkout -- todelete.txt`



## Git diff

Working director + staging area  `git diff`

Working directory + last commit  `git diff HEAD`

Staging Area + last commit  `git diff --staged HEAD`

For only 1 file (if there are many files)  `git diff -- filename.txt`

last commit + second last commit  `git diff HEAD HEAD^1 or git diff HEAD HEAD~1`

local(master) + remote(origin/master)  `git diff master origin/master`



## Branches

list all branches  `git branch -a`

new branch  `git branch nameofbranch`

switch branch  `git checkout nameofbranch`

rename branch  `git -branch -m nameofbranch newname`

delete a branch  `git branch -d nameofbranch`

delete a branch on remote  `git push origin --delete nameofbranch`



## Merge

simple merge (fast forward)  `git merge tobemereged tothisbranch` (needs checked out on tothisbranch)

Example - first checkout to master branch. Then, git merege features master. features branch will get mereged to master.



> **What is Fast Forword Merge?**
>
> Let us assume that I created a branch named features. After working on this branch for a while (three commits), I finally decided that I am done and then I pushed it to my own remote. Meanwhile, nothing else happened in the master branch, it remained in the same state right before I branched off. Because master has not been changed since the commit, Git will perform the merge using fast-forward. The whole series of the commits will be linear.
>

![1588542726937](C:\Users\Salman Farooqui\AppData\Roaming\Typora\typora-user-images\1588542726937.png)

> Another variant of the merge is to use -no-ff option (it stands for no fast-forward). In this case, the history looks slightly different (right side), there is an additional commit (dotted circle) emphasizing the merge. This commit even has the message informing us about the merged branch. The default behavior of Git is to use fast-forwarding whenever possible.
>
> In very simple cases, one of the two branches doesn\'t have any new commits since the branching happened. Merging is dead simple in those cases. In Git, this simplest form of integration is called a \"fast-forward\" merge. Both branches then share the exact same history.
>
> In a lot of cases, however, both branches moved forward individually. To make an integration, Git will have to create a new commit that contains the differences between them - the merge commit.
>

merege (no fast forward)  `git merge tobemereged --no-ff tothisbranch`



## Rebase

![1588542771450](C:\Users\Salman Farooqui\AppData\Roaming\Typora\typora-user-images\1588542771450.png)

git-rebase: Reapply all the commit from your branch to the tip of another branch. By reapplying commits git creates new ones. Those new commits (E and F after rebase), even if they bring the same set of change will be treated as completely different and independent by git.

Let\'s say you\'re working on feature X and when you\'re done, you merge your changes in. The destination branch will now have a single commit that would say something along the lines of \"Added feature X\". Now, instead of merging, if you rebased and then merged the destination, development history would contain all the individual commits in a single logical progression. This makes reviewing changes later on much easier.

- **If you have already pushed the branch you\'re working on upstream, you should not rebase, but merge instead.**
- After rebasing and pushing the changes to server, if you delete your branch, there will be no evidence of branch you have worked upon.

- After doing rebase we also get rid of an extra commit which we used to see if we do normal merge.

- rebasing also results in a perfectly linear project history. you can follow the tip of feature all the way to the beginning of the project with commands like git log

For example - we want to integrate the changes from branch-B into branch-A

![1588542806330](C:\Users\Salman Farooqui\AppData\Roaming\Typora\typora-user-images\1588542806330.png)

Use -

`git checkout branch-A`

`git rebase branch-B`

This moves the entire branch-A to begin on the tip of the branch-B.



### Explanation (how it works) -

1.

First, Git will \"undo\" all commits on branch-A that happened after the lines began to branch. However, of course, it won\'t discard them: instead you can think of those commits as being \"saved away temporarily\".

![1588542848716](C:\Users\Salman Farooqui\AppData\Roaming\Typora\typora-user-images\1588542848716.png)



2\.

Next, it applies the commits from branch-B that we want to integrate. At this point, both branches look exactly the same.

![1588542860818](C:\Users\Salman Farooqui\AppData\Roaming\Typora\typora-user-images\1588542860818.png)



3\.

In the final step, the new commits on branch-A are now reapplied - but on a new position, on top of the integrated commits from branch-B (they are re-based). The result looks like development had happened in a straight line.

![1588542882370](C:\Users\Salman Farooqui\AppData\Roaming\Typora\typora-user-images\1588542882370.png)





### Interactive Rebasing

Interactive rebasing gives you the opportunity to alter commits as they are moved to the new branch. Typically, this is used to clean up a messy history before merging a feature branch into master.



To begin an interactive rebasing session, pass the i option to the git rebase command:

`git rebase -i master`



This will open a text editor listing all of the commits that are about to be moved:

pick 33d5b7a Message for commit \#1

pick 9480b3d Message for commit \#2

pick 5c67e61 Message for commit \#3

This listing defines exactly what the branch will look like after the rebase is performed. By changing the pick command and/or re-ordering the entries, you can make the branch's history look like whatever you want. For example, if the 2nd commit fixes a small problem in the 1st commit, you can condense them into a single commit with the fixup command:

pick 33d5b7a Message for commit \#1

fixup 9480b3d Message for commit \#2

pick 5c67e61 Message for commit \#3



When you save and close the file, Git will perform the rebase according to your instructions, resulting in project history that looks like the following:

![1588542934162](C:\Users\Salman Farooqui\AppData\Roaming\Typora\typora-user-images\1588542934162.png)

Eliminating insignificant commits like this makes your feature's history much easier to understand. This is something that git merge simply cannot do.



### Incorporating Upstream Changes Into a Feature

Keep in mind that it's perfectly legal to rebase onto a remote branch instead of master. This can happen when collaborating on the same feature with another developer and you need to incorporate their changes into your repository.

For example, if you and another developer named John added commits to the feature branch, your repository might look like the following after fetching the remote feature branch from John's repository:

![1588542968768](C:\Users\Salman Farooqui\AppData\Roaming\Typora\typora-user-images\1588542968768.png)

You can resolve this fork the exact same way as you integrate upstream changes from master: either merge your local feature with john/feature, or rebase your local feature onto the tip of john/feature.

![1588542981884](C:\Users\Salman Farooqui\AppData\Roaming\Typora\typora-user-images\1588542981884.png)

Note that this rebase doesn't violate the Golden Rule of Rebasing because only your local feature commits are being moved---everything before that is untouched. This is like saying, "add my changes to what John has already done." In most circumstances, this is more intuitive than synchronizing with the remote branch via a merge commit.

By default, the git pull command performs a merge, but you can force it to integrate the remote branch with a rebase by passing it the \--rebase option.



## Backup git in one file

`git bundle create something.bundle --all`

something.bundle files will be created in your folder. Just move it anywhere. Email it.

<br>

Then to use it.

`git clone something.bundle newFolder`

A newFolder will be created with all git contents and files in it.
