# 📖 Introduction

# ✒️ 타입과 인터페이스의 차이점

### ✏️ TS에서 타입을 정의하는 방법

```ts
// named type
type State = {
    name: string;
    capital: string;
};

// interface
interface State {
    name: string;
    capital: string;
}
```

-   타입스크립트에서 사용자가 타입을 정의하는 방식은 **named type (타입 정의)** 과 **interface (인터페이스)** 로 나뉜다.

### ✏️ 인터페이스와 타입 정의의 유사점

```ts
// Index Signature 사용도 둘 다 가능하다.
type TDict = { [key: string]: string };

interface TDict {
    [key: string]: string;
}

type TFn = (x: number) => string;
interface TFn {
    (x: number): string;
}
```

-   명명된 타입은 인터페이스로 정의하던, 타입으로 정의하던 간에 상태에는 **차이가 없다.**
-   또한 **인덱스 시그니쳐** 와 **함수 타입** 도 인터페이스나 타입으로 정의할 수 있다. 왜냐하면 함수도 객체로 취급하기 때문이다.

```ts
// 인터섹션 혹은 extends 를 통해 확장이 가능하다.
interface TDictWithPop extends TState {
    population: number;
}
type TStateWithPop = TDict & {
    population: number;
};

// 타입 및 인터페이스를 implements 키워드를 통해 적용할 수 있다.
class StateT implements TState {
    name: string = '';
    capital: string = '';
}
```

-   인터페이스는 타입을 확장할 수 있고, 타입 또한 인터페이스를 확장할 수 있다.
-   하지만 인터페이스의 경우 **유니온 타입 같은** 복잡한 타입을 확장하지 못한다. 복잡합 타입을 확장하고 싶다면 타입 및 `&` 연산자를 사용하자.
-   클래스를 구현할 때도 타입과 인터페이스를 모두 사용할 수 있다.

### ✏️ 인터페이스와 타입 정의의 차이점

```ts
type A = { a: 'a' };
type B = { b: 'b' };

// interface 에서는 구현이 불가능한 타입 연산이다.
type ABName = (A | B) & { name: string };
```

-   인터페이스에는 **유니온 타입** 개념을 적용할 수 없다. 하지만 타입에서는 가능하다.
-   `type` 키워드는 일반적으로 `interface` 보다 쓰임새가 다양하며 유니온 타입, 매핑된 타입, 조건부 타입 `infer` 등 다양한 기능에 사용 가능하다.

```ts
type Pair = [number, number];
type StringList = string[];
type NamedNums = [string, ...number[]]; // 0번째 인덱스에 string을 요구

// 비슷하게 정의는 가능하지만, 이는 유사 배열 객체를 의미한다.
interface SimilarArray {
    0: number;
    1: number;
    length: 2;
}
```

-   튜플과 배열 타입도 `type` 키워드를 통해 더욱 간결하게 표현할 수 있다.
-   인터페이스로도 비슷하게 구현은 가능하지만, 이는 **유사 배열 객체** 에 대한 스펙을 정의하는 것이기에 배열 메서드 사용은 불가하다.

```ts
interface IState {
    name: string;
    capital: string;
}
// 같은 Interface를 한 차례 더 선언하여 병합시킴.
interface IState {
    population: number;
}

const wyoming: IState = {
    name: 'Wyoming',
    capital: 'Cheyenne',
    population: 500000,
};
```

-   인터페이스에는 타입에서 지원하지 않는 보강 기능을 지원한다. 그 중 하나가 바로 **선언 병합** 이다.
-   타입스크립트에서 컴파일러가 동일한 이름으로 선언된 두 개의 개별 선언을 단일 정의로 병합하는 것을 **선언 병합** 이라고 한다.
-   각 인터페이스의 속성은 고유해야 하며 고유하지 않다면 타입이 같아야 한다. 만약 그렇지 않을 경우 오류를 일으킨다.

# ✒️ 타입 연산과 제네릭 사용으로 반복 줄이기

### ✏️ DRY 원칙을 타입에서도 적용하자.

