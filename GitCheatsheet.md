# Git Cheatsheet

## Basic Commands

### git init
Now that we have started working on the screenplay, let’s turn the < project > directory into a Git project. We do this with:

	git init

The word init means initialize. The command sets up all the tools Git needs to begin tracking changes made to the project.


### Git Environments
Nice! We have a Git project. A Git project can be thought of as having three parts:

* A Working Directory: where you'll be doing all the work: creating, editing, deleting and organizing files
* A Staging Area: where you'll list changes you make to the working directory
* A Repository: where Git permanently stores those changes as different versions of the project
The Git workflow consists of editing files in the working directory, adding files to the staging area, and saving changes to a Git repository. In Git, we save changes with a commit, which we will learn more about in this lesson.


### git status
As you write the code, you will be changing the contents of the working directory. You can check the status of those changes with:

	git status


### git add
In order for Git to start tracking text1.txt, the file needs to be added to the staging area.

We can add a file to the staging area with:

	git add <<filename>>

The word filename here refers to the name of the file you are editing, such as text1.txt.


### git diff
Imagine that we type another line in scene-1.txt. Since the file is tracked, we can check the differences between the working directory and the staging area with:

	git diff <<filename>>


### git commit
A commit is the last step in our Git workflow. A commit permanently stores changes from the staging area inside the repository.

git commit is the command we'll do next. However, one more bit of code is needed for a commit: the option -m followed by a message. Here's an example:

	git commint -m <<Commit messages>>
	
Standard Conventions for Commit Messages:

* Must be in quotation marks
* Written in the present tense
* Should be brief (50 characters or less) when using -m


### git log
Often with Git, you'll need to refer back an earlier version of a project. Commits are stored chronologically in the repository and can be viewed with:

	git log
	
## BACKTRACKING
### git show `HEAD`
In Git, the commit you are currently on is known as the `HEAD` commit. In many cases, the most recently made commit is the `HEAD` commit.

To see the `HEAD` commit, enter:

	git show HEAD

The output of this command will display everything the git log command displays for the `HEAD` commit, plus all the file changes that were committed.

### git checkout `HEAD` 
What if you decide to change the ghost's line in the working directory, but then decide you wanted to discard that change?

You could rewrite the line how it was originally, but what if you forgot the exact wording? The command

	git checkout HEAD <<filename>>

will restore the file in your working directory to look exactly as it did when you last made a commit.

### git add << filename1 >> << filename2 >>
The < project > we are working on contains multiple files. In Git, it's common to change many files, add those files to the staging area, and commit them to Git in a single commit.

For example, say you want to change the character "LARRY" to "LAERTES" in the script. The name currently appears in two files. After you change the name in both files, you could add the changed files to the staging area with:

	git add filename_1 filename_2

Note the word filename above refers to the name of the file you are adding to the staging area, such as text3.txt.

### git reset `HEAD` << filename >>

What if, before you commit, you accidentally delete an important line from text2.txt? Unthinkingly, you add text2.txt to the staging area. The file change is *unrelated* to the other changes on text3.txt and you don't want to include it in the commit.

We can *unstage* that file from the staging area using

	git reset HEAD <<filename>>

This command *resets* the file in the staging area to be the same as the `HEAD` commit. It does not discard file changes from the working directory, it just removes them from the staging area.

### git reset << SHA >>
Creating a project is like hiking in a dark forest. Sometimes you take a wrong turn, then another wrong turn. Before you know it, you're surrounded by bears.

Git enables you to rewind to the part before you made the wrong turn and create a new destiny for the project. You can do this with:

	git reset <<SHA>>
	
This command works by using the first 7 characters of the SHA of a previous commit.  For example, if the SHA of the previous commit is ``5d692065cf51a2f50ea8e7b19b5a7ae512f633ba``, use:
	
	git reset 5d69206
	
To better understand ``git reset commit_SHA``, notice the diagram on the right. Each circle represents a commit.

Before reset:

* `HEAD` is at the most recent commit


After resetting:

* `HEAD` goes to a previously made commit of your choice
* The gray commits are no longer part of your project
* You have in essence rewinded the project's history


## Branching

Up to this point, you've worked in a single Git branch called `master`. Git allows us to create *branches* to experiment with versions of a project. Imagine you want to create version of a story with a happy ending. You can create a new branch and make the happy ending changes to that branch only. It will have no effect on the `master` branch until you're ready to merge the happy ending to the `master` branch.

In this lesson, we'll be using Git branching to develop multiple versions of a resumé.

### git branch (see current branch)
You can use the command below to answer the question: “which branch am I on?”

	git branch
	
