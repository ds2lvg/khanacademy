- 머터리얼 컬러지정: https://material.io/resources/color/#!/?view.left=0&view.right=0

## 1.플렉스 레이아웃 핵심
- react-native에서도 사용 가능, 여기서는 웹 기준으로 설명
- Container 와 Item 에 적용 되는 속성이 구분
  - Container : display, flex-direction, flex-wrap, flex-flow, jutify-content, align-item, align-content
  - Item : order, flex-grow, flex-shrink, flex, align-self

```js
  <Container>
    <Item>
    <Item>
    <Item>
  <Container>
```
- main axis(중심축), cross axis(반대축)이 존재
  - 중심축을 기준으로 교차축 즉 반대되는 축이 존재.

### Container
- flex-direction(row) : main axis 설정 [row, row-reverse, column, column-reverse]
- flex-wrap(nowrap) : 안감싸면 한줄로 처리, 감싸면 여러줄 처리 [nowrap, wrap, wrap-reverse] 
- flex-flow(row nowrap) : flex-direction + flex-wrap
- jutify-content(flex-start) : main axis에서 Item들을 어떻게 배치(정렬+간격)할 것인지
  - flex-start    : main axis 앞쪽에 배치(row라면 왼쪽 배치, column이라면 상단 배치)
  - flex-end      : main axis 끝쪽에 배치(row라면 오른쪽 배치, column이라면 하단 배치)
  - center        : 중앙 배치
  - space-around  : 좌우 둘러싼 마진 줘서 배치(첫번째의 왼쪽과 마지막 요소의 오른쪽은 그만큼 좁음)
  - space-evenly  : 똑같은 마진 줘서 배치(첫번째와 마지막 요소도 동일)
  - space-between : Item 사이에만 똑같은 마진줘서 배치 (첫번째의 왼쪽과 마지막 요소의 오른쪽은 마진 안줌)
- align-items(stretch) : cross axis에서 Item들을 어떻게 배치(정렬)할 것인지
  - stretch    : 스트레칭 하듯 부모 크기만큼 늘림 -> flex-direction:row일때 Item를 height:100%로 렌더링하는 이유
  - flex-start : 컨텐츠 크기에 맞게 변경해서 main axis 앞쪽에 배치
  - flex-end   : 컨텐츠 크기에 맞게 변경해서 main axis 끝쪽에 배치
  - center     : 중앙 배치
  - baseline   : 패딩등 크기가 다르자면, 텍스트를 기준으로 균등하게 배치
- align-content(flex-start) : 화면이 줄었을때 cross axis에서 Item들을 어떻게 배치(정렬+간격)할 것인지
  - flex-wrap: wrap가 선언시 작동
  - flex-start: main axis 앞쪽에 배치(row라면 왼쪽 배치, column이라면 상단 배치)
  - flex-end: main axis 끝쪽에 배치(row라면 오른쪽 배치, column이라면 하단 배치)
  - center : 중앙 배치
  - space-around : 시작점과 끝점을 나눠서 배치(시작점과 끝점도 여백 존재)
  - space-between : 시작점과 끝점으로 나눠서 배치(시작점과 끝점은 서로 끝점에 여백제거하고 붙임)

### Item
- order(1) : 배치 순서 바꿈
- flex-grow(0) : container가 화면 보다 넓을때 Item이 차지하는 비율
- flex-shrink(0) : container가 화면 보다 좁을때(줄어들때) Item이 차지하는 비율
- flex-basis(auto) : Item이 차지하는 비율(%)을 고정해서 지정
- flex(0 0) : flex-grow: 0; flex-shrink:0;의 축약형, flex-basis가 자동으로 0으로 적용
- align-self(auto) : 각각 Item별로 다르게 정렬
  - flex-start: 컨텐츠 높이에 맞게 높이 변경
  - flex-end: main axis 끝쪽에 배치
  - center: 중앙 배치
  - stretch: 스트레칭 하듯이 부모 크기만큼 쭉 늘림
  - baseline: 패딩이 있다면 텍스트를 기준으로 균등하게 배치