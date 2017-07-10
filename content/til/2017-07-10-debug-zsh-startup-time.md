+++
title = "How to debug zsh startup time"
slug = "debug-zsh-startup-time"
tags = [
  "til",
]
categories = [
  "zsh",
]
date = "2017-07-10T19:43:21-04:00"
+++

Use the zprof module to profile the zsh functions.

Add `zmodload zsh/zprof` at the beginning of the `.zshrc` file.

Add `zprof` at the end to print the result of the profling.

In my case the problem was nvm due to [this issue.](https://github.com/creationix/nvm/issues/539)

[source](http://jb-blog.readthedocs.io/en/latest/posts/0032-debugging-zsh-startup-time.html)
