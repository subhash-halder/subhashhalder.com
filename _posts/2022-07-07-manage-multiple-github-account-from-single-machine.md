---
title: "How to Manage Multiple GitHub Repositories from a Single Machine"
description: "Managing multiple GitHub repositories can be a challenge, but it doesn't have to be. In this blog post, I'll show you how to use a few simple techniques to use multiple GitHub account from single machine."
author: subhash
date: 2022-07-07 20:18
categories: ["Productivity", "GitHub", "Git"]
tags: [git, github, multiple repositories, manage repositories, productivity, software development, tips, tricks, workflow]
---

## Problem
Many developers use their working laptop to contribute to open-source projects or personal projects. However, it can be a pain to set up a GitHub email for every repository. Sometimes we forget to update the email and contribute to a personal repository with our work email, or vice versa. Today I will show a simple solution to this problem.

Let say I am working in an Organization with GitHub username as **subhash-xyz** and my personal GitHub username is **subhash-halder**, first thing I do is set two different base folder for two username (you can use it for multiple account also).

```bash
# Organization projects base path
$ ~/Documents/work/xyz/
# Personal projects base path
$ ~/Documents/work/personal/
```

## Create two different ssh keys

Now we need to create two different ssh keys for two different account.
```bash
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
This is the basic command to create ssh key, here we have to run the command two time and save the key in two different files. For more details of ssh key creation please visit this [link](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent).


I have created two ssh file as follows
```bash
$ ~/.ssh/github_xyz
$ ~/.ssh/github_personal
```
Now we have to add the public key to the respective GitHub Account.

## Create a global .gitconfig

Now create a .gitconfig file in you home directory and paste the following code, please update it according to your need.

```bash
# ~/.gitconfig
[includeIf "gitdir:~/Documents/work/personal/"]
  path = ~/.gitconfig-personal
[includeIf "gitdir:~/Documents/work/xyz/"]
  path = ~/.gitconfig-xyz
```

This tells Git if the git folder is inside these folder use the respective config files.

## Create the folder specific git configuration
Since we have specified that we will use different configuration file for different folder now we need to create them.
### **` ~/.gitconfig-personal`**
```bash
[user]
  name = Subhash Halder
  email = halder.subhash@gmail.com

[github]
  user = "subhash-halder"

[core]
  sshCommand = "ssh -i  ~/.ssh/personal_github"
```

### **` ~/.gitconfig-xyz`**
```bash
[user]
  name = Subhash Halder
  email = halder.subhash@xyz.com

[github]
  user = "subhash-xyz"

[core]
  sshCommand = "ssh -i  ~/.ssh/personal_xyz"
```

## You can now use git command without worry

```bash
# If I run git clone command from personal folder it will use my personal ssh key
~/Documents/work/personal $ git clone git@github.com:subhash-halder/subhashhalder.com.git
~/Documents/work/personal $ cd subhashhalder.com
# If you run git command from inside the git folder it will show the config from the local configuration file.
~/Documents/work/personal/subhashhalder.com $ git config user.email
~/Documents/work/personal/subhashhalder.com $ halder.subhash@gmail.com
# If you run git command from inside the git folder of your Organization dir it will show the config from the local configuration file.
~/Documents/work/xyz $ git clone git@github.com:xyz/demo.git
~/Documents/work/xyz $ cd demo
~/Documents/work/xyz/demo $ git config user.email
~/Documents/work/xyz/demo $ halder.subhash@xyz.com
```
