---
title: 刪除commit
date: 2023-05-29
keywords: Git, 刪除commit
---

刪除commit，參考此篇  
[https://gitbook.tw/chapters/using-git/reset-commit.html](https://gitbook.tw/chapters/using-git/reset-commit.html)
```
git reset HEAD^
git push -f origin develop
```

拉remote的code
```
git branch -a
git checkout develop
git pull --rebase ec-console develop

git checkout 1.7.0
git pull --rebase ec-console 1.7.0
```