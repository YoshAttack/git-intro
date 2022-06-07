---
marp: true
theme: default
paginate: true
---



# Introduction to Version Control with Git

### About Me
* I am a (post-doctoral) research associate in the Department of Astronomy (and Virginia Initiative on Cosmic Origins).
* My background is in computational astrophysics, and star and planet formation, but I do occasionally also get involved in observational projects.

### Credits
* These slides are heavily based on Erich Purpur's "Introduction to Git" (https://github.com/epurpur/git-intro), but credit also goes to Ricky and Pete Alonzi.
* These slides are also incorporate aspects of the Software Carpentry lesson on Git (https://swcarpentry.github.io/git-novice/).


---
## Things to have before we begin

* Github account: https://github.com/
   - Create a free account if you don't have one yet
* A text editor or IDE. If you don't already have one, you can use something very simple like Notepad++: https://notepad-plus-plus.org/
   - Anaconda/Spyder
   - Visual Studio Code
   - Any plain text editor (`nano`, `emacs`, `vim`, etc.)
* `git`

---
## Goals

1. Why version control?
2. Everyone has a GitHub account
3. Create and fork a repository
4. Introduce git/github workflows
5. Know where to find help

---
## Some terms

* git - version control software installed on your local machine
* GitHub - a for profit company owned by Microsoft. It provides cloud-based hosting of repositories.
* GitLab - an alternative to GitHub, not owned by Microsoft ([why you might want to switch](https://about.gitlab.com/2017/07/19/git-wars-switching-to-gitlab/)). "Free software deserves free tools."
* Bitbucket - another alternative to GitHub (also owned by a for profit company).
* repository ("repo") - Basic unit in git: a record of all changes to specified files.
* fork - personal copy of another users repo.
* branch - a parallel version of a repo (main branch is called "main", formerly "master").

[Github's Git Glossary](https://help.github.com/en/articles/github-glossary)

[Github's Git Handbook](https://guides.github.com/introduction/git-handbook/)

---
## What is Version Control?
* Version control is the practice of tracking and managing changes to software code.
* Version control systems (VCS) are software tools that help you manage changes to source code over time.
* Version control software keeps track of every modification to the code in a special kind of database.
* If a mistake is made, you can "turn back the clock" and compare earlier versions of the code to help fix the mistake while minimizing disruption.

---

## Has this ever happened to you?

![bg right:50% w:500](images/phd101212s.png)


---
## A brief history of Version Control

* We have likely all experienced or used some form of "automated" or implicit version control.
    - The "undo" and "redo" buttons!
    - "Track changes" or "version history" in Word, Google Docs, Overleaf, etc.

* Subversion (SVN) (2000s) Centralized (client-server)
Centralized version control systems are based on the idea that there is a single “central” copy of your project somewhere (probably on a server), and programmers will “commit” their changes to this central copy. “**Committing**” a change simply means recording the change in the central system.

---

![bg w:60%](images/centralvcs.png)

---

* git (2005): A *distributed* VCS.
In software development, distributed version control is a form of version control in which the complete codebase, including its full history, is *mirrored* on every developer's computer.
* See also Mercurial.

![bg right:55% w:650](images/distributedvcs.png)

---
### Why is it important?
There are many benefits but some of the key points are:
   * A single source of truth across teams, code, and assets.
   * Traceability and history for every change ever made.
   * Speed of development: Many collaborators can work on code in parallel.
   * Also functions as a multi-tiered backup system.
---
# Let's Get Started!

We will be using the git command line tool for this workshop. There are also GUI (graphical user interface) tools available, but version control is (still) generally done at the command line. 
* There is a limit to what the GUI tools can do and eventually you'll run into those barriers.

Let's see which version of git you have and also to check if it is installed.
```
git --version
```
---
### Set Config Values

```
git config --global user.name "Your Name"
git config --global user.email "Your Email"

git config --list

git config --help

```
(Type "q" to exit)

```
git help
```

---
## Two Common scenarios while using Git

### Scenario 1. Initialize a repository from existing code
This is the case that you have a local code base that you want to start tracking using git. So let's quickly create a few files. First, navigate to where you want them to be created.

```
# first you should probably navigate to a place you like to save temporary files/folders such as your desktop

mkdir testgit

cd testgit

touch testfile.txt

ls
```
---
Now, open up testfile.txt in any text editor and copy the following code into testfile.txt. This is python code, but you don't need to know python in order to follow this example.

```
def add_function(a, b):
    return a + b
    
def subtract_function(a, b):
   return a - b
```
Just to prove a point, make sure that you are inside the `testgit` directory (folder)

```
pwd
```
---
#### Track this code with git

```
git init

ls -la

#or 

ls -a
```
Because the .git directory is hidden, you need to use ```ls - la``` to see hidden files. The .git directory contains everything related to our repository. If you want to stop tracking this repo, you can just remove the .git directory as you would any other using ```rm -rf .git```

---
#### Our First Commit

Before we make a commit, let's check the status of the directory. You'll see which files are untracked in our directory.

```
git status
```

![bg right:60% w:650](images/gitworkflow.png)

---
#### Add files to staging area

Add all the files
```
git add -A
```
or add them individually

```
git add testcode.py
git add yourfilename.txt
```

if you made a mistake, you can remove them from the staging area

```
git reset yourfilename.txt
```
---

#### Commit the changes

When you make a commit, you want to write a *descriptive* message log. Not something generic like "made a bunch of changes". If you need to look back in the commit logs for when you made an error, you will thank your former self that you left some clues about where to look. 
```
git commit -m "Descriptive and detailed message about changes here"
```

Let's look at the status again to make the commit worked.
```
git status
```

Or we can look at the commit we just made

```
git log
```
---
### Scenario 2: Track an Existing Remote project with Git
This is probably more likely how you will use `git` in reality.
Someone, or a group, has probably has written some code that you want to download and use for yourself, make changes to, and develop over time. 

---
#### First, fork my repository
On github, in the top right corner of the screen, click 'Fork'. You'll make a copy of my *git-intro* repository for yourself. We are doing this so that we are not all pushing changes to my remote repository at once, but instead to your own repository as if it was the 'main'. 

  NB: If you are not a collaborator in a repo, you can't branch the repo. However, you can "fork" it, which just creates a clone of the repo on GitHub under your profile.
  1. You can't push changes directly back to the original repo.
  2. You'll want to work on keeping your fork in sync with the project
      * via adding the original repo as a remote and then using pull requests, or
      * fetching regularly from the original repo.

---
#### Clone a remote repository
This creates a local clone of a remote repository, and a `.git` directory is created to track changes.
```
git clone <url> <where to clone>
```
If you want to clone this repo to your current working directory, use the following. Remember, this should be a URL to **your own fork** of the remote repository, not mine!
```
git clone https://github.com/your-user-name/git-intro 
```
Navigate into the git-intro directory

```
cd git-intro
```

Check to see if this worked and view what is inside your current directory
```
ls -la
```
---
#### View information about the remote repository

List the information about the repository
```
git remote -v
```

List all of the branches in our repository (both locally and remote). We have not covered branches yet but we will very shortly!
```
git branch -a
```
---
#### Committing and Pushing Changes
Now that we have a locally cloned repo, we want to make changes to it. 

First, let's check the status of our code:

```
git status
```
Now, make a change to the `testcode.txt` file. Let's add a new `divide` function. Add the following code onto the end of the `textcode.txt` file:

```
def divide(a, b):
    return a / b
```

---

Let's see what `git` tells us about the changes:
```
git diff
```

Next, make a commit *LOCALLY*.
Remember, we are recording changes to the file on our local computer and no changes have yet been made to the remote repository.
```
git add testcode.txt
git commit -m 'Added divide function'
```
---
#### Push changes to the remote repository

Remember, we are now working on a project that could potentially have multiple developers. People could have been working on different parts of the code at the same time we were doing our own work, so first you want to pull any changes from the main branch of the remote repository that have been made since the last time we pulled from that repository.

Origin is the name of the remote repository. Main is the name of the branch we are pushing to.
```
git pull origin main
```
If everything is okay, let's now push our changes to the remote repository.
```
git push origin main
```

---
## Branches

So far, we have been working only on the `main` branch of our repository. But this really isn't how you should use version control and git in your day to day workflow. The `main` branch is typically the last tested/stable/robust version of your code.
This is where branching comes in. 

A common workflow is to create a branch for your desired feature/change, then begin working off of that branch.

---
#### Creating a branch

```
git branch calc-divide
```
Now let's see our local branches to make sure it worked correctly.
```
git branch
```
You see the `*` next to `main`, indicating we are still working in the main branch, even though we created a new one called `calc-divide`.

Now we move into our new branch; we must check out the branch in order to work in it.
```
git checkout calc-divide
```
Let's make sure we are now working in the right place.
```
git branch
```
---

If that is all correct, now we can work on our features. So let's make some changes to our code. Once we have done that let's commit the changes to our local branch.
```
git status

git add testcode.txt

git commit -m "changed divide function"
```
---
#### Push branch to remote repository
```
git push -u origin calc-divide
```
The '-u' tells git we want to associate our local `calc-divide` branch without remote `calc-divide` branch. 
```
git branch -a
```
Now see all our branches, both local and remote, to see that we have now created and pushed our `calc-divide` branch to our remote repository. 

We will now merge our `calc-divide` branch with our main branch. 

---

#### Checkout our local main branch
We want to do first do the merge locally *before* pushing it to the remote repository.
```
git checkout main
```
Pull any changes that have been made *upstream* while we have been working.
```
git pull origin main
```
Merge the branch
```
git merge calc-divide
```
Check to see that we have merged the branch
```
git branch --merged
```
Now, push the local main branch to the remote main branch
```
git push origin main
```
---
#### Delete the Branch
Now that we're done making changes and merged our branch, we can delete the branch. First, let's delete it locally:
```
git branch -d calc-divide
```
Don't forget, this branch now also exists in the remote repository. 
```
git branch -a
```
You should see that we still have the 'calc-divide' branch in the remote repository. Now delete it from the remote repository.
```
git push origin --delete calc-divide
```

This seems like a lot of work but it becomes very fast once you begin to do it regularly!

---
# Other Resources - Learn More!
There is a lot more you can do with git/GitHub.
We just touched on basic workflow using the command line.
There are a whole suite of tools available including using GitHub in the browser, downloading and installing a GUI version of GitHub to your desktop, and so on.
We can't possibly cover everything here, but there are lots of other resources that you can look at if you are in need. 

---
### Corey Schafer Youtube
From Erich Purpur - "Corey Schafer is just a guy with a YouTube page. However, when I am learning a new programming skill, tool, library, or concept, I first go to his page to see if he has done a tutorial and he usually has! When I was first learning Git/Github workflow, I constantly referred to his walkthrough video. [Here is a link](https://www.youtube.com/watch?v=HVsySz-h9r4). In fact, when I was preparing this tutorial I basically copied his video as I found it very easy to follow and did not think I could do it any better."

---
### StackOverflow
[StackOverflow](https://stackoverflow.com/) is basically a community question/answer site with users worldwide. You can ask technology specific questions and get an answer from a knowledgeable person and probably very quickly. If you google your question such as: "How to delete a branch github", odds are high one of the first results will be from StackOverflow. But be sure to look for an answer first before asking a new question on StackOverflow. 

---
### GitHub Flow
[https://guides.github.com/introduction/flow/](https://guides.github.com/introduction/flow/)

### Additional Items
  * Command Line Interface (CLI) [cheat sheet](https://education.github.com/git-cheat-sheet-education.pdf)
  * [Create your own Webpage](https://pages.github.com/)
---

### Thank you for your attention!

### Questions?


Thanks again to Erich Purpur, Ricky Patterson, and Software Carpentry.

