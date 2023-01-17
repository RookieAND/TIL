# ✒️ Array Method in JS

#### 1. Array.from(arrayLike[, mapFn[, thisArg]])

유사 배열 객체 (Array-like Object) 나 Iterable한 객체를 얕게 복사함.
이후 이를 기반으로 새로운 Array 객체를 생성하여 반환한다.

- `arrayLike` 는 배열로 반환하고자 하는 유사 배열 객체 혹은 Iterable 객체
- `mapFn` 은 배열의 모든 요소에 대해 호출할 mapping 함수이다.
- `thisArg` 는 mapFn 실행 시에 this로 사용할 값을 전달할 수 있다.

```javascript
console.log(Array.from("test")); // ["t", "e", "s", "t"]
console.log(Array.from([1, 2, 3], (x) => x * 2)); // [2, 4, 6]
console.log(Array.from({ length: 2 }, () => Array(2).fill(0))); // [[0, 0], [0, 0]]
```

> length 속성이 담긴 객체는 유사 배열 객체인가?

- 그렇다, JS 에서 배열은 `Object` 이기 때문이다. 배열은 기본적으로 `length` 라는 속성을 가진 Object이기 때문.
- `Array.from` 메소드의 첫번째 인자로 `length` 속성이 포함된 객체를 넣으면, JS는 이를 유사 배열 객체로 인식한다.

```javascript
typeof []; // object
console.log(Array.from({ length: 20 }, () => Array(10).fill(0))); // 0 으로 초기화된 20 x 10 짜리 2차원 배열
```

- 그렇다고 해서 length 속성을 추가한 객체와 Array가 동일하다는 뜻은 아니다.
- 배열의 prototype인 Array에는 배열로서 동작하기 위한 메소드와 iterator 관련 속성이 추가로 담겼기 때문이다.
- 따라서 Object에 배열 관련 메소드와 `Symbol.iterator`, `length` 속성을 추가한 것이 바로 Array 이다.

#### 2. Array.of(element0[, element1[, ...[, elementN]]])

인자로 전달받은 값들을 기반으로 새로운 배열 인스턴스를 생성합니다.

- Array 생성자와의 차이는 단일 정수형 인자의 처리 방법에 있음.
- `Array.of(7)` 의 경우 하나의 요소 `7` 을 가진 배열을 생성함.
- `Array(7)` 의 경우 길이가 7이지만 값이 비어있는 배열을 생성함.

```javascript
Array.of(7); // [7]
Array.of(1, 2, 3); // [1, 2, 3]

Array(7); // [ , , , , , , ]
Array(1, 2, 3); // [1, 2, 3]
```

#### 3. Array.prototype.find(callback [, thisArg])

인자로 받은 콜백 함수를 만족하는 첫 번째 요소의 값을 반환한다.
만약 콜백 함수를 만족시키는 결과가 없다면 `undefined`를 반환한다.

- `callback` 배열의 각 요소에 대해서 실행할 콜백 함수. 해당 함수는 3개의 인자를 받습니다.
  - `element` : 함수에서 처리할 현재 요소
  - `index` 함수에서 처리할 현재 요소의 인덱스
  - `array` : find 함수를 호출한 배열

```javascript
const number = [1, 2, 3, 4, 5, 6, 7, 8];

const firstEven = number.find(
	(elm, index, array) => elm % 2 === 0 && array[index] % 2 === 0
);
console.log(firstEven); // 2
```

#### 3. Array.prototype.findIndex(callback [, thisArg])

인자로 받은 콜백 함수를 만족하는 첫 번째 요소의 **인덱스**을 반환한다.
만약 콜백 함수를 만족시키는 결과가 없다면 `-1`를 반환한다.

- `callback` 배열의 각 요소에 대해서 실행할 콜백 함수. 해당 함수는 3개의 인자를 받습니다.
  - `element` : 함수에서 처리할 현재 요소
  - `index` 함수에서 처리할 현재 요소의 인덱스
  - `array` : find 함수를 호출한 배열

```javascript
const number = [1, 2, 3, 4, 5, 6, 7, 8];

const firstEven = number.findIndex(
	(elm, index, array) => elm % 2 === 0 && array[index] % 2 === 0
);
console.log(firstEven); // 1
```

#### 3. Array.prototype.some(callback [, thisArg])

배열 안에 있는 요소 중 어느 하나라도 인자로 받은 콜백 함수를 통과하는지 테스트한다.
만약 테스트에 통과한 요소가 한 개라도 있다면 `true` 를, 그렇지 않다면 `false` 를 반환한다.

- `callback` 배열의 각 요소에 대해서 실행할 콜백 함수. 해당 함수는 3개의 인자를 받습니다.
  - `element` : 함수에서 처리할 현재 요소
  - `index` 함수에서 처리할 현재 요소의 인덱스
  - `array` : find 함수를 호출한 배열

```javascript
const number = [1, 2, 3, 4, 5, 6, 7, 11];

const isBiggerThan10 = number.some((elm, index, array) => array[index] > 10);
console.log(isBiggerThan10); // true, 11은 10보다 크기 때문.
```

#### 3. Array.prototype.every(callback [, thisArg])

배열 안에 있는 요소 전부가 인자로 받은 콜백 함수를 통과하는지 테스트한다.
만약 테스트에 통과하지 못한 요소가 한 개라도 있다면 `false` 를, 그렇지 않다면 `true` 를 반환한다.

- `callback` 배열의 각 요소에 대해서 실행할 콜백 함수. 해당 함수는 3개의 인자를 받습니다.
  - `element` : 함수에서 처리할 현재 요소
  - `index` 함수에서 처리할 현재 요소의 인덱스
  - `array` : find 함수를 호출한 배열

```javascript
const number = [1, 2, 3, 4, 5, 6, 7, 8];

const isSmallerThan10 = number.every((elm, index, array) => array[index] < 10);
console.log(isSmallerThan10); // true, 배열 안의 모든 요소가 10보다 작기 때문.
```
