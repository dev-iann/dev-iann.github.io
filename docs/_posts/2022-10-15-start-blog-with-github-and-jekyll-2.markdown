---
layout: post
title:  "Github에서 Jekyll로 Blog 하기 - 2"
date:   2022-10-10 13:45:03 +0900
categories: github blog
---

Github에서 Jekyll로 Blog 하기 2편
=

안녕하세요. 개발자 Iann입니다.

지난 포스트 [Github에서 jekyll로 Blog 하기 1편][github-jekyll-blog-1]에서 로컬에서 jekyll 프로젝트를 만들어 실행시켜보는 것 까지 했습니다. 이번편에서는 프로젝트를 github에 올려 블로그를 배포해보도록 하겠습니다.

배포를 위해 github에 repository를 생성해야 합니다.

Github에 블로그 배포하기
-

---
### Repository 생성
---

![Repository 생성](/assets/images/2022/10/github_repository_for_jekyll_1.png)

public으로 Repository를 생성합니다.

![Repository 생성](/assets/images/2022/10/github_repository_for_jekyll_2.png)

생성 버튼을 누르면 Repository name을 입력하게 됩니다. 블로그 용도로 배포하기 위해서는 `<user>.github.io`의 형식으로 입력해야 합니다. `<user>`는 계정을 입력하면 됩니다. 회사 블로그를 만드는 경우에는 `<organization>.github.io`로 만들면 됩니다. 여기서 작성한 것이 나중에 블로그에 접속하는 url이 됩니다. 계정이나 회사명과 일치하게 사용하지 않아도 되지만, url은 특정 누군가(or 회사)를 나타내는 것이기도 하니 맞춰주는게 좋겠죠. 저는 이미 만들어 놓은 상태라 이미 있다고 나옵니다.

---
### Repository 설정
---

![Repository 설정](/assets/images/2022/10/github_repository_for_jekyll_3.png)
생성한 Repository로 들어가 Settings를 눌러 설정으로 들어갑니다. 왼쪽 메뉴 목록에서 Pages로 들어갑니다.

어떤 Branch의 어떤 경로를 기준으로 배포할 것인지 Branch 항목에서 설정합니다 앞서 gh-pages branch의 /docs 폴더에서 작업했으므로 거기에 맞추어 설정했습니다. 변경후 옆의 Save를 누르면 됩니다.

---
### 블로그 배포
---
배포 과정은 정말 간단합니다. 1편에서 만든 jekyll 프로젝트를 github에 올리면 됩니다.

```
git remote add origin https://github.com/<user>/<repository>
```
git 용도로 폴더를 만들었으니 remote 주소를 등록해줍니다.

```
git push -u origin <BRANCH>
```
배포하려는 프로젝트를 push 해줍니다. push 이후 pages 설정화면으로 들어가면 배포 진행상황이 보여집니다. 오류가 발생한다면, 메세지를 참고하여 오류를 잡아주면 됩니다. 배포가 성공적으로 완료가 되면 블로그 url를 입력해서 확인해보면 끝!!




[github-jekyll-blog-1]: https://dev-iann.github.io/github/blog/2022/10/10/start-blog-with-github-and-jekyll-1.html{:target="_blank"}
