---
#layout: post
title:  "github action에서 input값으로 변수 사용하기"
date:   2020-01-24 21:00:00 +0900
tags: [commit-every-single-day, github-action]
---
## step에서 input 값으로 변수 사용하기

github action에서는 변수를 사용할 수 있는데,  
각각의 step의 서비스에 이 변수를 input값으로 사용할 수 없다.

```
jobs:
  build:
    runs-on: ubuntu-latest
      GITHUB_ACTOR: ${{ secrets.GITHUB_ACTOR }}
      GITHUB_REPOSITORY: ${{ secrets.GITHUB_REPOSITORY }}
    steps:
      - name: set variables
        id: variables
        run: |
          echo ::set-output name=username::$GITHUB_ACTOR
          echo ::set-output name=repository::$GITHUB_REPOSITORY

      - name: Check last commit
        uses: ./ 
        id: check-commit
        with:
          username: ${{ steps.variables.outputs.username }}
          repository: ${{ steps.variables.outputs.repository }}
``` 
이런식으로 변수를 output 데이터로 갖는 'set variables'라는 step을 생성해 주고  
이 데이터를 input데이터(with)로 사용할 수 있도록 `${{ steps.'id명'.outputs.'이름' }}` 로 데이터를 불러온다.

[참고하기](https://github.community/t5/GitHub-Actions/How-to-pass-environment-variable-to-an-input/td-p/32003)

참고로, secrets.GITHUB_ACTOR, secrets.GITHUB_REPOSITORY 는 github에서 제공해 주는 변수이다.  
github repository 와 관련된 내용을 secrets 에 등록하려면 github의 개인 설정 -> secret 에서 등록할 수 있다.  
gitlab과 다른 점이라면 gitlab에서는 secret으로 지정한 내용을 보거나 수정할 수 있는데 
github에서는 이미 등록하면 확인하거나 수정할 수 없어서 삭제 후 재 설정 해야 한다.