+++
tags = [ "til "]
categories = [ "network" ]
date = "2017-07-13T20:10:09-04:00"
title = "How to find your ip with command line"
slug = "2017-07-13-how-to-find-your-ip-with-command-line"
+++

When you need to determine your public IP - eg for a script - you can use some public websites as an API with Curl:

For example, I have used [ifconfig.me](http://ifconfig.me/):

~~~bash
$ curl ifconfig.me
57.22.56.xxx
~~~

Alternatively, the `dig` command is faster:

~~~bash
$ dig +short myip.opendns.com @resolver1.opendns.com
57.22.56.xxx
~~~
