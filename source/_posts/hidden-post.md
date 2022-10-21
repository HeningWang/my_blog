---
title: How to hide posts in hexo
date: 2022-08-21 11:16:56
tags: [hexo]
---
# How to hide posts in hexo

Sometimes, we want to keep certain posts invisible to the public, but still traceable in the source folder for later use.

## Move files to _drafts folder

One way for doing that is to move a certain post to _drafts folder. Per default, hexo will not render and generate files in _drafts folder. Later one can use this command to publish it back to _posts folder.

```
$ hexo publish [layout] <title>
```
## Use hexo-hide-post plugin
### Installation
Another solution is to install this plugin from github: [hexo-hide-post](https://github.com/prinsss/hexo-hide-posts)
```
$ npm install hexo-hide-posts --save
```
### Configuration
After installation, add configurations in your _config.yml file.
```
# hexo-hide-posts
hide_posts:
  enable: true
  # Change the filter name to fit your need
  filter: hidden
  # Generators which you want to expose all posts (include hidden ones) to.
  # Common generators: index, tag, category, archive, sitemap, feed, etc.
  public_generators: []
  # Add "noindex" meta tag to prevent hidden posts from being indexed by search engines
  noindex: true
```
e.g. Set filter to secret, so you can use secret: true in front-matter instead.
### Usage
Add meta information to the front matter of your post accordingly, e.g.
```
---
title: 'Lorem Ipsum'
date: '2019/8/10 11:45:14'
hidden: true
---
```