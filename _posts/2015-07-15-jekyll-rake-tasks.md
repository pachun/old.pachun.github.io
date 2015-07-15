---
layout: post
title: "jekyll rake tasks"
description: ""
category:
tags: ["jekyll", "rakefile"]
---

When I first created my blog using jekyll [I followed these
steps](http://jekyllbootstrap.com/usage/jekyll-quick-start.html), which
instructed me to clone [a repository that contained a skeleton jekyll project](https://github.com/plusjade/jekyll-bootstrap). As I began trying out new
[jekyll themes](http://jekyllthemes.org/), I realized none of them had these
really helpful rake tasks that the first skeleton project had.

![jekyll rake tasks](/assets/images/jekyll_rake_tasks.png)

I find myself going back for that Rakefile [every time](http://www.grammarly.com/handbook/grammar/adjectives-and-adverbs/18/every-time/) I try a new theme. I recommend using it.

```
brew install wget
cd your_blog
wget https://raw.githubusercontent.com/plusjade/jekyll-bootstrap/master/Rakefile
```

Some of the content generating tasks output jekyll-bootstrap specific header
content, which will crash your localhost jekyll server. You can just deal with
those lines, deleting them after every page generation and restarting your
server, or you can [use this copy, which has those lines removed and will not
crash your jekyll server](https://gist.github.com/pachun/5726e08b00c1b4a1a0c7).

