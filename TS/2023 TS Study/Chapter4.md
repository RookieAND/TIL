# 📖 Introduction

# ✒️ 추론 가능한 타입 사용으로 효율적인 추론하기

### ✏️ 타입 체커의 추론을 믿자

```ts
// 굳이 number를 명시하지 않아도 잘 추론된다.
let x : number = 12;
let y = 12;

// 객체형 또한 객체 리터럴로 잘 추론된다.
const person: {name: string} = {name: 'baik gwangin'};
const person = {name: 'baik gwangin'};

// string 이라고 추론했으나 리터럴 타입 "y" 가 더 정확하다.
const axis1: string = 'x'; // string ("x" 가 더 정확)
const axis2 = 'y'; // "y"
```

- 타입스크립트는 타입을 위한 언어이지만, 모든 변수에 **타입을 선언** 하는 것은 대단히 비효율적인 스타일이다.
- 타입 체커는 변수의 초기값을 통해 타입을 추론할 수 있으며, 이는 원시 값이 아닌 객체, 배열도 포함된다.

```ts
interface Product {
    id: number;
    name: string;
    price: number;
}

function logProduct(product: Product) {
    // id, name, price의 타입은 자동으로 추론된다.
    const {id, name, price} = product;
    console.log(id, name, price);
}

// Express 의 req, res는 이미 Request, Response로 추론되었다.
app.get("/", (req, res) => { // (req: Request, res: Response) (X)
    res.json({isSuccess: true, result: 'OK'})
})
```

- **비구조화 할당** 의 경우 할당된 변수의 타입을 자동으로 추론해준다.
- 정보가 부족해서 타입 체커가 타입을 판단하기 어려운 경우 (함수의 매개변수) 도 있지만, 그렇지 않은 경우가 더 많다.
- TS 에서 변수의 타입은 **선언부** 에서 결정된다. 이를 활용하여 타입 구문을 최대한 생략하면 코드가 클린해진다.

### ✏️ 가끔 타입을 명시하고 싶다면 언제?

```ts
interface Product {
    id: number;
    name: string;
    price: number;
}

const product = {
    id: 1212122,
    name: 'Kurby',
    price: 20000
}

// product 에 명시된 타입이 없기 때문에 사용 단계에서 오류가 난다.
logProduct(product); // 오류
```

- 객체 리터럴을 정의할 경우, 잉여 속성 체크가 동작하여 선택적 속성이 있는 타입의 오류를 쉽게 잡아낼 수 있다.
- 만약 타입 구문을 제거한다면 잉여 속성 체크가 동작하지 않아 객체의 **선언부** 가 아닌 **사용 단계** 에서 오류가 발생한다.
- 함수의 반환 타입에도 타입을 명시하면 좋다. 보다 직관적으로 반환 값을 추론할 수 있으며 입력과 출력 타입이 무엇인지도 알아야 하기에 함수를 제대로 설계할 수 있다.

# ✒️ 다른 타입에는 다른 변수 사용하기

### ✏️ 변수의 값은 변하나 타입은 변하지 않는다.

```ts
let id = '12-34-56'; // id는 string 으로 추론됨.
id = 123456; // 오류 : number 는 string 에 할당할 수 없습니다.

// 이런 경우 유니온 타입을 지정하여 두 타입 모두 들어올 수 있도록 해준다.
// 하지만 그렇게 좋은 코드는 아니다. 관리하기가 더 까다로워졌기 때문이다.
let id: string | number = '12-34-56';
id = 123456;

// 차라리 다른 역할을 하는 변수 두 개를 만들어 관리함이 옳다.
let id = '12-34-56';
let serial = 123456;
```

- JS 에서는 하나의 변수에 다른 타입의 값을 할당할 수 있으나, TS에서는 불가능하다.
- 타입을 바꿀 수 있는 방법은 **Type Narrowing** 을 통해 타입을 좁히는 것이며, 새로운 타입을 포함하도록 확장하는 것이 아니다.
- 이런 경우 유니온 타입을 지정하기 보다는 **별도의 변수** 를 만들어 혼동을 막는 것이 더 좋다.

