# ✒️ Promise

#### 1. JS 자체는 싱글 스레드 언어이다.

-   JS는 기본적으로 오직 하나의 스레드만을 사용하여 프로세스를 진행시킨다.
-   따라서 코드가 순차적으로 실행되며, 위에서 코드가 막힐 경우 진행이 불가하다.
-   하지만 Node.js나 브라우저에서 비동기 작업을 위한 여러 보조를 진행해준다.

> 비동기 (Asyncronous) 란?

-   요청과 결과가 동시에 일어나지 않는것을 의미한다.

#### 2. Callback Function

1. 정의

-   콜백함수는 다른 함수에 매개변수로 넘겨준 함수를 의미함.
-   JS 에서 함수는 1급 객체이기에, 함수를 또 다른 함수의 인자로 줄 수 있음.

2. 사용 목적

-   콜백함수는 자바스크립트의 비동기성을 표현하고 관리하는 가장 일반적인 기법이다.
-   함수를 명시적으로 호출하지 않고, 특정 시점에서 호출시키기 위함.
-   비동기 코드를 동기적으로 작동하는 것처럼 하기 위해, 콜백함수를 사용함.

3. 문제점

-   JS 를 활용한 비동기 작업을 처리할 시, 여러 개의 콜백 함수를 연결시키면 문제가 됨.
-   A 작업이 끝나면 B를 수행하고, B가 끝났다면 C를 수행하는 연쇄 작업에서 두드러짐.
-   콜백함수를 남발하면, 순차적으로 읽기 어려운 코드가 생성이 되고 복잡도가 증가한다.

```javascript
step1(function (value1) {
    step2(function (value2) {
        step3(function (value3) {
            step4(function (value4) {
                step5(function (value5) {
                    step6(function (value6) {
                        // this is callback hell...
                    });
                });
            });
        });
    });
});
```

-   작업의 성공과 실패를 표현하는 기준이 없다. (Promise의 경우에는 `Fulfilled`, `Reject` 로 표현한다)
-   따라서 프로미스 등장 이전에는 분할 콜백 패턴 또는 에러 우선 스타일 패턴이 등장했다.

```javascript
// 분할 콜백 패턴 (success는 성공, error는 실패 시 실행할 콜백 함수)

const success = function(data) {
	console.log(data);
};

const fail = function(err) {
	console.log(err);
};

$.ajax({
    url: `https://example/api/v1/users?param1=${param1}&param2=${param2}`,
    method: 'GET',
    async = true,
    dataType: 'html',
    success : success,
    error: fail,
});
```

-   비동기 함수 호출 시, Web API 쓰레드에 요청을 하기에 해당 요청에 대한 제어권이 개발자에게는 없다.
-   즉, 비동기 작업이 개시된 이후에는 제어권을 위임한 곳에서 작업이 원활하게 이루어지기를 바래야만 한다.

#### 3. Promise

1. 정의

-   Promise란, 비동기 작업의 최종 성공 혹은 실패를 나타내는 객체이다.
-   단, 반드시 비동기 작업의 최종 결과를 반환하는 것이 아니라 미래에 결과를 제공하겠다는 "약속" 을 반환함.

2. Promise의 state (상태)

-   Pending (대기) : 비동기 작업의 결과가 이행되지도, 실패하지도 않은 상태
-   Fulfilled (이행) : 비동기 작업이 성공적으로 완료됨.
-   Rejected (거부) : 비동기 작업을 최종적으로 실패하였음.

```javascript
// 1초 뒤에 resolve 함수를 호출하도록 하는 Promise 객체 생성
const testPromise = new Promise((resolve, reject) => {
    setTimeout(() => resolve('Resolved Promise'), 1000);
});

testPromise.then((result) => {
    console.log(result);
}); /// 1초 후 Resolved Promise 출력

// 1초 뒤에 rejected 함수를 호출하도록 하는 Promise 객체 생성
const testPromise2 = new Promise((resolve, reject) => {
    setTimeout(() => reject('Rejected Promise'), 1000);
});

