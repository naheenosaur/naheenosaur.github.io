---
title:  "bash에서 git branch 표시하기"
date:   2019-12-20 21:00:00 +0900
tags: [bash, git]
key: 20191220_01
---
## console에 git branch 표시 

git을 사용할 때, 혹시라도 branch를 착각해서 실수하는 일이 없도록 명령창에 branch를 표시했다.  
개인적으로 결과창과 구분이 되도록 bash에 host나 경로에 색이 있는 것을 선호하기 때문에 색도 같이 넣었다.   

```bash
git_branch() {
        git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1) /'
}

export PS1='\[\e]0;\w\a\]\[\e[32m\]\u@\h \[\e[33m\]\w \[\e[31m\]$(git_branch)\[\e[35m\]$ ' 
```
- `\[\e[숫자m\]` 형태는 색을 말하고
- `\u@\h`는 유저@호스트의 형태
- `\w\`는 현재의 경로를 말한다. 



