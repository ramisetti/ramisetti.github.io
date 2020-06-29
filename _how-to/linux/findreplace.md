---
layout: how-to
title: Search and replace string in multiple files 
subtitle: simplified
group: linux
date: 16/06/2020
#revised: 05/09/2019
---
# Any one of the following approaches can be used.

1. Using find command, xargs and perl <sup>[1](#1)</sup>
   ```sh
    find . -type f | xargs perl -pi~ -e 's/SEARCHSTRING/REPLACESTRING/g;'
   ```
   &#13;
2. Using find and sed <sup>[2](#2)</sup>
   ```sh
   find . -type f -exec sed -i 's/foo/bar/g' {} +
   ```
   ```sh
    Enter passphrase (empty for no passphrase):
   ```
   &#13;

**References:**

1. https://blogs.oracle.com/rammenon/recursively-replacing-a-string-in-all-files-in-a-directory-linux
2. https://victoria.dev/blog/how-to-replace-a-string-in-a-dozen-old-blog-posts-with-one-sed-terminal-command
