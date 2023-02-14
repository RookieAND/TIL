# 📖 Introduction

# ✒️ TS와 JS 간의 관계

### ✏️ Typescript is superset of Javascript

-   기본적으로 타입스크립트는 자바스크립트의 상위 호환 (superset) 이다. 이는 문법적으로도 적용되기에 JS로 작성된 코드의 오류가 없다면, 이는 곧 유효한 TS 코드라고도 말할 수 있다.
-   JS로 작성된 코드에 어떠한 이슈가 존재할 경우, 이는 문법 오류가 아니더라도 Type을 체크하는 과정에서 발견될 수 있다. 하지만 **문법의 유효성** 과 **동작의 이슈**, 즉 런타임 과정에서 발생하는 문제는 서로 다르다.
-   설령 TS 코드 내의 타입 체커에게 오류를 지적 당하더라도, TS는 코드를 JS로 파싱할 수 있으며 이를 실행할 수도 있다. 즉 문법적인 오류가 존재하더라도 **JS로의 변환**과 **코드의 실행은 가능**하다는 것이다.

### ✏️ Benefit of migration

-   자바스크립트 파일의 확장자 명은 `.js` 혹은 `.jsx` 이지만 타입스크립트 파일의 확장자 명은 `.ts` 혹은 `.tsx` 이다. 하지만 두 언어는 완전히 다른 체계의 언어가 아니라는 점을 유의하자.
-   타입스크립트는 자바스크립트의 완벽한 상위 호환이기 때문에 **자바스크립트로 작성된 파일은 이미 타입스크립트라고** 말할 수 있다. 실제로 `.js` 확장자를 `.ts` 로 변경한다고 해도 달라지는 것은 없다.
-   이렇기에 기존의 자바스크립트로 작성된 파일을 타입스크립트로 마이그레이션 하는 과정은 매우 간단해진다. 기존의 코드를 유지하면서 일부 타입 지정이 필요한 부분만 수정하면 되기 때문이다.
-   만약 자바스크립트로 작성된 코드를 그대로 Java 같이 다른 체계의 언어로 마이그레이션 할 경우, 차라리 기존의 코드를 뒤엎고 새로운 코드를 작성하는 것이 더 빠를 수 있다.

### ✏️ JS to TS is Ok, but TS to JS is..

-   모든 JS 프로그램은 TS 프로그램이다. 하지만 모든 TS 프로그램은 전부 JS 프로그램이 아니다. TS 에서는 JS에 없는, **타입 지정을 명시하는 문법** 을 가지기 때문이다.

```ts
function greet(who: string): string {
    console.log(`Hello, ${who}!`);
}
```

-   예를 들어 상단의 코드는 유효한 TS 프로그램이지만, 이를 JS 환경에서 실행시키면 오류를 내뿜는다. `: string` 같은 문법은 TS에서 사용되는 문법이지, JS에는 없는 문법이기 때문이다.

```js
let city = 'new york city`
// Uncaught TypeError: city.toUppercase is not a function
// TS는 자동 추론을 통해 city 변수의 타입을 string 으로 인식한다.
console.log(city.toUppercase());
```

-   상단의 코드는 JS로 작성되었으며, 컴파일 과정에서는 에러가 나지 않지만 이를 실행시키는 런타임 과정에서는 `TypeError` 가 발생한다.
-   또한 TS는 사용자가 특정 변수의 타입을 명시하지 않아도 초기 값을 통해 **타입 추론** 을 진행한다. 이는 TS가 정적 타입 시스템으로서 가지는 특징이다.

```js
const nations = [
    { name: 'Japan', capital: 'Tokyo' },
    { name: 'Korea', capital: 'Seoul' },
];

for (const nation of nations) {
    // JS : undefined, undefined 출력
    // TS : 컴파일 과정에서 capitol 속성이 없음을 명시
    console.log(state.capitol);
}
```

-   JS 환경에서 상단의 코드는 오류 없이 실행되나, 이를 TS에서 실행하려 하면 타입 체커에 의해 추가적인 타입 구문 없이 **타입 체커 과정에서** 오류를 잡아낸다.

```ts
interface Nation {
    name: string;
    capital: string;
}

const nations: Nation[] = [
    { name: 'Japan', capital: 'Tokyo' },
    { name: 'Korea', capitol: 'Seoul' }, // '{ name: string; capitol: string; }' 형식은 'Nation' 형식에 할당할 수 없습니다.
];

