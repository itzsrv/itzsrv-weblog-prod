---
title: Linux Terminal Tips for Dev
date: 2020-05-24
description: Tips to enhance productivity while using Linux Terminals
tags: ['post']
draft: false
---

This is Linux blog post. Few tips and tricks to enhance productivity while using Linux Terminals!

At times, while using Linux Terminals, we get stuck over steps where either we don't remember exactly which command can help us out from that point or the one we know makes us take a long route to achieve what we needed. Here, we will see few tips and tricks to overcome some of those difficlut times. The examples shown below are being run in bash shell on mac, but you can try them in other shells as well (POSIX PLAIN).

## Short-cuts

**`ctrl + a`**

If you have typed a long command in your terminal and your cursor currently is at the end of the command but you want to navigate your cursor to the start of the command, you can use : `ctrl + a`  
```bash
$ find ~/Docs/GitHub/learn -type d -name file|
$ |find ~/Docs/GitHub/learn -type d -name file
```

**`ctrl + e`**

If you have typed a long command in your terminal and your cursor currently is somewhere but not at the end of the command, now if you want to navigate your cursor to the end of the command, you can use : `ctrl + e`  
```bash
$ |find ~/Docs/GitHub/learn -type d -name file
$ find ~/Docs/GitHub/learn -type d -name file|
```

**`option + left/right arrow`**

If you have typed a long command in your terminal and you want to navigate your cursor word by word of the command, you can use : `option + left/right arrow`  

**`ctrl + k`**

If you have typed a long command in your terminal and you want to delete some part from a point untill the end of the command, then navigate your cursor to that point, and use : `ctrl + k`  

**`ctrl + u`**

If you have typed a long command in your terminal and you want to delete some part from a point untill the beginning of the command, navigate your cursor to that point, and use : `ctrl + u`

**`!<command initials>`**

If you want to rerun any command that you have run recently without finding that command in your history through up and down arrow keys, then you can simply use the first few initials of that command with exclamation sign : `!fi`
```bash
$ !fi
find ~/Docs/GitHub/learn -type d -name '*-2'
```
Shell starts looking your history and the first command that it finds mathing with that initials, it runs it for you. 

Same can be achieved by giving the history number with the exclamation sign
```bash
$ history
192  brew cask list   
193  brew uninstall virtualbox

$ !192
```
Shell runs `brew cask list`, as soon as you hit enter.

**`ctrl + r`**

If you want to interactively search for a command from your history, you can use : `ctrl + r`
```bash
(reverse-i-search)`fi': find ~/Docs/GitHub/learn -type d -name file
```
You can start looking for your command by typing the initials, and as soon you find your command to run, hit enter!

**`ctrl + l`**

If you want to clear the screen, use : `ctrl + l`

But it doesnt clear the scroll back, so you can scroll up and down to see your previous command outputs

**`command + k`**

If you want to clear the screen as well as the scroll back, you can use : `command + k`

---

## Important Operations through Terminals 

Here are few examples to automate various dev tasks through command line

### Resize multiple images 

We often feel the need to resize images for various purposes. Like we as web developers, resize images to small or medium pixels alot. Its painful to go to each picture and resize it one by one from GUI Applications. Here, we will see how to achieve the same from terminal. Before going further, copy all the original images to another directory and navigate into that directory.
```bash
sips -Z 640 *.jpg
```
All the jpg images present inside this directory are resized to 640 pixels with same aspect ratio as original images.

-Z : to maintain the aspect ratio

640 : to resize to 640 pixels

The output directory can be defined by adding `--out` option.
```bash
sips -Z 640 *.jpg --out ~/Desktop/Project/small
```
The images inside `small` directory will have the same name as the original files. If you want to append the file name with some keyword, you can use below command
```bash
find . -type f -exec bash -c 'mv "$0" "${0%\.jpg}-300.jpg"' {} \;
```
