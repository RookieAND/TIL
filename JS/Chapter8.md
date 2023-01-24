# ✒️ Array Method in ES6

#### Array.from(arrayLike[, mapFn[, thisArg]])

유사 배열 객체 (Array-like Object) 나 Iterable한 객체를 얕게 복사함.
이후 이를 기반으로 새로운 Array 객체를 생성하여 반환한다.

-   `arrayLike` 는 배열로 반환하고자 하는 유사 배열 객체 혹은 Iterable 객체
-   `mapFn` 은 새롭게 배열을 생성하기 전, 각 요소를 대상으로 진행할 mapping 함수이다.
-   `thisArg` 는 mapFn 실행 시에 각 요소 별로 this를 지정할 수 있도록 해준다.

```javascript
console.log(Array.from('test')); // ["t", "e", "s", "t"]
console.log(Array.from([1, 2, 3], (x) => x * 2)); // [2, 4, 6]
console.log(Array.from({ length: 2 }, () => Array(2).fill(0))); // [[0, 0], [0, 0]]
```

> 유사 배열 객체는 무엇인가?

-   `length` 프로퍼티가 있으며 인덱스가 존재하는 객체가 바로 **유사 배열 객체** 이다.
-   하단의 경우 객체 내부에 인덱스가 존재하며, `length` 프로퍼티가 있으므로 유사 배열 객체다.

```javascript
console.log(Array.from({ 0: 'first', 1: 'second' length: 3 })); // ['first', 'second', undefined]
console.log(Array.from({ length: 5 }, () => 0)); // [0, 0, 0, 0, 0]
```

-   그렇다고 해서 유사 배열 객체가 Iterable 하다는 보장은 없다. Iterable 객체 또한 유사 배열 객체가 아닐 수 있다.
-   Iterable한 객체와 유사 배열 객체를 배열처럼 다루고 싶다면, `Array.from` 메서드를 통해 실제 배열로 변환시키자.

#### Array.of(element0[, element1[, ...[, elementN]]])

인자로 전달받은 값들을 기반으로 새로운 배열 인스턴스를 생성합니다.

-   Array 생성자와의 차이는 단일 정수형 인자의 처리 방법에 있음.
-   `Array.of(7)` 의 경우 하나의 요소 `7` 을 가진 배열을 생성함.
-   `Array(7)` 의 경우 길이가 7이지만 값이 비어있는 배열을 생성함.

```javascript
Array.of(7); // [7]
Array.of(1, 2, 3); // [1, 2, 3]

Array(7); // [ , , , , , , ]
Array(1, 2, 3); // [1, 2, 3]
```

#### Array.prototype.find(callback [, thisArg])

인자로 받은 콜백 함수를 만족하는 첫 번째 요소의 값을 반환한다.
만약 콜백 함수를 만족시키는 결과가 없다면 `undefined`를 반환한다.

-   `callback` 배열의 각 요소에 대해서 실행할 콜백 함수. 해당 함수는 3개의 인자를 받습니다.
    -   `element` : 함수에서 처리할 현재 요소
    -   `index` 함수에서 처리할 현재 요소의 인덱스
    -   `array` : find 함수를 호출한 배열

```javascript
const number = [1, 2, 3, 4, 5, 6, 7, 8];

const firstEven = number.find((elm, index, array) => elm % 2 === 0 && array[index] % 2 === 0);
console.log(firstEven); // 2
```

#### Array.prototype.findIndex(callback [, thisArg])

인자로 받은 콜백 함수를 만족하는 첫 번째 요소의 **인덱스**을 반환한다.
만약 콜백 함수를 만족시키는 결과가 없다면 `-1`를 반환한다.

-   `callback` 배열의 각 요소에 대해서 실행할 콜백 함수. 해당 함수는 3개의 인자를 받습니다.
    -   `element` : 함수에서 처리할 현재 요소
    -   `index` 함수에서 처리할 현재 요소의 인덱스
    -   `array` : find 함수를 호출한 배열

```javascript
const number = [1, 2, 3, 4, 5, 6, 7, 8];

const firstEven = number.findIndex((elm, index, array) => elm % 2 === 0 && array[index] % 2 === 0);
console.log(firstEven); // 1
```

#### Array.prototype.copyWithin(target[, start[, end]])