for (const nation of nations) {
    // 만약 Nation 인터페이스를 선언하여 사용하지 않았더라면?
    // TS는 오류를 내뿜지 않고 그대로 코드를 실행했을 것이다.
    console.log(state.capitol);
}
```

-   또한 추가적인 타입 구문을 통해 사용자는 **코드의 의도** 를 TS에게 전달할 수 있고, 이는 TS가 코드의 잠재적인 문제를 쉽게 찾을 수 있도록 해준다.
-   상단의 코드는 별도의 인터페이스를 생성하고 이를 적용하여 `nations` 배열 내부의 인자는 반드시 `Nation` 인터페이스의 양식을 따르도록 TS에게 의도를 전달해준 케이스이다.

### ✏️ 타입 체커 통과 유무에 따른 차이점

-   TS의 타입 시스템은 기본적으로 JS의 런타임 동작을 모델링 한다. 하지만 JS에서 허용되었던 방식이 TS에서는 통하지 않을 수 있다.

```ts
// TS는 컴파일 과정에서 하단의 코드에 오류를 지적하지 않는다.
const x = 2 + '3';
const y = '2' + 3;

// TS 는 컴파일 과정에허 하단의 코드에 오류를 지적한다.
const a = null + 7; // 여기서는 null 을 사용할 수 없습니다.
const b = [] + 12; // '+' 연산자를 'never[]' 및 'number' 형식에 적용할 수 없습니다.
```

-   따라서 JS는 TS에 포함되어 있지만, 그렇다고 해서 **모든 JS 코드가 TS의 타입 체커를 통과하는 것은 아니라는 것**을 알아야 한다.
-   상단의 코드는 JS 에서 모두 정상적으로 실행되는 코드이다. 하지만 TS에서는 일부 코드에 대한 유효성 검사를 진행하여 컴파일 단계에서 에러를 체크했다.
-   만약 현재 JS로 작성된 코드가 하단의 코드처럼 TS에서 막히는 경우가 많다면, TS로의 마이그레이션을 고민해야 할 수도 있다.

### ✏️ TS의 타입 체커는 정확하지 않을 때가 있다.

```ts
const name = ['Baik', 'Kim'];
// index 의 유효 범위가 넘어갈 것이라는 생각까지는 TS에서 해주지 않는다.
console.log(name[2].toUpperCase());
```

-   설령 작성된 코드가 TS의 타입 체크를 통과하더라도 런타임 에러를 일으킬 수 있다. TS는 `name` 배열을 사용할 때 유효한 index 내에서 사용할 것이라 생각했지만, 실제로는 그렇지 않았기 때문이다.
-   이러한 오류가 발생하는 원인은 **TS가 이해하는 값의 타입** 과 **실제 런타임 환경에서 적용되는 값** 의 차이가 존재하기 때문이다. TS의 정적 타입은 항상 정확성을 보장해주지 않는다.

# ✒️ TS 설정 이해하기

### ✏️ tsconfig.json

-   TS에서는 별도의 설정을 통해 어디서 소스 파일을 찾을지, 어떤 종류의 출력을 생성할지 제어할 수 있다.
-   주로 `tsconfig.json` 파일 내부에서 설정을 제어하며, `tsc --init` 명령어를 통해 생성이 가능하다.

### ✏️ noImplicitAny

```ts
// noImplicitAny 옵션이 꺼져 있다면, a외 b는 any로 추론된다.
function add(a, b) {
    return a + b;
}
add(10, null);
```

-   `noImplicitAny` 설정은 TS가 암시적으로 타입을 `any` 로 추론하지 않도록 하는 옵션이다. TS는 타입 정보를 가졌을 때가 가장 효율적이기에, 되도록이면 `noImplicitAny` 설정을 켜두어야 한다.

### ✏️ strictNullChecks

```ts
// strictNullChecks 옵션이 꺼져 있다면, x에는 정상적으로 null이 할당된다.
const x: number = null;

// 사용자가 직접 타입을 지정하여 해당 변수에 null 도 할당될 수 있음을 명시.
const y: number | null = null;
```

-   `strictNullChecks` 옵션은 `null` 과 `undefined` 가 모든 타입에서 허용되는지를 체크하는 속성이다.
-   만약 null을 할당하는 것을 허용하고자 한다면, 사용자의 의도를 타입에 명시함으로서 오류를 정정할 수 있다.

```ts
const element = document.getElementById('status');
element.innerText = 'Done!'; // element는 null 일 수 있습니다.

