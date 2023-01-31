# ✒️ ES2020

### ✏️ BigInt

- `Number` 값이 나타낼 수 있는 최대치인 `2의 53제곱 - 1` 보다 더 큰 정수를 표현할 수 있는 내장 객체
- 정수 리터럴 뒤에 `n` 을 붙이거나 `BigInt()` 를 호출하여 생성할 수 있음.
- `Math` 객체의 메서드 사용이 불가능하며, 연산에서 `Number` 와 혼용하여 사용할 수도 없다.
- 소수점 결과를 포함하는 연산을 BigInt 와 사용하면 소수점 이하는 사라진다.

```javascript
const theBiggestInt = 9007199254740991n;

const alsoHuge = BigInt(9007199254740991); // 9007199254740991n
```

### ✏️ New Method of Promise

#### 1. Promise.allSettled(iterable)

- Iterable한 객체에 주어진 모든 Promise 요소를 받고, 각 Promise에 대한 결과를 나타내는 배열을 반환함.
- 배열 내부의 Promise 중 거부된 작업이 있더라도, 다른 Promise 작업을 계속 수행함.

```javascript
Promise.allSettled([
	new Promise((resolve, reject) => setTimeout(() => resolve(1), 1000)),
	new Promise((resolve, reject) =>
		setTimeout(() => reject(new Error("에러 발생!")), 2000)
	),
	new Promise((resolve, reject) => setTimeout(() => resolve(3), 3000)),
]).then((results) => results.forEach((result) => console.log(result.status))); // fulfilled, rejected, fulfilled
```

### ✏️ Nullish Coalescing (??)

- 기존의 삼항 연산자 `(?)` 나 기본 값 연산자 `(||)` 및 보호 연산자 `(&&)` 에서 `null`, `undefined`, `""`, `NaN` 등은 모두 falsy 한 값으로 처리되었다.
- 하지만 ES2020 에서 도입된 Nullish Operator `(??)` 를 도입하면 `null` 과 `undefined` 같은 Nullish 한 값만을 걸러낼 수 있다.

```javascript
// || 연산자의 경우 좌변이 falsy 하다면 무조건 우변을 반환
console.log("" || "empty string"); // "empty string"
console.log(undefined || "empty string"); // "empty string

// ?? 연산자의 경우 좌변이 Nullish 한 값일 경우 우변을 반환
console.log("" ?? "empty string"); // ""
console.log(undefined ?? "empty string"); // "empty string"
```

### ✏️ Optional Chaining (?.)

- 기존에는 객체 내부의 특정한 프로퍼티가 있는지를 체크하기 위해 조건식을 사용해야 했다.
- 하지만 Optional Chaining `(?.)` 을 사용하면 프로퍼티가 존재하지 않을 경우 전체 값을 `undefined` 로 반환시킨다.

```javascript
const obj = {
	a: 10,
	b: {
		c: 20,
	},
};

// 기존에는 && 연산자를 통해 프로퍼티가 있는지를 체크해야 했다.
console.log(obj && obj.b && obj.b.c); // 20

// Optional Chaining을 활용하면 간단한 절차로 안전한 접근이 가능하다.
console.log(obj?.b?.c); // 20
```

- 단, 메서드나 함수를 사용할 시에는 `?.` 가 하나의 연산자이므로 꼭 점을 붙여야 한다.

```javascript
const a = {
	b() {
		return true;
	},
};

// a.b?() 가 아니라 a.b?.() 로 점을 붙여서 작성해주어야 한다.
a.b?.(); // true
```

### ✏️ Module Namespace Export

- ES2020 부터는 `* as` 문법을 export 에서도 동일하게 사용할 수 있게 되었다.
- 기존에는 import 에서만 불러온 모듈의 이름을 변경할 수 있었으나, 이제는 export 에서도 가능하게 되었다.

```javascript
// test.js
const testVar = "test";

export * as Test from "./test.js"; // export { testVar } 와 동일

// import.js
import * as Test from "./test.js";

console.log(Test.testvar);
```

### ✏️ import.meta

> 아직 추가적인 학습이 필요한 파트다.

- `import.meta` 는 모듈에 대한 메타 데이터를 객체로 묶어 반환한다.
- 객체에 포함된 `url` 속성 값은 쿼리 매개변수와 해시가 모두 포함되며, 이는 스크립트를 가져온 URL 이거나 문서의 URL 일 수 있다.
- 하단의 예제는 URL 에 포함된 쿼리 매개변수를 인계하고, 이를 전달받는 과정이다.

```html
<script type="module">
	import "./index.mjs?someURLInfo=5";
</script>
```

```javascript
// index.mjs
new URL(import.meta.url).searchParams.get("someURLInfo"); // 5
```

- 또한 해당 모듈의 파일 경로를 `import.meta.url` 로 알아낼 수 있다.

```javascript
// CommonJS 에서는 __dirname 을 활용하여 찾았어야 했다.
const path = require("path");

const filePath = path.join(__dirname, "index.txt");

// ES2020 부터는 아래와 같이 사용이 가능하다.
import { fileURLToPath } from "node:url";

const filePath = fileURLToPath(new URL("./index.txt", import.meta.url));
```

### ✏️ globalThis

- 브라우저의 전역 객체는 `window` 이며 Node.js의 전역 객체는 `global` 이다.
- 실행 환경에 따라서 전역 객체가 달라지기에 과거에는 런타임에서 분기 처리를 반드시 해주어야 했다.
- ES2020에서는 어떤 환경에서는 항상 전역 객체를 참조하는 globalThis를 추가하였다. 이제 더 이상의 분기 처리를 진행하지 않아도 된다.
- 단, 브라우저에서는 전역 객체에 직접 접근할 수 없기 때문에 `globalThis` 가 전역 객체의 Proxy를 참조하도록 했다.

```javascript
// Web Browser 에서 globalThis 사용 시

console.log(globalThis); // Window {0: Window, window: Window, self: Window, document: document, name: '', location: Location, …}
```
