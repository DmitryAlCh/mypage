---
layout: post
title:  "Linux commands"
date:   2022-05-30 18:17:11 +0200
categories: [linux]
published: false
---

Without Linux, computing and especially WEB would evolve in different way and 
most likely would be less affordable. Commands and approaches I use frequently are here.

### Helpful commands

#### <a name="history">`history` - history
Becomes really useful paired with <a name="grep">`grep`,
that allow to narrow `history` output results.
Whenever I remember only a part of command, that I have used (copy-pasted form internet, 
cause this is what You do in Linux, find commands You barely understand and run with `sudo` prvilleges),
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
Now it will become the last entered command, so can hit `UP` or `fc` arrow to edit it.

#### <a name="purging old kernels">Purging old kernels.
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

#### <a name="vim as default editor">Setting VIM as default editor
Using `fc` command makes only when VIM is Your editor.
```
echo "export EDITOR='vim'" >> ~/.bashrc
```

#### <a name="VIM">VIM 
Using daily as main code-editor for typescript. Mostly using only basic commands.
Enjoy the pattern: `action` + `in or around` + `symbol`.
* `c` + `i` + `{` - change text in curly braces.
* `y` + `i` + `[` - copy text in rectangular braces. 
* `d` + `i` + `(` - delete text in usual braces.
* `v` + `a` + `{` - visually select all around curly braces.  

And same pattern modified: `action` + `on what exactly`.
* `c` + `3w` - change 3 words
* `d` + `7l` - change 7 letters  

Whole line actions (regardless of curor position in line):
* `dd` - delete whole line 
* `cc` - change whole line 
* `yy` - copy whole line  

Navigation inside the text:
* `gg` - move to top of file
* `G` - move to bottom

Navigation between files(buffers):
* `ctrl` + `o` go to previous buffer
* `ctrl` + `i` got to next buffer

Going into insert mode for code is usually:
* `Shift` + `o` - above current line
* `o` - below current line

Aside from basic moves (`j`, `k`) that is enough to get Me trough the day.
Vim is a rabbit whole in a way. There are multiple to do an operation. Generally 
less keystrokes approach should win.

Full config includes plugins also.
* `NerdTREE` - absolute must have, as it gives project tree view on the left
* `GruvBox` - colorsheme.

There are 10 more in my [config](https://github.com/DmitryAlCh/dotfiles/tree/main/nvim), 
they should be doing synatx highlighting, autocompletion and fuzzy search.

#### Tmux
Whereas different terminal emulators might do have different shortcuts to 
splitting the screen, or adding a tab, `tmux` being an app running inside any 
terminal emulator, allows to remember those key-combos once.

##### <a name="cd -">`cd -` navigate back to previous directory
##### <a name="ctrl + l">`Ctrl + l` clear screen
##### <a name="ctrl + r">`Ctrl + r` switch to command search mode 
#### Neovim build from source
`sudo make distclean` to remove old stuff
`sudo make`
`sudo make install`