testPromise2.catch((result) => {
    console.log(result);
}); /// 1초 후 Resolved Promise 출력
```

-   Promise의 상태가 fulfilled 일 경우에는 `then` 핸들러를 호출한다.
-   Promise의 상태가 Reject 일 경우에는 반대로 `catch` 핸들러를 호출한다.
-   Promise의 상태와 관련없이 마지막으로 호출되는 `finally` 핸들러를 호출한다.
-   두 메소드의 반환값 또한 새로운 Promise이므로 새로운 작업을 연결시킬 수 있다.
-   then, catch 메소드를 통해 Promise를 연결하는 방식을 Promise Chaining 이라고 부른다.

# ✒️ Promise Function

#### 1. Promise.resolve(), Promise.reject()

-   `Promise.resolve()` 함수는 fulfilled 상태가 되는 Promise를 생성한다.
-   `Promise.reject()` 함수는 reject 상태가 되는 Promise를 생성한다.

```javascript
// 1초 뒤에 resolve 함수를 호출하도록 하는 Promise 객체 생성
const testPromise = new Promise.resolve('immediately Resolved');
const testPromise2 = new Promise.reject('immediately Rejected');

// "Result : immediately Resolved" 출력
testPromise.then(
    (result) => {
        console.log(`Result : ${result}`);
    },
    (error) => {
        console.log(`Error : ${error}`);
    }
);

// "Error : immediately Rejected" 출력
testPromise2.then(
    (result) => {
        console.log(`Result : ${result}`);
    },
    (error) => {
        console.log(`Error : ${error}`);
    }
);
```

-   `then` 메소드는 두 개의 매개변수를 받으며, Promise가 반환되거나 거부되었을 때에 해당되는 핸들러 함수이다.
-   첫번째 매개변수가 resolved 되었을 때, 두 번째 매개변수가 rejected 되었을 때의 핸들러 함수이다.

#### 2. Promise.all(iterable)

-   Iterable한 객체에 주어진 모든 Promise 요소를 받고, 새로운 Promise 를 반환.
-   배열 내부의 Promise의 결과 값을 담은 배열이 새로운 Promise의 result가 됨.
-   비어있는 Iterable 객체를 전달할 경우, Fulfilled 처리된 Promise를 반환함.
-   해당 함수의 경우 배열 내의 모든 Promise 가 이행될 때까지 대기 상태로 남음.

```javascript
// Promise 최종 이행까지 총 6초 소요, 순차적으로 1, 2, 3이 반환됨
// 인자로 받은 배열의 index에 따라서 result 배열의 순서 또한 변경됨.
Promise.all([
    new Promise((resolve) => setTimeout(() => resolve(1), 3000)), // 1
    new Promise((resolve) => setTimeout(() => resolve(2), 2000)), // 2
    new Promise((resolve) => setTimeout(() => resolve(3), 1000)), // 3
]).then((result) => console.log(result)); // [1, 2, 3]
```

-   배열 내부의 Promise 중 하나라도 거부될 경우, 이후의 Promise도 동일한 이유로 거부 처리함.

```javascript
Promise.all([
    new Promise((resolve, reject) => setTimeout(() => resolve(1), 1000)),
    new Promise((resolve, reject) => setTimeout(() => reject(new Error('에러 발생!')), 2000)),
    new Promise((resolve, reject) => setTimeout(() => resolve(3), 3000)),
]).catch(alert); // Error: 에러 발생!
```

#### 3. Promise.allSettled(iterable)

-   Iterable한 객체에 주어진 모든 Promise 요소를 받고, 각 Promise에 대한 결과를 나타내는 배열을 반환함.
-   배열 내부의 Promise 중 거부된 작업이 있더라도, 다른 Promise 작업을 계속 수행함.

```javascript
Promise.allSettled([
    new Promise((resolve, reject) => setTimeout(() => resolve(1), 1000)),
    new Promise((resolve, reject) => setTimeout(() => reject(new Error('에러 발생!')), 2000)),
    new Promise((resolve, reject) => setTimeout(() => resolve(3), 3000)),
]).then((results) => results.forEach((result) => console.log(result.status))); // fulfilled, rejected, fulfilled
```

#### 4. Promise.race(iterable)

-   Iterable한 객체에 주어진 모든 Promise 요소를 받고, 가장 먼저 이행되거나 거부된 결과를 반환한다.
-   비어있는 객체를 전달할 경우, 영원히 Pending 상태에 빠지게 되므로 사용에 주의하자.

```javascript
// 가장 짧은 시간 내에 끝난 첫번째 Promise의 결과가 return 됨.
Promise.race([
    new Promise((resolve, reject) => setTimeout(() => resolve(1), 1000)),
    new Promise((resolve, reject) => setTimeout(() => reject(new Error('에러 발생!')), 2000)),
    new Promise((resolve, reject) => setTimeout(() => resolve(3), 3000)),
]).then((result) => console.log(result)); // 1
```
