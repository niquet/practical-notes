# Git and Github

- 

## Introduction to Git

- Git is one of many existing Version Control System (VCS)
- Git is a distributed version control system
- Version control is a software that tracks and manages changes to files over time
- VCS' generally allow users to revisit earlier versions of the files, compare changes between versions, undo changes, and more
  - Track changes across multiple files
  - Compare versions
  - Go back to old versions
  - Revert to a previous version
  - Collaborate and share changes
  - Combine changes
- Git is a version control software that runs locally on your machine
- You can use Git without registering for an account, the internet, or GitHub
- GitHub is a service that hosts GitHub repositories in the cloud
- GitHub makes it easier for people to collaborate with other people
- You need to sign up for an account to use GitHub

- Git is a command-line tool
- To use it, you run various git commands in a Unix shell
- All documentation and resources online will refer to the command line
- Can be a very fast experience once you get comfortable with it
- Some of the advanced Git features are only available on the command-line
- Commands are always the same, no matter what machine you are on
- Not the most beginner and user friendly experience, but at the very core of Git
- Can be difficult to learn and remember the commands at first
- There are many graphical user interfaces (GUIs) for Git that allow you to use Git without the command-line
  - GitHub Desktop
  - SourceTree
  - Tower
  - GitKraken
  - Ungit
-  Lower barrier-of-entry for beginners
-  Friendler to use with a potentially better epxerience
-  You might prefer the visual experience
-  Obfuscate and hide away the inner-workings of Git
-  Often lead to dependence on a particular piece of software
-  When things go seriously wrong, it can be challenging to fix without the command-line
-  Interfaces, buttons, and menus vary between different UIs

## Installing Git

### MacOS

- Most Macs these days come with Git pre-installed
- Check if and which version you have installed by running the following command

```bash
git --version
```

- If you have Git installed, you should see a command-line output matching the pattern `git version X.XX.X`
- If you don't have Git installed, you will get a `command not found: git` command-line output

- To download the newest version of Git, go to the git documentation website and click download
- Next, download the latest binary installer for Git
- Double clicking on the downloaded file
- Open the .pkg file
- Install Git by following the steps of the installer
- Check if the installation was successful by running the following command

```bash
git --version
```

### Windows

- Git was designed to run on Unix-based interfaces (like Bash)
- Windoes comes with a different default command-line interface called Command Prompt (not Unix-based)
- Git Bash emulates the unix-based Git command-line experience for Windows machines (and comes with Git too)

1. Download Git For Windows (from the official Git website)
2. Find the downloaded .exe file and open it to execute the Git Setup Wizard
3. Complete the setup
4. Launch Git Bash

### Git UIs

- ...

## Setting up Git

- You will need to configure Git after a successful installation
- You will need to provide your **name** and tour **email**
- Configure the name that Git will associate with your work by running the command below

```bash
git config --global user.name "username"
```

- Configure the email that Git will associate with your work
- When using GitHub, you will want your Git email address to match your GitHub account's email address

```bash
git config --global user.email "email@address.com"
```

## Git Basics

- Git repository ("repo") is a workspace which tracks and manages files within a folder
- We can have as many repos on our machine as needed, all with separate histories and contents
- Anytime we want to use Git with a project, we need to create a new Git repo (before you can do anything git-related, you need to initiliaze a repo first)
- Use `git init` to create a new git repository
- Note: Initialize the repo in the top-level folder containing your project (you do this only once per project)

```bash
git init
```

- `git status` givees information on the current status of a git repository and its contents

```bash
git status
```

- The `.git` folder 

### Commits

- 

### Branches

- 

## Introduction to GitHub

- 

## Credits

<a name="git-github-bootcamp"></a>\[1\] C. Steele, "The Git & Github Bootcamp: Master the essentials and the tricky bits: rebasing, squashing, stashing, reflogs, blobs, trees, & more!", Jan. 2024. [Online]. Available: [https://www.udemy.com/course/git-and-github-bootcamp](https://www.udemy.com/course/git-and-github-bootcamp). [Accessed: 3 Apr. 2024].

\[[1](#git-github-bootcamp)\]
