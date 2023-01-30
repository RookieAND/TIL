# ✒️ New Feature in ES2016

#### Array.prototype.includes(value [, index])

- 배열에 인자로 받은 원소가 있다면 `true`를, 없다면 `false`를 반환한다.
- 두번째 인자로 검색을 시작할 인덱스를 지정할 수 있으며, 음의 인덱스도 가능하다.

```javascript
const number = [1, 2, 3, 4, 5];

number.includes(0); // false
number.includes(1); // true

number.includes(1, 3); // false, number의 3번째 인덱스 이후부터는 1이 없다.
number.includes(5, -2); // true, number의 끝에서 2번째 인덱스 이후에 5가 있음.
```

#### Pow Operator

- ES2016 이전에는 지수 계산을 위해 `Math.pow` 메서드를 사용했었다.
- 하지만 `**` 연산자를 통해 지수 연산을 간단하게 수행할 수 있게 되었다.

```javascript
console.log(Math.pow(4, 2)); // 16
console.log(4 ** 2); // 16
```

# ✒️ New Feature in ES2017

### String Padding

#### String.prototype.padStart(targetLength, [, padString])

- 문자열 시작 부분에 패딩을 추가할 수 있다.
- `targetLength` : 패딩을 채웠을 때 문자열의 목표 길이
- `padString`? : 현재 문자열에 채워넣을 다른 문자열

#### String.prototype.padEnd(targetLength, [, padString])

- 문자열 끝 부분에 패딩을 추가할 수 있다.
- `targetLength` : 패딩을 채웠을 때 문자열의 목표 길이
- `padString`? : 현재 문자열에 채워넣을 다른 문자열

```javascript
"Hello".padStart(10, "1"); // "11111Hello"
"Hello".padEnd(10, "1"); // "Hello11111"
```

#### 여러 개의 문자열을 정렬하는 방법

- 문자열을 좌측 정렬하고 싶을 경우 `padEnd()` 함수를 활용하자
- 문자열을 우측 정렬하고 싶은 경우 `padStart()` 함수를 활용하자

```javascript
const strings = ["short", "medium", "long", "very long"];

const longestString = strings.sort((a, b) => b.length - a.length);

// 가장 긴 문자열이 최대 길이이므로, 이를 기준으로 좌측 / 우측 정렬 시행
strings.forEach((str) => console.log(str.padStart(longestString)));
strings.forEach((str) => console.log(str.padEnd(longestString)));
```

### Object Method

- 객체 내부의 값을 쉽게 접근할 수 있도록 두 종류의 메서드가 추가되었다.
- `Object.entries` 는 객체를 이루는 프로퍼티와 프로퍼티 값을 하나의 엔트리로 묶은 배열을 반환한다.
- `Object.values` 는 객체의 프로퍼티 값들을 하나의 배열로 묶어 반환한다.

```javascript
const person = {
	name: "baik gwangin",
	age: 25,
};

for (const [key, value] of Object.entries(person)) {
	console.log(`${key} : ${value}`); // name : baik gwangin, age: 25
}

for (const value of Object.values(person)) {
	console.log(value); // baik gwangin, 25
}
```

- 객체의 모든 속성의 설명자를 가지는 객체를 반환하는 `Object.getOwnPropertyDescriptors()` 메서드가 추가되었다.
- 특정 프로퍼티의 value, writable, get, set, configurable, enumerable 등을 반환한다.

```javascript
const person = {
	name: "baik gwangin",
	age: 25,
};

/**
 * age : {value: 25, writable: true, enumerable: true, configurable: true}
 * name : {value: 'baik gwangin', writable: true, enumerable: true, configurable: true}
 */
Object.getOwnPropertyDescriptors(person);
```

> Symbol 형 프로퍼티는 `Object.entries` 와 `Object.values` 의 대상이 아니다.

- 두 메서드는 심볼형 프로퍼티를 무시한다. 심볼형의 경우 **열거가 불가능한 속성** 을 가지고 있기 때문이다.
- 위와 같은 사유로 객체의 키 값을 순회하는 `for...in` 구문의 경우에도 심볼형 프로퍼티는 반복시키지 않는다.
- 만약 심볼형 프로퍼티가 필요한 경우에는 심볼형 프로퍼티들을 배열로 반환해주는 `Object.getOwnPropertySymbols` 메서드를 사용하자.

### Trailing commas

