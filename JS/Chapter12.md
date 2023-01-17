# ✒️ Class

#### 1. JS 에서 Class 란

- JS는 기본적으로 prototype 기반의 상속 방식을 채택해왔다.
- Class 문법은 이를 기반으로 한 객체 지향 언어 관련 기능을 지원한다.

```javascript
// ES5 이전에서는 생성자 함수, prototype, closer를 사용하여 OOP를 구현했음.
var Person = (function () {
	function Person(name) {
		this._name = name;
	}

	// Person 객체에 sayHi 메소드 추가
	Person.prototype.sayHi = function () {
		console.log("Hi! " + this._name);
	};

	// 생성자 함수를 return 함.
	return Person;
})();

var me = new Person("Baik");
me.sayHi(); // Hi! Baik
```

#### 2. Class 생성 방식

- JS에서는 클래스 선언과 클래스 표현식을 활용하여 클래스를 생성할 수 있다.
- 단, 클래스 선언 및 표현식의 경우 호이스팅되지 않는다. 이 점을 주의하자.

```javascript
// 클래스 선언
class Person {
	constructor(name) {
		this.name = name;
	}

	greet() {
		console.log(`Hi, my name is ${name}`);
	}
}

const baik = new Person("Baik Gwangin");
baik.greet(); // Hi, my name is Baik Gwangin
```

- 프로토타입 방식과의 차이점은 `constructor` 메소드, 즉 생성자 메소드가 추가되었다는 점이다.
- 또한 생성자 메소드의 경우 딱 한번만 선언되어야 한다, 그렇지 않으면 `SyntaxError` 를 일으킨다.

#### 3. static 메소드

- 클래스의 인스턴스가 아닌 클래스 자체에서 접근 가능한 메소드가 정적 메소드이다.
- Java 와 같은 여타 다른 언어들처럼, `static` 키워드를 사용하여 생성이 가능하다.

```javascript
// 클래스 선언
class Person {
	constructor(name) {
		this.name = name;
	}

	greet() {
		console.log(`Hi, my name is ${name}`);
	}

	static info() {
		console.log("Person Class Information");
	}
}

Person.info(); // "Person Class Information"

// info는 정적 메소드이기 때문에 인스턴스에서 접근할 수 없다.
const baik = new Person("Baik Gwangin");
baik.info(); // TypeError : baik.info() is not a function
```

#### 4. set, get

- JS에서도 setter, getter 메소드를 활용하여 클래스 내의 값을 가져올 수 있다.
- 메소드 앞에 set, get 키워드를 붙이고 메소드 명을 클래스 내부의 값으로 지정.

```javascript
// 클래스 선언
class Person {
	constructor(name) {
		this.name = name;
	}

	// name 속성에 대한 setter 함수
	set name(newName) {
		this.name = newName;
		console.log(this.name);
	}

	// name 속성에 대한 getter 함수
	get name() {
		console.log(this.name);
	}
}

const baik = new Person("Baik Gwangin");
baik.name; // getter 호출 ("Baik Gwangin")
baik.name = "Baik GwangHyeon"; // setter 호출 ("Baik GwangHyeon")
```

#### 5. 클래스 상속

- 기존 클래스를 상속하여 새로운 클래스를 생성할 경우에는 `extends` 키워드를 사용.
- this 를 쓰기 전에 `super()` 메소드를 통해 상속받은 클래스 인스턴스를 먼저 생성해야 함.

```javascript
class Person {
	constructor(name) {
		this.name = name;
	}

	greet() {
		console.log(`Hi, my name is ${name}`);
	}
}

class Adult extends Person {
	// Person 클래스의 인스턴스를 super 메소드로 먼저 생성.
	constructor(name, age) {
		super(name), (this.age = age);
	}
}

// Adult 클래스 인스턴스는 Person의 속성 및 메소드를 상속받았음.
const baik = new Adult("Baik Gwangin", 25);

baik.greet(); // Hi, my name is Baik Gwangin
console.log(baik.age); // 25
```
