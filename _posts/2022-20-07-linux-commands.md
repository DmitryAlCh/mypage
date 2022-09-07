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

##### <a name="cd -">`cd -` navigate back to previous directory
##### <a name="ctrl + l">`Ctrl + l` clear screen
##### <a name="ctrl + r">`Ctrl + r` switch to command search mode 
#### Neovim build from source
`sudo make distclean` to remove old stuff
`sudo make`
`sudo make install`

#### Purging old kernels.
Sometimes You can't `sudo apt-get upgrade` anymore it gives some error.
Especially when disk is encrypted, `boot` partition tends to be smallish.
So just check `df /boot/`: 
`You@At-your-pc > df /boot/
Filesystem     Type  Size  Used Avail Use% Mounted on
/dev/nvme0n1p2 ext4  704M  308M  345M  48% /boot
`
should be some sane `Use%`number. If it is above 90% cances are You have old kernels 
still around not deleted.
[Official doc](https://help.ubuntu.com/community/RemoveOldKernels#Manual_Maintenance)
What I use from guide:
`dpkg -l | tail -n +6 | grep -E 'linux-image-[0-9]+'`
`sudo update-initramfs -d -k old-kerenel-name`
`sudo dpkg --purge old-kerenel-name`