// 별도의 타입 가드를 통해 element가 null 이 아님을 체크해야 한다.
if (element) {
    element.innerText = 'Done!';
}

// 혹은 다음과 같이 Type Assertion 을 활용한 타입 단언으로도 가능하다.
const element = document.getElementById('status') as HTMLElement;
element.innerText = 'Done!'; // TS에서는 더 이상 null 인 케이스를 고려하지 않는다.
```

-   `strictNullChecks` 옵션은 코드의 작성을 가끔 어렵게 한다. TS에게 해당 값이 어디서부터 왔으며 가끔 필요하다면 `as` 키워드를 통한 타입 단언도 진행해주어야 한다.
-   하지만 프로젝트의 규모가 커질수록 `null` 과 `undefined` 에 대한 잠재적 위험도가 높아지기에 초기에 이러한 설정을 엄격히 잡아내면서 개발하는 것이 더욱 효율적이다.

# ✒️ 코드 생성과 타입은 무관계하다.

### ✏️ 타입스크립트 컴파일러가 하는 일

-   최신 TS / JS 코드를 모든 브라우저에서 동작할 수 있도록 구버전 (ES5, ES3) 의 JS 코드로 트랜스파일 한다.
-   작성된 코드의 타입 오류 여부를 체크한다.

### ✏️ 타입 오류가 있더라도 컴파일은 된다.

-   컴파일러가 하는 작업은 서로 독립적으로 기능한다. 따라서 타입 오류가 존재하는 코드더라도 **JS로의 컴파일은 가능** 하다. 상호 기능 간의 간섭이 불가능하기 때문이다.
-   타입스크립트 오류는 C나 Java의 Warning과 비슷하다. 문제가 될만한 부분을 지적하지만 빌드를 중단하지는 않는다.

> 따라서 **컴파일 과정** 에서 문제가 발생했다는 말은 틀렸다.

-   TS 코드에 오류가 발생했을 경우 **컴파일 과정에서 문제가 생겼다** 는 말은 기술적으로 틀린 말이다. 작성한 TS 코드가 유효한 JS 코드라면 타입에 문제가 생기더라도 컴파일을 진행하기 때문이다.
-   따라서 코드에 오류가 발생했을 때는 **타입 체크 과정에서 문제가 생겼다** 라고 말하는 것이 더욱 바람직하다. 앞서 말했듯 타입을 체크하는 과정과 컴파일 과정은 독립적으로 시행되기 때문이다.

### ✏️ 오류가 발생해도 컴파일이 되는게 정상인가?

-   타입 오류가 존재하지만 컴파일이 가능하다는 점은 확실히 이상하다고 느껴질 수 있다. 하지만 이는 **빠른 오류 수정에 큰 도움이 된다.**
-   만약 특정 기능에 문제가 생겼다고 가정해보자. TS는 오류를 지적하지만 컴파일된 결과물을 제공하므로 사용자는 오류 수정을 하지 않아도 다른 기능에 대한 테스트를 수행할 수 있다.
-   만약 오류가 발생할 시 컴파일을 막기 위해서는 tsconfig.json 내의 `noEmitOnError` 옵션을 설정하면 된다.

### ✏️ 런타임 과정에서는 타입 체크가 불가능하다.

```ts
interface Square {
    width: number;
}

interface Rectangle extends Square {
    height: number;
}

type Shape = Square | Rectangle;

function calculateArea(shape: Shape) {
    // 'Shape' 은(는) 형식만 참조하지만, 여기서는 값으로 사용되고 있습니다.
    if (shape instanceof Square) {
        return shape.width ** 2;
    }
    return shape.width * shape.height;
}
```

-   `instanceof` 는 런타임에 일어나지만 `Square` 은 타입이기에 런타임 시점에서는 아무런 역할을 할 수 없다. TS의 타입은 JS로 컴파일 되는 과정에서 **모두 제거** 되기 때문이다.
-   앞의 코드에서 다루고 있는 `shape` 변수의 타입을 명확히 하기 위해서는 런타임에서 타입에 대한 정보를 유지하는 방법이 필요하다.

1. 특정 인터페이스에만 존재하는 속성이 있는지를 체크해라.

```ts
function calculateArea(shape: Shape) {
    // 해당 조건식이 참이라면, TS는 shape를 자동으로 Square로 추론한다.
    if (!'height' in shape) {
        return shape.width ** 2;
    }
    // 조건식이 거짓이라면 TS는 shape를 Rectangle로 자동 추론한다.
    return shape.width * shape.height;
}
```

2. 런타임에서 접근 가능한 정보를 명시적으로 저장하는 '태그' 기법 사용

```ts
interface Square {
    kind: 'square';
    width: number;
}

