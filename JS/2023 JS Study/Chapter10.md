# ✒️ Enhanced Object Literal

#### 1. 변수를 키와 값으로 하는 객체 만들기.

- 특정 변수의 이름과 값을 key, value로 가지는 객체를 만들 수 있다.
- 굳이 프로퍼티에 대한 값을 적지 않고, 속성의 이름만 적어도 괜찮다.

```javascript
const name = "Baik Gwangin";
const major = "Computer Science";
const age = 25;

// ES5 이전에는 아래와 같이 객체 리터럴을 생성하였다.
// 속성의 이름과 일치하는 변수가 존재할 경우, 속성 값을 자동으로 할당시킨다.
const person = {
	name: name,
	major: major,
	age: age,
};

// 속성의 이름과 일치하는 변수가 존재할 경우, 속성 값을 자동으로 할당시킨다.
const person = {
	name,
	major,
	age,
};
```

#### 2. 객체에 함수 추가하기

- ES6 이후부터 객체에 함수를 추가할 경우, `function` 키워드를 생략할 수 있다.
- 화살표 함수도 사용이 가능하나. 이때는 `this` 가 상위 스코프에 바인딩된다.

```javascript
const name = "Baik Gwangin";
const major = "Computer Science";
const age = 25;

// 속성의 이름과 일치하는 변수가 존재할 경우, 속성 값을 자동으로 할당시킨다.
const person = {
	name,
	major,
	age,
	greet() {
		console.log(`Hello, ${this.name}`);
	},
	hello: () => console.log(`Hi!`),
};

person.greet(); // Hello, Baik Gwangin
person.hello(); // Hi!
```

#### 3. 객체의 프로퍼티를 동적으로 정의하기

- ES6부터는 객체를 생성함과 동시에 동적으로 프로퍼티를 정의할 수 있다.

```javascript
const key = "name";

// key의 값은 name이다. 이후 person 객체에서는 동적으로 프로퍼티를 할당한다.
// 이에 따라 person 객체의 name 속성에 "Baik Gwangin" 문자열을 할당시킨다.
const person = {
	[key]: "Baik Gwangin",
};

console.log(person.name); // "Baik Gwangin"
```
