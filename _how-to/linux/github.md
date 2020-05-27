---
layout: how-to
title: Unusual git commands 
subtitle: simplified
group: linux
date: 20/05/2020
---

# Less commonly used git commands


1. To clone from pull request XXX<sup>[1](#1)</sup> type the below commands into the terminal
   ```sh
    # git clone link-to-master-branch
    # change directory to master-branch
    git fetch origin pull/XXX/head:pull_XXX #XXX is the pull request number
    git checkout pull_XXX # pull_XXX is local branch name
  ```
2. To update/checkout a single file from remote origin master<sup>[2](#2)</sup> type the below commands into the terminal
   ```sh
    # change directory to repo
    git fetch
    git checkout master <filepath> 
  ```
    
**References:**

1. https://stackoverflow.com/questions/14947789/github-clone-from-pull-request
2. https://stackoverflow.com/questions/3334475/git-how-to-update-checkout-a-single-file-from-remote-origin-master

