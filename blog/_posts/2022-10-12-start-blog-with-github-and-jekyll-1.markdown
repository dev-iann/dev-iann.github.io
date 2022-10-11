---
layout: post
title:  "Github에서 Jekyll로 Blog 하기 - 1"
date:   2022-10-10 13:45:03 +0900
categories: github blog
---

Github에서 Jekyll로 Blog 하기 1편
=

안녕하세요. 개발자 Iann입니다.

기술 블로그를 작성해봐야지 생각을 하다가도, 작성할 플랫폼을 정하지 못해 미루고 미루다 드디어 github에 자리를 잡기로 했습니다. 고민의 후보들은 네이버 블로그, 티스토리, velog, github, linkedIn, 브런치 등이 있었습니다. 네이버 블로그의 경험이 조금 있지만 네이버 블로그는 개발자가 이용하기엔 적합하지 않았습니다. 가장 기본이면서 중요한 코드 블록을 지원하지 않으니까요. 그러면서 자연스레 정형화된 플랫폼보다는 자유도가 높은 github이 좋겠단 생각이 들었습니다. 또한 Repository와 같은 공간에 자리잡게 되어 포트폴리오로서의 역할도 가능합니다.

그런데 도전해본 사람들은 알겠지만, github page를 이용한 블로그 만들기는 하나의 기술영역이라 쉽지가 않습니다. 공부겸, 기록겸 "Github에서 Jekyll로 Blog하기"로 포스팅을 시작합니다.

Github과 Jekyll
-
github은 [github pages][github-pages]라는 서비스를 통해 무료로 호스팅을 제공합니다. 보통 주소는

>https://username.github.io

의 형태가 됩니다. 하지만 무한정 무료는 아닙니다. `1GB`까지만이에요. 이 이상 사용하려면?! 당연히 돈을 내면 됩니다.

github pages를 통해 홈페이지를 만들순 있는데, 맨땅에 시작하려면 상당히 골치 아픕니다. 디자인부터 시작해서 블로그 구성도 다~~ 해야하거든요. 그래서 우리는 Jekyll의 도움을 받습니다. Jekyll은 SSG(Static Site Generator) 중 하나이고 마크다운 기반으로 글 작성을 쉽게해주는 역할을 합니다. 테마를 제공하여 멋드러진 블로그를 템플릿을 적용할 수 있어요. (Jekyll외에도 Next.js, Hugo, Gatsby, Nuxt 등의 다양한 SSG가 있습니다. [Jamstack][jamstack]에서 확인 가능하니 한번 살펴보세요!)


Jekyll 사용 준비
-
Jekyll은 Ruby 기반이기 때문에 Ruby 환경을 구성해야 합니다. 제가 구성환 Windows를 기준으로 설명하겠습니다. (설치와 터미널의 차이만 있고 그외엔 대부분 비슷해요!)

---
### Ruby+Devkit 설치
---
[Ruby Installer][ruby-installer]에서 Ruby+Devkit을 다운로드 받아 설치합니다.

![Ruby+Devkit 설치](/assets/images/ruby_installer_1.png)

![Ruby+Devkit 설치](/assets/images/ruby_installer_2.png)

![Ruby+Devkit 설치](/assets/images/ruby_installer_3.png)

`MSYS2 development toolchain` 선택!!

![Ruby+Devkit 설치](/assets/images/ruby_installer_4.png)

`ridk install` 할 수 있도록 항목 선택!

![Ruby+Devkit 설치](/assets/images/ruby_installer_5.png)

1, 2, 3번 모두 실행해줍니다.

---
### Jekyll, Bundle 설치
---
Jekyll, Bundle 모두 루비 패키지입니다. 콘솔을 열어 다음의 명령어를 실행하여 설치합니다.

```
gem install jekyll
```

jekyll이 설치되었다면 이제 bundle을 설치합니다.

```
gem install bundler -v '2.3.22'
```

2022년 10월 10일 기준 bundler 최신 버젼은 `2.3.23`인데 이 버젼에 문제가 있습니다. nokogiri 라는 패키지 빌드에 실패해서 실행 자체가 안되니 해당 버젼은 피해서 설치해주세요. 아마도 `Failed to build gem native extension` 이런 류의 오류 메세지가 나왔던 것 같습니다.

