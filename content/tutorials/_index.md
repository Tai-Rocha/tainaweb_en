---
title: 
description: |
  Here is a space of reports and tutorials
author: "Tainá Rocha"
show_post_thumbnail: true
show_author_byline: true
show_post_date: true
show_post_time: true
# for listing page layout
layout: list # list, list-sidebar

# for list-sidebar layout
sidebar: 
  title: tutorialss that Last
  description: |
    This is a list of tutorials.
    
    Check out the _index.md file in the /tutorials folder 
    to edit this content. 
  author: "Tainá Rocha"
  text_link_label: Subscribe via RSS
  text_link_url: /tutorials/index.xml
  show_sidebar_adunit: false # show ad container

# set up common front matter for all pages inside blog/
cascade:
  author: "The R Markdown Team @RStudio"
  show_author_byline: true
  show_post_date: true
  show_post_time: true
  show_disqus_comments: false # see disqusShortname in site config
  # for single-sidebar layout
  sidebar:
    text_link_label: View recent tutorialss
    text_link_url: /tutorials/
    show_sidebar_adunit: false # show ad container
---

** No content below YAML for the tutorials _index. This file provides front matter for the listing page layout and sidebar content. It is also a branch bundle, and all settings under `cascade` provide front matter for all pages inside tutorials/. You may still override any of these by changing them in a page's front matter.**
