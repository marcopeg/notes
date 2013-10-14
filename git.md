GIT Notes - Git by Console
==========================

## Clone from GitHub

Open a _Terminal_ session and point to the `/repos` folder which is meant to be a container for all your repositories. 

> At the end of this receipt your repo folder will be `/repos/repo-name`

    git clone git@github.com:account/repo-name

You need a valid _SSH Profile_ running on your computer.  
[https://help.github.com/articles/generating-ssh-keys]()
    



## Track Changes to Your Files

You use _GitHub_ to track changes on your files, pack them into _commits_ and collect a coherent history of your commits on _GitHub_ itself.

> As an advanced feature you can mix commits from different versions/forks of your repository.

    git status

This command list all your untracked files:

- new files
- deleted files
- changed files

**Display Differences**  
Git collects a detailed list of changes between the untracked version of each files and all it's history but it is a good idea to use a graphic client to look at changes!  
I use IntelliJ and SmartGit.




## Ingore Some Files

There may be some files you won't be part of you remote repository. Caches and temporaries, IDE settings and more.

You can create a `.gitignore` file into you repository root and list inside it all files you won't be part of your remote branches.

    // repositories/repo-name/.gitignore
    *.config
    cache/
 
> the `.gitignore` file need to be added to the repository and it will be part of your further commits!




## Prepare for Commit

Add files for the commit:

    git add file-path
    
Now `git status` will list all files that will be committed into the next commit.

If you want to remove a file from the next commit:

    git reset HEAD file-path

### Shortcuts

    // to add all changed files
    git add -u
    
    // to add both changed files and untracked files
    git add -a



## Commit

    git commit -m "notes fot this commit" 
    
If you just did a commit but find yourself to have forgot something you can **amend your last commit** by adding new changes to it:

    git add -A
    git commit --amend
    
> _Git_ ask you to edit your last commit notes (VI editor) if needed.


## Push to the Remote Origin

    git push origin master
    
Where `origin` means the source from which you cloned the repository and `master` is your local branch you want to push.



## Add a Remote Upstream

**WHY:** when you fork a repo you copy it's files into your own account, you can clone, edit and commit to your repo then create _pull requests_ to push your changes to the main repository. 

**But what happen when the main repository update it's files?**  
You still working on an old version of that repository but you probably won't!

**The solution** is to teach your repo about is original source then import that source when upate happens and merge it with your own changes.

    git remote add upstream git@github:account/repo-name

Once you have your upstream setted up you can show it's informations by:

    git remote show upstream



## Fetch the Upstream & Merge

When you know the original repository updates you might want to fetch fresh and up to date sources from that repo:

    git fetch upstream
    
This command download the remote upstream master branch into a local folder (into git's hidden archive).

The next step is to merge an upstream branch with your current working branch:
    
    git checkout master
    git merge upstream/master

The first command is to ensure you are working on your master branch, the second is to merge upstream commits into local active branch.

**NOTE:** for merge to work you need to have a clean working directory who means you need to commit or discard all untracked changes!





## Add New Branch

Branches are _versions_ of the same repository. You can work on a branch and commit on it some changes that have to be discussed before to be added to the very repository.

Each repository born with a `master` active branch but you can add how many new branches you want:

    git branch new-branch-name

A new branch start as a copy of the active branch so if you are in `branch01` and create `branch02` the latest wil start as a clone of the first.



## Switch Branch

You can quickly change branch and see your filesystem change accordling by:

    git checkout branch-name

You can also check which branch is currently in use by:

    git branch
    
(the starred one!)





## Push a Branch to GitHub

You can push branches to both `origin` and `upstream` (if you are allowed):

    git push origin branch-name
    git push remote branch-name



## Remove a Branch

    // remove local branch
    git branch -d branch-name
    
    // remote branch (GitHub)
    git push origin :branch-name
    

## Restore a Deleted Branch From Origin

    git fetch origin
    git checkout origin/test
    git checkout -b test
    
**NOTE:** above instructions works even on the `upstream`!