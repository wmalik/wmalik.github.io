---
layout: post
title: "Bye Wordpress, Hello Jekyll!"
description: "I switched to Jekyll"
modified: 2014-02-08
category: articles
tags: general
---

### Bye wordpress
My hosted [wordpress][wp_url] account expired last week. I had two options, I could renew my hosted subscription, or I could install wordpress on a remote machine myself. I chose the latter, primarily so that I don't have to pay an annual subscription fee. However, while installing wordpress, I realized how big a beast wordpress is. I don't think it is worth it to maintain and configure php, mysql, nginx (or apache) just for writing blog posts.

### Hello Jekyll
[Jekyll][jekyll_url] is a static site generator which can be used as a blogging engine. It converts plain text files to static HTML pages which can be served by your `$WEBSERVER`. The best part is that Jekyll does not need a database software (e.g. MySQL) to store data, everything (data and metadata) is stored in cleartext files under one directory. That makes it very easy to move the blog around. Writing blog posts is easy, just write the post in your `$EDITOR`, copy the file to the `_posts` directory and reload Jekyll. That's it (more or less).


Jekyll is lightweight, fast and quite easy to configure and maintain. If you are not using it, I would definitely recommend that you give it a try.

There are [loads][tuts_url] of resources and tutorials for jekyll, and there are some nice themes available [here][themes_url].

[wp_url]: http://wordpress.com
[jekyll_url]: http://jekyllrb.com
[tuts_url]: https://encrypted.google.com/search?hl=en&q=jekyll%20tutorials
[themes_url]: http://jekyllthemes.org/
