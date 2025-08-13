# Git

By Muhammad Abdullah

* * *

Git helps developers keep track of the history and structure of changes made
in their code repository as a distributed systems tool. This tutorial will
help you understand the core Git commands and procedures.

## Installation & Configuration

This guide will cover how to install Git on a Linux system, configure it
correctly and perform core tasks. The below is for installing the git on your
linux:

    
    
    sudo apt-get install git

Once installed, configure it by setting your username & password:

    
    
    git config --global user.name "Your Name"
    git config --global user.email "Your Email"
        

## Check if Git is Installed & Configured

The following will ensure that Git is installed and set up on your Linux; do
this in your terminal. Type the following command:

    
    
    git --version

If Git is installed, then this command will return the installed version of
Git, something like:

    
    
    git version 2.34.1

If Git is not installed, then you will get a message saying the command is not
found, in which case you'd need to install Git.

You'll need to verify whether Git has been configured with your user details:
your name and email address. This is crucial because Git uses this information
to attach commits to you.

To see what the global username configuration is currently set to, you would
run:

    
    
    git config --global user.name

To check the globally configured email, use:

    
    
    git config --global user.email

If set up, these will return the name and email that you setup. If they output
nothing then you haven't configured your username and email globally.

## Set up SSH Key for GitHub

The SSH protocol allows you to connect to and authenticate to remote servers
and services securely. SSH keys can securely connect you to your Git server
without being asked for a username and password each time.

To generate an SSH key, copy the following command and paste it in your
terminal:

    
    
    ssh-keygen -t rsa -b 4096 -C "your.email@example.com"

Run the command. You can give it a passphrase if you want, or just press
"Enter" if you don't want to set a passphrase.

Start ssh-agent in the background:

    
    
    eval "$(ssh-agent -s)"

Add your SSH private key to the SSH agent:

    
    
    ssh-add ~/.ssh/id_rsa

To copy the SSH Key, better to display it on your terminal first by using
follwing command:

    
    
    cat ~/.ssh/id_rsa.pub

A key will be shown on your terminal. Just copy it using Ctrl+Shift+C. Go to
your GitHub account and go to Settings > SSH and GPC Keys > New SSH Key and
paste your copied SSH key.

After walking through the above procedure, just execute the following command
to test your connection:

    
    
    ssh -T git@github.com

If successful, you will have a message like:

    
    
    Hi username! You've successfully authenticated, but GitHub does not provide shell access.

## Integrate Git with VS Code

First, make sure you have downloaded and opened the Visual Studio Code on your
linux. If you have already logged in to your github in VS code, skip this
part.

You will see an icon that looks like a profile of a user located at the bottom
left corner of your VS Code window. This icon will appear once you try to sign
in to github but have not done so yet. Once you click on the profile icon, it
will show a dropdown menu. If you are not signed in yet, then it should have
"Sign in to GitHub" or "Sign in with GitHub" appearing in the menu.

Once you click "Sign in to GitHub," VS Code will either open a dialog box or
direct you to your default web browser for authentication. If VS Code
redirects you to a browser, you are going to be asked to log in to your GitHub
account. You will then be asked to allow VS Code to access the account. This
will enable VS Code to handle all repository, setting synchronization, and
other activities with Git.

After you log in, come back to VS Code and click on the profile icon again. In
the dropdown menu, right at the top, you should now see your GitHub username -
that is how you would know you've signed in successfully.

## Master Basic Git Commands

You can either initialize any directory to be a git repository and then push
it on your GitHub account, or you can clone any existing git repository to
your directory. You will be covering both approaches as you go further with
the tutorial. Let's start with initializing a directory to be a git
repository, and then later on, go the other way.

###

  1. Initialize a Repository

Open VS code and go to File > Open Folder... and select your directory. Press
Ctrl+Shift+G to open Source Control in the VS Code. If your directory isn't a
Git repository, you'll see an option to Initialize Repository. Click it and
that's it.

