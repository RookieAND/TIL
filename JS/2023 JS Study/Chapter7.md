# ✒️ Loop

#### 1. loop in Array

-   일반적으로 배열을 순회하는 방법은 아래의 방법들이 있다.
-   for 문, `Array.prototype.forEach` 함수을 활용하면 배열의 요소를 순회할 수 있다.

```javascript
const number = [1, 2, 3, 4, 5]

// for 문을 사용한 정석적인 반복문
for (let i = 0; i < number.length; i++>) {
	console.log(number[i])
}

// 배열의 forEach 함수를 활용한 반복
number.forEach((elm) => console.log(elm))
```

#### 2. for - of 문

-   ES6에서 추가된 for - of 문은 반복 가능한 객체 (Iterable) 를 순회하게 해준다.
-   즉, `[Symbol.iterator]` 속성을 가지는 컬렉션들을 대상으로 만들어진 반복문이다.

```javascript
const number = [1, 2, 3, 4, 5];

// for - of 문을 사용해 요소를 순회하는 방식
for (const num of number) {
    console.log(num);
}
```

-   배열 뿐만이 아니라 Map, Set, String 같은 Iterable 객체들은 모두 사용이 가능하다.

```javascript
// 문자열의 각 문자를 순회하여 순서대로 출력시킨다.
const numberString = '12345';

for (const num of numberString) {
    console.log(num);
}

// Set 자료구조 또한 for - of 문을 통해 반복이 가능하다.
const numberSet = new Set([1, 2, 3]);

for (const num of numberSet) {
    console.log(num);
}

// Map 자료구조 또한 for - of 문을 통해 반복이 가능하다.
const recipeMap = new Map([
    ['cucumber', 500],
    ['tomatoes', 350],
    ['onion', 50],
]);

for (let [key, value] of recipeMap) {
    console.log(key, value);
}
```

#### 3. for - in 문

-   객체의 열거 가능한 속성들을 순회하도록 해주는 반복문이다.
-   객체의 key 값에만 접근이 가능하며, value에는 접근이 불가능하다.
-   임의의 순서대로 객체의 각 속성들에 대해 반복하기에 일관된 순서를 보장하지 않음.

```javascript
const obj = { a: 1, b: 2, c: 3 };

for (const prop in obj) {
    console.log(`${prop}`); // a, b, c
}
```

-   주의할 점은, 해당 객체의 속성 뿐만이 아니라 상속 받은 prototype의 속성까지 열거한다는 점이다.
-   만약 객체의 속성만 열거하고 싶다면, `Object.prototype.hasOwnProperty` 메서드를 통해 확인해야 한다.

> 열거할 수 있는 속성이란 대체 무엇인가?

-   내부적으로 `[[Enumerable]]` 프로퍼티가 `true` 로 설정된 프로퍼티를 의미함.
-   이러한 속성의 경우 for - in 구문을 통해 접근하여 순회할 수 있음.