interface Rectangle {
    kind: 'rectangle';
    width: number;
    height: number;
}

type Shape = Square | Rectangle;

function calculateArea(shape: Shape) {
    // kind 속성의 값 유무에 따라 shape 변수의 타입을 추론한다.
    if ((shape.kind = 'square')) {
        return shape.width ** 2;
    }
    return shape.width * shape.height;
}
```

3. 타입을 클래스로 만들어 런타임 과정에서도 값으로서 접근 가능하게 함

```ts
class Square {
    constructor(public width: number) {}
}

class Rectangle extends Square {
    constructor(public width: number, public height: number) {
        super(width);
    }
}

// 클래스 둘을 하나의 유니온 타입으로 묶어 선언
type Shape = Square | Rectangle;

function calculateArea(shape: Shape) {
    // 클래스 인스턴스를 체크하는 것이기에 instanceof 사용이 가능해진다.
    // 조건식이 참이라면 TS는 자동으로 shape를 Square 타입으로 추론한다.
    if (shape instanceof Square) {
        return shape.width ** 2;
    }
    return shape.width * shape.height;
}
```

### ✏️ 런타입 타입은 선언된 타입과 다를 수 있다.

```ts
interface LightApiResponse {
    lightSwitchValue: boolean; // boolean | string 이라면?
}

async function setLight() {
    const response = await fetch('/light');
    const result: LightApiResponse = await response.json();
    setLightSwitch(result.lightSwitchValue);
}
```

-   상단의 코드에서는 `fetch('/light')` 의 응답이 반드시 `LightApiResponse` 를 반환할 것이라는 보장이 없다. 만약 `lightSwitchValue` 속성이 boolean이 아닌 string 타입이어도 코드는 실행이 된다.
-   따라서 요청에 따라 타입이 달라질 수 있는 상황은 가능한 한 피하고, 달라질 수 있는 타입에 대한 경우를 모두 고려하여 설계하는 것이 좋다.

### ✏️ TS 타입으로는 함수의 오버로드가 불가능하다.

```ts
function add(a: number, b: number) {
    return a + b;
}

// 중복된 함수 구현이다. 오버로딩이 아닌 에러를 일으킨다.
function add(a: string, b: string) {
    return Number(a) + Number(b);
}
```

-   TS는 컴파일 과정에서 타입과 관련된 코드가 제거됨을 잊지 말자. 하나의 함수에 대해 여러 개의 선언문을 작성할 수는 있지만 이는 **타입 단계에서만 동작하며** 실제 구현체는 오직 하나 뿐이다.

### ✏️ TS 연산은 런타임 성능에 영향을 미치지 않는다.

-   타입 및 타입 연산자는 JS로 컴파일 되는 과정에서 모두 제거되기에 실제 런타임 성능에 영향을 주지 않는다. TS의 정적 타입은 런타임 과정에서 별도의 비용을 요구하지 않는다.
-   단, 실제 TS 파일을 JS로 컴파일하는 **빌드 타임 오버헤드** 가 발생한다. 또한 TS가 컴파일하는 코드는 호환성을 높이기 위해 낮은 버전의 JS로 컴파일 할지, 성능 중심의 네이티브 구현체를 선택할지에 대한 문제에 직면할 수 있다.
-   하지만 위 두 문제는 **컴파일 단계에서의 문제** 이며 런타임에서는 영향을 미치지 않는다.

### ✏️ 타입 연산은 런타임에 영향을 주지 않는다.

```ts
// TS로 작성된 asNumber 함수, 컴파일 단계에서는 잘 작동되지만..
function asNumber(val: number | string): number {
    return val as number;
}

