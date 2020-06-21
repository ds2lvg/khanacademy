## callback function
- 제어권을 콜백함수에 넘긴다.
- 함수 A의 인자로 콜백함수 B를 전달하면 A가 B의 제어권을 갖게 된다.
  - 특별한 요청(bind)가 없는 한 A에 미리 정해놓은 방식에 따라 B를 호출한다.
  
### forEach
``` arr.forEach(callback[, thisArg]) ```
- callback
  - currentValue: 배열에서 현재 처리 중인 요소
  - index: 배열에서 현재 처리중인 요소의 인덱스
- thisArg: callback을 실행할때 this로서 사용하는 값

```javascript
var arr = [1, 2, 3, 4, 5];
var entries = [];
arr.forEach(function(v, i) {
  entries.push([i, v, this[i]]);
}, [10, 20, 30, 40, 50]);
console.log(entries);
// [[0,1,10], [1,2,20], [2,3,30], [3,4,40], [4,5,50]]
```

```javascript

```

```javascript

```

```javascript

```

```javascript

```