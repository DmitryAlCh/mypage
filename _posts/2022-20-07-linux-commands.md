---
layout: post
title:  "Linux commands"
date:   2022-05-30 18:17:11 +0200
categories: [linux]
published: false
---

#### Helpful commands

##### <a name="fc">`fc` - open last command in editor

Imagine writing longer commands and making a typo in middle/beginning,
like: `sudo apt-gEt update && sudo apt upgrade && sudo apt autoremove`
receiving: `sudo: apt-gEt: command not found`
Usual approach would be to use the UP key, and then with arrow keys, move 
towards the typo. 
While typing `fc` will bring the text-editor of Your choice. Ofc have little 
value if the editor is not VIM, since navigating text in `NANO`