# ✒️ 타입 넓히기

### ✏️ 타입 넓히기란?

```ts
interface Vertor3 {
    x: number;
    y: number;
    z: number;
}

function getComponent(vector: Vector3, axis: 'x' | 'y' | 'z') {
    return vector[axis];
}

// 할당되는 시점에 넓히기가 동작하여 string 으로 추론되었음.
// const 였다면 'x' 리터럴 타입이지만, let 이기에 할당 가능한 집합인 string으로 추론됨.
let x = 'x'
let y = {x: 10, y: 20, z: 10};
getComponent(y, x) // 오류 : string 타입의 인수는 'x' | 'y' | 'z' 에 할당 불가.
```


- 런타임에 모든 변수는 유일한 값을 가지지만, 타입 체커가 작동하는 **정적 분석** 시점에서 변수는 **가능한 값들의 집합인 타입** 을 가진다.
- 상수를 사용해서 변수를 초기화하면 타입 체커는 타입을 결정해야 하고, 할당된 단일 값을 기반으로 할당 가능한 값들의 집합을 유추해야 한다.

```ts
// [string, number], (string | number)[], ['x', 1] 등등.. 다양한 경우의 수가 있음
// 이 경우 TS 타입 체커는 (string | number)[] 으로 타입을 추론함.
const mixed = ['x', 1];
```

- 이러한 과정을 **넓히기 (widening)** 라고 한다. 그러나 넓히기를 통해 추론된 타입은 항상 사용자의 의도와는 맞지 않는다.

### ✏️ const로 타입 넓히기를 제어해보자

```ts
// 앞선 예제에서 x 를 const 로 선언한다면 'x' 리터럴 타입으로 추론됨, 오류 해결
const x = 'x'
let y = {x: 10, y: 20, z: 10};
getComponent(y, x) // 정상

// 하지만 객체나 배열의 경우에는 const 로도 해결이 불가능하다.
// 여기서 v 는 {x: number} 객체 리터럴 타입으로 추론되었다.
const v = {
    x: 1,
}

v.x = 'Gwangin' // 오류 : string은 number 타입에 할당시킬 수 없습니다.
```


- `const` 키워드를 사용하여 넓히기를 제어할 수 있다. `let` 보다는 확실히 좁은 타입으로 추론하기 때문이다.
- 하지만 객체의 경우에는 문제가 해결되지 않는다. 이를 해결하기 위해서는 TS의 **기본 동작** 을 재정의 해야 한다.

### ✏️ TS의 기본 동작을 재정의하는 방법

```ts
// v 객체의 x 속성에 유니온 타입으로 명시적 타입 지정 진행.
const v: {x: 1 | 3 | 5} = {
    x: 1,
}

// 함수의 매개변수와 반환 값에 타입을 명시함으로서 타입 체커에게 정보 제공
function getComponent(vector: Vector3, axis: 'x' | 'y' | 'z'): number {
    return vector[axis];
}

// as const 로 선언할 경우 최대한 좁혀지기 때문에 문제가 된다.
const a1 = [1, 2, 3]; // number[]
const a2 = [1, 2, 3] as const; // readonly [1, 2, 3]
```


- 첫번째로는 **명시적 타입 구문** 을 제공한다. 초기 선언부에 타입을 명시하여 타입 추론을 의도대로 진행하도록 한다.
- 두번째로는 타입 체커에 추가적인 문맥을 제공하는 것이다. 예를 들면 함수의 리턴 타입이나 매개변수의 타입을 지정하는 것이다.
- 마지막으로 `const` 단언문을 사용하지 않아야 한다. 이 경우 타입 체커는 최대한 좁은 타입으로 추론하기 때문에 넓히기가 동작하지 않는다.