배열의 일부를 복사하여 동일한 배열의 다른 위치에 덮어쓰고 반환한다. 배열의 길이는 변경되지 않으며 원본 배열에 영향을 준다.

한 마디로 정리하자면 자기 자신의 일부를 복사하고, 이를 내부에 붙여넣는 메소드다. 대신 배열의 전체 길이는 변하지 않는 선에서 말이다.

-   `target` : 복사한 값을 넣을 시작 위치 인덱스
-   `start`? : 복사한 요소들을 붙일 인덱스 (default = 0)
-   `end`? : 배열의 복사를 종료시킬 인덱스 (default = array.length)

```javascript
// number[1] 에 number.slice(3) 을 붙인다.
// 따라서 결과는 [1, 4, 5, 6, 7, 8, 7, 8]
let number = [1, 2, 3, 4, 5, 6, 7, 8];
console.log(number.copyWithin(1, 3));

// number[2] 에 number.slice(5, 7) 을 붙인다.
// 따라서 결과는 [1, 2, 6, 7, 5, 6, 7, 8]
number = [1, 2, 3, 4, 5, 6, 7, 8];
console.log(number.copyWithin(2, 5, 7));

// number[4] 에 number 배열 전체를 붙인다.
// 따라서 결과는 [1, 2, 3, 4, 1, 2, 3, 4]
number = [1, 2, 3, 4, 5, 6, 7, 8];
console.log(number.copyWithin(4));
```

#### Array.prototype.fill(target[, start[, end]])

배열의 요소를 특정한 값으로 변경시킨다. 변경의 시작과 끝 인덱스를 설정할 수 있다.

-   `target` : 새롭게 수정될 배열의 요소 값
-   `start`? : 변경을 시작할 인덱스
-   `end`? : 변경을 종료할 인덱스

```javascript
// number의 2번째 부터 4번째 인덱스까지를 0으로 수정한다.
// 따라서 결과는 [1, 2, 0, 0, 5, 6, 7, 8]
let number = [1, 2, 3, 4, 5, 6, 7, 8];
console.log(number.fill(0, 2, 4));

// number의 4번째 인덱스부부터 끝까지 0으로 수정한다.
// 따라서 결과는 [1, 2, 3, 4, 0, 0, 0, 0]
number = [1, 2, 3, 4, 5, 6, 7, 8];
console.log(number.fill(0, 4));

// number의 모든 요소를 0으로 수정한다.
// 따라서 결과는 [0, 0, 0, 0, 0, 0, 0, 0]
number = [1, 2, 3, 4, 5, 6, 7, 8];
console.log(number.fill(0));
```

#### Array.prototype.some(callback [, thisArg])

배열 안에 있는 요소 중 어느 하나라도 인자로 받은 콜백 함수를 통과하는지 테스트한다.
만약 테스트에 통과한 요소가 한 개라도 있다면 `true` 를, 그렇지 않다면 `false` 를 반환한다.

-   `callback` 배열의 각 요소에 대해서 실행할 콜백 함수. 해당 함수는 3개의 인자를 받습니다.
    -   `element` : 함수에서 처리할 현재 요소
    -   `index` 함수에서 처리할 현재 요소의 인덱스
    -   `array` : find 함수를 호출한 배열

```javascript
const number = [1, 2, 3, 4, 5, 6, 7, 11];

const isBiggerThan10 = number.some((elm, index, array) => array[index] > 10);
console.log(isBiggerThan10); // true, 11은 10보다 크기 때문.
```

#### Array.prototype.every(callback [, thisArg])

배열 안에 있는 요소 전부가 인자로 받은 콜백 함수를 통과하는지 테스트한다.
만약 테스트에 통과하지 못한 요소가 한 개라도 있다면 `false` 를, 그렇지 않다면 `true` 를 반환한다.

-   `callback` 배열의 각 요소에 대해서 실행할 콜백 함수. 해당 함수는 3개의 인자를 받습니다.
    -   `element` : 함수에서 처리할 현재 요소
    -   `index` 함수에서 처리할 현재 요소의 인덱스
    -   `array` : find 함수를 호출한 배열

```javascript
const number = [1, 2, 3, 4, 5, 6, 7, 8];

const isSmallerThan10 = number.every((elm, index, array) => array[index] < 10);
console.log(isSmallerThan10); // true, 배열 안의 모든 요소가 10보다 작기 때문.
```
