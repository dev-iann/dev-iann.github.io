---
layout: post
title:  "Jekyll 블로그에 Utterances로 댓글 기능 사용하기"
hide_title: false
date:   2022-10-30 22:00:00 +0900
# feature-img: "assets/img/sample.png"              # Add a feature-image to the post
# thumbnail: "assets/img/thumbnails/sample-th.png"  # Add a thumbnail image on blog view
# color: rgb(80,140,22)                             # Add the specified color as feature image, and change link colors in post
# bootstrap: true                                   # Add bootstrap to the page
tags: [github, jekyll, blog, utterances, comment]
---

안녕하세요. 개발자 Iann입니다.

지난 포스트
- [Github에서 jekyll로 Blog 하기 1편][github-jekyll-blog-1]
- [Github에서 jekyll로 Blog 하기 2편][github-jekyll-blog-2]
- [Github에서 jekyll로 Blog 하기 3편][github-jekyll-blog-3]

까지 완료하면서 블로그의 틀이 만들어졌습니다. 이번 편에서는 댓글 기능을 붙여보겠습니다.

<br>

Jekyll은 Static site generator입니다. 블로그가 만들어지고 난 이후, 사용자와의 인터랙션을 통한 데이터 저장이 불가능합니다.
하지만, Utterances는 Github의 issues를 활용하여 댓글을 issues에 등록하는 형태로 댓글 저장을 가능하게 합니다.
(와 레알 머리 좋음.... 감탄....)

<br>

물론 이외의 다른 댓글 서비스들도 있습니다. 외부 저장소에 댓글을 저장하는 서비스들이고 이들은 당연하게도 기본적으로 `유료`입니다. Disqus라는 잘 알려진 무료 댓글 서비스가 있지만, 광고가 노출될뿐만 아니라 최근엔 속도가 아주 느려 사용자들의 외면을 받고 있습니다.

<br>

뭐 암튼, 무료에다가 Github 내의 Issue 기능을 이용한 Utterances를 이용해보기로 합시다.

먼저, [Utterances App을 설치][install-utterances]합니다.

<br>

![Utterances App 설치](/assets/images/2022/10/install_utterance_1.png)

<br>

![Utterances App 설치](/assets/images/2022/10/install_utterance_2.png)

<br>

설치라고 뭐 특별한게 없습니다. 댓글을 저장하기 위해(issue를 생성하기 위해) Repository를 정하고, issue 읽기/쓰기 권한을 부여하는 겁니다. 댓글만을 위해 따로 Repository를 생성하기도 합니다. 전 그냥 큰 고민하지 않고 블로그 Repository에 추가했습니다.

<br>

다음은 설정입니다.

![Utterances App 설정](/assets/images/2022/10/install_utterance_3.png)

<br>

댓글을 연결할 Repository를 적습니다. 보통 `<owner>/<repo>`의 형태입니다.

<br>

![Utterances App 설정](/assets/images/2022/10/install_utterance_4.png)

<br>

다음은 블로그 Post와 댓글(Issue)를 어떻게 매핑 시킬지 정합니다.
뭔가 말이 어려워 보이지만, "Post를 Issue로 생성해서 댓글을 매핑 시킬껀데 Issue 제목을 뭘로 할까?" 라고 보면 됩니다.

- Issue title contains page pathname
- Issue title contains page URL

<br>

이 두가지는 URL 기반으로 `issue`를 만듭니다. 첫번째는 origin 이후의 url기준, 두번째는 URL full path 기준입니다.
앞으로 여러가지 블로그 설정을 하면서 URL이 달라질 가능성이 있어 URL 기반은 패스합니다.

- Issue title contains page title
- Issue title contains page og:title

<br>

다음은 제목 기반 `issue` 매핑입니다. 두가지 모두 제목 기반이지만 첫번째에는 블로그 제목이 포함된다는 점이 다릅니다.

- Specific issue number

`issue` 번호 기반 매핑입니다. 이 경우 `issue`가 자동 생성되지 않으므로 사용자가 게시물과 매핑할 `issue`를 생성해야 합니다. 귀찮겠죠? ㅋㅋ

- Issue title contains specific term
마지막으로 특정 문구를 포함한 `issue`와 매핑 시킵니다. 조건을 만족한 `issue`가 없을 경우 자동 생성해준다고 합니다.

<br>

항목들을 보다보니 Post와 댓글(issue)을 매핑시키는 것과 동시에, 댓글 외의 issue와 구분을 위한 목적들도 눈에 보입니다.

그리하여 고른 것은 `Issue title contains page og:title` 요 항목입니다. 설마 제목이 중복될 일은 없겠지.... 라고 생각하지만,
글이 많아지면 어떻지 모르겠습니다 ㅋㅋ

<br>

![Utterances App 설정](/assets/images/2022/10/install_utterance_5.png)

<br>

다음은 `Issue Label` 입니다. issue를 생성할 때 Label을 달아서 이게 댓글과 매핑된다는 표시를 해주는 용도입니다.

블로그 용도만을 위한 Repository인지라 크게 의미는 없지만 `간지`를 위해 설정해둡니다.

<br>

![Utterances App 설정](/assets/images/2022/10/install_utterance_6.png)

<br>

그리고 적당한 테마를 고르고 나면, 하단에 `script`가 나오는데 댓글 기능을 이용할 부분에 이 스크립트를 넣어주면 됩니다.
전 모든 게시글에 댓글 기능을 넣을꺼라 `_layouts/post.liquid` 파일 하단을 살펴봅니다. (테마에 따라 확장자가 다를 수 있어요.)

<br>

```
<!--Utterances-->
{% if site.comments.utterances.repo and site.comments.utterances.issue-term %} {% include social/utterances.liquid %} {% endif %}
```

<br>

제가 사용하는 테마에는 Utterance 코드가 이미 들어가 있습니다. 설정만 해주면 되겠네요. `_config.yml` 파일에서 설정합니다.

```
comments:
  utterances: https://utteranc.es
    repo: dev-iann/dev-iann.github.io
    issue-term: og:title
    theme: github-light
    label: Comment

```

생성된 `script`와 거의 비슷하죠? 그러면 하단에 댓글 창이 나타납니다!!



[github-jekyll-blog-1]: https://dev-iann.github.io/2022/10/10/start-blog-with-github-and-jekyll-1.html{:target="_blank"}
[github-jekyll-blog-2]: https://dev-iann.github.io/2022/10/15/start-blog-with-github-and-jekyll-2.html{:target="_blank"}
[github-jekyll-blog-3]: https://dev-iann.github.io/2022/10/15/start-blog-with-github-and-jekyll-3.html{:target="_blank"}
[install-utterances]: https://github.com/apps/utterances{:target="_blank"}