# ✒️ 타입 좁히기

### ✏️ 타입 좁히기란?

```ts
function checkItsType(x: unknown) {
    if (typeof x === 'string') {
        console.log('x is string'); // 여기서 x는 string 으로 추론되었다.
    }
}

// 잘못된 null 체크, null은 JS에서 객체로 처리되기에 타입이 좁혀지지 않았다.
const el = document.getElementById('foo'); // HTMLElement | null
if (typeof el === 'object') {
    console.log(el) // 아직도 HTMLElement | null 로 추론된다.
}
```

- 타입 체커가 **넓은 타입으로부터 좁은 타입으로** 타입을 추론해나가는 과정을 **타입 좁히기 (Type Narrowing)** 라고 한다.
- 단, 타입을 좁히는 과정에서 실수가 나올 수 있으니 본인이 올바른 방법으로 타입을 좁혔는지 체크해야 한다.

### ✏️ 명시적 태그를 활용해 타입 좁히기

```ts
interface UploadEvent {type: 'upload'; filename: string; }
interface DownloadEvent {type: 'download'; filename: string; }

// 태그된 유니온을 활용하여 타입을 명확히 좁혀나가는 방식.
type Event = UploadEvent | DownloadEvent;
function handleEvent(e: AppEvent) {
    if (e.type === 'upload') {
        console.log(e) // 타입이 UploadEvent 로 추론되었음.
    } else {
        console.log(e) // 타입이 DownloadEvent 로 추론되었음.
    }
}


// 함수의 반환이 true 인 경우, is 키워드에 의해 el을 HTMLInputElement 타입으로 좁힌다.
// is 키워드는 커스텀 함수의 return 값이 True 인 경우 뒤의 타입으로 추론하라는 의미를 가진다.
function isHTMLInputElement(el: HTMLElement): el is HTMLInputElement {
    return 'value' in el;
}

const el = document.getElementById('foo'); 
if (isHTMLInputElement(el)) {
    console.log(el) // 타입이 좁혀져 HTMLInputElement 으로 추론되었음.
}
```

- **태그된 유니온** 을 활용하여 명시적으로 타입을 지정하고 이후 타입 좁히기를 진행할 수 있다.
- 또한 **사용자 정의 타입 가드** 를 통해서 타입 식별을 위한 커스텀 함수를 도입할 수도 있다.


# ✒️ 한꺼번에 객체 생성하기

### ✏️ 객체를 생성할 때는 한번에 생성하자.

```ts
// pt의 타입은 빈 객체 {} 로 추론되었음.
const pt = {}; 
pt.x = 1; // 오류 : 'x' 속성은 {} 에 없습니다.

// pt 의 타입은 {x : number} 로 추론되었음.
const pt = { 
    x: 1;
}
pt.x = 2; // 이후 속성을 변경해도 오류가 발생하지 않음.
```

- 객체를 생성할 때는 속성을 하나씩 추가하지 말고 **일괄적으로 추가**하여 타입 추론을 유리하게 만들자.

```ts
interface Point {
    x: number;
}

// pt의 타입은 빈 객체 {} 이나, 단언에 의해 Point로 추론되었음.
const pt = {} as Point; 
pt.x = 1; // 정상
```

- 객체를 꼭 나눠서 만들어야 한다면 `as` 키워드를 통한 타입 단언으로 타입 체커를 통과할 수는 있다.

```ts
const nameTitle = {name: 'Baik'}
const birthDate = {year: 1999, month: 1, day: 26};

// Person 은 {name: string, year: number, month: number, day: number} 로 추론된다.
const Person = {...nameTitle, ...birthDate};
```

- 여러 객체를 모아 하나의 큰 객체를 생성할 때도 **Spread 연산자** 를 사용하여 한번에 객체를 생성하자.
- 만약 선택적 필드 (옵셔널) 방식으로 생성하려면 헬퍼 함수를 사용하여 부분적으로 적용하자.
