---
title: GWT and HTML under control
date: 2011-01-16T16:34:48Z
slug: "gwt-and-html-under-control"
aliases:
  - /blog/2011/01/gwt-and-html-under-control/
  - /posts/gwt-and-html-under-control/
disqus_identifier: "366559782"
categories: ["programming"]
tags: ["GWT"] 
---

In the past weeks I've seen two articles about making clean design with GWT. That was really interesting, but something still stugling me a lot. Both authors agreed not to use  layout widgets provided by GWT to build UI neither UiBinder but use instead a big ugly static String to store an HTML code/

Take a closer look at those two articles and come back later to find out a better way to improve your design based on GWT.

[Lightweight GWT layouts](http://i-proving.ca/space/Technologies/GWT/Lightweight+GWT+layouts)

[Tags first GWT](http://www.zackgrossbart.com/hackito/tags-first-gwt/)

The purpose is to write a clean HTML with the least number of tags and have that HTML code under control with only CSS. I do agree that using the old GWT layout system is a really mess producing a lot of tables and ugly code.

However, since GWT 2.0, Google brought us [a better layout system](http://www.google.com/events/io/2010/sessions/gwt-ui-overhaul.html) with the widgets positionned using only div and css. With this system you can normally reach a clean design without generating table tags and a lof HTML mess.

But since the web is all about simple html and css, I agree with Zack Grossbart that you need to have a perfect control of your UI with the least html tags and just css.

But with the version 2.0 GWT also introduces a wonderful feature called UiBinder. With UiBinder you can describe in a declarative way your design separated from your logic. You can especially use the HtmlPanel panel to mix up classic HTML/CSS and GWT widgets. This is exactly what they wanted to do ! Use classic HTML and CSS to describe the UI and GWT only for the control. By using UiBinder they can take advantage of a powerfull tool having a built-in feature to bind GWT widgets from the xml to the Java and many mores.

Putting your html within a static String is not really that clean and maintainable. So please, for God sake, do not do it ! [Learn the API](http://code.google.com/intl/fr/webtoolkit/doc/latest/DevGuideUiBinder.html) and [follow the best practices](http://www.google.com/events/io/2010/sessions.html#GWT).

[update] I've now coded a rewrite of the wordpress login page example just by using UiBinder and the HtmlPanel, see [this post]({{< relref "blog/2011-01-16-wordpress-login-form-with-gwt-and-uibinder.md" >}}).