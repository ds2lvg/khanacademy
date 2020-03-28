## 01.불변 객체 필요한 경우
- 값으로 전달 받은 객체에 변경을 가하더라도 원본 객체는 변하지 않아야 하는 경우에 필요
- 불변성을 유지하지 않으면 아래처럼 기존 객체를 가져다가 바뀐 내용의 객체를 만들면 같다고 판단해버린다.

```javascript
const user = {
  name: '연우',
  gender: 'male',
};

const chageName = (user, newName) => {
  const newUser = user;
  newUser.name = newName;
  return newUser;
};

const user2 = chageName(user, 'Jung');

if(user !== user2) {
  console.log('유저 정보가 변경되었습니다.');
}

console.log(user.name, user2.name); // Jung Jung
console.log(user === user2); // true
```

## 02. 새로 객체를 반환해서 불변성 유지
- 함수에서 기존 객체의 내용을 가져다 써도 결과를 새로운 객체로 반환하면 결과는 다른 객체이다.

```javascript
const user = {
  name: '연우',
  gender: 'male',
};

const chageName = (user, newName) => {
  return {
    name: newName,
    gender: user.gender,
  }
};

const user2 = chageName(user, '아이린');

if(user !== user2) {
  console.log('유저 정보가 변경되었습니다.');
}

console.log(user.name, user2.name); // 아이린 아이린
console.log(user === user2); // false
```

## 03.얇은 복사
- 바로 아래 단계 값만 복사
- 함수에서 순회하면서 새로운 객체에 내용을 첫번째 단계의 내용을 복사해서 리턴한다.

```javascript
const user = {
  name: '연우',
  gender: 'male',
};

const copyObject = (target) => {
  const result = {};
  for (let prop in target) {
    result[prop] = target[prop];
  }
  return result;
};

const user2 = copyObject(user);
user2.name = '아이린';

if(user !== user2) {
  console.log('유저 정보가 변경되었습니다.');
}

console.log(user.name, user2.name); // 아이린 아이린
console.log(user === user2); // false
```

## 04. 깊은 복사
- 모든 단계의 값을 복사
- 아랫단계가 object일 경우 재귀호출로 복사한다.

```javascript
const user = {
  name: '연우',
  urls: {
    portfolio: 'http://jongmin.site/',
    blog: 'https://likejirak.tistory.com/',
    facebook: 'https://www.facebook.com/alexander.kim.735507',
  }
};

const copyObjectDeep = (target) => { 
  let result = {};
  if (typeof target === 'object' && target !== null) {
    for (let prop in target) {
      result[prop] = copyObjectDeep(target[prop]);
    }
  } else {
    result = target;
  }
  return result;
};

const user2 = copyObjectDeep(user);
user2.name = '아이린';

console.log(user === user2); // false

user.urls.portfolio='http://portfolio.com';
console.log(user.urls.portfolio === user2.urls.portfolio); // false

user2.urls.blog = '';
console.log(user.urls.blog === user2.urls.blog); // false
```

## 05. JSON을 활용한 깊은 복사
- json문자열로 바꿧다가 JSON으로 파싱하면 불변성이 유지된다.
- 함수와 숨겨진 프로퍼트등 JSON으로 변경할 수 없는 것들은 무시된다.

```javascript
const copyObjectVialJSON = (target) => {
  return JSON.parse(JSON.stringify(target));
}

const obj = {
  a: 1,
  b: {
    c: null,
    d: [1,2],
    fn1: function() { console.log(3) }
  },
  fn2: function() { console.log(4) }
}

const obj2 = copyObjectVialJSON(obj);

console.log(obj);  // { a: 1, b: { c: null, d: [ 1, 2 ], fn1: [Function: fn1] }, fn2: [Function: fn2] }
console.log(obj2); // { a: 1, b: { c: null, d: [ 1, 2 ] } }
```