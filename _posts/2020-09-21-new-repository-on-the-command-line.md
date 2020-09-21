---
layout: post
title:  "New Repository On The Command Line"
date:   2020-09-21 11:45:00 +0800
---

# new repository on the command line
```bash
echo "# sample" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M master
git remote add origin https://github.com/user/sample.git
git push -u origin master
```

# push an existing repository from the command line
```bash
git remote add origin https://github.com/user/sample.git
git branch -M master
git push -u origin master
```
