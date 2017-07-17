+++
date = "2017-07-16T22:34:32-04:00"
title = "Fix the Git completion with Oh-my-Zsh on Mac"
slug = "fix-git-completion-zsh-mac-homebrew"
tags = ['zsh', 'git']
categories = ['programming']
banner = "img/banners/git-logo.svg"

+++


In a [previous post]({{< relref "blog/2016-11-27-my-development-environment.md" >}}), I have explained how I have setup oh-my-zsh with the git plugin.
I am also using homebrew to manage the packages installed on my Mac.
After upgrading Git recently, I have noticed the Git completion was not as powerful anymore.

<!--more-->

To explain how the Zsh completion is cool and supposed to be, here is an example of a correct completion:

~~~bash
git commit --<tab>
--all                           -- stage all modified and deleted paths
--allow-empty                   -- allow recording an empty commit
--allow-empty-message           -- allow recording a commit with an empty message
--amend                         -- amend the tip of the current branch
--author                        -- override the author name used in the commit
--cleanup                       -- specify how the commit message should be cleaned up
--date                          -- override the author date used in the commit
--dry-run                       -- only show list of paths that are to be commited or not, and any untracked
--edit                          -- edit the commit message before committing
--file                          -- read commit message from given file
--gpg-sign                      -- GPG-sign the commit
--include                       -- update the given files and commit the whole index
--interactive                   -- interactively update paths in the index file
--message                       -- use the given message as the commit message
--no-edit                       -- do not edit the commit message before committing
--no-gpg-sign                   -- do not GPG-sign the commit
--no-post-rewrite               -- bypass the post-rewrite hook
--no-status                     -- do not include the output of git status in the commit message template
--no-verify                     -- do not look for suspicious lines the commit introduces
--null                          -- separate dry run entry output with NUL
--only                          -- commit only the given files
--patch                         -- use the interactive patch selection interface to chose which changes to commit
--porcelain                     -- output dry run in porcelain-ready format
--quiet                         -- suppress commit summary message
--reedit-message                -- use existing commit object and edit log message
--reuse-message                 -- use existing commit object with same log message
--short                         -- output dry run in short format
--signoff                       -- add Signed-off-by line at the end of the commit message
--squash               --fixup  -- construct a commit message for use with rebase --autosquash
--status                        -- include the output of git status in the commit message template
--template                      -- use file as a template commit message
--untracked-files               -- show files in untracked directories
--verbose                       -- show unified diff of all file changes
~~~

The completion displays all the parameters for a Git command with a short documentation.

After updating Git with the `brew update` command here is what it looked like:

~~~bash
$git commit --<tab>
--all               --author=           --dry-run           --fixup=            --message=
--only              --reedit-message=   --short             --template=         --verbose
--allow-empty       --cleanup=          --edit              --include           --no-edit
--patch             --reset-author      --signoff           --untracked-files   --verify
--amend             --date              --file=             --interactive       --no-verify
--quiet             --reuse-message=    --squash=           --untracked-files=
~~~

Yep, all the integrated documentation was gone :(
I was preparing for a Git training at Ippon and I was disappointed not to be able to show this very useful feature to my colleagues new to Git.

It turned that the culprit is homebrew installing a completion for Git which overrides the Zsh completion.
You can verify this with:

~~~bash
$ls -l /usr/local/etc/bash_completion.d
total 64
lrwxr-xr-x  1 raphael  admin  64 Oct 21  2016 tig-completion.bash -> ../../Cellar/tig/2.2_1/etc/bash_completion.d/tig-completion.bash
~~~

The solution is to reinstall git with homebrew and specify not to install the bash completion.

~~~bash
$ brew uninstall git
$ brew install git --without-completions
~~~


And voilà!
