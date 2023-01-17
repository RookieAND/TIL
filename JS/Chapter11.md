# ✒️ Symbol

#### 1. Symbol은 대체 무엇인가?

- Symbol은 항상 고유한 값이며, 주로 객체 속성의 식별자로서 사용된다.
- Symbol은 JS에서 지원하는 윈시형 타입 (primitive) 이며, 수정이 불가능하다.

#### 2. Symbol은 어떻게 생성하는가?

- 심벌의 경우 `Symbol()` 함수를 통해 생성할 수 있다.
- 함수 호출 시 인자로 받는 문자열의 경우, 생성된 심볼에 대한 설명문이다.
- console.log 등과 같은 디버깅 시에 각 심볼을 구분하기 위한 용도로 쓰인다.

```javascript
const symbol = Symbol("Baik");
const cloneSymbol = Symbol("Baik");

// 두 심벌의 값은 동일하나, 각 심벌은 항상 고유 (unique) 하므로 다름.
console.log(symbol === cloneSymbol); // false
```

#### 3. Symbol은 대체 왜 쓰는 걸까?

- 객체의 속성을 **고유하게 설정**함으로서 각 속성 키의 충돌을 방지하기 위해 쓰인다.
- 객체의 속성 키는 문자열이나, Symbol을 사용하여 고유한 속성을 만들 수 있다.

```javascript
const obj = {};

const sym1 = Symbol();
const sym2 = Symbol("foo");
const sym3 = Symbol("foo");

obj[sym1] = "propertyValue1";
obj[sym2] = "propertyValue2";
obj[sym3] = "propertyValue3"; // sym2와의 충돌이 일어나지 않음!

console.log(obj); // {Symbol(): 'propertyValue1', Symbol(foo): 'propertyValue2', Symbol(foo): 'propertyValue3'}

console.log(obj[sym1]); // propertyValue1
console.log(obj[sym2]); // propertyValue2
console.log(obj[sym3]); // propertyValue3
```

- 심벌은 열거 가능하지 않기 때문에 `for - in` 반복문에 걸리지 않는다.
- 따라서 `Object.getOwnPropertySymbols()` 함수를 통해 객체 속성의 배열을 얻는다.
- 또한 객체를 JSON으로 변환시킬 경우, 키가 심볼인 프로퍼티들은 무시된다.

```javascript
const obj = {
	[Symbol("first")]: "first",
	[Symbol("second")]: "second",
	[Symbol("third")]: "third",
	fourth: "fourth",
	fifth: "fifth",
};

// for - in 구문을 사용할 경우, 키가 심볼인 프로퍼티는 열거되지 않는다.
for (const property in obj) {
	console.log(property); // fourth, fifth
}

// 객체를 JSON으로 변환시킬 경우, 키가 심볼인 프로퍼티들은 무시된다.
JSON.stringfy(obj); // {"fourth": "fourth", "fifth": "fifth"}

// Object.getOwnPropertySymbols() 메소드로 심볼로 이루어진 배열을 받는다.
const symbols = Object.getOwnPropertySymbols(obj);
console.log(symbols); // [Symbol(first), Symbol(second), Symbol(third)]

for (const symbol of symbols) {
	console.log(obj[symbol]); // first, second, third
}
```

- 특별한 용도로 JS 엔진 내에 미리 생성되어 상수로 존재하는 내장 심볼도 존재한다.
- 가장 대표적인 예시로는 iterable한 객체인지를 판별하는 `[Symbol.iterator]` 프로퍼티다.

```javascript
Array.prototype[Symbol.iterator];
String.prototype[Symbol.iterator];
Map.prototype[Symbol.iterator];
Set.prototype[Symbol.iterator];
arguments[Symbol.iterator];
NodeList.prototype[Symbol.iterator];
HTMLCollection.prototype[Symbol.iterator];
```

# ✒️ Symbol Function

#### 1. Symbol.for()

- Symbol.for() 함수의 경우 인자로 받은 문자열과 일치하는 키를 가진 Symbol을 검색한다.
- Symbol 값들은 전역 Symbol 레지스트리에 저장되어 있는데, 여기서 검색을 진행한다.
- 검색에 성공했다면 이를 반환하고, 실패할 경우 새로운 Symbol 값을 생성하여 반환한다.

```javascript
const origin = Symbol.for("origin");
const newest = Symbol.for("origin");

console.log(origin === newest); // true
```

#### 2. Symbol.keyFor()

- Symbol.keyFor() 함수의 경우 인자로 받은 문자열과 일치하는 키를 가진 Symbol을 검색한다.
- 검색에 성공했다면 이를 반환하고, 실패할 경우 `undefined` 를 반환한다.
- 단순히 키를 사용하여 심볼이 존재하는지만을 알고 싶을 경우 사용하는 함수.

> Symbol 함수로 만든 심볼과 Symbol.for() 함수로 만든 심볼은 무슨 차이가?

1. Symbol() 로 생성된 심볼

- Symbol 함수로 만들어진 심볼의 경우 키를 가지고 있지 않는다.
- Symbol 함수로 만들어진 심볼의 경우 전역 심볼 레지스트리에 저장되지 않는다.

2. Symbol.for() 로 생성된 심볼

- Symbol.for() 함수로 만들어진 심볼의 경우 키를 가지고 있다.
- Symbol.for() 함수로 만들어진 심볼의 경우 전역 심볼 레지스트리에 저장된다.

3. 결론

- Symbol.for() 함수의 경우 여러 모듈이 하나의 심볼을 공유하기 위해 사용.
- Symbol.for() 함수로 생성된 심볼은 키를 가지며 이를 활용할 수 있기 때문.