-   **DRY (Dont' Repeat Yourself)** 원칙은 비슷한 코드를 반복하여 코드의 가독성을 해치지 말라는 의미다.

```ts
interface Point2D {
    x: number;
    y: number;
}

// 타입 적용 전, 중복된 객체 리터럴 타입이 보인다.
function distance(a: { x: number; y: number }, b: { x: number; y: number }) {
    return Math.sqrt(Math.pow(a.x - b.x, 2) + Math.pow(a.y - b.y, 2));
}

function distance(a: Point2D, b: Point2D) {
    return Math.sqrt(Math.pow(a.x - b.x, 2) + Math.pow(a.y - b.y, 2));
}
```

-   타입 중복을 막는 첫 번째 방법은 바로 **타입에 이름을 붙이는 것** 이다.

```ts
function get(url: string, opts?: Options): Promise<Response> {
    /**...*/
}
function post(url: string, opts?: Options): Promise<Response> {
    /**...*/
}

// 명명된 타입으로 이를 분리하여 하나의 함수 타입을 공유하도록 함
type HTTPFunction = (url: string, opt?: Option) => Promise<Response>;
const get: HTTPFunction = (url, opts) => {
    /**...*/
};
const post: HTTPFunction = (url, opts) => {
    /**...*/
};
```

-   만약 몇몇 함수가 같은 타입 시그니쳐를 공유하고 있다면 이를 명명된 타입으로 분리할 수 있다.
-   이미 존재하는 타입을 확장하는 경우 인터섹션 연산자 `&` 를 사용하고, 인터페이스의 경우 `extends` 키워드를 쓰자.

```ts
interface State {
    userId: string;
    pageTitle: string;
    recentFiles: string[];
    pageContents: string;
}

// TopNavState를 확장하여 State를 구성하였다.
interface TopNavState {
    userId: string;
    pageTitle: string;
    recentFiles: string[];
}

// State의 부분 집합으로서 TopNavState를 구성하였다.
// State를 indexing 하여 속성 타입 (string) 의 중복을 제거할 수 있다.
type TopNavState = {
    userId: State['userId'];
    pageTitle: State['pageTitle'];
    recentFiles: State['recentFiles'];
};

// 이를 Mapped Type 으로 묶어 표현하면 더욱 간결해진다.
type Pick<T, K> = { [key in K]: T[key] };
type TopNavState = {
    [key in 'userId' | 'pageTitle' | 'recentFiles']: State[key];
}; // = Pick<State, keyof State>
```

-   매핑된 타입 (mapped type) 은 배열의 필드를 돌아 요소를 하나씩 꺼내 사용하는 기법을 의미한다.
-   기본 문법은 `{[P in K] : T}` 이며, `Pick<T, K>` 유틸 타입을 사용하여 두 가지 타입을 제네릭 인자로 받아 결과로 반환 받을 수 있다.

```ts
interface SaveAction {
    type: 'save';
}
interface LoadAction {
    type: 'load';
}

type Action = SaveAction | LoadAction;
// type ActionType = 'save' | 'load'; // 타입의 반복. 좋지 않음
type ActionType = Action['type']; // 기존의 타입을 재활용 했기에 좋음.
type ActionType = Pick<Action, 'type'>; // 상단의 타입과 동일하게 작동함.
```

-   인덱싱을 통해서도 별도의 타입 정의 없이 기존의 타입에서 원하는 결과를 얻어낼 수도 있다.

```ts
interface Options {
    width: number;
    height: number;
    color: number;
    label: number;
}

// 굳이 새로운 타입을 이렇게까지 만들어야 할까? 아니다.
interface OptionsUpdate {
    width?: number;
    height?: number;
    color?: number;
    label?: number;
}

// 매핑된 타입과 keyof 키워드를 사용하여 간단하게 타입 제작이 가능하다.
type KeyofOptions = keyof Options; // 'width' | 'height' | 'color' | 'label'
type OptionsUpdate = { [key in keyof Options]?: Options[key] };
```

-   `keyof` 연산자는 타입을 받아 해당 타입의 속성들을 유니온 타입으로 묶어 반환시킨다.
-   `?` 연산자는 해당 속성을 Optional 하게 만든다. 만약 Optional 속성을 제거하고 싶다면 반대로 `-?` 를 사용하자.
-   `readonly` 연산자는 해당 속성을 read-only 하게 만든다. 만약 해당 속성을 제거하고 싶다면 반대로 `-readonly` 를 사용하자.

```ts
const INIT_OPTIONS = {
    width: 640,
    height: 480,
    color: '#00FF00',
    label: 'VGA',
};

type initOptions = typeof INIT_OPTIONS;
```

-   값의 형태를 가진 타입, 즉 리터럴 타입을 그대로 정의하고 싶다면 `typeof` 연산자를 사용해라.
-   `typeof` 는 타입의 영역에서 사용될 경우 해당 값을 그대로 리터럴 타입으로서 변환한 후 적용시킨다.

### ✏️ 제네릭 타입을 제한하는 방법

```ts
interface Name {
    first: string;
    last: string;
}

type DancingDuo<T extends Name> = [T, T]; // T는 Name의 부분집합

// 적절한 타입을 대입했기에 통과되었다.
const couple1: DancingDuo<Name> = [
    { first: 'Fred', last: 'Astaire' },
    { first: 'Ginger', last: 'Rogers' },
];

// 매개변수 T 는 Name의 부분집합인데, last 속성이 없어 오류를 발생시킨다.
const couple2: DancingDuo<{ first: string }> = [
    { first: 'Fred', last: 'Astaire' },
    { first: 'Ginger', last: 'Rogers' },
];
```

-   제네릭 타입 내의 매개변수를 제한할 수 있는 방법은 `extends` 키워드를 사용하는 것이다.
-   `extends` 키워드를 사용하면 제네릭 매개변수가 특정 타입을 확장한다고 선언하는 것과 같다.
-   이외에도 표준 라이브러리에서 지원하는 `Pick`, `Omit`, `Partial` 같은 유틸 타입도 사용하는 것이 좋다.

# ✒️ 동적 데이터에 인덱스 시그니쳐 사용하기

### ✏️ 인덱스 시그니쳐로 유연하게 매핑하기

-   JS의 객체는 문자열 키를 타입의 값과 관계 없이 그대로 매핑하지만, TS에서는 인덱스 시그니쳐를 명시하여 유연한 매핑이 가능하다.

```ts
type Rocket = { [prop: string]: string };
const rocket: Rocket = {
    name: 'Falcon 9',
    variant: 'v1.0',
    thrust: '4,940 kN',
};
```

-   여기서 나오는 `[props: string]` 이 바로 **인덱스 시그니쳐** 이며, 이는 아래와 같은 뜻을 담고 있다.

    -   키의 이름 : 키의 위치만 표시하는 용도이며 타입 체커에서는 사용하지 않는다.
    -   키의 타입 : `string`, `number`, `symbol` 의 조합이어야 한다. 보통은 `string` 을 쓴다.
    -   값의 타입 : 어떤 타입이던 허용된다.

### ✏️ 인덱스 시그니쳐의 단점

```ts
type Rocket = { [prop: string]: string };
const rocket: Rocket = {
    mame: 'Falcon 9', // name 이 아닌 mama 이어도 허용된다.
    variant: 'v1.0',
    thrust: '4,940 kN',
};
```

-   하지만 인덱스 시그니쳐를 사용하면 잘못된 키를 포함할 수 있으며, 특정 키가 필요하지 않는다는 단점을 가진다.
-   또한 키마다 다른 타입을 가질 수도 없으며, 일부의 경우에는 자동 완성 기능이 동작하지 않을 수 있다.
-   인덱스 시그니쳐는 이러한 특성 때문에 주로 **동적으로 변화하는 데이터** 를 표현할 때 사용된다.

```ts
// 정확하긴 하지만 너무 길어서 사용하기가 번거롭다.
type Row3 =
    | { a: number }
    | { a: number; b: number }
    | { a: number; b: number; c: number }
    | { a: number; b: number; c: number; d: number };

// Record 유틸 함수를 사용하면 간단하게 표현이 가능하다.
type Row3 = Record<'a' | 'b' | 'c' | 'd', number>;

// 매핑된 타입을 사용하는 것도 가능하다.
type Row3 = {
    [key in 'a' | 'b' | 'c' | 'd']: number;
};
```

-   결론적으로 **인덱스 시그니쳐** 방식은 지나치게 유연하다. 따라서 이를 보완해야 할 필요가 있다.
-   특정 타입에 가능한 필드가 **제한되어 있다면** 인덱스 시그니쳐 대신 `Record` 유틸 함수나 매핑된 타입을 쓰자.

# ✒️ Number 인덱스 시그니쳐 대신, Array나 Tuple, ArrayLike를 쓰자.

### ✏️ JS 에서 객체의 키는 무조건 문제열이다.

```js
const obj = {
    1: 2,
    3: 4, // 두 속성은 모두 문자열로 변환된다.
};
const arr = [1, 2, 3, 4];
arr['1']; // 정상, 배열도 객체이기에 index 속성으로 접근이 가능하다. (TS에서는 불가)
```

-   JS 에서는 객체의 키 값에 무조건 문자열만 올 수 있으며, 다른 값이 들어올 경우 문자열로 변환된다.
-   JS에서 객체와 관련된 여러 혼선을 미연에 방지하기 위해 TS는 객체의 숫자 키를 허용하고, 이는 **문자열 키와 다르다고** 인식한다.

```ts
// TS 에서는 숫자 키를 허용한다. Array 에 대한 타입 선언에 쓰인다.
interface Array<T> {
    [n: number]: T;
}

// 만약 키 타입이 number 지만 배열과 비슷한 형태를 쓸 경우, ArrayLike를 쓰자.
// Array 타입으로 지정될 경우 사용하지 않을 push, concat 같은 속성도 불러오게 된다.
function checkedAccess<T>(xs: ArrayLike<T>, i: number): T {
    if (i<xs.length>) return xs[i];
    throw new Error('OutOfIndexRangeError');
}
```

-   만약 `string` 타입 대신 `number` 를 타입의 인덱스 시그니쳐로 사용할 경우 TS는 `Array` 혹은 `Tuple` 타입을 대신 사용할 것이다.
-   왜냐면 TS는 number 타입의 속성을 별도로 구분하므로, 숫자 속성이 **특별한 의미를 지닌다는 오해** 를 부를 수 있다. 즉 해당 타입을 배열로 추론할 수 있다.
-   어떤 길이를 가지는 배열과 비슷한 형태의 **튜플** 을 만들고 싶다면 `ArrayLike` 타입을 사용하자.

# ✒️ 변경 관련된 오류를 방지하기 위해 readonly 사용하기

### ✏️ readonly 속성이 필요한 이유

```ts
function printTriangles(n: number) {
    const nums = [];
    for (let i = 0; i < n; i++) {
        nums.push(i);
        console.log(arraySum(nums));
    }
}

// 아래와 같이 함수를 정의하면 요소의 합을 구할 수 있다.
// 하지만 계산이 끝나면 원래 배열이 비게 된다는 문제가 발생한다.
function arraySum(arr: number[]) {
    let sum = 0,
        num;
    while ((num = arr.pop()) !== undefined) [(sum += num)];
    return sum;
}
```

-   `readonly` 접근 제어자는 요소를 수정할 수 없게끔 막는 역할을 한다.
-   배열의 경우 요소를 수정할 수 없고, length를 변경할 수 없으며, 배열을 수정하는 어
    떠한 메소드의 호출도 막는다.

```ts
// 배열을 수정하지 않으면서, 반복을 통해 요소의 합을 구할 수 있다.
function arraySum(arr: readonly number[]) {
    let sum = 0;
    for (const num of arr) {
        sum += num;
    }
    return sum;
}
```

-   JS 에서는 명시적인 언급이 없는 한, 함수가 매개변수를 변경하지 않는다고 가정하지만 이것은 타입 체크에 문제를 일으킬 수 있다.
-   따라서 명시적으로 `readonly` 접근 제어자를 사용하여 변경하지 않음을 기술하는 것이 더욱 좋다.

```ts
type T = { readonly inner: { x: number } };
const o: T = { inner: { x: 0 } };

// 허용됨, readonly 는 오직 inner 속성에만 해당되기 때문. 내부의 x는 아님.
o.x = 1;
```

-   하지만 `readonly` 는 **얕게 동작하기 때문에** 이를 항상 유의해야 한다.
-   `ts-essentials` 라이브러리의 `DeepReadonly` 제네릭으로 이를 해결할 수는 있다.

# ✒️ 매핑된 타입을 사용하여 값을 동기화하기

### ✏️ readonly 속성이 필요한 이유

```ts
interface ScatterProps {
    xs: number[];
    ys: number[];

    xRange: [number, number];
    yRange: [number, number];
    color: string;

    onClick: (x: number, y: number, index: number) => void;
}

// 보수적 접근법 : 정확하게 동작하나 너무 자주 그려질 가능성이 있음.
function shouldUpdate(oldProps: ScatterProps, newProps: ScatterProps) {
    let k: keyof ScatterProps;
    for (k in oldProps) {
        if (oldProps[k] !== newProps[k]) {
            if (k !== 'onClick') return true;
        }
    }
}

// 매핑된 타입을 통해 객체를 생성하고, 이를 활용한다.
const REQUIRES_UPDATE: { [key in keyof ScatterProps]: boolean }; = {
    xs: true,
    ys: true,
    xRange: true,
    yRange: true,
    color: true,
    onClick: false
}

// 다음과 같이 as 키워드를 사용하여 re-mapped types 기법을 활용할 수도 있다.
type RequireUpdateOptions = { [key in keyof ScatterProps as key extends 'onClick' ? never : key]: true };

function shouldUpdate(oldProps: ScatterProps, newProps: ScatterProps) {
    let k: keyof ScatterProps;
    for (k in oldProps) {
        // 반드시 같아야 하는 속성을 객체로서 정의하여 매핑된 타입을 적용하였다.
        if (oldProps[k] !== newProps[k] && REQUIRES_UPDATE[k]) {
            return true;
        }
    }
}
```

-   매핑된 타입을 사용해서 관련된 값과 타입을 **동기화** 할 수 있으며, 새로운 속성 추가 시 선택을 강제할 수도 있다.
