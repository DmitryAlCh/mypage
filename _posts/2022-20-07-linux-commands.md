---
layout: post
title:  "Linux life"
date:   2022-05-30 18:17:11 +0200
categories: [linux]
last_edit: 2022-11-24
published: true
---

Without Linux, computing and especially WEB would evolve in different way and 
most likely would be less affordable. Summary of commands/approaches, I use frequently. 
WIP.

### Helpful commands

#### <a name="history">`history` - history
Becomes really useful paired with <a name="grep">`grep`,
that allow to narrow `history` output results.
Whenever I remember only a part of command, that I have used (copy-pasted form internet, 
cause this is what You do in Linux, find commands You barely understand and run with `sudo` privileges),
just add the remembered part after `grep`:
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

#### <a name="dpkg --purge">`dpkg --purge` Purging old kernels.
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
sudo update-initramfs -d -k old-kernel-name
sudo dpkg --purge old-kernel-name
```

#### <a name="fc">`fc` - open last command in editor
Making typos in commands is annoying.
```
sudo apt-gEt update && sudo apt upgrade && sudo apt autoremove`
sudo: apt-gEt: command not found
```
Instead of hitting `UP` key, and key navigate to typo, `fc` would 
bring in last command inside VIM, edit, and `:wq`.

#### <a name="vim as default editor">`VIM`- Setting as default editor
Using `fc` command makes only when VIM is Your editor.
```
echo "export EDITOR='vim'" >> ~/.bashrc
```

### <a name="VIM">`VIM` getting through the day 
Why? No need to ever touch mouse, in combination with `Tmux` becomes a super power.
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

Copy using system clipboard (from Vim to any other program): 
* `"` + `*` + 'y' - copies visually select text to clipboard

Aside from basic moves (`j`, `k`) that is enough to get Me trough the day.
Vim is a rabbit whole in a way. There are multiple to do an operation. Generally 
less keystrokes approach should win.

Full configuration includes plugins also.
Would emphasize `NerdTREE` as an absolute must have, as it gives project tree view on the left,
with easy file operations context menu.
<p align="center">
    <img alt="nerdree context menu" src="{{site.base_url}}/assets/images/nerdtreemenu.png" />
</p>
There are 12 more plugins in my [config](https://github.com/DmitryAlCh/dotfiles/tree/main/nvim), 
they should be doing syntax highlighting, auto completion and fuzzy search. Not really needed 
to get started.

### <a name="installing neovim">Adding Neovim to Ubuntu.
Official guide covers it all: [https://github.com/neovim/neovim/wiki/Building-Neovim](https://github.com/neovim/neovim/wiki/Building-Neovim)
Now need to make it to actually launch `neovim`, when You type `vim` in terminal 
emulator(no rational justification to do it, just type `nvim`).
<a name="update-alternatives --config vim">`update-alternatives` command comes to help
```
> sudo update-alternatives --config vim
There are 2 choices for the alternative vim (providing /usr/bin/vim).

  Selection    Path                 Priority   Status
------------------------------------------------------------
* 0            /usr/local/bin/nvim   100       auto mode
  1            /usr/bin/vim.basic    30        manual mode
  2            /usr/local/bin/nvim   100       manual mode

Press <enter> to keep the current choice[*], or type selection number:
```
Sometimes after pulling latest changes, build breaks, most of the times need 
to purge old stuff with `sudo make distclean`.

### <a name="tmux">Tmux
Whereas different terminal emulators might do have different shortcuts to 
splitting the screen, or adding a tab, `tmux` being an app running inside any 
terminal emulator, allows to remember those key-combos once. (no mouse again)  
Great cheat sheet found here: [https://tmuxcheatsheet.com/](https://tmuxcheatsheet.com/)
An experience changer -> rebinding `ctrl` + `b` to `ctrl` + `a`. 
My [config](https://github.com/DmitryAlCh/dotfiles/blob/main/tmux/tmux.conf).  
Great supplement to VIM.