// 컴파일 후 변환된 코드는 아래와 같이 타입 체크가 삭제되어 있다.
function asNumber(val) {
    return val;
}
```

-   TS 의 타입 체커는 잘못된 타입 선언에 대한 오류를 지적할 수 있으나, 실제 코드의 작동 과정까지 건드리는 것은 아닌다. 즉 값 자체를 정제하진 않는다.

```ts
// 런타임 과정에서 실제 val의 타입을 체크하고, 그에 맞는 로직을 구현
function asNumber(val: number | string): number {
    return typeof val === 'string' ? Number(val) : val;
}
```

-   따라서 사용자는 런타임 과정에서의 타입을 별도로 체크해주어야 하고, 이를 통해 값을 명시적으로 변환해주는 로직을 별도로 구현해주어야 한다.

# ✒️ 구조적 타이핑에 익숙해지기

### ✏️ JS는 덕 타이핑 기반이다.

-   JS는 본질적으로 덕 타이핑 기반이다. 만약 어떤 함수의 매개변수 값이 그대로 주어진다면, **그 값이 어떻게 만들어졌는지는 전혀 고려하지 않는다.**
-   TS 또한 매개변수 값이 요구사항을 만족한다면 **타입을 고려하지 않는 동작** 을 그대로 모델링 할 수밖에 없다.
-   하지만 컴파일러가 이해한 타입과 사람이 이해한 타입이 항상 일치하지 않기 때문에 예상치 못한 결과가 나올 수 있다.

> **덕 타이핑** 이란?

-   동적 타이핑에 한 종류로, 객체의 속성 및 메소드의 집합이 객체의 **타입을 결정하는 것** 을 의미한다.
-   클래스 상속이나 인터페이스 구현 대신, 객체가 어떤 타입에 걸맞는 변수와 메소드를 지니면 **자동으로 해당 타입에 속하는 것으로 간주** 한다.

```ts
interface Vector2D {
    x: number;
    y: number;
}

interface NamedVector {
    name: string;
    x: number;
    y: number;
}

function calculateLength(v: Vector2D) {
    return Math.sqrt(v.x ** 2 + v.y ** 2);
}

const nv = { name: 'Test', x: 3, y: 4 }; // NamedVector
const nvLength = calculateLength(nv); // 5
```

-   `NamedVector` 는 `Vector2D` 의 속성인 `x` 와 `y` 를 모두 가지고 있기에 calculateLength 함수로 호출이 가능하다. TS에서는 두 인터페이스 간의 구조가 호환되는지를 자동으로 체크해준다.
-   사실 우리가 생각하기에 `NamedVector` 와 `Vector2D` 는 서로 다른 인터페이스이기 때문에 호환이 안된다고 생각하겠지만, 실제로는 **호환이 가능하기 때문에** 정상적으로 함수가 호출된다.
-   따라서 `NamedVector` 는 `Vector2D` 의 상속 관계를 굳이 선언하지 않아도 함수에서 호출이 가능하다. TS는 JS의 런타임 동작을 모델링 하기에 런타임 과정에서는 JS에서 문제가 되지 않는 코드라면 정상적으로 작동되는 것이다.

> 타입은 **봉인되어 있지 않음** 을 반드시 유의하자.

-   우리가 설정한 타입이 가지는 속성만을 정확히 보유한 값만이 올 것이라고 생각하지 말자. TS는 할당에 필요한 속성 값만을 갖추고 있다면 모두 할당 가능한 값이라고 본다. 그 외 나머지 속성에 대한 존재 여부는 체크하지 않는다.

```ts
interface Vector3D {
    x: number;
    y: number;
    z: number;
}

