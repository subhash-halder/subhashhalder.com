---
title: "My Most Useful Docker Commands"
description: In this article I will try to note down all the useful docker command I used in my daily life.
author: subhash
date: 2022-06-12 20:18
categories: ["DevOps", "Docker"]
tags: [docker, docker commands, docker tutorial, docker for beginners]
---

## Installing Docker
For Installing docker please follow the instruction according to your OS [install link](https://docs.docker.com/engine/install/).


I am in Mac so if you have Homebrew it's easy to install docker just run the command in terminal and you are ready to go, then you have to start the docker desktop.

```bash
brew install docker
```

To check if docker is installed correctly try to run the command from a terminal.

```bash
docker --version
```

> NOTE: In linux if it requires sudo permission please use this command to run any docker command without sudo 
{: .prompt-tip }
>
> 1. Create the docker group.
> ```bash
> sudo groupadd docker
> ```
> 2. Add your user to the docker group.
> ```bash
> sudo usermod -aG docker $USER
>  ```
> Logout and login again to take effect, or follow the [StackExchange post](https://superuser.com/questions/272061/reload-a-linux-users-group-assignments-without-logging-out) if it works.

## Docker commands
### spin up a new image
```bash

```
