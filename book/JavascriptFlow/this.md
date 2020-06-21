## call apply bind

```javascript
function fn() { 
  console.log(this);
  Array.prototype.forEach.call(arguments, v => console.log(v))
}
var obj = { k: "val" }

// call: 매개변수를 thisArg와 연결해서 즉시호출
// fn.call(thisArg, arg1, arg2, ...);
fn.call(obj, 1, 2, 3, 4, 5, 6); // { k: "val" }, 1,2,3,4,5,6

// apply: 매개변수로 들어온 배열을 thisArg와 연결해서 즉시호출
// fn.apply(thisArg, [argsArray]);
fn.apply(obj, [1, 2, 3, 4, 5, 6]); // { k: "val" }, 1,2,3,4,5,6

// bind: 매개변수를 thisArg와 연결해서 커링(currying) 함수 생성
// fn.bind(thisArg, arg1, arg2, ...);
var fnBind = fn.bind(obj, 1, 2, 3);
fnBind(4,5,6); // { k: "val" }, 1,2,3,4,5,6
```

## setTimeout에서 this 변경
```javascript
var cb = function() {
  console.log(this);
}
var obj = { k: "val" }
setTimeout(cb, 100) // Window
setTimeout(cb.bind(obj), 100) // { k: "val" }
```
