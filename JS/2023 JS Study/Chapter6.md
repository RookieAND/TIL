# ✒️ Destructuring

#### 1. 비구조화 할당이란 무엇인가?

- 배열의 값, 혹은 객체의 프로퍼티를 풀어서 개별적인 변수에 할당시키는 것이다.
- 주로 배열 내부의 값, 혹은 객체의 일부 속성에 대한 값을 추출할 때 용이하다.
- **iterable** 한 객체의 경우 모두 비구조화 할당을 활용할 수 있다.

#### 2. 객체 비구조화 할당

- 객체의 경우 필요한 속성 값을 비구조화 할당을 통해서 추출할 수 있다.

```javascript
const person = {
	firstName: "Baik",
	lastName: "Gwangin",
};

// ES5 이전 버전에서는 속성 접근자 (.) 를 통해 객체의 속성값을 할당시켰다.
const oldFirstName = person.firstName;
const oldLastName = person.lastName;

// ES6 이후부터는 비구조화 할당 기법을 활용하여 할당이 가능하다.
const { firstName, lastName } = person;
```

- 비구조화 할당 시, 객체의 속성 값을 받을 변수의 이름을 변경할 수도 있다.
- 또한 변수에 값이 할당되지 않았을 경우를 고려하여 기본 값을 설정할 수 있다.

```javascript
const person = {
	firstName: "Baik",
	lastName: "Gwangin",
};

// 콜론 (:) 이후 새로운 이름을 기입함으로서 변수명을 명명할 수 있음.
// 이후 등호 (=) 를 사용하여 기본 값을 설정할 수도 있음.
const { firstName: fn = "Kim", lastName: ln = "Cheolsu" } = person;
```

- 중첩 객체의 경우에도 비구조화 할당을 통해 특정 값만을 추출하여 저장할 수 있다.

```javascript
const person = {
	name: {
		firstName: "Baik",
		lastName: "Gwangin",
	},
	hobby: "listening music",
	major: "computer science",
};

// ES5 이전 버전에서 중첩 객체의 특정 속성 값을 할당하는 방식.
const oldFirstName = person.name.firstName;

// ES6 이후부터는 비구조화 할당을 통해 중첩 객체의 속성을 추출할 수 있다.
const {
	name: { firstName },
} = person;

console.log(oldFirstName); // "Baik"
console.log(firstName); // "Baik"
```

#### 3. 배열 비구조화 할당

- 배열을 비구조화 할당할 경우, 객체와는 다르게 **인덱스** 를 기준으로 할당시킨다.

```javascript
const fruits = ["apple", "banana", "lemon"];

// 배열의 인덱스를 기준으로 배열로부터 요소를 추출하여 변수에 할당시킨다.
// 변수 apple, banana, lemon가 선언된 후, 우측의 배열이 비구조화 되어 할당된다.
const [apple, banana, lemon] = fruits;

// 비구조화 할당을 진행할 경우, 반드시 초기화할 값을 지정해주어야 한다.
const [apple, banana, lemon]; // SyntaxError: Missing initializer in destructuring declaration
```

- 문자열의 경우에도 배열 비구조화 할당에서 쓰이는 문법을 그대로 활용할 수 있다.

```javascript
const sentence = "test";

// 변수 t, e에는 각각 문자 "t" 와 "e" 가 할당된다.
const [t, e] = sentence;
```

- 생성하려는 변수의 수가 배열의 요소보다 적을 경우, 변수의 갯수만큼의 할당이 진행된다.
- 즉 비구조화 할당을 받을 변수가 2개이고 초기화의 대상인 배열의 길이가 3이라면, 1개의 값은 누락된다.

```javascript
const fruits = ["apple", "banana", "lemon"];

// apple 변수와 banana 변수에는 각각 "apple", "banana" 가 할당된다. "lemon" 문자열은 누락된다.
const [apple, banana] = fruits;
```

- 의도적으로 필요하지 않은 반환 값을 무시할 수 있습니다.

```javascript
const fruits = ["apple", "banana", "lemon"];

// lemon 변수에는 "lemon" 이 할당되고, 이전의 값들은 모두 누락된다.
const [, , lemon] = fruits;
```

- 만약 나머지 값을 모두 얻고 싶다면 나머지 (Rest) 연산자를 활용하자.
- 나머지 연산자의 경우 남은 배열의 요소를 새롭게 묶어 배열로 반환시킨다.
- 단, 나머지 연산자 이후에 추가적인 값을 받는 쉼표 (,) 가 존재해서는 안된다.

```javascript
const fruits = ["apple", "banana", "lemon"];

// apple 변수에는 "apple" 가 할당된다. 나머지 요소의 경우 leftFruits 변수에 배열로 할당된다.
const [apple, ...left] = fruits;

console.log(apple); // apple
console.log(left); // ["banana", "lemon"]

// rest 연산자 이후 또 다른 변수를 할당시킬 수 없다.
const [apple, ...left, lemon] = fruits; // SyntaxError: rest element may not have a trailing comma
```

- 중첩된 배열의 경우에도 동일하게 비구조화 할당을 시행할 수 있다.

```javascript
const games = [["mario", "mario brothers", "mario karts"], "sonic", "zelda"];

const [[mario], sonic] = games;
console.log(mario); // mario
console.log(sonic); // sonic
```