- 객체나 배열에새로운 요소를 추가하려는 경우, 마지막 요소인지와 관계 없이 뒤에 쉼표를 찍는 것이 허용된다.
- 배열에 후행 쉼표가 2개 이상 사용될 경우 구멍이 생성되며, 구멍이 있는 배열을 `희소 (Sparse)` 라고 한다.
- 함수나 메서드의 경우에도 후행 쉼표를 사용하여 매개변수를 표현할 수 있다.
- 단, `JSON` 의 경우 후행 쉼표를 허용하지 않으니 유의해야 한다.

```javascript
const array = [1, 2, 3, , , ,];
console.log(array.length); // 6

const object = {
	foo: "bar",
	baz: "qwerty",
	age: 42, // 후행 쉼표를 붙여 속성을 추가할때 오류가 없도록 한다.
};
```

### Atomics

- Atomics 객체는 아토믹 연산을 위한 정적 메서드를 제공하며, `SharedArrayBuffer` 객체와 같이 사용된다.
- Atomics를 활용하면 여러 스레드가 공유하는 데이터에서 정확하게 값을 읽고 쓸 수 있게 해준다.
- Atomics를 활용한 작업은 다음 작업이 시작되기 전에 완료되고, 중단되지 않는 것이 보장된다.
- Atomics 은 생성자가 아니며 모든 속성과 메서드가 정적이므로, new 연산자 및 함수 호출이 제한된다.

> SharedArrayBuffer 란?

- `ArrayBuffer` 는 JS가 바이너리 데이터를 다루기 위해 제공하는 객체이다.
- SharedArrayBuffer의 경우에는 서로 다른 쓰레드가 같은 메모리의 영역을 읽고 쓸 수 있도록 만들어진 객체이다.
- 한 마디로, 다수의 쓰레드가 동시에 접근할 수 있는 데이터를 제공하는 객체라고 보면 된다.

```javascript
// main.js
const { Workers } = require("worker_threads");

const sharedArrayBuffer = new SharedArrayBuffer(8);
const array = new Int32Array(sharedArrayBuffer);

const worker = new Worker("./Worker.js");

worker.on("message", (data) => {
	console.info("data from worker", data);
	console.info("main", array);
});

worker.postMessage({ array });

// workers.js
const { parentPort } = require("worker_threads");

parentPort.on("message", (data) => {
	const { array } = data;
	console.info("data from main", array);
	array[0] = 2;
	array[1] = 4;

	console.info("worker", array);
	parentPort.postMessage("ok");
});

// node main.js
/*
  data from main Int32Array(2) [ 0, 0 ]
  data from worker ok
  main Int32Array(2) [ 2, 4 ] (메인 쓰레드 수정됨)
  worker Int32Array(2) [ 2, 4 ] 
*/
```

- 하단의 예시는 Node.js에서 제공하는 worker_threads 모듈을 사용한 예시이다.
- 메인 쓰레드에서 worker를 생성하고, postMessage를 사용해 32 bit 정수를 2개 저장할 수 있는 `SharedArrayBuffer` 생성
- 이후 worker에서는 전달 받은 데이터를 수정하는데, 이것이 메인 쓰레드의 원본 데이터를 변경시킨다는 점에서 차이를 보인다.
- `ArrayBuffer` 의 경우에는 postMessage로 **복사된 데이터** 를 보내지만, `SharedArrayBuffer` 는 그렇지 않기 때문.

#### Atomic Method

- 하지만 멀티 쓰레드 작업을 시행할 경우 **race Condition (경쟁 상태)** 에 대한 문제를 신경써야 한다.
- Atomics 의 정적 메서드의 경우 다른 워커의 작업이 끝나기를 기다리거나 lock을 걸어 동시성 문제를 해결한다.

```javascript
const sharedArrayBuffer = new SharedArrayBuffer(8);
const array = new Int32Array(sharedArrayBuffer);

Atomics.add(array, 0, 5); // array의 0번째 인덱스에 5를 적용
console.info(Atomics.load(array, 0)); // 5
```

1. `Atomics.add(sharedArrayBuffer, index, value)` : 배열의 특정 인덱스에 새로운 값을 더해준다.
2. `Atomics.sub(sharedArrayBuffer, index, value)` : 배열의 특정 인덱스에 새로운 값을 빼준다.
3. `Atomics.load(sharedArrayBuffer, index)` : 배열의 특정 인덱스에 저장된 값을 가져온다.
4. `Atomics.store(sharedArrayBuffer, index, value)` : 배열의 특정 인덱스에 새로운 값을 저장한다.
5. `Atomics.and()`, `Atomics.or()`, `Atomics.xror()` : 배열의 특정 인덱스에 저장된 값에 대한 비트 연산을 수행한다.
