[[Index]] 

## Vscode Tips & Tricks

  

```bash 

To search and replace all instances of a value in vscode:

  
  

ctrl + f : Enter the value in the box

  

. Then hit arrow on left side of box

  

. enter the value you want to find in the 1st box

  

. enter the value you want to replace in 2nd box


to open vscode in a certain directory cd to that dir and enter: code .

Also add git graph and indent rainbow in vscode

```  

# Git Commands

  

## To clone repository

  

```bash   

  

1) git clone <url>  
 
2) git checkout <branch_name>

3) Make needed changes to file and save 

4) Check files edited - git status
  

5) To add untracked Files - Remember to save the file 1st

  

 

  

git add <file_name>

git add <dir>/* a etc

git add -i 

git add - p

then you see:

  

*** Commands ***

  1: status       2: update       3: revert       4: add untracked

  5: patch        6: diff         7: quit         8: help

What now>

hit 4 (add untracked), then you see

  

What now> 4

  1: file1

  2: file2

Add untracked>>
 

  

6) Commit the file -  git commit -m "comment"

  

7) Push changes to repo


git push

``` 

  

## Checkout branch and create new branch locally

  

```bash   

git checkout <branch name> 

git checkout –b <new branch name>

``` 

  

## Discard changes in working directory

  

```bash 
git checkout -- <filename>
``` 

  

## Check branches  

  

```bash 
git branch  
``` 

  

## Shows the status of the repository

  

```bash 
git status  
``` 

  

## Using Linux in git:

  

```bash 
git rm  etc
``` 

  

## Pull in changes from repo:

  

```bash 
git pull  
``` 

  

## Revert commit:  

  

```bash 
git revert <git_id> 
```  

## Remove Files From Git Commit


In order to remove some files from a Git commit, use the “git reset” command with the “–soft” option and specify the commit before HEAD. 

```bash 

  $ git reset --soft HEAD~1

```   

When running this command, you will be presented with the files from the most recent commit (HEAD) and you will be able to commit them.

Now that your files are in the staging area, you can remove them (or unstage them) using the “git reset” command again.
 
```bash  
$ git reset HEAD <file>

``` 



## Delete branch:

  

```bash   
git branch -D <branch_name>   
```  


## To checkout local changes not yet pushed to git:
 
  
```bash  
git checkout filename 
``` 

  

## To checkout local changes not yet pushed to git on a different branch:  

```bash 
git checkout  --filename
``` 

## Checkout file that is in source control  

```bash      

git checkout origin/<branch_name> -- filename

``` 


## Check Logs

  

```bash 
git log
``` 

  
# Dealing with conflicts when merging  in Vscode

```bash 

• Checkout source branch

• git merge to destination branch

• Any conflicts ctrl + f <<<<<< and remove old changes

• git status

• git add any changed file

• git commit + push

• In vscode select stage change option and save file

```  

# Update Branch locally after deleting in Bitbucket/Github

  

```bash   

 -> git branch -D branch_name

 -> git pull

```  


# Etiquette for Pull Requests:
```bash    
-> Keep the Jira ID in the heading of the PR (This automatically links both together)

 git commit -m "comment"

``` 

## Git add and commit together - For files already present in git

  
```bash  

git commit -am "comment"  

``` 

## Shortcut Commands for Editing in Vscode

```bash 
-   Copy: Ctrl + C
    
-   Cut: Ctrl + X
    
-   Paste: Ctrl + V

-   Select all: ctrl + A

-   Undo: ctrl + u

-   Redo: ctrl + r
``` 

[[Index]] 
