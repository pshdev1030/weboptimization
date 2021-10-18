# The Dom

## Critical Rendering Path

- 브라우저가 HTML,CSS,Javascript를 화면에 실제 픽셀로 변환하는 단계의 순서.

1. HTML을 가져와서 DOM(Document Object Model)을 생성한다.
2. CSS를 가져와서 CSSOM(CSS Object Model)을 생성한다.
3. 둘을 합쳐서 Render Tree를 만든다.
4. 레이아웃 단계를 거쳐서 모든 것이 페이지의 어느 위치에 갈 것인지 정한다.
5. 실제 화면에 픽셀을 그린다.

## Converting HTML to the DOM

- URL을 요청하고 엔터를 누르면 브라우저가 서버로 HTML 요청을 보낸다.

1. 브라우저가 HTML을 읽어와서 지정된 인코딩에 따라 개별문자로 변환한다.
2. HTML은 태그를 만날 때 마다 W3C HTML5표준에 맞춰 토큰을 만든다.
   <img src="https://developers.google.com/web/fundamentals/performance/critical-rendering-path/images/full-process.png">
3. 토큰은 해당 속성 및 규칙을 정의하는 `객체`로 변환됩니다.
4. HTML 마크업 간의 관계를 정의하기 떄문에 생성된 객체는 트리 데이터 구조 내에 연결됩니다.(노드가 됩니다.) 이는 상하위 관계도 포함합니다. 즉 HTML객체는 body의 상위이고, body는 paragraph 객체의 상위인 식입니다.
   <img src="https://developers.google.com/web/fundamentals/performance/critical-rendering-path/images/dom-tree.png">

이러한 구조로 부분 HTML을 먼저 로드하여 성능개선을 할 수 있다.

## Converting CSS to the CSSOM

1. 유효한 토큰이 존재하는지 확인한다.
2. 토큰을 노드화한다.
   <img src="https://developers.google.com/web/fundamentals/performance/critical-rendering-path/images/cssom-tree.png">

부분 css의 사용은 불가능하다. 잘못된 스타일을 사용 할 수 있기 때문이다.
브라우저는 모든 CSS를 받고 처리할 때 까지 페이지 랜더링을 차단한다.

- 선택자의 차이

```css
h1 {
  font-size: 16px;
}
div p {
  font-weight: 12px;
}
```

첫 번째 태그는 h1에 일괄적으로 적용하지만 두 번째 태그는 모든 p 태그를 찾고
부모노드가 div인 것을 찾아서 스타일을 적용하기 때문에 브라우저가 더 많은 작업을 해야한다.

## The Render Tree

- DOM 과 CSSOM트리를 합친 트리이다.
- 눈에 보이는 내용만 포함한 트리이다.
  - `display:none`과 같은 style이 있는 노드의 경우 포함되지 않는다.
  - html태그는 눈에 보이는 내용이 없으므로 제거한다.
    <img src="https://developers.google.com/web/fundamentals/performance/critical-rendering-path/images/render-tree-construction.png">

## Layout

- width
  - `%`의 경우 메타태그에서 설정한 뷰포트의 너비 or 부모의 너비를 상속한다.
  - 아래의 태그의 경우 뷰포트 너비를 디바이스의 너비로 설정한다. 기본값은 980px이다.
  ```HTML
  <meta name="viewport" content="width=device-width">
  ```
- 스타일이나 내용을 변경하여 렌더트리를 업데이트 할 경우 레이아웃 단계를 다시 실행할 가능성이 크다.
- 뷰포트 너비가 변하면 브라우저는 레이아웃 단계를 다시 시행해야 한다.
  (폰을 돌리거나 브라우저 크기를 조정할 떄 일어난다.)

이를 피하기 위해 여러번의 레이아웃 이벤트를 피하고자 업데이트를 한 번에 반영하는 것이다.
