# ✒️ Loop

#### 1. loop in Array

- 일반적으로 배열을 순회하는 방법은 아래의 방법들이 있다.
- for 문, `Array.prototype.forEach` 함수을 활용하면 배열의 요소를 순회할 수 있다.

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

- ES6에서 추가된 for - of 문은 반복 가능한 객체 (Iterable) 를 순회하게 해준다.
- 즉, `[Symbol.iterator]` 속성을 가지는 컬렉션들을 대상으로 만들어진 반복문이다.

```javascript
const number = [1, 2, 3, 4, 5];

// for - of 문을 사용해 요소를 순회하는 방식
for (const num of number) {
	console.log(num);
}
```

- 배열 뿐만이 아니라 Map, Set, String 같은 Iterable 객체들은 모두 사용이 가능하다.

```javascript
// 문자열의 각 문자를 순회하여 순서대로 출력시킨다.
const numberString = "12345";

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
	["cucumber", 500],
	["tomatoes", 350],
	["onion", 50],
]);

for (let [key, value] of recipeMap) {
	console.log(key, value);
}
```

#### 3. for - in 문

- 객체의 열거 가능한 속성들을 순회하도록 해주는 반복문이다.
- 객체의 key 값에만 접근이 가능하며, value에는 접근이 불가능하다.
- 임의의 순서대로 객체의 각 속성들에 대해 반복하기에 일관된 순서를 보장하지 않음.

```javascript
const obj = { a: 1, b: 2, c: 3 };

for (const prop in obj) {
	console.log(`${prop}`); // a, b, c
}
```

- 주의할 점은, 해당 객체의 속성 뿐만이 아니라 상속 받은 prototype의 속성까지 열거한다는 점이다.
- 만약 객체의 속성만 열거하고 싶다면, `Object.prototype.hasOwnProperty` 메서드를 통해 확인해야 한다.

> 열거할 수 있는 속성이란 대체 무엇인가?

- 내부적으로 `[[Enumerable]]` 프로퍼티가 `true` 로 설정된 프로퍼티를 의미함.
- 이러한 속성의 경우 for - in 구문을 통해 접근하여 순회할 수 있음.

# ✒️ Iterable? Iterator? 이것들은 대체 뭘까?

#### 1. Iterable

- 1-1. 정의

  - ES6에서 새롭게 규정된 것으로 "반복 가능한 객체" 를 의미한다.
  - Iteration Protocol 을 통해 해당 객체가 Iterable한지를 평가한다.

- 1-2. 조건

  - 객체 내에 `[Symbol.iterator] (= @@iterator)` 메소드가 존재해야 한다.
  - `[Symbol.iterator]` 메소드는 반드시 **Iterator** 객체를 반환해야 한다.

#### 2. Iterator

- 2-1. 정의

  - Iterable 객체에서 반복을 위해 설계된, 특별한 인터페이스를 의미한다.
  - Itrable 한 객체가 반복하면서 어떠한 값을 반환할지를 결정하게 된다.

- 2-2. 조건

  - Iterator 객체 내에는 반드시 `next` 메서드가 존재해야 한다.
  - `next` 메소드의 경우 반드시 `IteratorResult` 객체를 반환해야 한다.
  - `IteratorResult` 객체의 경우 `done: boolean` 과 `value: any` 속성을 가진다.
  - 이전 `next` 메서드 호출의 결과에서 done 속성이 `true` 였다면, 이후 호출에도 done 속성이 `true` 여야 한다.

#### 3. 한번 Iterable한 객체를 구현해보자.

- 하단의 예제는 1부터 10까지의 숫자를 반복시키는 Iterable한 객체를 생성하였다.
- `done` 속성의 경우 Iterator의 반복이 완료되었는지를 판별하기에, Iterator는 `done` 의 속성이 true가 될 때까지 반복을 진행한다.

```javascript
const testIterable = {
	[Symbol.iterator]() {
		let i = 1;
		// [Symbol.iterator] 메소드는 Iterator 객체를 반환함.
		return {
			// Iterator 객체 내에는 반드시 next 메소드가 존재해야 함.
			next() {
				while (i < 10) {
					// next 메소드의 경우 반드시 IteratorResult 객체를 리턴해야 함.
					return { value: i++, done: false };
				}
				// 반복이 끝났을 경우 done 속성의 값은 true, value는 생략 가능.
				return { done: true };
			},
		};
	},
};

// 실행 시 1 부터 9 사이의 자연수가 여러 줄에 걸쳐 출력됨.
for (const num of testIterable) {
	console.log(num);
}
```

#### 4. Well - formed Iterable

- Iterator면서 동시에 Iterable한 객체를 Well - formed Iterable 라고 한다.
- 즉, `iter[Symbol.iterator] === iter` 인 경우를 Well - formed Iterable 라고 한다.
- Well - formed Iterable의 장점은 얼마나 반복을 진행했는지를 기억할 수 있다는 점이다.

```javascript
const iterable = {
	i: 1,
	next() {
		while (this.i < 10) {
			return { value: this.i++, done: false };
		}
		return { done: true };
	},

	// iterable 객체에 Symbol.iterator 메서드가 존재하며 자기 자신을 반환한다.
	// 왜냐면 iterable 객체의 경우 iterator가 되기 위한 조건을 모두 갖추었기 때문이다.
	[Symbol.iterator]() {
		return this;
	},
};

console.log(iterable.next()); // {value: 1, done: false}
console.log(iterable.next()); // {value: 2, done: false}
console.log(iterable.next()); // {value: 3, done: false}

// 앞에서 next 메서드를 호출했기 때문에, 4부터 반복시 시작된다.
for (const num of iterable) {
	console.log(num); // 4, 5, ... 9
}
```

- Itrable 객체를 구현할 경우 **제너레이터** 를 활용하여 구현의 번거로움을 줄일 수 있다.
- 제너레이터 함수를 실행할 경우 반환되는 객체의 경우 Well - formed Iterable로 평가된다.

```javascript
function* iterator(numbers) {
	for (const num of numbers) {
		yield num;
	}
}

const iterable = iterator([...Array(10).keys()]);
for (const number of iterable) {
	console.log(number);
}
```
