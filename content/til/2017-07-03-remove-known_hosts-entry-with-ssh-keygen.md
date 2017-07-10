+++
title = "remove known_hosts entry with ssh keygen"
slug = "remove-known_hosts-entry-with-ssh-keygen"
tags = ["ssh"]
categories = ["programming"]
date = "2017-07-03T09:26:07-04:00"
+++

The easiest way to remove an entry from the `.ssh/known_hosts` file is to use `ssh-keygen`:

```bash
ssh-keygen -R hostname
```