const v = { x: 4, y: 3, z: 5 };
function normalize(v: Vector3D) {
    const length = calculateLength(v);
    return { x: v.x / length, y: v.y / length, z: v.z / length };
}
```

-   상단의 코드는 오직 `Vector2D` 만을 받는 calculateLength 함수를 사용했음에도 문제없이 코드가 실행되었다.
-   `Vector3D` 와 호환되는 객체로 해당 함수를 호출하면, 구조적 타이핑 관점에서 `Vector2D` 와 호환이 된다. `x` 와 `y` 속성을 모두 갖추었기 때문이다.
-   타입스크립트에서는 호출에 사용되는 매개변수의 속성들이 **타입에 선언된 속성들만을 가지지 않아도 된다.** 그 외에도 추가적인 속성을 가질 수 있는, 즉 열린 타입이라는 의미이다.

```ts
function calculateLengthL1(v: Vector3D) {
    let length = 0;
    for (const axis of Object.keys(v)) {
        // `string` 은 `Vector3D` 의 인덱스로 사용할 수 없기에, 엘리먼트는 암시적으로 any 입니다.
        const coord = v[axis];
        length += Math.abs(coord);
    }
    return length;
}
```

-   우리가 생각했을 때 `axis` 는 `Vector3D` 의 속성인 `x, y, z` 중 하나만을 가질 것이라 생각했지만, 사실 매개변수 `v` 는 `x, y, z` 뿐만 아니라 다른 속성을 가지고 있을 수도 있다.

```ts
const v3D = { x: 3, y: 4, z: 5, address: 'Hwaseong' };
calculateLengthL1(v3D); // NaN
```

-   `v` 는 어떤 속성이던 가질 수 있기 때문에 `axis` 의 타입은 `string` 도 될 수 있다. 따라서 TS는 `v[axis]` 의 속성을 number 라고 확정지을 수가 없는 상태이다.
-   만약 axis 에 `address` 속성이 들어간다면? 이는 그대로 런타임 과정에서 실행되어 `NaN` 이라는 결과를 발생시킬 것이다.

# ✒️ 만악의 근원, any 타입 피하기

-   TS의 타입 시스템은 점진적이고 선택적이다. 코드에 타입을 새롭게 추가할 수 있고 타입 체커를 `any` 로 언제든지 해제할 수 있기 때문이다.
-   하지만 `any` 를 사용하면 TS에서 제공하는 강력한 장점들을 누리기 어렵다. 따라서 사용을 최대한 자제하는 것이 좋다.

### ✏️ any는 타입 안전성이 없다

```ts
let age: number;
age = '12' as any; // 허용이 된다. 하지만 좋지 못하다.
```

-   `age` 변수는 number 타입으로 선언되었지만 이후 `any` 타입으로 단언되어 `string` 도 할당이 가능하게 되었다. 이렇게 될 경우 타입 체커가 올바르게 작동하지 못하게 된다.

### ✏️ any는 함수 시그니쳐를 무시한다.

-   함수를 작성할 때는 약속된 타입의 입력을 제공하고, 약속된 타입의 출력을 반환하는 시그니쳐를 명시해야 하는데 `any` 타입을 사용할 경우 잘 지켜지지 않게 된다.

```ts
function calculateAge(birth: Date): number {
    return ...
}

let birthDate: any = '1990-01-19'
calculateAge(birthDate) // 코드는 정상적으로 실행된다.
```

-   분명 `calculateAge` 는 인자를 `Date` 타입으로 받아야 하나 `any` 는 이러한 제약 조건을 깡그리 무시하고 함수를 실행시켜 결과 값을 받아낸다.

### ✏️ any 타입에는 TS의 언어 서비스가 적용되지 않음

-   특정 객체나 심벌에 타입이 존재한다면, TS는 해당 객체의 프로퍼티 목록을 나열하는 등의 유용한 기능들을 제공한다. 하지만 `any` 타입은 이러한 도움을 받지 못한다.

### ✏️ any 타입에는 리팩터링 과정에서 버그를 은닉함

-   특정 코드를 수정하고자 할 경우, 일부 기능에 `any` 를 사용하게 된다면 어느 시점부터 문제가 되는 값이 들어오게 되었는지 체크하는 것이 사실상 불가능해진다.
-   타입 체커는 any를 무조건 통과시키기 때문에 문제가 없다고 판단하지만 런타임 과정에서는 문제가 발생할 것이다. 만약 `any` 가 아닌 구체적인 타입을 사용했다면 예방이 가능하다.

### ✏️ any 타입은 타입 설계를 감추고, 신뢰도를 떨어트린다.

-   객체를 interface로 설계하여 이를 유동적으로 관리하는 것은 어렵지만, 이것이 귀찮다고 `any` 를 사용해버리면 해당 객체 내부의 속성을 감추기 때문에 추후 설계를 확인하기 어려워진다.
-   타입스크립트는 런타임 이전에 타입 체커를 통해 개발자가 오류를 사전에 잡아내도록 하는 목적이 가장 큰데, `any` 타입을 사용한다는 것은 오히려 독이 될 수 있다.

# 📒 References
