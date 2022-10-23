---
layout: post
title:  "Github에서 Jekyll로 Blog 하기 - 3"
hide_title: false
date:   2022-10-23 23:45:00 +0900
# feature-img: "assets/img/sample.png"              # Add a feature-image to the post
# thumbnail: "assets/img/thumbnails/sample-th.png"  # Add a thumbnail image on blog view
# color: rgb(80,140,22)                             # Add the specified color as feature image, and change link colors in post
# bootstrap: true                                   # Add bootstrap to the page
tags: [github, jekyll, blog]
---

Github에서 Jekyll로 Blog 하기 3편
=

안녕하세요. 개발자 Iann입니다.

지난 포스트
- [Github에서 jekyll로 Blog 하기 1편][github-jekyll-blog-1]
- [Github에서 jekyll로 Blog 하기 2편][github-jekyll-blog-2]

에 이어 3편을 작성해보겠습니다. 이번 주제는 Jekyll 테마 적용하기 입니다.

<br>
먼저 적용할 테마를 골라야 합니다.

- [Github jekyll-theme][github-jekyll-theme]
- [jamstackthemes.dev][jamstackthemes.dev]
- [jekyllthemes.org][jekyllthemes.org]
- [jekyllthemes.io][jekyllthemes.io]

구글링을 해도 여러가지가 나오지만, 사이트에서 쇼핑하듯 쭉 살펴볼 수도 있습니다. 특히 `jekyllthemes.io`에는 고퀄리티 테마가 많은데 유료입니다. 하하.. 블로그에 진심이시다면 투자해볼만한 퀄리티입니다.

<br>

전 `jamstackthemes.dev`에서 [Type-on-Strap][type-on-strap]이라는 테마를 선택했습니다. 심플하면서도 심심하지 않은 테마가 맘에 들었습니다.

<br>

---
### 테마 적용하기
---

테마를 결정했다면 적용시켜 보겠습니다.


`Gemfile` 파일에 테마 설치를 위해 추가합니다. 테마를 따로 설정하지 않았다면 `minima`가 있을텐데 이를 아래로 바꾸어줍니다.
```
gem "type-on-strap"
```
`_config.yml`의 테마 설정도 바꾸어줍니다. theme도 `minima`가 세팅되어 있을겁니다. 이를 바꾸어줍니다.
```
theme: type-on-strap
```
그리고 설치.
```
bundle install
```
오류 없이 실행된다면 정상적으로 설치된겁니다. 로컬에서 한번 실행해봅니다.
```
bundle exec jekyll serve
```

<br>

![테마 적용](/assets/images/2022/10/apply_jekyll_theme_1.png)

<br>

뭔가 이상합니다. 작성했던 글도 다 사라지고 휑하니 빈집에 오류 메세지만 보입니다 ㅋㅋ
대부분의 테마가 그러하겠지만, `Type-on-Strap` 테마는 커스터마이징이 되어 있어 그에 맞게 추가 설정이 필요합니다.
테마별 설정은 해당 테마의 github, README.MD에 상세히 설명되어 있습니다. 저도 문서를 보며 맞춰보겠습니다.

<br>

![폴더구조](/assets/images/2022/10/apply_jekyll_theme_2.png)

<br>

먼저 폴더 구조에 맞게 하나하나 생성해줍니다. 기본적으로 필요한 파일들이 있는데 일일이 확인하기 귀찮으니 git clone 받아서 폴더를 복사했습니다.

<br>

![폴더적용](/assets/images/2022/10/apply_jekyll_theme_3.png)

<br>

이제 목록에 홈화면에 글 목록이 나옵니다. 그래도 아직 뭔가 이상하네요. README.md 를 보고 더 설정해봅니다.

<br>

![설정 적용](/assets/images/2022/10/apply_jekyll_theme_4.png)

<br>

설정을 만져서 블로그 포스트만 나오도록 했습니다. 테마에서 추가로 지원하는 기능들이 꽤나 많습니다. 이 부분은 시간을 두고 적용해보는걸로..! 여기까지, jekyll의 테마 적용하기입니다!



[github-jekyll-blog-1]: https://dev-iann.github.io/github/blog/2022/10/10/start-blog-with-github-and-jekyll-1.html{:target="_blank"}
[github-jekyll-blog-2]: https://dev-iann.github.io/github/blog/2022/10/15/start-blog-with-github-and-jekyll-2.html{:target="_blank"}
[github-jekyll-theme]: https://github.com/topics/jekyll-theme{:target="_blank"}
[jamstackthemes.dev]: https://jamstackthemes.dev/ssg/jekyll/{:target="_blank"}
[jekyllthemes.org]: http://jekyllthemes.org/{:target="_blank"}
[jekyllthemes.io]: https://jekyllthemes.io/{:target="_blank"}
[type-on-strap]: https://github.com/sylhare/Type-on-Strap{:target="_blank"}