![Git Branch Diagram](https://s3.amazonaws.com/codecademy-content/courses/learn-git/git-diagram-1.svg)

The diagram to the right illustrates branching.

The circles are commits, and together form the Git project's commit history.

* New Branch is a different version of the Git project. 
* It contains commits from Master but also has commits that Master does not have.

### git branch << new_branch >> (create new branch)
Right now, the Git project has only one branch: `master`.

To create a new branch, use:

	git branch <<new_branch>>

Here << new\_branch >> would be the name of the new branch you create, like `photos` or `blurb`. Be sure to name your branch something that describes the purpose of the branch. Also, branch names can’t contain whitespaces: `new-branch` and `new_branch` are valid branch names, but new branch is not.

### git checkout << new_branch >> (switch branch)
The `master` and `fencing` branches are identical: they share the same exact commit history. You can switch to the new branch with

	git checkout branch_name

You will be now able to make commits on the fencing branch that have no impact on `master`. Let's switch to the fencing branch.

Print the Git commit log using `git log`

Notice the output:

* The commits you see were all made in the master branch. fencing inherited them.
* This means that every commit master has, fencing also has.

Note: if you find that your cursor is stuck in Git log, press q to escape.

### git merge << branch_name >>
What if you wanted include all the changes made to the `fencing` branch on the `master branch`? We can easily accomplish this by merging the branch into `master` with:

	git merge <<branch_name>>

While merging branch, keep in mind:

* Your goal is to update `master` with changes you made to `fencing`.
* `fencing` is the giver branch, since it provides the changes.
* `master` is the receiver branch, since it accepts those changes.

1. You are currently on the fencing branch. Switch over to the master branch - `git checkout master`
2. From the terminal, type 
	```
	git merge fencing
	```
to merge the `fencing` branch into the `master` branch.

Notice the output: The merge is a "fast forward" because Git recognizes that `fencing` contains the most recent commit. Git *fast forwards* `master` to be up to date with `fencing`.


### git merge (with merge conflict)
Assuming you make changes to both the `fencing` branch and `master` branch, you will encounter merge conflict and git will require you to resolve the conflicts. 

#### On `fencing` branch:

1. check active brach - ``git branch``
2. change branch if necessary - ``git checkout << branch >>`` 
3. make changes on files
4. add changes to stanging - ``git add << filename >> ``
5. commit with message - ``git commit -m "commit message"``

#### On `master`:

1. check active brach - ``git branch``
2. change branch if necessary - ``git checkout << branch >>`` 
3. make changes on files
4. add changes to stanging - ``git add << filename >> ``
5. commit with message - ``git commit -m "commit message"``


#### To merge both branches, to master:

1. check active brach - ``git branch``
2. change to master if necessary - ``git checkout master`` 
3. Try merge - ``git merge fencing``
4. The output will show merge conflict as such:

		CONFLICT (content): Merge conflict in resumé.txt
		Automatic merge failed; fix conflicts 
		and then commit the result.
		
5. Conflicts appears like this 

	```
	<<<<<<< HEAD
	master version of line
	=======
	fencing version of line
	>>>>>>> fencing
	```
	
	From the code editor:

	Delete the content of the line as it appears in the master branch

	Delete all of Git's special markings including the words HEAD and fencing. If any of Git's markings remain, for example, >>>>>>> and =======, the conflict remains.

6. add changes to staging again (remain on `master`) - `git add <<filename>> `
7. commit with comment - `git commit -m "message"`


### Delete a branch
In Git, branches are usually a means to an end. You create them to work on a new project feature, but the end goal is to merge that feature into the `master` branch. After the branch has been integrated into `master`, it has served its purpose and can be deleted.

The command

	git branch -d branch_name

will delete the specified branch from your Git project.

### Summary 
Let's take a moment to review the main concepts and commands from the lesson before moving on.

* *Git branching* allows users to experiment with different versions of a project by checking out separate *branches* to work on.

The following commands are useful in the Git branch workflow.

* `git branch`: Lists all a Git project's branches.
* `git branch branch_name`: Creates a new branch.
* `git checkout branch_name`: Used to switch from one branch to another.
* `git merge branch_name`: Used to join file changes from one branch to another.
* `git branch -d branch_name`: Deletes the branch specified.

## Git Teamwork
So far, we've learned how to work on Git as a single user. Git offers a suite of collaboration tools to make working with others on a project easier.

Imagine that you're a science teacher, developing some quizzes with Sally, another teacher in the school. You are using Git to manage the project.

In order to collaborate, you and Sally need:

* A complete replica of the project on your own computers
* A way to keep track of and review each other's work
* Access to a definitive project version

You can accomplish all of this by using *remotes*. A remote is a shared Git repository that allows multiple collaborators to work on the same Git project from different locations. Collaborators work on the project independently, and merge changes together when they are ready to do so.

### git clone
Sally has created the remote repository, science-quizzes in the directory curriculum, which teachers on the school's shared network have access to. In order to get your own replica of science-quizzes, you'll need to *clone* it with:

	git clone remote_location clone_name


In this command: 

* `remote_location` tells Git where to go to find the remote. This could be a web address, or a filepath, such as:

		/Users/teachers/Documents/some-remote

* `clone_name` is the name you give to the directory in which Git will clone the repository.

### git orgin
One thing that Git does behind the scenes when you clone science-quizzes is give the remote address the name origin, so that you can refer to it more conveniently. In this case, Sally's remote is origin.

You can see a list of a Git project's remotes with the command:

	git remote -v
	
To do so:

1. cd << project_dir >>
2. type `git remote -v`
3. notice output: 

		origin    /home/ccuser/workspace/curriculum/science-quizzes (fetch)
		origin    /home/ccuser/workspace/curriculum/science-quizzes (push)
		
	* Git lists the name of the remote, `origin`, as well as its location.
	* Git automatically names this remote `origin`, because it refers to the remote repository of origin. However, it is possible to safely change its name.

	
### git fetch
If your clone will no longer be up-to-date, an easy way to see if changes have been made to the remote and bring the changes down to your local copy is with:

	git fetch

Steps:
1. cd << project_dir >>
2. type `git fetch`

### git merge
Even though Sally's new commits have been fetched to your local copy of the Git project, those commits are on the origin/master branch. Your local master branch has not been updated yet, so you can't view or make changes to any of the work she has added.

In Lesson III, Git Branching we learned how to merge braches. Now we'll use the git merge command to integrate origin/master into your local master branch. The command:

	git merge origin/master

will accomplish this for us.

Steps:

1. cd << project_dir >>
2. make sure git branch is master - `git branch`
3. if not, change to master - `git checkout master`
4. commit local changes to local master with comment - `git commit -m "commit_message"`
5. merge with origin/master - `git merge origin/master`
6. Notice the output:

		Updating a2ba090..bc87a1a
		Fast-forward
		 biology.txt | 2 +-
		 1 file changed, 1 insertion(+), 1 deletion(-)

	* Git has performed a "fast-forward" merge, bringing your local master branch up to speed with Sally's most recent commit on the remote.

### git workflow
Now that you've merged `origin/master` into your local `master` branch, you're ready to contribute some work of your own. The workflow for Git collaborations typically follows this order:

1. Fetch and merge changes from the remote
2. Create a branch to work on a new project feature
3. Develop the feature on your branch and commit your work
4. Fetch and merge from the remote again (in case new commits were made while you were working)
5. Push your branch up to the remote for review

Steps 1 and 4 are a safeguard against merge conflicts, which occur when two branches contain file changes that cannot be merged with the `git merge` command. Step 5 involves `git push`, a command you will learn in the next exercise.

The command:

	git push origin your_branch_name

will push your branch up to the remote, `origin`. From there, Sally can review your branch and merge your work into the `master` branch, making it part of the definitive project version.

Example:

1. cd << project_dir >>
2. create new branch - `git branch <branch_name>`
3. switch to new branch - `git checkout <branch_name>`
4. make code changes
5. add changes to staging - `git add <filename>`
6. commit to branch - `git commit -m "<commit_message>"`
7. push to share with remote - `git push origin <branch_name>`
8. notice output:

		To /home/ccuser/workspace/curriculum/science-quizzes
		 * [new branch]      bio-questions -> bio-questions

	* Git informs us that the branch `bio-questions` was pushed up to the remote. Sally can now review your new work and can merge it into the remote's `master` branch.

### Summary
Let's review.

* A remote is a Git repository that lives outside your Git project folder. Remotes can live on the web, on a shared network or even in a separate folder on your local computer.
* The Git Collaborative Workflow are steps that enable smooth project development when multiple collaborators are working on the same Git project.

We also learned the following commands

* `git clone`: Creates a local copy of a remote.
* `git remote -v`: Lists a Git project's remotes.
* `git fetch`: Fetches work from the remote into the local copy.
* `git merge origin/master`: Merges origin/master into your local branch.
* `git push origin <branch_name>`: Pushes a local branch to the origin remote.

Git projects are usually managed on Github, a website that hosts Git projects for millions of users. With Github you can access your projects from anywhere in the world by using the basic workflow you learned here.