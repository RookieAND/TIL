# ✒️ ES2021

### ✏️ New Method of Promise

#### 1. Promise.any(iterable)

- Iterable한 객체에 주어진 모든 Promise 요소를 받고, 첫번째로 fulfilled 상태가 된 Promise 를 return 한다.
- 배열 내부의 Promise 들이 모두 rejected 되었거나 빈 iterable 객체가 인자로 들어갔다면 `AggregateError` 가 발생된다.

```javascript
Promise.any([
	new Promise((resolve, reject) => setTimeout(() => resolve(1), 1000)),
	new Promise((resolve, reject) =>
		setTimeout(() => reject(new Error("에러 발생!")), 2000)
	),
	new Promise((resolve, reject) => setTimeout(() => resolve(3), 3000)),
]).then((result) => console.log(result)); // 1, 가장 빠르게 fulfilled 된 Promise의 결과 반환

// 배열 내의 Promise 가 전부 rejected 되었을 때 에러 발생.
Promise.any([
	new Promise((resolve, reject) => setTimeout(() => reject(1), 1000)),
	new Promise((resolve, reject) => setTimeout(() => reject(2), 2000)),
	new Promise((resolve, reject) => setTimeout(() => reject(3), 3000)),
]).catch((result) => console.log(result)); // AggregateError: All promises were rejected

// 인자로 전달 받은 iterable 객체가 비어있을 경우에도 에러 발생.
Promise.any([]).catch((result) => console.log(result)); //AggregateError: All promises were rejected
```

> Promise.race() 와 다른 점

```javascript
// race() 의 경우에는 성공 실패 여부와 관련 없이 가장 먼저 종료된 Promise 만을 체크한다.
Promise.race([
	new Promise((resolve, reject) => setTimeout(() => reject(1), 1000)),
	new Promise((resolve, reject) => setTimeout(() => reject(2), 2000)),
	new Promise((resolve, reject) => setTimeout(() => reject(3), 3000)),
]).catch((result) => console.log(result)); // 1

// any() 의 경우에는 먼저 성공한 Promise 만을 바라보며, 모두 실패할 경우 AggregateError 를 발생시킨다.
Promise.any([
	new Promise((resolve, reject) => setTimeout(() => reject(1), 1000)),
	new Promise((resolve, reject) => setTimeout(() => reject(2), 2000)),
	new Promise((resolve, reject) => setTimeout(() => reject(3), 3000)),
]).catch((result) => console.log(result)); // AggregateError: All promises were rejected
```

- `Promise.race()` 의 경우에는 Promise의 상태와 관련 없이 가장 먼저 작업을 끝마친 Promise 결과 값을 반환한다.
- 하지만 `Promise.any()` 의 경우에는 반드시 fulfilled 상태인 Promise만 체크하며, rejected 된 경우는 체크하지 않는다.

### ✏️ New Method of String

#### 1. String.prototype.replaceAll

- `replaceAll()` 메서드는 문자열 내의 특정 부분을 일괄적으로 변경시킬 수 있습니다.
- 기존에는 정규 표현식의 `g` 옵션을 통해 전역으로 모든 문자열을 변경시켜야 했습니다.

```javascript
// 문자열 내의 1을 전부 2로 변환해보자.
const str = "12121212121212121212";

// ES2021 이전에는 아래와 같은 방식으로 변환을 진행하였다.
console.log(str.replace(/1/g, "2")); // 22222222222222222222
console.log(str.split("1").join("2")); // 22222222222222222222

// ES2021 이후에는 replaceAll 메서드 하나만으로도 변경이 가능하다.
console.log(str.replaceAll("1", "2")); // 22222222222222222222
```

> replace() 와 replaceAll() 의 차이?

- `replace()` 메서드의 경우 첫번째 인자가 일반 문자열이라면, 가장 첫번째로 탐색한 부분 문자열만 변경하지만 `replaceAll()` 은 동일한 부분 문자열을 일괄적으로 변경한다.
- 정규 표현식이 첫번째 인자로 들어간 경우에도, `g` 전역 플래그가 없다면 `replace` 는 처음으로 탐색한 부분 문자열만을 수정하나, `replaceAll` 은 변경 가능한 모든 부분 문자열을 일괄 수정한다.

### ✏️ Numeric separators

- `_` 를 사용하여 숫자를 단위 별로 구분지어 시각적으로 더 읽기 쉽게 만들어준다.

```javascript
10000000000; // 100억
10_000_000_000; // 100억

console.log(10_000_000_000); // 10000000000
```

### ✏️ Logical Assignment

- 기존의 `&&`, `||` 연산자를 활용한 할당 코드를 더욱 축약시킬 수 있는 연산자 `&&=`, `||=` 가 추가되었다.
- ES2020 에서 추가된 Nullish Operator `(??)` 또한 `??=` 로 축약하여 값을 할당시킬 수 있다.

```javascript
let isFalsy = false;
let isNullish = null;

// ES2021 이전의 문법을 활용한 코드
isFalsy = false || "Falsy";
isNullish = undefined ?? "Nullish";

// ES2021 에서 추가된 Logical Assignment 을 활용한 코드
isFalsy ||= "Falsy"; // isFalsy가 Falsy 한 값이라면 "Falsy" 문자열 대입
isNullish ??= "Nullish"; // isNullish 가 Nullish 한 값이라면 "Nullish" 문자열 대입
```

### ✏️ WeakReferences

내일 추가 조사
