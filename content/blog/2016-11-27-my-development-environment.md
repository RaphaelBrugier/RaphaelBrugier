+++
title = "My development environment"
slug = "my-development-environment"
tags = ["git", "dotfiles", "terminal"]
categories = ["programming"]
banner = "img/banners/oh-my-zsh-logo.png"
date = "2016-11-27T18:02:03-05:00"
+++

A customized development environment could be a huge productivity boost in the day to day work.
In this post, I will share the tools and configurations I currently use.

<!--more-->


# Terminal: iTerm 2 
I use [iTerm 2](https://iterm2.com/documentation-shell-integration.html) 2 instead of the default terminal on mac.
On linux, I used terminator.

iTerm 2 adds several features, the most important for me is the ability to have multiple panes and tabs.



# Shell: Zsh

Zsh is a shell very similar to bash, but it adds a lot of cooler features: 

- a programmable autocompletion. You can navigate with the arrows keys the completion for example when browsing a directory
- a super history substring search


      brew install zsh
      
# oh-my-zsh and Zgen to customize Zsh
[Oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh) is probably the most well known set of plugins to enhance Zsh.
It's a super git repository of plugins, which you can enable to add aliases and parameters completion to your favorites tools.
I have used it for 3 years and have now switched to [Zgen](https://github.com/tarjoilija/zgen) to manage my plugins.
Zgen loads the plugins faster than oh-my-zsh and more importantly can load plugins from various github repositories.


Here is my current list of plugins:

~~~bash
  #from zsh
  zgen oh-my-zsh
  zgen oh-my-zsh plugins/autojump
  zgen oh-my-zsh plugins/colored-man-pages
  zgen oh-my-zsh plugins/docker
  zgen oh-my-zsh plugins/docker-compose
  zgen oh-my-zsh plugins/git
  zgen oh-my-zsh plugins/golang
  zgen oh-my-zsh plugins/sudo

  #theme
  zgen load bhilburn/powerlevel9k powerlevel9k

  #extra plugins
  zgen load djui/alias-tips
  zgen load zsh-users/zsh-syntax-highlighting
  zgen load zsh-users/zsh-history-substring-search
  zgen load zsh-users/zsh-autosuggestions
~~~


## autojump

[autojump](https://github.com/wting/autojump) is a faster way to navigate the directories, to jump to a directory that contains "Foo" in its name:

      j foo
      
Install it with homebrew and then just add the corresponding oh-my-zsh plugin to load it in your $PATH:

    brew install autojump

## djui/alias-tips

This Zsh plugin will suggest the corresponding shorter alias after you type a command. Very useful when used in combination with one of the the oh-my-zsh plugins which could add a lot of aliases.


# dotfiles
I've started versioning my "dotfiles" configuration a few years ago.
For this I use a git repository in my home: ~/dotfiles
The repository structure follows the hierarchy to install in my home directory.
For each file, a symbolic link is created in my home directory to point to the real file in my dotfile repository.

The install script replace the "_" character with a dot:

~~~bash
!/bin/bash

# adapted from https://github.com/skeeto/dotfiles/blob/master/install.sh

## Install each _-prefixed file or dir
find . -regex "./_.*" -type f -print0 | sort -z | while read -d $'\0' file
do
    dotfile=${file/.\/_/.}

    ## Install directory first
    if [ ! -e $(dirname ~/$dotfile) ]; then
    	echo "create directory: "
    	echo $(dirname ~/$dotfile)
        mkdir -p -m 700  $(dirname ~/$dotfile)
    fi

    ## Create a link to the repository version
    echo Installing $dotfile
    ln -fs $(pwd)/$file ~/$dotfile
    chmod go-rwx $file
done
~~~


# Dev tools:

The other tools I use every day, which would require a post on their own for each

### IntelliJ
The best IDE! The refactoring tools are still impressive, and too often unknown.

### Docker
I use Docker to test technologies, start and throw away tools for my projects, etc