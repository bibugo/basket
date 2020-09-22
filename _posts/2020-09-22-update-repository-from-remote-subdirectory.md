---
layout: post
title:  "Update Repository From Remote Subdirectory"
date:   2020-09-22 16:57:00 +0800
---

# github actions
`.github/workflows/update.yml`

```yaml
# This is a basic workflow to help you get started with Actions

name: Update packages from coolsnowwolf/lede

# Controls when the action will run. Triggers the workflow on push or pull request
# events but only for the master branch
on:
  schedule:
    - cron: '0 8 * * *'
    - cron: '0 18 * * *'

env:
  REPO_URL: https://github.com/coolsnowwolf/lede
  PREFIX: package/lean

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  update:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - uses: actions/checkout@v2

      # Runs a single command using the runners shell
      #- name: Run a one-line script
      #  run: echo Hello, world!

      # Runs a set of commands using the runners shell
      #- name: Run a multi-line script
      #  run: |
      #    echo Add other actions to build,
      #    echo test, and deploy your project.

      - name: init
        env:
          DEBIAN_FRONTEND: noninteractive
        run: |
          sudo timedatectl set-timezone "Asia/Shanghai"
          git config --global user.name 'bibugo'
          git config --global user.email 'bibugo@users.noreply.github.com'
      - name: fetch split rebase
        run: |
          git remote add upstream $REPO_URL
          git fetch upstream master:upstream-master
          git checkout upstream-master
          git subtree split --prefix=$PREFIX -b upstream-lean
          git checkout master
          git rebase upstream-lean
          git push -f origin master
```
