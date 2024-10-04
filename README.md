# git

## Setting Up Git :
* instaling git 
  - *sudo apt install git-all*
  -  *git --version*

* Configure git with email & username
   -   git config --global user.email "you@example.com"
   -   git config --global user.name "Your Name"
   -   for check if added : git config --get user.email

## Git commits to commit :
* in the **work** directory , i create a subdirectory named **hello** inside this directory i general a file titled **hello.sh**
   - writting  **echo "Hello, World"** 
* initialize the hello directory using **git init**
* checking the status of the directory using **git status**
    - according to the terminal output , i add the file to staging area and commit the changes
* change the **hello.sh** content :
    - #!/bin/bash
    - echo "Hello, $1"
* staging the changed file and commit the changes

* Modify the **hello.sh** file
    - #!/bin/bash
    - Default is "World"
    name=${1:-"World"}
    echo "Hello, $name"
* Make separated commits
    - the first commit for the 3 line to comments
    - the second includ the line 4 and 5
## History :
* showing the history of the working directory using **git log**
* showing the One-line history using **git log --oneline** 
* **Controlled entries**
   - customize the output by specifying the number of entries using **git log --oneline -n 2**
    - the second entrie is to specifying and view commits made withing  the last 5 min using **git log --oneline -n 2 --since="5 minutes ago"**
* **Personalized Format**   
    - customize the format of commits including the commit hash, date, message, branch information , and author name using **git log --oneline --pretty=format:'* %h %ad | %s (%D) [%an]'
## Check it out :
* revert the working tree and captured in the first snapshot then print the content of **hello.sh** file using **git rev-list --max-parent=0 HEAD** and **git checkout 732189ce1788c2bce37d0902d5d10d8ff623c60f** and **cat hello.sh** .
* revert the working tree to the second most recent snapshot using **git rev-list --max-count=2 HEAD | tail -n1** and  and **git checkout 1427897a695698479c88c1d81e7dfe05c968f8d4** then print the content of **hello.sh** .
* return to the  last commit **git checkout master**

## TAG me :
* taging the current version as v1 using **git tag v1**
* tagging the previous version using,commit before the current one **git tag v1-beta HEAD^**
* navigating between tagged versions using **git checkout v1** and **git checkout v1-beta** 
* listing tags using **git tag**

## Changed your mind :
* modify the file and restore the changes usign **git restore hello.sh**
* adding the changes to staged area and reset the changing using **git reset hello.sh** and **git restore hello.sh**
* committing and reverting using **git revert HEAD** this is commit after revert *14b63dd (HEAD -> master) Revert "staged_and_commited"* and commit before *ef19f44 staged_and_commited*.  
* taging and removing commits using **git tag oops** then **git reset --hard  v1**  
* displaying logs with deleted commits using **git reflog**
* cleannign unreferenced commits using **git gc --prune=now**
* author information
    - add the author and commit it 
    - modify the last commit without changing the message using **git commit --amend --no-edit**
## Move it :
* create a subdirectory named lib and moving hello.sh into her using **git mv hello.sh lib/**
* creating Makefile int the root directory with the provided content and commit it to the repo
 ## Blobs, trees and commits :
 **Explain:**
 * *git rev-parse HEAD* git hash of HEAD
 * git cat-file -p HEAD
 * *git ls-tree* : i do that for know this SHA-1 hash blob or tree
 * git ls-tree -r <commit_hash> --name-only | grep lib/
 * git show <commit_hash>:hello.sh 
 ___
 * Exploring **.git/** using **ls -la .git/ref or objectif** and **cat .git/config or HEAD**
* Hash the latest object  using **git log -1 --pretty=%H** then **git cat-file -t the commit** then **git cat-file -p the commit**
* dump the content of the /lib directory and hello.sh using **git ls-tree -r HEAD -- lib** then **git cat-file -p HEAD:lib/hello.sh**

## Branching :
* creating local branch named greet and switch to it using **git branch greet** then **git checkout greet**
* create greeter and file it by content 
* update hello.sh
* update makefile
*switch again to master branch to see the different between branches using **git diff master..greet -- makefile lib/hello.sh lib/greeter.sh**
* generate readme.md file
* draw a commit tree diagram using **git log --graph --oneline --all**
(
* f005157 (HEAD -> master) create readme by master
| * d25b46c (greet) update Makefile by greet
| * e0a7523 update hello.sh by greet
| * 1315ae7 create greeter
|/  
* 4c36418 Create Makefile
* 357e9ed move hello.sh to lib
* 4d6a51f author information
| * 14b63dd (tag: opps) Revert "committing and reverting"
| * ef19f44 committing and reverting
|/  
* 4430b10 (tag: v1) The second commit should include changes made to lines 4 and 5
* 8afcccb (tag: v1-beta) The first commit should be for the comment in line 3.
* 2b4f2d9 update hello v1
* 1ccc012 hello.sh v1
)
## Conflicts, merging and rebasing :
* mergin from the master branch into the greet branch using **git checkout greet** then **git merge master**
* go back to master and make teh changes to the hello.sh using the same commands git
* go to greet and try to merge it with main but there is a conflict
    * <<<<<<< HEAD
source lib/greeter.sh

name="$1"
if [ -z "$name" ]; then
    name="World"
fi

Greeter "$name"
    =======
echo "What's your name"
read my_name

echo "Hello, $my_name"
>>>>>>> master 

* resolve the conflict using **git config --global merge.tool meld** and after that **git mergetool**
* go back before start mergin :
    * git reflog .
    * return to commit before do merge : git reset --hard bd9243b
* rebase teh greet branch on top of the latest changes in the master branch using **git rebase master**

* merge from greet branch into master branch

* fast-forwarding mergin rebasing:
    * Merging keeps branch history by creating a merge commit, while Rebasing rewrites the history to make it linear without a merge commit.
    * Fast-Forward happens when a branch is directly ahead of the current branch, allowing Git to simply move the pointer forward without creating a new merge commit.

## Local and remote repositories :
* clone hello repo using **git clone hello cloned_hello**
* show logs for the cloned directory using **cd cloned_hello** then **git log --oneline**
* display the name of the remote repo using **git remote -v** then **git remote show origin**
* list all remote and local branch using **git branch -a**
* make changes to original repo in readme file 
* go to cloned_hello and fetch the changes from remote using **git fetch** then  **git merge origin/master**
* add local branch named greet using **git checkout -b greet origin/greet**
* add remote to your git repo and push master and greet using **git remote add new-remote url** then **git push new-remote master/greet**

 ## Bare repositories :
 * A bare repository is a Git repository that doesn't have a working directory; it only contains the version control information and is typically used as a central shared repository for collaboration.
 * using **cd ~/Desktop/linear-stats/git/work** then
**git clone --bare hello hello.git**
* using **git remote add shared ../hello.git**
* change in Readme file
* commit changes and push them
* switch to cloned repo and pull the changes using **git pull origin master**
* for push from cloned_hello :
    * git remote -v
    * git remote add shared ../hello.git
    * github  https://github.com/Mossab601/git.git (fetch)
     github  https://github.com/Mossab601/git.git (push)
     origin  https://learn.zone01oujda.ma/git/midbenke/git.git (fetch)
     origin  https://learn.zone01oujda.ma/git/midbenke/git.git (push)
     shared  ../hello.git (fetch)
     shared  ../hello.git (push)

*Thanks*.


git remote add origin https://github.com/Mossab601/git.git

git push origin --all

*thank you*
