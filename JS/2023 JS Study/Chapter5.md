# ✒️ String Method in JS

#### 1. indexOf(str: string)

-   인자로 받은 값이 문자열에서 처음 나타나는 인덱스를 return 한다.
-   만약 문자열 내에서 값을 찾지 못했을 경우, `-1` 을 return 한다.

```javascript
const str = 'this is sentence';
str.indexOf('this'); // 0
str.indexOf('are'); // -1
```

#### 2. slice(start: number, end: number)

-   두 인덱스를 매개변수로 받아, 해당 인덱스 사이에 위치한 문자열을 잘라 return 한다.
-   기존의 문자열은 변형시키지 않는다.

```javascript
const str = 'this is sentence';
const is = str.slice(5, 7);
console.log(is); // is
```

#### 2-1. substring(start: number, end: number)

-   두 인덱스를 매개변수로 받아, 해당 인덱스 사이에 위치한 문자열을 잘라 return 한다.
-   원래의 문자열에는 영향을 주지 않는다.

> substring과 slice의 차이점

-   만약 `start > end` 일 경우, substring은 이를 바꾸어 결과를 계산한다.
-   하지만 slice의 경우 겹치는 영역이 없다 판단하고 빈 문자열을 return 한다.

```javascript
let str = 'Hello World';

let a = str.substring(5, 0);
let b = str.slice(5, 0);

console.log('a: ' + a);
console.log('b: ' + b);
```

-   만약 `start` 가 음수일 경우, substring은 이를 0으로 변경하여 계산한다.
-   하지만 slice의 경우 **음의 인덱스**를 적용하여 오른쪽에서부터 이를 세어 계산한다.

```javascript
let str = 'Hello World';

let a = str.substring(-8, 10); // (0, 10)
let b = str.slice(-8, 10); // (3, 10)
let c = str.substring(-8); // (0)
let d = str.slice(-8); // (3)

console.log(a); // Hello Worl
console.log(b); // lo Worl
console.log(c); // Hello World
console.log(d); // lo World
```

#### 3. toUpperCase(), toLowerCase()

-   문자열의 모든 문자를 대문자 / 소문자로 변경한다.

```javascript
const str = 'aPple';
str.toUpperCase(); // APPLE
str.toLowerCase(); // apple
```

#### 3. startWith(str: string, size: number), endWith(str: string, size: number)

-   해당 문자열이 인자로 받은 값으로 시작하는지, 아니면 끝나는지에 대한 여부를 boolean으로 return 한다.
-   두 번째 매개변수로 문자열의 일부만을 확인할 수도 있다.

```javascript
const str = 'ABCDE';
str.startWith('A'); // true
str.endWith('F'); // false

str.endWith('BC', 3); // true, str의 첫 3개 문자인 ABC만을 고려하여 판단.
```

#### 4. includes(str: string)

-   매개변수로 받은 값이 특정 문자열 내부에 포함되는지에 대한 여부를 boolean으로 return 한다.

```javascript
const str = 'ABCDE';
str.includes('AB'); // true
str.includes('EF'); // false
```

#### 5. repeat(count: number)

-   매개변수로 받은 횟수만큼 해당 문자열을 반복시켜 return 한다.

```javascript
const str = 'ABCDE';
str.repeat(3); //ABCDEABCDEABCDE
```

#### 6. trim(), trimStart(), trimEnd()

-   해당 문자열 내부에 존재하는 공백을 모두 제거한 결과 값을 return 한다.
-   기존의 문자열에는 영향을 주지 않는다.
-   trimStart() / trimLeft() 의 경우에는 문자열의 시작 부분에 존재하는 공백을 제거한다.
-   trimEnd() / trimRight() 의 경우에는 문자열의 끝 부분에 존재하는 공백을 제거한다.

```javascript
const greeting = '   Hello world!   ';
console.log(greeting); // "   Hello world!   "
console.log(greeting.trimStart()); // "Hello world!   "
```

#### 7. replace(), replaceAll()

-   첫 번째 매개변수에는 변경할 문자열 혹은 정규식 패턴이, 두 번째 매개변수에는 변경된 문자열이 들어온다.
-   `replace()` 의 경우 오직 첫 번째로 조건을 만족하는 문자열을 변경하고 나머지는 그대로 둔다.
-   `replaceAll()` 의 경우 조건을 만족하는 문자열을 전부 변경시킨다.

```javascript
const test = 'My name is Baik gwangin';
const test2 = 'dog dog dog dog';
test.replace('Baik', 'Kim'); // 'My name is Kim gwangin'
test2.replaceAll('dog', 'cat'); // cat cat cat cat
```

#### 8. charAt(idx: number)

-   인덱스를 매개변수로 받아 해당 문자열의 특정 인덱스에 위치한 글자를 return 한다.

```javascript
const sentence = 'lazy dog.';
const index = 3;
sentence.charAt(index); // y
```

#### 9. split(pattern: str)

-   매개변수로 받은 문자열을 기준으로 해당 문자열을 나누고, 이를 하나의 배열에 묶어 return 한다.

```javascript
const str = 'The quick brown fox';

const words = str.split(' '); // ['The', 'quick', 'brown', 'fox']
console.log(words[3]); // fox
```
