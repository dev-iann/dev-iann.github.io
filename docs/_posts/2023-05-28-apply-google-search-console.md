---
layout: post
title:  "jekyll에 Google Analytics 연동하기"
hide_title: false
date:   2022-11-13 23:00:00 +0900
# feature-img: "assets/img/sample.png"              # Add a feature-image to the post
# thumbnail: "assets/img/thumbnails/sample-th.png"  # Add a thumbnail image on blog view
# color: rgb(80,140,22)                             # Add the specified color as feature image, and change link colors in post
# bootstrap: true                                   # Add bootstrap to the page
tags: [github, jekyll, blog, googleanalytics, ga]
---


안녕하세요. 개발자 Iann입니다.

---
### 검색 엔진 등록하기
---

<br>

블로그를 만들었다면, 검색에 노출이 되어 여러사람이 보면 좋겠죠? 구글 검색에 노출시키기 위해서는 블로그 페이지에 관련 코드를 포함시켜야 하지만, `google analytics`를 연동했다면 이 과정이 생략됩니다. 내친김에 검색 엔진에도 등록해봅니다.

[Google Search Console][google-search-console]에 접속합니다.

<br>

![Google Search Console](/assets/images/2022/11/google_search_console_1.png)

<br>

`Google Search Console`을 처음 사용하면 위와 같은 화면이 나옵니다. 시작하기를 누릅니다.

<br>

![Google Search Console](/assets/images/2022/11/google_search_console_2.png)

<br>

오른쪽의 URL 접두어 메뉴에서 블로그 URL을 입력합니다.

<br>

![Google Search Console](/assets/images/2022/11/google_search_console_3.png)

입력한 URL이 본인 소유의 페이지라는 것을 증명해야 합니다. 보통은 구글에서 지정한 특정 태그를 넣어야 합니다. 하지만, `Google Analytics`를 연동하면서 고유의 `측정 ID`를 입력했으므로 이걸로 대체할 수 있습니다. `Google 애널리틱스` 항목을 선택하고 `확인`을 누릅니다. 검증 과정은 시간이 좀 걸립니다. 기다리다보면 검증 결과가 나옵니다.

<br> 

![Google Search Console](/assets/images/2022/11/google_search_console_4.png)

<br>

오잉? 실패했습니다.. 결론을 바로 말하자면, `head` 태그 아래에 `Google Analytics`에서 가이드하는 태그가 페이지에 정확히 들어가있어야 합니다..

<br>

```html
<script async src="https://www.googletagmanager.com/gtag/js?id={{ site.google_analytics }}"></script>
  <script>
    window.dataLayer = window.dataLayer || [];
    function gtag() {
      dataLayer.push(arguments);
    }
    gtag('js', new Date());

    gtag('config', '{{ site.google_analytics }}');
</script>
```

<br>

태그는 위와 같습니다. 제가 사용한 `Type-on-Strap` 테마의 경우 브라우저의 페이지 소스보기로 확인해보면,

<br>

```html
<script async src="https://www.googletagmanager.com/gtag/js?id={{ site.google_analytics }}"></script>
```

<br>

위 스크립트 태그만 들어가 있습니다. `Google Anaytics`에서 가이드하는 나머지 태그는 `cookie_consent_init.js` 파일에 있었는데 페이지 소스상으론 확인이 안됩니다. <font color="#ff0000"><b>구글에서 가이드하는 태그의 내용이 정확하게 들어가야 되는 것</b></font>으로 보입니다. 그래서 위 코드를 그대로 넣어주었습니다. 제가 사용하는 테마의 경우, head.liquid 파일이었습니다.

<br>

```javascript
if (isCookieConsent.toLowerCase() === 'true') {
  addCookieConsentListener();
  if (readCookie(cookieName) === 'true') {
      // googleAnalytics();
  } else {
  document.getElementById('cookie-notice').style.display = 'block';
  }
} else {
  // googleAnalytics();
}
```

<br>

`cookie_consent_init.js`파일에 있던 google anaytics 관련 코드는 주석처리 했습니다.

<br>

![Google Search Console](/assets/images/2022/11/google_search_console_5.png)

<br>

네, 소유권이 확인되었습니다. 그.러.나 등록된 포스트 제목으로 검색해도 검색되지 않습니다. 검색 노출에 용이하게 하기 위해 `Sitemap`을 등록해봅니다.
<br>
Jekyll 프로젝트의 루트에 sitemap.xml을 추가하고 다음의 내용을 저장합니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  {% for post in site.posts %}
    <url>
      <loc>{{ site.url }}{{ post.url }}</loc>
      {% if post.lastmod == null %}
        <lastmod>{{ post.date | date_to_xmlschema }}</lastmod>
      {% else %}
        <lastmod>{{ post.lastmod | date_to_xmlschema }}</lastmod>
      {% endif %}

      {% if post.sitemap.changefreq == null %}
        <changefreq>weekly</changefreq>
      {% else %}
        <changefreq>{{ post.sitemap.changefreq }}</changefreq>
      {% endif %}

      {% if post.sitemap.priority == null %}
          <priority>0.5</priority>
      {% else %}
        <priority>{{ post.sitemap.priority }}</priority>
      {% endif %}

    </url>
  {% endfor %}
</urlset>
```