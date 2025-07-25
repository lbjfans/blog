---
share: true
title: git
tags:
  - sync
dir: posts/sync/
date: 2025-07-25T20:40:00+08:00
summary: git操作
---

A本地上传到gitee
```
git config --global user.name "lbjfans"
git config --global user.email "1293328185@qq.com"

git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin https://gitee.com/lbjfans/learn_lora.git
git push -u origin main
```

B本地从gitee获取数据
```
git clone https://gitee.com/lbjfans/learn_lora.git

git config --global user.name "lbjfans"
git config --global user.email "1293328185@qq.com"
git init
git branch -M main
git remote add lora https://gitee.com/lbjfans/learn_lora.git
git pull lora main
```