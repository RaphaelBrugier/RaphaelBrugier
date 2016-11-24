+++
title = "git commit fixup"
date = "2016-11-23T18:37:21-05:00"
slug = "git-commit-fixup"
categories = ["programming"]
tags = ["git", "tips"]
banner = "img/banners/git-logo.svg"

+++
In this article, I will describe a git option to quickly fix a previous commit.
This sometimes happens when I want to fix a typo in a previous commit after few new commits.
The goal is to keep a "clean" git history with consistent commits adding features to facilitate the code reviews.

<!--more-->

(!) Of course, like any "rewriting history" command which modifies a previous command, it should be used with caution and never used to modify a commit already push.

## The options
The option is `--fixup` to create a commit fixing a previous one:

    git commit --fixup <commit to fix>

And to automatically apply the `fixup` commit when rebasing, add `--autosquash`:

    git rebase -i origin/master --autosquash


## Example:

Starting with the following git history:

    $git log --oneline --decorate
    d36dc2f code code code
    7add401 add README
    fb5b59c initial commit


If you realize you did a typo in the initial README file you can fix and want to modify the 'add README' commit instead of creating a new commit for a typo:

    ${fix the typo}
    $git add .
    $git commit --fixup 7add401


Git has created a commit with a message prefixed with '!fixup' :

    $git log --oneline --decorate
    7fd8071 (HEAD -> master) fixup! add README
    d36dc2f code code code
    7add401 add README
    fb5b59c initial commit


Now, you can rewrite the git history of the 3 previous commits with:

    git rebase --interactive --autosquash HEAD~3


And the result:

    $git log --oneline --decorate
    3ec6daa (HEAD -> master) code code code
    d6c4c24 add README
    fb5b59c initial commit

The `--autosquash` option has automatically reordered and applied the fixup commit.
Note that this option only works with an interactive rebase.

Since the `--autosquash` option only applies to fixup commit, it is safe to  enable it by default in the git config:
    
    git config --global rebase.autosquash true

And voilà! No more almost empty commits to fix typos.

[source](https://robots.thoughtbot.com/autosquashing-git-commits)