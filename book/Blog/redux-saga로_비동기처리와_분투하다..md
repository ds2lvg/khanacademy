https://webcache.googleusercontent.com/search?q=cache:E9zZxYBSHZcJ:https://github.com/reactkr/learn-react-in-korean/blob/master/translated/deal-with-async-process-by-redux-saga.md+&cd=4&hl=ko&ct=clnk&gl=kr

## redux-saga
- "Task" 개념을 Redux로 가져오기위한 지원 라이브러리
  - Task: 일의 절차와 같은 독립적인 실행 단위로써, 각각 평행적으로 작동
- 비동기처리를 Task로써 기술하기 위한 준비물인 "Effect"와 비동기처리를 동기적으로 표현하는 방법을 제공
  - Effect: Task를 기술하기 위한 커맨드
    - put: Action을 dispatch한다.
    - take: Action을 기다린다. 이벤트의 발생을 기다린다.
    - call: Promise의 완료를 기다린다.
    - fork: 다른 Task를 시작한다.

## redux-saga 장점
- Mock 코드를 많이 쓰지 않아도 된다.
- 작은 코드로 더 분할할 수 있다.
- 재이용이 가능해진다.

## redux-thunk로 만들어진 코드와 비교
### 통신처리의 상태를 어떻게 가지게 할 것인가.

```javascript
// api.js
export function user(id) {
  return fetch(`http://localhost:3000/users/${id}`)
    .then(res => res.json())
    .then(payload => ({ payload }))
    .catch(error => ({ error }));
}
```

- Thunk

```javascript
// actions.js
export function fetchUser(id) {
  return dispatch => { 
    // 통신처리를 개시하기 전에 REQUEST_USER Action을 dispatch한다.
    dispatch(requestUser(id)); 
    // API.user 함수를 불러내서 통신처리를 개시
    API.user(id).then(res => {
      const { payload, error } = res;
      // 완료되면 SUCCESS_USER 혹은 FAILURE_USER Action을 dispatch
      if (payload && !error) { 
        dispatch(successUser(payload));
      } else {
        dispatch(failureUser(error));
      }
    });
  };
}
```
- Saga

```javascript
// sagas.js
function* handleRequestUser() {
  while (true) {
    // take Effect로 REQUEST_USER Action이 dispatch되길 기다린다.
    const action = yield take(REQUEST_USER);
    // (누군가가 REQUEST_USER Action을 dispatch한다.)
    // call Effect로 API.user 함수를 불러와서 통신처리의 완료를 기다린다.
    const { payload, error } = yield call(API.user, action.payload);
    // put Effect를 사용하여 SUCCESS_USER 혹은 FAILURE_USER Action을 dispatch한다.
    if (payload && !error) {
      yield put(successUser(payload));
    } else {
      yield put(failureUser(error));
    }
  }
}

export default function* rootSaga() {
  // fork Effect로 인해 handleRequestUser Task가 시작
  yield fork(handleRequestUser); 
}
```

- effect 오브젝트는 생성될뿐 아무것도 하지않으므로, redux-saga로 넘겨서 실행시켜야 한다.
- 

### 어디서 통신처리를 적을 것인가
### 어디로부터 통신처리를 불러올 것인가

## 초보적인 셋업방법과 실수하기 쉬운 부분을 설명

## 실전적인 redux-saga의 사용법을 소개