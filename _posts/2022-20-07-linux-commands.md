---
layout: post
title:  "Linux commands"
date:   2022-05-30 18:17:11 +0200
categories: [linux]
published: false
---

Without Linux, computing and especially WEB would be very different and 
most likely less affordable. Commands and appraches I use frequently are here.

### Helpful commands

#### <a name="history">`history` - shows commands You entered
Becomes really useful paired with <a name="grep">`grep`,
that allow to narrow `history` output results.
Whenever I remember only a part of command, that I have used (copy-pasted form internet, 
cause this is what You do in Linux, find commands You barely understand and run with `sudo` prvilleges),
I do:
```
> history | grep tail
  638  [10/10 21:02:43] history | grep tail
  639  [10/10 21:02:46] history | grep tails
  640  [10/10 21:03:29] dpkg -l | tail -n +6 | grep -E 'linux-image-[0-9]+'
```
Now in order to use the command, hit `!640` - it will execute the command with 
number 640.
```
> !640
rc  linux-image-5.11.0-27-generic  5.11.0-27.29~20.04.1  amd64  Signed kernel image generic
rc  linux-image-5.11.0-40-generic  5.11.0-40.44~20.04.2  amd64  Signed kernel image generic
rc  linux-image-5.11.0-43-generic  5.11.0-43.47~20.04.2  amd64  Signed kernel image generic
rc  linux-image-5.13.0-28-generic  5.13.0-28.31~20.04.1  amd64  Signed kernel image generic
ii  linux-image-5.15.0-50-generic  5.15.0-50.56~20.04.1  amd64  Signed kernel image generic
```
When need to edit command before use, add `:p`:
```
> !640:p 
dpkg -l | tail -n +6 | grep -E 'linux-image-[0-9]+'
```
Now it will become the last entered command, so can hit `UP` arrow to edit it.

#### Purging old kernels.
Sometimes You can't `sudo apt-get upgrade` anymore it gives some error.
Especially when disk is encrypted, `boot` partition tends to be smallish.
So just check <a name="df">`df /boot/`: 
```
> df /boot/
Filesystem     Type  Size  Used Avail Use% Mounted on
/dev/nvme0n1p2 ext4  704M  308M  345M  48% /boot
```
`Use%` should be small, found no better explanation than here:
[Official doc](https://help.ubuntu.com/community/RemoveOldKernels#Manual_Maintenance)
What I use from guide:
```
dpkg -l | tail -n +6 | grep -E 'linux-image-[0-9]+'
sudo update-initramfs -d -k old-kerenel-name
sudo dpkg --purge old-kerenel-name
```
#### <a name="fc">`fc` - open last command in editor
Making typos in commands is annoying.
```
sudo apt-gEt update && sudo apt upgrade && sudo apt autoremove`
sudo: apt-gEt: command not found
```
Instead of hitting `UP` key, and key navigate to typo, `fc` would 
bring in last command inside VIM, edit, and `:wq`.
#### Setting VIM as default editor
#### Tmux
##### <a name="cd -">`cd -` navigate back to previous directory
##### <a name="ctrl + l">`Ctrl + l` clear screen
##### <a name="ctrl + r">`Ctrl + r` switch to command search mode 
#### Neovim build from source
`sudo make distclean` to remove old stuff
`sudo make`
`sudo make install`


