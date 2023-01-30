# ✒️ ES2019

### ✏️ New Method of Array

#### 1. Array.prototype.flat(depth)

- `flat()` 메서드는 지정된 깊이까지 배열을 재귀적으로 평면화 시킨 후, 변경된 결과를 반환한다.
- 만약 depth 인수가 지정되지 않았다면 default 로 1을 가진다.
- depth 인수가 `Infinity` 인 경우 모든 중첩 배열을 평면화 할 수 있다.

```javascript
const arr = ["a", "b", ["c", "d", ["e", "f"]]];

arr.flat(); // ["a", "b", "c", "d", ["e", "f"]]
arr.flat(2); // ["a", "b", "c", "d", "e", "f"]
arr.flat(1).flat(1); // 체이닝 기법 또한 사용이 가능하다.
```

#### 2. Array.prototype.flatMap()

- `flat()` 메서드와 작동 방식은 유사하나, 각 요소에 대한 매핑을 선행적으로 수행한다.
- 이후 결과를 `depth 1` 로 평면화된 새로운 배열로 반환한다.

```javascript
let arr = [[1], [2], [3], [4]];

arr.flatMap((x) => x * 2); // [2, 4, 6, 8]
arr.flat().map((x) => x * 2); // [2, 4, 6, 8]
```

### ✏️ New Method of Object

#### 1. Object.fromEntries()

- `Object.fromEntries()` 메서드는 키와 값이 한 쌍으로 묶인 배열을 객체로 반환한다.
- 인자로는 단순히 배열 뿐만이 아니라 iterable 한 객체가 모두 들어갈 수 있다.

```javascript
const entries = new Map([
	["foo", "bar"],
	["baz", 42],
]);

const arr = [
	["0", "a"],
	["1", "b"],
	["2", "c"],
];

const objArr = Object.fromEntries(arr);
const objMap = Object.fromEntries(entries);

console.log(objArr); // { 0: "a", 1: "b", 2: "c" }
console.log(objMap); // { foo: "bar", baz: 42 }
```

### ✏️ New Method of String

#### 1. String.prototy.trim() / trimStart() / trimEnd()

- `trim()` 메서드는 문자열에 존재하는 모든 공백 문자를 제거한다.
- `trimStart()` 메서드는 문자열 시작 부분에 존재하는 공백 문자를 모두 제거한다.
- `trimEnd()` 메서드는 문자열 끝 부분에 존재하는 공백 문자를 모두 제거한다.

```javascript
const greeting = "   Hello world!   ";

console.log(greeting.trimStart()); // "Hello world!   ";
console.log(greeting.trimEnd()); // "   Hello world!";
console.log(greeting.trim()); // "Hello world!";
```

### ✏️ New Method of Function

#### 1. Function.prototype.toString()

- `toString()` 메서드는 함수의 소스 코드를 나타내는 문자열을 반환한다.
- ES2016까지는 주석을 포함하지 않았으나, ES2019에서는 주석과 개행문자도 포함시켜 반환한다.

```javascript
function a() {}

a.toString(); // 'function a() {\n}'
```

### ✏️ New Method of Symbol

#### 1. Symbol.prototype.description()

- `description` 속성은 해당 심벌 객체의 설명을 내포하고 있다.

```javascript
const baikSym = Symbol("Baik");
console.log(baikSym.description); // 'Baik'
```
