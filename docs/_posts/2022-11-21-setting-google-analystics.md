---
layout: post
title:  "jekyll에 Google Analytics 연동하기"
hide_title: false
date:   2022-11-21 23:00:00 +0900
# feature-img: "assets/img/sample.png"              # Add a feature-image to the post
# thumbnail: "assets/img/thumbnails/sample-th.png"  # Add a thumbnail image on blog view
# color: rgb(80,140,22)                             # Add the specified color as feature image, and change link colors in post
# bootstrap: true                                   # Add bootstrap to the page
tags: [github, jekyll, blog, googleanalytics, ga]
---

안녕하세요. 개발자 Iann입니다.

<br>

블로그는 만들었는데, 내 블로그에 방문자는 얼마나 되는지 어떤 아티클을 많이 보는지 궁금할텐데요. 오늘은 이것을 확인할 수 있는 `Google Analytics`를 `Jekyll`에 붙여보도록 하겠습니다.

네이버 블로그나 티스토리 velog 등의 블로그 서비스에는 방문자 카운팅 기능들이 들어있지만, Github pages... Jekyll은 모든 것이 수작업입니다. 가내수공업......ㅋㅋ 백엔드 없는 SSG의 정겨움이라고 해두겠습니다.

블로그에 방문자가 들어올때마다 `Google Analytics`의 api를 호출해서 `Google Analytics`에 방문 이력을 쌓도록 작업해보겠습니다.

---
### Google Analytics 만들기
---

블로그의 방문자 측정을 위해 `Google Analytics`에 일종의 프로젝트를 만듭니다.
먼저, [Google Analytics][google-analytics]에 접속합니다.

<br>

![Google Analytics](/assets/images/2022/11/google_analytics_1.png)

google 계정이 없다면 만들고, 정상적으로 로그인했다면 위와 같은 화면이 나옵니다. `측정 시작` 버튼을 누릅니다.

<br>

![Google Analytics](/assets/images/2022/11/google_analytics_2.png)

계정 이름을 입력하라고 나오는데, 이 계정이 프로젝트라고 생각해도 무방합니다. 즉, 계정 이름 = 프로젝트 명이라 생각하고 적당히 넣어줍니다. 이 계정 이름은 어차피 수정 가능하니 크게 고민 안해도 됩니다.

<br>

![Google Analytics](/assets/images/2022/11/google_analytics_3.png)

다음으로 속성 이름입니다. 번역된 문구이긴 하지만 `Google Analytics`의 사상을 공부하지 않으면 당최 뭐가 뭔지 직관적이지 않습니다. 속성은 프로젝트 내의 어떤 서비스라고 생각하면 됩니다. 대충 `Github Pages`라고 이름 지어 줍니다. 속성 이름도 나중에 수정 가능하니 마음 가는대로 작성해도 괜찮습니다.

<br>

![Google Analytics](/assets/images/2022/11/google_analytics_4.png)

다음으로 비즈니스 정보입니다. 일개 블로그인데 비즈니스랄꺼까지야.. 대충 선택하고 만들기 버튼을 누릅니다.

<br>

![Google Analytics](/assets/images/2022/11/google_analytics_5.png)

뭔가 나왔습니다. `Google Analytics`에 블로그 정보를 입력해봅시다. 블로그는 웹으로 접근할 것이기에 웹을 선택합니다.

<br>

![Google Analytics](/assets/images/2022/11/google_analytics_6.png)

그리고 나오는 화면에서 블로그 주소를 입력하고, 스트림 이름은 적당히 작성해줍니다.

<br>

![Google Analytics](/assets/images/2022/11/google_analytics_7.png)

측정 ID가 나왔습니다. 우린 이걸 Jekyll에 집어 넣어 방문자들이 블로그에 접근할 경우 이 측정 ID로 `Googla Analytics`에 데이터를 보내게 해야합니다. 화면 상단의 `태그 안내 보기`를 누르면

<br>

![Google Analytics](/assets/images/2022/11/google_analytics_8.png)

직접 설치 탭을 누르면 스크립트 코드가 나옵니다. Jekyll에 이 코드가 들어가야 합니다.

<br>

---
### Jekyll 프로젝트에 측정 ID 입력하기
---

<br>

```yml
google_analytics: G-XXXXXXXXXX                          # Tracking ID, e.g. "UA-000000-01"
```

Jekyll 테마마다 다르겠지만, 대부분 `_config.yml`에 `google analytics`를 연동하는 설정이 있습니다. 제가 사용중인 `Type-on-Strap` 테마에도 지원하고 있어 측정아이디를 입력했습니다.

<br>

그.런.데 현재의 `google analytics`는 `GA4`로 불리우고 그 이전엔 `UA`로 불리웁니다. 뭔가 달라졌단 이야기죠. `Type-on-Strap`에서 제공하는 `_config.yml`을 보면, 주석에 `UA-000000-01`이라는 문구가 눈에 들어옵니다. `GA4`는 `G-XXXXXXXXXX` 포맷의 측정 ID를 갖지만, `UA`는 `UA-000000-01` 포맷을 측정 ID로 갖는듯 합니다. 주석에 `UA`가 있어 왠지 쎄한 느낌이 들어 코드를 조금 들여다보기로 합니다.

Jekyll에서 `google_analytics`로 전체 검색을 해봅니다.

```liquid
{% if site.google_analytics %}
    <!-- Global site tag (gtag.js) -->
    <script async src="https://www.googletagmanager.com/gtag/js?id={{ site.google_analytics }}"></script>
    <!-- Page analysis (analytics.js) -->
    <script async src='https://www.google-analytics.com/analytics.js'></script>
{% endif %}
```

<br>

`head.liquid` 파일에 위의 직접 설치 탭에서 보았던 코드 일부가 들어가 있습니다. 일부만 있고.. 더 있어야 될 것 같은데.... 누락된 코드를 찾으러 `gtag` 키워드로 다시 검색해봅니다.

```javascript
function googleAnalytics() {
  if (analyticsName.toLowerCase() !== '') {
    // Google tag manager
    window.dataLayer = window.dataLayer || [];
    function gtag() { dataLayer.push(arguments); }
    gtag('js', new Date());
    gtag('config', analyticsName);
    gtag('config', analyticsNameGA4, { 'anonymize_ip': true });

    // Google analytics
    window.ga = window.ga || function () { (ga.q = ga.q || []).push(arguments) };
    ga.l = +new Date;
    ga('create', analyticsName, 'auto');
    ga('send', 'pageview');
  }
}

if (isCookieConsent.toLowerCase() === 'true') {
  addCookieConsentListener();
  if (readCookie(cookieName) === 'true') {
      googleAnalytics();
  } else {
  document.getElementById('cookie-notice').style.display = 'block';
  }
} else {
  googleAnalytics();
}

```

그랬더니 `cookie_consent_init.js`에 나머지 코드가 들어 있습니다. 이 파일은 `gulp`에서 지지고 볶는걸로 보이는데, 다 처리되어 있다고 생각하고 일단 패스!

<br>

`_config.yml`을 푸쉬하고 블로그에 접속해봅니다. 그리고 `google analytics` 보고서를 봅니다.

<br>

![Google Analytics](/assets/images/2022/11/google_analytics_9.png)

오... 카운트가 들어왔네요. 상세 조회하면 페이지별로 조회수도 볼 수 있습니다.


[google-analytics]: https://analytics.google.com/analytics/web/provision{:target="_blank"}
[google-analytics]: https://search.google.com/search-console/about{:target="_blank"}