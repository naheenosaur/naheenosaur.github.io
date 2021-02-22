---
title:  "git commit한 파일 제거"
date:   2021-01-30 21:00:00 +0900
tags: [git]
key: 20210130_02
---

# git commit한 파일 제거

1. git commit 한 파일을 이제 빼고자 할 때
    1. `git rm --cached 파일명` 
    2. `git commit --amend` 
2. 이걸 응용해서 이미 커밋에 올라가있는 파일을 제거하려면
    1. `git rebase -i 이전커밋`
    2. `git rm --cached 파일명`
    3. `git commit --amend`
    4. `git rebase --continue`
3. 좀 더 응용해서 initial commit 에 잘 못넣은 ( git ignore 에 추가하지 못한 ) 파일 제거
    1. `git rebase -i --root`
    2. `git rm --cached 파일명`
    3. `git commit --amend`
    4. `git rebase --continue`