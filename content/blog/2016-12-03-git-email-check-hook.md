+++
title = "Add a Git hook to automatically verify a repository's email"
tags = ["git", "dotfiles"]
categories = ["programming"]
banner = "img/banners/git-logo.svg"
date = "2016-12-03T13:40:15-05:00"
slug = "git-verify-email-hook"
+++
I use Git a lot and I often have to switch between my personal repositories (ie: Github) and my professional (Ippon) repositories on the same laptop.
My default Git email is configured to my personal email and I have often forgotten to configure it to my professional email when creating/cloning a repository for my company.
Like everything in Git, this can be automated to avoid mistakes.

In this post, I will show how I use a Git hook to check the email configured in any repository before every commit.

<!--more-->

# Email configuration per repositories:

When you first installed Git you probably have set your username and email globally with these commands:

~~~bash
$ git config --global user.name "Raphael Brugier"
$ git config --global user.email "raphael.b_____r@gmail.com"
~~~

You can override this values per repository:

~~~bash
$ cd some-ippon-project
$ git config user.email "rbrugier@ipponusa.com"
## print the configuration
$ git config user.email
> rbrugier@ipponusa.com
~~~

# Git hooks
Git hooks are scripts that Git executes before or after Git commands. 
For example: commit, push, rebase, ...

They are located in every Git repository in the .git/hooks/ folder.
By default, Git creates hooks suffixed with ".sample"
To create a custom hook, create a new file without the suffix for the hook you want to create  and add the Bash code for your needs:

~~~bash
$ vim .git/hooks/pre-commit
## add the hook code as any bash script
~~~

# Global Git hooks

Since [its version 2.9](https://github.com/blog/2188-git-2-9-has-been-released), Git offers an option to share the hooks globally between the repositories. I prefer this option to duplicating hooks in every repository, even if you could use templates to automatically copy them in every new repository.

Create a hooks folder, for example in your [dotfiles]({{< relref "blog/2016-11-27-my-development-environment.md" >}}), and configure Git globally to use this folder as the hooks folder for every repository:

~~~bash
$ mkdir -p ~/dotfiles/_git/hooks/
$ git config --global core.hooksPath ~/dotfiles/_git/hooks/
~~~

Git has added the option in your global Git config:

~~~bash
$ cat ~/.gitconfig
...
[user]
    name = Raphael Brugier
    email = raphael.b_____r@gmail.com
[core]
    hooksPath = ~/dotfiles/_git/templates/hooks/
...
~~~

# Check email hook

The goal is now to use a hook to validate the email set in the repository before committing.

I follow a simple convention to differentiate my personal and professional repositories.
My professional repositories are located on `~/devs/ippon` and my personal on `~/devs/github/` or `~/devs/projects/`

Write a bash script to check the email configured based on the repository's path as a pre-commit hook :

~~~bash
$ vim ~/dotfiles/_git/hooks/pre-commit
~~~

~~~bash
#!/bin/sh
PWD=`pwd`
if [[ $PWD == *"ippon"* ]] # 1)
then
  EMAIL=$(git config user.email)
  if [[ $EMAIL == *"ippon"* ]] # 2)
  then
    echo "";
  else
    echo "email not configured to Ippon in Ippon directory";
    echo "run:"
    echo '   git config user.email "rbrugier@ipponusa.com"'
    echo ''
    exit 1; # 3)
  fi;
fi;
~~~ 

1. check if the current path contains "ippon"
2. check if the configured email also contains "ippon"
3. if not, exit the script with an error. This will cancel the commit.


# Result
Test the hook is working in a new repository:

~~~bash
$ mkdir -p ~/devs/ippon/testhook/
$ cd ~/devs/ippon/testhook
$ git init
$ git commit -m "test"
> email not configured to Ippon in Ippon directory
> run:
>   git config user.email "rbrugier@ipponusa.com"
~~~

# Conclusion
Git hooks are powerful to automate tests and avoid mistakes. Other common usages of hooks are to trigger tests before a commit or a deployment after a push.
 