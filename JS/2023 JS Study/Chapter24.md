# ✒️ ES2022

### ✏️ New feature of Class

#### 1. 클래스 속성

- 이제 더 이상 클래스 속성을 `constructor` 안에 기입하지 않고, 클래스 안에 작성해도 무방함.

```javascript
class Person {
	name = "Baik Gwangin";
	age = 26;
}
```

### 2. 다양한 class 속성 지원

- 클래스 속성자 앞에 `#` 을 붙이면 자동으로 private 속성으로 변환된다. (기존에는 `_` 를 붙였음)
- static 속성은 앞에 `static` 키워드를 붙이면 된다. (기존에는 필드를 클래스 자체에 정의했음)
- computed 속성의 경우 `[]` 안에 식을 적으면 되나, 이 경우에는 private 속성을 붙일 수 없다.

```javascript
const first = "ho";
const second = "bby";

class Person {
	name = "Baik Gwangin";
	age = 26;
	static isAlive = true;
	#major = "computer engineering";
	#school = "konkuk glocal univ";
	[a + b] = "cooking"; // hobby: cooking
}

// ES2022 이전에는 아래와 같이 static 필드를 정의
Person.isAlive = true;
```

- `private` 메서드와 `private static` 메서드 기능 또한 추가되었다.
- 이에 따라 getter 와 setter 또한 이를 활용하여 구현이 가능해졌다.

```javascript
class Banner extends HTMLElement {
  #slogan = "Hello there!"
  #counter = 0

  // private getter 및 setter(접근자)
  get #slogan() {return #slogan.toUpperCase()}
  set #slogan(text) {this.#slogan = text.trim()}

  get #counter() {return #counter}
  set #counter(value) {this.#counter = value}

  constructor() {
    super();
    this.onmouseover = this.#mouseover.bind(this);
  }

  // private method
  #mouseover() {
    this.#counter = this.#counter++;
    this.#slogan = `Hello there! You've been here ${this.#counter} times.`
  }
}
```

- public 필드의 경우 클래스에 존재하지 않는 필드에 접근을 시도하면 `undefined` 가 반환된다.
- 하지만 private 필드의 경우 클래스에 존재하지 않는 속성을 접근할 경우 예외 처리가 된다.
- 그러나 사용자가 존재하는 필드에 대해 잘못된 접근자를 쓴 경우에도 예외 처리가 발생한다.
- 이를 방지하기 위해 `in` 키워드를 통해 `private` 속성과 메소드의 존재 여부를 확인한다.

```javascript
class VeryPrivate {
	#variable = "var";
	#method() {}
	get #getter() {}
	set #setter(text) {
		this.#variable = text;
	}

	static isPrivate(obj) {
		return (
			#variable in obj && #method in obj && #getter in obj && #setter in obj
		);
	}
}
```

### ✏️ String.matchAll

- `Regexp.exec` 의 경우 매칭된 결과를 앞에서부터 하나씩 보여주기에 `null` 이 나오기 전까지 여러번 실행을 해야 했다.
- `String.matchAll` 은 모든 매칭 결과를 순회할 수 있도록 iterator 를 반환시킨다.
- 단, 정규식 표현에 반드시 `g` 속성을 부여하여 전역으로 모든 문자열을 변경시켜야 한다.

```javascript
const str = "Ingredients: cocoa powder, cocoa butter, other stuff";
const matches = str.matchAll(/(cocoa) ([a-z]+)/g);

console.log(matches); // ['cocoa powder', 'cocoa', 'powder', index: 13, input: 'Ingredients: cocoa powder, cocoa butter, other stuff', groups: undefined]

while (true) {
	const { value, done } = matches.next();
	if (done) break;
	console.log(value);
}
```

### ✏️ Top Level await

- ES2022 이전까지는 `await` 의 경우 반드시 `async` 함수 내에서만 작성되었어야 했다.
- 이로 인해 코드의 최상위 레벨에서는 불필요하게 `async` 를 붙여서 비동기 처리를 진행해야 했다.
- 하지만 이제 `await` 이 모듈 최상단에서도 정상적으로 작동할 수 있도록 개선되었다.

```javascript
async () => {
    await fetch('http://....');
};

// 개선된 문법
() => {
    await fetch('http://...');
}
```

### ✏️ at

- 배열과 문자열에 `at()` 메서드를 사용하여 Python 에서 지원하던 음의 인덱스 또한 접근이 가능하다.

```javascript
const arr = [1, 2, 3, 4, 5];
const str = "my name is baik gwangin";

console.log(arr.at(-1)); // 5
console.log(str.at(-2)); // i
```

### ✏️ Detail Error Case

- 이제 에러 객체에 구체적인 원인을 적을 수 있는 cause 속성이 추가되었다.

```javascript
const err = new Error("test Error", { cause: "just throw error" });
console.log(err.cause); // 'just throw error'
```
