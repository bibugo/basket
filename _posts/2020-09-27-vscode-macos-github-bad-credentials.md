---
layout: post
title:  "VS Code for macOS connect to github bad credentials"
date:   2020-09-27 21:07:00 +0800
---

After change github password, connect VS Code for macOS to github, and get an error:
```
Bad credentials
```
<br />

1. open '`Keychain access`'
2. search '`github`'
3. delete '`vscode-github.login`'
4. connect github again


That's all.
VS Code use `Keychain access` to store credentials but not git config.
<br />
PS. git commandline to set credentials:
````bash
git config --global user.email=your@mail.com
git config --global credential.helper store
# input password first pull / push
git pull / push
# the credentials is store in ~/.git-credentials
# to remove credentials
rm ~/.git-credentials
# or use this command
git config --global --unset-all user.email
git config --global --unset credential.helper
```