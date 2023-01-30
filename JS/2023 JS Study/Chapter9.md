# ✒️ Spread Operator

#### 1. 전개 구문이란?

- Iterable 한 객체를 0개 이상의 인수, 혹은 요소로 확장시키는 문법
- 문자열, 배열, 객체 등을 Spread Operator (...) 로 전개시킬 수 있다.

```javascript
const meat = ["pork", "beef", "chicken"];
const veggie = ["tomato", "tomato", "beans"];

// veggie 와 meat 배열 내의 요소를 풀어 menu 배열에 추가하였다.
const menu = [...veggie, ...meat];
console.log(menu); // ["tomato", "tomato", "beans", "pork", "beef", "chicken"]
```

#### 2. 배열의 스프레드 연산자

- Spread Operator (...) 를 사용하여 만들어진 배열의 경우, 새로운 배열을 반환한다.

```javascript
const origin = [1, 2, 3, 4, 5];
const swallowCopied = origin;

// ES5 이전에는 빈 배열을 만들고, concat 함수를 통해 깊은 복사 구현
const oldDeepCopied = [].concat(origin);

// ES6 이후부터는 Spread Operator를 사용하여 간단하게 깊은 복사 구현
const deepCopied = [...origin];

origin.push(6);
console.log(swallowCopied); // [1, 2, 3, 4, 5, 6]
console.log(deepCopied); // [1, 2, 3, 4, 5]
```

#### 3. 함수의 스프레드 연산자

- 배열의 각 요소를 함수의 인수를 사용할 때는 `apply()` 함수를 사용했음.
- 하지만 ES6 이후부터는 전개 구문을 활용하여 배열의 요소를 인자로 전달할 수 있음.

```javascript
function sum(a, b, c) {
	console.log(a + b + c);
}

const arr = [1, 2, 3];

sum.apply(null, arr); // 6
sum(...arr); // 6
```

> apply, call 함수의 차이점이란?

- `Function.prototype.apply()` 함수의 경우 인자를 배열로 받아 함수를 호출.
- `Function.prototype.call()` 함수의 경우 여러 개의 인자를 받아 함수를 호출.

```javascript
const max = Math.max.apply(null, [5, 6, 2, 3, 7]);
const min = Math.min.call(null, 5, 6, 2, 3, 7);

console.log(max, min); // 7, 2
```

#### 4. 객체 리터럴과 스프레드

- 객체의 경우에도 내부의 속성 값을 스프레드 연산자를 통해 확장시킬 수 있음.

```javascript
const name = {
	firstName: "Baik",
	lastName: "Gwangin",
};

const person = { age: 25, ...name };
console.log(person); // {age: 25, firstName: 'Baik', lastName: 'Gwangin'}
```

#### 5. 나머지 (Rest) 연산자

- Rest Operator는 Spread Operator와 다르게 여러 원소를 하나로 압축시킨다.
- Rest Operator를 사용했을 경우, 우측에 더 이상의 콤마 (,) 가 와서는 안된다.

```javascript
const fruits = ["apple", "banana", "lemon"];

// apple 변수에는 "apple" 가 할당된다. 나머지 요소의 경우 leftFruits 변수에 배열로 할당된다.
const [apple, ...left] = fruits;

console.log(apple); // apple
console.log(left); // ["banana", "lemon"]

// rest 연산자 이후 또 다른 변수를 할당시킬 수 없다.
const [apple, ...left, lemon] = fruits; // SyntaxError: rest element may not have a trailing comma
```

- 함수의 마지막 인자에 Rest Operator를 사용할 경우, 후속 인자를 배열에 넣는다.
- 또한 함수를 정의할 때는 오직 하나의 Rest Operator 만을 넣을 수 있다.

```javascript
function callbackFunc(a, b, ...rest) {
	console.log(a, b, rest);
}

callbackFunc(1, 2, 3, 4, 5); // 1, 2, [3, 4, 5]
callbackFunc(1, 2, 3); // 1, 2, [3]
callbackFunc(1, 2); // 1, 2, [] (빈 배열 할당)
```
