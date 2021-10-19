# Optimizing the DOM

## size of HTML

1. HTML의 크기를 최소로 해야한다.

- 사전처리 및 상황별 최적화

  - 주석들을 없애야함(브라우저로 주석을 보내 줄 이유가 없다.)
  - 헤더의 인코딩방식을 다르게 한다.
  - 반복되는 데이터를 압축(AAA=>3A)
  - 중복되는 style 정의를 합친다.
  - 공백(스페이스나 탭)을 제거한다.

2. 압축해야한다.

- GZIP을 이용한 텍스트 압축
  - GZIP은 텍스트 기반 압축 툴이다.
  - 모든 최신 브라우저는 이를 지원하고 자도으로 요청한다.
  - 일부 CDN의 경우 GZIP이 활성화 되었는지 확인해야한다.
  - 개발자 도구 네트워크탭에서 Size/Content열을 통해 압축된 크기를 확인할 수 있다.

사전처리 및 상황별 최적화 후 GZIP을 적용하여 최소화된 출력을 압축하면 큰 절감효과를 얻을 수 있다.

3. 브라우저에 의해 캐싱해야한다.

- 서버를 통해 리소스를 받아오기 전까지 랜더링이 되지 않기 떄문에 캐싱을 통해 서버로의 요청을 줄이면 최적화할 수 있다.
- 브라우저의 모든 HTTP요청은 브라우저 캐시로 라우팅되어 유효한 캐시가 있는지 먼저 확인한다.
- 이는 요청 헤더와 응답헤더의 조합에 의해 제어된다.
- 기본적으로 브라우저는 사용자를 대신하여 헤더설정을 관리한다.( 자동으로 캐싱한다. )
- 웹서버에서 `Cache-Control`헤더를 통해 관리할 수 있다.

## Unblocking CSS

- HTML,CSS는 기본적으로 랜더링 차단 리소스이다.
- 랜더링하려면 렌더트리가 필요하고, 렌더트리를 만들기 위해선 서버에서 CSS를 가져올 때 까지 기다려야한다.
- 미디어 유형과 미디어 쿼리를 통해 일부 css를 비차단 리소스로 표시할 수 있다.
- 브라우저는 차단동작이든 비차단 동작이든 관계없이 모든 CSS 리소스를 다운로드 한다.

```css
body {
  font-size: 16px;
}

@media screen and (orientation: landscape) {
  .menu {
    float: right;
  }
}

@media print {
  body {
    font-size: 12px;
  }
}
```

- print 파일을 분리된 파일로 옯긴다.
- 미디어 속성을 통해 해당 css가 언제 적용될 지 브라우저에게 전달할 수 있다.(비차단 리소스로 표시하기)

```css
body {
  font-size: 16px;
}

@media screen and (orientation: landscape) {
  .menu {
    float: right;
  }
}
```

```css
@media print {
  body {
    font-size: 12px;
  }
}
```

```HTML
<link rel="stylesheet" href="style.css">
<link rel="stylesheet" href="style-print.css" media="print">
```