> v2.3.23 bundler는 문제가 있으니 피하자.

여기까지 작업이 되었다면 로컬에서 Jekyll을 구동할 준비가 된겁니다.


Jekyll 프로젝트 만들기
-

---
### Git 프로젝트 만들기
---
그럼 이제 로컬에서 Jekyll 프로젝트를 만들어보겠습니다. Jekyll 프로젝트를 최종적으로는 github에 올릴 것이기에 git 프로젝트 만들기부터 시작합니다.

>만약 git이 설치되어 있지 않다면 [Download][git-download]페이지에서 git을 받아 설치합니다.

```
git init <REPOSITORY-NAME>
```
적당한 경로로 이동 후 git 초기화를 해줍니다.

```
cd <REPOSITORY-NAME>

mkdir docs
cd docs
```

git 초기화로 만들어진 경로로 이동한 뒤, 적당한 폴더를 하나 더 만듭니다. 물론 만들지 않아도 상관은 없지만, 적당히 관리를 위해 폴더를 하나 만들어줍니다.

```
git checkout --orphan gh-pages
git rm -rf
```
블로그 작성을 위해 branch를 만들어줍니다. 습관적으로 development branch를 만들었다가 아.. 개발보다는 출판에 가깝구나란 생각에 `publishing` 같은걸 할까 하다가 가이드에 있는대로 따라갑니다 ㅋㅋ

`--orphan ` 옵션은 생성하는 branch의 히스토리를 비워줍니다. 그리고 git rm -rf를 통해 존재하는 파일을 초기화 시킵니다. 아예 백지상태에서 시작하는거죠.

---
### Jekyll 프로젝트 만들기
---
git branch까지 준비되었다면 이제 Jekyll 프로젝트를 만들어봅니다.

```
jekyll new --skip-bundle .
```

경로를 나타내는 `.` 을 유의해서 써주세요. 여기까지 하면 Jekyll의 기본 파일들이 생성됩니다. 현재 폴더를 보면 여러가지 파일이 생성되어 있습니다. 

다음으로 Gemfile을 엽니다.

```
# gem "jekyll", "~> 4.2.2"
# This is the default theme for new Jekyll sites. You may change this to anything you like.
gem "minima", "~> 2.5"
# If you want to use GitHub Pages, remove the "gem "jekyll"" above and
# uncomment the line below. To upgrade, run `bundle update github-pages`.
gem "github-pages", "~> 227", group: :jekyll_plugins
```

`gem "jekyll"` 부분은 #을 붙여 주석처리합니다. 그리고 `# gem "github-pages"` 이라고 되어 있는 부분을 주석해제하고, 버전 정보를 추가해줍니다. 작성일 기준 최신 버젼이 227인데, 버젼 정보는 [Dependency versions][github-pages-dependencies version]을 참고하면 됩니다.

---
### 로컬에서 Jekyll 실행해보기
---
프로젝트까지 만들었으니 실행해보겠습니다.

```
bundle install
```

패키지 인스톨을 해줍니다. 만약 설치한 루비 버젼이 3.0.0 이상이면 webrick을 추가해줍니다.

```
bundle add webrick
```

이제 로컬에서 실행해 봅니다.


```
bundle exec jekyll serve
```

메세지가 나오는데 대충 http://127.0.0.1:4000/으로 서버를 띄웠다는 내용입니다. 위 url을 브라우저에서 열어보면 Jekyll의 샘플 프로젝트 블로그가 보입니다!!

여기까지, 잠깐 끊어가도록 하겠습니다. ㅎㅎ


[github-pages]: https://pages.github.com/{:target="_blank"}
[jamstack]: https://jamstack.org/generators/{:target="_blank"}
[ruby-installer]: https://rubyinstaller.org/{:target="_blank"}
[git-download]: https://git-scm.com/downloads{:target="_blank"}
[github-pages-dependencies version]: https://pages.github.com/versions/{:target="_blank"}