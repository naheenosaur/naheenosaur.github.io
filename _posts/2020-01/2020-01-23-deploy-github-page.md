---
title:  "github action에서 github page 배포하기"
date:   2020-01-23 21:00:00 +0900
tags: [commit-every-single-day, git-action, github-page]
---

github action에서 실행된 결과를 바탕으로 새로운 github page를 만드는 방법을 알아봤다.  
github page는 기본적으로 repository에 배포가 된 파일이어야 한다.  

나는 간단한 서비스였기 때문에 서비스 로직은 master에,   
page로 만들고 싶은 부분은 gh-pages 브렌치에 따로 분리를 했다.

github page의 주소는 `'이름'.github.io`로 repository 이름을 하지 않은 경우  
`'이름'.github.io/'레파지토리이름'`으로 발행된다.

간단하게 배포하기 위해 `crazy-max/ghaction-github-pages@v1`을 사용했다.  
모듈의 작동방식은 내가 원하는 html파일을 생성 하고 그 파일을 갖고 있는 gh-pages 브랜치를 생성해 준다.

```
  - name: Build github page
    if: steps.check-commit.outputs.committed == 'true'
    run: |
      mkdir public
      cat > public/index.html << EOL
      ${{ steps.check-commit.outputs.github-page }}
      EOL
      touch .nojekyll

  - name: Deploy
    if: steps.check-commit.outputs.committed == 'true'
    uses: crazy-max/ghaction-github-pages@v1
    with:
      target_branch: gh-pages
      build_dir: public

```

도중 만났던 이슈로는  
발행이 되지 않고 `Your site is having problems building: Unable to build page. Please try again later.`라는 에러가 발생했는데  
이것은 jekyll 파일을 변환할 수 없다는 것을 의미한다.  
그래서 jekyll을 사용하지 않겠다고 지정해야하는데, html 파일을 만들어 준 동일한 경로에  
`touch .nojekyll` 을 이용하여 파일을 생성해 주면 된다.  
[자세한 내용 보기](https://help.github.com/en/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line)


