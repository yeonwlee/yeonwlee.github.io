---
title: Chirpy 테마에서 목차 및 검색 오류 수정하기
author: yeonwlee
date: 2024-01-26 19:12:00 +0900
categories: [Blogging]
tags: [jekyll, 문제해결]
---

- [1. 문제](#1-문제)
- [2. 원인 추정](#2-원인-추정)
  - [2.1 프로젝트 세팅을 잘못했나?](#21-프로젝트-세팅을-잘못했나)
  - [2.2 헤드라인 태그의 내부 형식 때문인가?](#22-헤드라인-태그의-내부-형식-때문인가)
  - [2.3 라이브러리를 못 불러오나?](#23-라이브러리를-못-불러오나)
- [3. 문제 해결](#3-문제-해결)
  - [라이브러리를 내장 하자](#라이브러리를-내장-하자)
  - [추가로 발견한 예비 문제도 수정하자](#추가로-발견한-예비-문제도-수정하자)

<br>

---

<br>

Chirpy 테마가 아주 유용해보여서 기존 테마에서 변경 적용하던 중 문제에 봉착했다.

## 1. 문제

목차를 생성하지 못하고, 검색이 특정 페이지에서만 동작

## 2. 원인 추정

### 2.1 프로젝트 세팅을 잘못했나?

여러 번 다른 방법으로 해봤을 때, 딱히 그런 문제는 아닌 것으로 판단했다.

### 2.2 헤드라인 태그의 내부 형식 때문인가?

목차 라이브러리에는 문제가 없다고 가정,
헤드라인 태그의 내부 형식이 타 chirpy 사용자와 다르다는 걸 발견해서
\_include/refactor-content.html을 손봤다. 하지만 타 블로거와 동일한 형식으로 만들어도 별 효과가 없었다.

### 2.3 라이브러리를 못 불러오나?

사실 이걸 제일 먼저 확인했어야함. 이또한 웹페이지니, **개발자 도구의 콘솔 내용을 우선 확인하는게 현명했다.**
2.2 하다가 콘솔에 에러 나있는 걸 발견해서 확인해보니 라이브러리를 못 읽어오는 것이 아닌가. 명시적으로 에러가 난 라이브러리는 목차나 검색창에 대한 라이브러리는 아니었지만 라이브러리를 못 읽어 오는게 비단 저것만은 아닐 수 있다고 생각했다.

- 목차 관련해서는 jquery를 못 불러오는지 $ 에 대한 에러도 나있었다.

충격. 여튼 이미 이리저리 코드를 둘러본 터라, 문제에 대한 해결은 빠른 편이었다.

## 3. 문제 해결

### 라이브러리를 내장 하자

\_include/jsdelivr-combine.html과

\_data/cors.yml

을 봤을 때, 내부적으로 CDN 링크를 이어 자원을 가져오려 하는 것 같은데 결과적으로 못 가져 오고 있는 것이었다.
이렇게 된 김에, CDN으로 끌어오는 것보다 라이브러리들을 직접 갖고 있는게 해당 CDN 리소스가 문제가 생겼을 때 안전할 것으로 보여서 라이브러리를 직접 다운 받았다.

\_config.yml 파일에서 주석으로 안내되어 있었던 대로,
<https://github.com/cotes2020/chirpy-static-assets>
에서 라이브러리들을 다운, assets/lib/ 하위에 넣어줬다(\_data/basic.yml에 정의된 경로)

그리고,
\_include/origin-type.html에서
기본적 설정을

assign type = 'cors'에서

assign type = 'basic'으로 변경했다.

그리고 다시 확인 해보니, 잘 동작하더라.

### 추가로 발견한 예비 문제도 수정하자

이리저리 코드를 둘러보다가, 생각지 못하게 목차가 동작하지 않을 수 있는 경우가 있을 것을 발견했다. 바로, 포스팅 내에 h2 태그가 없으면 목차가 자동으로 생성되지 않을 것이란 거였다.

main 하위의 h2 태그가 있어야 목차를 생성하는 라이브러리의 객체를(tocbot) init하도록 구현되어 있기 때문.

그래서 수정했다.

\_javascript/modules/components/toc.js를 보면,

```javascript
export function toc() {
  if (document.querySelector("main h2")) {
    // see: https://github.com/tscanlin/tocbot#usage
    tocbot.init({
      tocSelector: "#toc",
      contentSelector: ".content",
      ignoreSelector: "[data-toc-skip]",
      headingSelector: "h2, h3, h4",
      orderedList: false,
      scrollSmooth: false
    });
  }
}
```

요렇게 되어있는데, 다른 목차 관련 설정은 차치하고

if (document.querySelector('main h2')).. 부분을
main h2,h3,h4 로 맞춰줬다.

그리고 재빌드하면 오케이.