If you're using the terminal then go to that directory you want it to be a git
repository and type a command:

    
    git init

It will initialize current directory to a git repository. Confirm that indeed
it is now a git repo. Type:

    
    ls -a

If you find .git in that list, then it is indeed a git repo. Once you've
created a Git repository in your directory and you have made changes you'd
like to push up to GitHub. For that go to the github account and Click the "+"
icon in the top-right corner of the GitHub page and select "New repository."
Enter a name for your repository, such as my-project. You can also add a
description, set the repository to public or private. Click "Create
repository." After the repository is created you will be taken to the main
page for that repository. You will want to copy the HTTPS or SSH URL for the
repository. It will look something like this:

    
    https://github.com/username/my-project.git

In the terminal, now link your local repository to the remote repository on
GitHub using:

    
    git remote add origin https://github.com/username/my-project.git

Replace [https://github.com/username/my-project.git]() with the URL of your
GitHub repository.

###

  2. Clone a Repository

f you want to clone some existing repository, then navigate into the directory
where you'd like to store the cloned repo using the cd command on the command
line. Use the another git command followed by the URL of the repository:

    
    git clone https://github.com/username/repository.git

Replace [https://github.com/username/repository.git]() with the URL you copied
from the repository. Then you can go inside that repo by again using cd
command.

###

  3. Status of Files in Repository

If you've either initialized or cloned a repository you can view status of the
existing files in that repository simply by typing:

    
    git status

It will say things like - the files are either untracked, modified, staged or
deleted.

###

  4. Staging Files in Repository

If you have a specific file and you want to keep a track of it, you'll stage
it by using command:

    
    git add Filename

To stage all changes in a repo, better to type:

    
    git add .

When you run this command, it says to Git in effect, "I'd like to include the
changes I've made to this file in my next snapshot of the project." Once a
file has been added (using git add), it is now out of the "modified" or
"untracked" state and into the "staged" area. It's now ready to be committed
into the repository.

###

  5. Committing Files in Repository

Once you staged the files, then these are ready to be captured. It will be
committed by using the following command:

    
    git commit -m "Commit message"

It takes a snapshot of the staged changes in your repository along with an
informative message describing what changes were done. That message tells
others and, even more importantly, yourself in the future, what was done with
this commit.

###

  6. Pushing to Remote Repository

The following command is used to upload your local commits to the remote
repository on GitHub.

    
    git push origin main

###

  7. Pulling from Remote Repository

The following command fetches updates from the main branch of the remote
repository and then merges those updates into your current local branch.

    
    git pull origin main

###

  8. Creating and Switching Branches

To create a new branch in a repository, use the command:

    
    git checkout -b new-branch

After creating a branch, if you want to switch to that branch, then type:

    
    git checkout branch-name

These are some of the commands that might allow you to use various branches on
your project-interactively isolate various changes or features you are going
to work on independently of others.

###

  9. Merging Branches

To merge a branch into the current branch:

    
    git merge branch-name

This command merges changes from the branch-name into the current branch. In
other words, when this command is run, Git takes all the changes made on
branch-name and integrates them into the branch on which you are working. This
is quite often done after you have completed work in a feature branch and
would like to bring the changes into the main branch, like main or develop.
This will be automated if there are no conflicts. If there are conflicts, Git
will require you to resolve the conflicts before the merge can complete.

###

  10. Viewing Commit History

To view the commit history:

    
    git log

This is very useful in order to review changes made during some time and all
the history of changes which have been carried on during certain period of
time in order to understand who made what change and when.

## Conclusion

By following these steps, you will get a solid understanding of Git. As you
become comfortable with these commands, you'll be able to branch out and
continue into more advanced Git topics: rebasing, cherry-picking, interactive
staging, and others. Of course, practice makes perfect, but in no time you'll
be a professional!

Here's you can look for advanced [Git
Commands](https://drive.google.com/file/d/1FWMZdpV878CFS6qhC3mWU2jEbUZZZudA/view).

