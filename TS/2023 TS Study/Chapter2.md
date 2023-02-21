# 📖 Introduction

# ✒️ TS의 타입 시스템

### ✏️ 편집기를 사용하여 타입 시스템 탐색하기.

-   타입스크립트를 설치하면 **타입스크립트 컴파일러** 와, 단독으로 실행 가능한 **타입스크립트 서버** 를 실행할 수 있다.
-   타입스크립트 또한 **언어 서비스** 를 제공하는데, 여기에는 코드 자동 완성, 도움말, 코드 포맷팅, 검색 및 리팩터링이 모두 포함된다.

### ✏️ Typescript 아키텍쳐

1. 컴파일러

-   언어 변환 기능을 수행하는 Core Typescript Complier를 기반으로 작동하며, **파서, 바인더, 타입 체커, 에미터, 전처리기** 로 구성되어 있다.

    -   파서 (parser) : 소스 코드를 해석하고 토큰화하여 `AST` (Abstract Syntax Tree) 를 생성한다. 이때 `AST` 의 경우 구문 분석으로 생성된 파싱 트리 중에서 불필요한 부분을 제외한 결과물이다.
    -   바인더 (binder) : 인터페이스나 모듈, 혹은 함수에 선언이 존재할 경우 이를 하나의 `Symbol` 로 보고 규칙을 정의한다. 타입 시스템은 바인더를 통해 각 선언들을 추론할 수 있다.
    -   타입 체커 (type checker) : 타입이 선언된 구문을 분석하고 타입이 적절한지에 대한 여부를 확인한다.
    -   에미터 (emitter) : `*.ts` 같은 타입스크립트 파일을 `*.js`, `*.d.ts`, `*.js.map` 유형의 파일로 생성하는 기능을 수행한다.
    -   전처리기 (pre-processer) : 타입스크립트 파일에 선언된 `import` 문이나 `<reference path="">` 같은 외부 호출 선언이 존재할 경우, 참조 가능한 파일을 가져와 정렬된 목록을 생성한다. 컴파일러는 전처리기부터 생성된 목록을 사용하여 파일을 호출하고 컴파일을 수행한다.

2. 언어 서비스

-   타입스크립트의 언어 서비스는 코드를 컴파일하여 도움말이나 코드의 포맷팅, 코드 색상 지정 같은 코드 작성에 필요한 기능을 제공한다.

3. 독립된 서버 (tsserver)

-   `tsserver` 는 독립된 서버이며 타입스크립트 컴파일러와 언어 서비스를 캡슐화하고, JSON 프로토콜을 통해 이를 외부에 공개한다. Node 기반에서 실행이 가능하다.
-   정리하자면 백그라운드 단에서 타입스크립트와 관련된 서비스 (컴파일러, 언어 서비스) 를 가동시키는 독립적인 서버라고 필자는 이해하였다.
-   이러한 서버는 우리가 사용하는 IDE 에 아주 잘 들어맞으며, IDE를 활용해 tsserver 에서 제공하는 언어 서비스를 활용하는 것이다.

4. 트랜스파일러

-   타입스크립트로 작성된 코드를 자바스크립트로 변경해주는 **트랜스 파일러** 다.

### ✏️ 편집기를 활용하여 추론된 타입을 확인할 수 있다.

-   편집기에서 특정 식별자 (심벌) 에 마우스 커서를 올리면 타입스크립트가 해당 타입을 어떻게 판단하고 있는지 확인할 수 있음.
-   **함수의 반환 값** 과 **조건문의 분기에 따른 결과**, **객체 내부의 프로퍼티** 에 대해서도 타입스크립트는 값을 자동으로 추론해준다.
-   하지만 사용자의 의도와 다르게 타입이 추론될 수 있으니, 이 경우에는 반드시 사용자가 **명시적으로 타입을 지정** 해주어야 한다.
-   편집기에서 띄우는 타입 오류를 살펴보는 것 또한 타입스크립트가 어떻게 각각의 타입을 추론하는지 파악하는데 도움이 된다.

```ts
// num 변수의 타입을 지정하지 않았으나 해당 심벌은 number로 추론되었음을 편집기에서 확인 가능.
let num = 10;

// add 함수의 return 값에 대한 타입을 지정하지 않았으나 number로 추론되었음을 확인 가능.
function add(a: number, b: number) {
    return a + b;
}

function sayHi(msg: string | null) {
    if (msg) {
        console.log(msg); // 여기서는 msg 가 string 으로 추론되었음을 확인할 수 있다.
    }
}
```

-   타입스크립트에서 제공하는 언어 서비스는 라이브러리와 라이브러리의 타입 선언을 탐색할 때 큰 도움이 된다.
-   편집기 내에서는 `Go to Definition` 이라는 옵션을 제공하는데, 이를 통해 TS에 포함된 DOM 선언 타입인 `lib.dom.d.ts` 파일로 이동된다.

```ts
// lib.dom.d.ts 내의 fetch 함수에 대한 타입 선언
declare function fetch(input: RequestInfo, init?: RequestInit): Promise<Response>;

type RequestInfo = Request | string;

declare var Request: {
    prototype: Request;
    new (input: RequestInfo, init?: RequestInit): Request;
};
```

-   이렇게 타입스크립트가 동작을 어떻게 모델링하는지 알고 싶다면 타입 선언 파일 `*.d.ts` 를 찾아서 연구하는 것도 좋다.
-   실제 `Axios` 라이브러리에서 제공하는 `index.d.ts` 내부에서도 Axios 와 관련된 선언 타입 정보를 모두 제공하였다.

```ts
export interface AxiosResponse<T = any, D = any> {
    data: T;
    status: number;
    statusText: string;
    headers: AxiosResponseHeaders;
    config: AxiosRequestConfig<D>;
    request?: any;
}

// 이를 직접 활용하여 필자가 작성한 GET 유틸 함수
export async function getAsync<T, D>(url: string, config?: AxiosRequestConfig): APIResult<T> {
    try {
        const response = await API.get<T, AxiosResponse<T, D>, D>(url, {
            responseType: 'json',
            ...config,
        });
        return { isSuccess: true, result: response.data };
    } catch (err) {
        return { isSuccess: false, result: preProcessError(err) };
    }
}
```

# ✒️ 타입 대수 이해하기

### ✏️ 타입은 값들의 집합이다.

-   코드를 실행하기 전, 타입스크립트가 오류를 체크하는 순간에는 **타입** 을 가지고 있으며, 이는 **할당 가능한 값들의 집합** 을 의미한다.
-   할당 가능한 값이라는 의미는 집합의 관점에서 **~ 의 부분 집합** (두 타입 간의 관계) 혹은 **~ 의 원소** (값과 타입의 관계) 라고 볼 수 있다.

> 즉, **어느 범주까지 할당이 가능하니~** 에 대한 여부라고 필자는 이해하였다.

```ts
const x: number = 0; // 값 0 은 타입 number 의 원소이다. 즉 number 집합에 속한다.
const y: never = 'never'; // never 집합은 공집합이다. 어떠한 원소도 가지지 않는다.
const z:
```

![](https://velog.velcdn.com/images/rookieand/post/a820f537-1420-4255-ae58-e56f123dc27f/image.png)

-   이 집합은 타입의 범위라고 부르며, **최상위 타입** 은 `unknown` 이며 **최하위 타입** 은 `never` 이다. `unknown` 은 어떤 값이던 가질 수 있고, `never` 는 어떤 값도 가질 수 없다.

### ✏️ 리터럴 타입과 유니온 타입

-   **리터럴 집합** 은 오직 한 가지 값만 포함하는 타입을 의미한다. 타입스크립트에서는 **Unit Type** 이라고도 부른다.

```ts
type A = 'A'; // 'A' 라는 값만 포함하는 리터럴 타입.
type B = 'B'; // 'B' 라는 값만 포함하는 리터럴 타입.

type AB = A | B; // 'A' 값 또는 'B' 값을 가질 수 있는 유니온 타입.
```

-   각각의 집합을 여러 개로 묶기 위해서는 **유니온 타입** 을 사용해야 하며, 타입 사이에 `|` 를 넣어 하나로 묶는다.
-   유니온 집합은 값 집합들의 합집합을 의미한다. 즉 여러 종류의 값을 모두 할당할 수 있음을 나타낸다.

### ✏️ 집합의 관점에서 타입 체커가 하는 기능

```ts
type C = 'C';

const a: AB = 'A'; // 'A' 형식은 집합 {'A', 'B'} 의 원소다.
const c: AB = 'C'; // 'C' 형식은 AB 에 할당할 수 없다. 집합 {'A', 'B'} 의 원소가 아니기 때문.
```

-   집합의 관점에서 타입 체커는 하나의 집합이 다른 집합의 **부분 집합인지를** 주로 검사한다.
-   상단의 코드에서 값 `A` 는 집합 `{'A', 'B'}` 에 포함되지만 `C` 는 그렇지 않기에 에러를 띄웠다.

### ✏️ 인터섹션 타입과 extends

-   인터섹션 타입의 경우 **두 집합** 이 가진 요소를 모두 포함하는 관계를 의미하며, 타입 사이에 `&` 을 넣어 하나로 묶는다.
-   인터섹션 타입은 두 집합의 **교집합** 을 구하기 위해 사용되며, 두 집합의 요소를 **모두 가지는** 집합인지를 체크한다.

```ts
interface Person {
    name: string;
}

interface LifeSpan {
    birth: Date;
    death?: Date;
}

type PersonSpan = Person & LifeSpan;

// 두 인터페이스 내의 필요 속성을 모두 가졌으므로 정상적으로 타입이 체크되었다.
const ps: PersonSpan = {
    name: 'Baik Gwangin',
    birth: new Date('1999/01/26'),
};
```

-   `Person` 과 `LifeSpan` 인터페이스는 서로 공통된 속성이 없으나, 타입 연산자는 인터페이스의 속성이 아닌 **값의 집합** 에 적용된다.
-   변수 `ps` 에 할당된 객체에는 Person의 속성인 `name` 과 LifeSpan 의 속성인 `birth` 가 모두 있어 정상적으로 타입이 적용된다.
-   **구조적 타이핑 관점** 에 의하면 `name` 과 `birth` 속성이 아닌 다른 속성도 할당될 수 있다. 하지만 TS 에서 지원하는 **잉여 속성 체크** 에서는 이것을 지적하기에 간과할 수 있다.

```ts
interface Person {
    name: string;
}

interface LifeSpan extends Person {
    birth: Date;
    death?: Date;
}

// LifeSpan 인터페이스는 Person 인터페이스에서 확장되었으므로 name 속성을 가져야 한다.
const ps: LifeSpan = {
    name: 'Baik Gwangin',
    birth: new Date('1999/01/26'),
};
```

-   집합의 관점에서 `extends` 키워드는 **~ 의 부분집합** 이라는 의미로 해석이 가능하다.
-   따라서 LifeSpan 타입의 경우 상위 집합인 Person 의 속성인 `name` 속성을 가져야 하고, `birth` 속성까지 가져야 비로소 성립이 된다.

### ✏️ keyof 관련 교차 공식

1.  `keyof (A & B)` = `(keyof A)` | `(keyof B)`
2.  `keyof (A | B)` = `(keyof A)` & `(keyof B)`

-   `keyof` 는 **항상 접근 가능한 유형의 키** 를 반환한다. 따라서 인터섹션 타입의 경우 A 와 B 에 속한 속성을 모두 갖춰야 한다.
-   하지만 유니온 타입의 경우에는 공통된 속성이 없다면 항상 접근 가능한 키가 없어지게 된다. 하단의 예시를 보자.

```ts
interface Person {
    name: string;
}

interface Human {
    name: string;
    birth: Date;
    death?: Date;
}

type K = keyof (Person | Human); // "name"
type I = keyof (Person & Human); // keyof Human : "name" | "birth" | "death"
```

-   유니온 타입의 경우 만약 두 인터페이스 간의 공통된 속성이 없다면 **항상 접근 가능한 유형의 키** 가 없다. 지금은 `name` 이라는 공통 속성이 있지만, 만약 없다면 `never` 타입으로 반환될 것이다.
-   반대로 인터섹션 타입의 경우 두 인터페이스의 속성을 **모두** 가지고 있어야 하므로, 각 인터페이스의 속성을 모두 합친 유니온 타입으로 반환된다.

### ✏️ 할당과 상속의 관점을 전환해보자

```ts
interface Vector1D {
    x: number;
}

interface Vector2D {
    x: number;
    y: number;
}

interface Vector3D {
    x: number;
    y: number;
    z: number;
}
```

-   상단의 코드에서 `Vector3D` 는 `Vector2D` 의 서브타입이고, `Vector2D` 는 `Vector1D` 의 서브타입이다.
-   혹은 `Vector3D` 는 `Vector2D` 를 상속 받았고, `Vector2D` 는 `Vector1D` 를 상속 받았다고 할 수도 있다.

```ts
// 타입 K 는 string 을 상속 받았다. 즉 string 의 부분 집합이다.
function getKey<K extends string>(val: any, key: K) {}

getKey({}, 'x'); // 정상, 'x' 는 string을 상속, 즉 string의 부분집합.
getKey({}, 111); // 오류, 111 는 string을 상속하지 않음, string의 부분집합이 아님.
```

-   `extends` 키워드는 제네릭 타입 내에서 한정자로 쓰이며, 여기서는 **~의 부분 집합** 의 개념으로도 사용된다.
-   string 타입을 상속한다고 생각하면 어렵지만, 단순히 부분 집합을 가진다는 의미로 해석하면 쉽게 이해할 수 있다.

```ts
type StringDate = string | Date;
type StringNumber = string | number;

type Inter = StringDate & StringNumber; // string
```

![](https://velog.velcdn.com/images/rookieand/post/443e5471-6706-4d2a-bf49-20ca9b1e1756/image.PNG)

-   타입이 **엄격한 상속 관계를 맺지 않았다면** 집합으로 관계를 생각하는 것이 더욱 바람직하다. 두 타입이 완전 부분 집합 관계가 아니더라도 범위에 대한 관계는 명확하기 때문이다.
-   `StringDate` 와 `StringNumber` 는 서로 상속 관계가 아니지만 인터섹션을 통한 교집합을 명확히 찾아낼 수 있다.

```ts
const list = [1, 2]; // type : number[]

// number[]' 형식은 '[number, number]' 형식에 할당할 수 없습니다.
// 대상에 2개 요소가 필요하지만, 소스에 더 적게 있을 수 있습니다.
const tuple: [number, number] = list;
```

-   배열과 튜플의 관계에서도 타입이 집합이라는 점을 이용할 수 있다. `number[]` 은 `[number, number]` 타입의 부분 집합이 아니지만 그 반대는 가능하다.

### ✏️ 타입스크립트 용어와 집합 이론 용어 간의 대응 표

![](https://velog.velcdn.com/images/rookieand/post/6da739ae-0a98-49ce-9455-4095cf8653ab/image.PNG)

# ✒️ 타입 공간과 값 공간의 심벌 구분

### ✏️ 타입스크립트의 심벌은 둘 중 한 곳에 있다

-   타입스크립트의 심벌은 타입 공간, 혹은 값 공간 중 한 곳에 존재한다. 심벌은 이름이 같더라도 속하는 공간에 따라 다른 것을 나타낸다.

```ts
// 타입 공간에서의 Cyclinder 는 인터페이스로서의 기능을 한다.
interface Cyclinder {
    radius: number;
    height: number;
}

// 값 공간에서의 Cyclinder 는 함수로서의 기능을 한다.
const Cyclinder = (radius: number, height: number) => ({ radius, height });

function caculateVolume(shape: unknown) {
    if (shape instanceof Cyclinder) {
        return shape.radius; // '{}' 형식에 radius 가 없습니다.
    }
}
```

-   상황에 따라 심벌을 타입으로, 혹은 값으로 쓰일 수 있기 때문에 이로 인한 오류가 발생할 수 있다.
-   `instanceof` 는 JS의 런타임 연산자이고 값에 대한 연산을 진행하기 때문에 타입이 좁혀지지 않았다.
-   타입스크립트 코드에서 타입과 값은 번갈아 나올 수 있고, 주로 타입은 타입 선언 혹은 `as` 연산자를 통한 단언에 나온다.

### ✏️ class, enum 은 값과 타입 둘 다 가능하다.

```ts
class Cyclinder {
    radius = 1;
    height = 1;
}

function caculateVolume(shape: unknown) {
    if (shape instanceof Cyclinder) {
        return shape.radius; // 정상, 클래스는 현재 타입으로 사용되고 있다.
    }
}
```

-   클래스가 타입으로 쓰일 때는 속성과 메서드가 주로 사용되며, 값으로 쓰일 때는 생성자가 사용된다.

### ✏️ typeof 연산자

-   타입의 관점에서, `typeof` 는 값을 읽어 타입스크립트 타입을 반환시킨다.
-   값의 관점에서 `typeof` 는 자바스크립트 런타임의 `typeof` 연산자 가 된다. 따라서 대상 심벌의 런타임 타입을 가리키는 문자열을 반환한다.

```ts
interface Person {
    first: string;
    last: string;
}

const person: Person = { first: 'Baik', last: 'Gwangin' };

type T1 = typeof person; // Person
const v1 = typeof person; // "object"
```

-   타입 `T1` 은 타입의 관점에서 person 변수의 값을 읽어 반환된 타입인 Person 이다.
-   하지만 변수 `v1` 에는 person 이 객체이므로 `"object"` 문자열이 반환되었다.

```ts
class Cyclinder {
    radius = 1;
    height = 1;
}

const v = typeof Cyclinder; // "function"
type T = typeof Cyclinder; // typeof Cyclinder
type T2 = InstanceType<typeof Cyclinder>; // Cyclinder
```

-   클래스는 자바스크립트에서 함수로 구현되기 때문에 값의 관점에서는 `typeof` 의 결과가 `"function"` 으로 나온다.
-   하지만 타입의 관점에서는 현재 Cyclinder 가 인스턴스 타입이 아니기 때문에 **생성자 함수 자체로서** 의 type을 가졌다.
-   이를 인스턴스 타입으로 전환하기 위해서는 `InstanceType` 제네릭을 활용하면 된다.

### ✏️ 속성 접근자는 반드시 대괄호만 사용하자.

```ts
interface Person {
    first: string;
    last: string;
}

const person: Person = { first: 'Baik', last: 'Gwangin' };

// 값의 관점에서는 대괄호를 사용하던, 속성 연산자 (.) 를 사용하던 괜찮다.
const first = person['first']; // person.first

// 타입의 관점에서는 반드시 대괄호를 사용하여 속성에 접근해야 한다.
type First = Person['first']; // string
```

### ✏️ 두 공간 사이에서 다른 의미를 가지는 패턴

1. `this` 키워드

-   값으로 쓰이는 this 는 자바스크립트의 this 키워드와 동일하다.
-   타입에서 쓰이는 this 는 다형성 this 라고 불리는 타입스크립트의 타입인데, 서브 클래스의 메서드 체인을 구현하기 위해 쓰인다.

2. `&`, `|` 연산자

-   값으로 쓰이는 `&`, `|` 연산자는 AND, OR 비트 연산이다.
-   타입으로 쓰이는 `&`, `|` 연산자는 인터섹션과 유니온 타입이다.

3. `const` 키워드

-   값으로 쓰이는 `const` 키워드는 새로운 변수를 선언하기 위해 쓰인다.
-   타입에서 `as const` 키워드는 리터럴, 혹은 리터럴 표현식의 추론된 타입을 변환한다.

4. `extends` 키워드

-   값으로 쓰이는 `extends` 키워드는 서브 클래스를 지정하여 상속 관계를 표현한다.
-   타입에서 `extends` 키워드는 제네릭 타입의 한정자, 혹은 서브 타입을 지정한다.

5. `in` 키워드

-   값으로 쓰이는 `in` 키워드는 `for - in` 구문 혹은 객체의 속성 포함 여부 등에 쓰인다.
-   타입에서 `in` 키워드는 mapped type, 즉 매핑된 키워드에서 쓰인다.

# ✒️ 타입 단언보다는 타입 선언을 사용하기

### ✏️ 변수에 값을 할당하고, 타입을 부여하는 방법

```ts
interface Person {
    name: string;
}

const alice: Person = { name: 'Alice' };
const bob = { name: 'bob' } as Person;
```

-   변수에 값을 할당하고 타입을 부여하는 방법은 **타입 단언 (Type Assertion)** 과 **타입 선언 (Type Declaration)** 이 있다.
-   타입 단언의 경우 타입스크립트가 자체적으로 타입을 추론하더라도 사용자가 단언한 타입으로 간주하기 때문에 되도록이면 타입 선언을 쓰자.
-   또한 강제로 타입을 지정하게 될 경우 특정 객체에 속성을 추가하거나 필요한 속성이 누락되었을 때 이를 잡아내지 못한다.

```ts
// (name: Person) 으로 적을 경우 name을 Person 타입으로 인식, 반환 타입 없음
const people = ['alice', 'bob', 'jan'].map((name): Person => {
    const person = { name };
    return person;
});
```

-   화살표 함수의 반환 타입을 작성할 때는 반드시 소괄호 외부에 작성해야 한다. 그렇지 않으면 매개변수의 타입을 명시하여 **반환 타입이 없다고** 판단하기 때문이다.

### ✏️ 타입 단언을 쓰는 케이스

```ts
const button = document.getElementById('button'); // HTMLElement | null
const button1 = document.getElementById('button')!; // HTMLElement 로 단언

interface Person {
    name: string;
}

const button = document.getElementById('button');
const el = button as Person; // 'HTMLElement | null' 형식을 'Person' 형식으로 변환한 작업은 실수일 수 있습니다. 두 형식이 서로 충분히 겹치지 않기 때문입니다.
```

-   타입 체커가 추론한 타입보다 **사용자가 판단하는 타입이 더욱 정확한 경우**에는 사용이 가능하다.
-   타입스크립트에서는 `!` 를 접미사로 붙이면 해당 값이 `null` 이 아니라는 단언문으로 해석된다.
-   하지만 서로 서브타입이 아닌 두 타입을 강제로 타입 단언하고자 한다면 에러를 일으킨다.
-   이를 해결하고자 한다면 모든 타입의 상위 집합인 `unknown` 을 사용함으로서 해결은 가능하다.

# ✒️ 객체 래퍼 타입 피하기

### ✏️ Wrapping 에 대해 알아보자.

-   자바스크립트에서는 일곱 가지의 원시형 타입이 존재하며, 이들은 불변하며 메서드를 가지지 않는다는 특징이 있다.
-   하지만 string 타입에서는 `replace`, `charAt` 같이 문자열과 연관된 메서드를 자유롭게 사용할 수 있다.

```js
const string = 'String';
string.charAt(3); // 메서드를 사용할 수 있다.
```

-   그 이유는 JS 에서 기본형에 메서드를 사용할 경우, 이를 **Wrapper 객체** 로 래핑한 후 메서드를 호출하기 때문이다.
-   `string` 원시형의 경우 `String` 래핑 객체로 변환되어 메소드를 호출하고, 마지막에 래핑한 객체를 버린다.
-   따라서 메서드 내의 `this` 는 원시 타입의 값이 아닌 래핑된 객체이며, 이는 원시 타입의 값과 거의 동일하게 동작한다.

### ✏️ 래퍼 객체와 기본형은 완전히 같지 않다.

```js
var x = 'hello';
x.language = 'English';
console.log(x.language); // undefined

'hello' === 'hello'; // true
'hello' === new String('hello'); // false
new String('hello') === new String('hello'); // false
```

-   하지만 `string` 기본형과 `String` 래퍼 객체가 완전히 동일하게 동작하는 것은 아니다.
-   우리는 `x` 에 실제로 `language` 속성을 넣었다고 생각했지만, 실제로는 래핑된 객체에 속성을 추가한 뒤 이를 버린 것이다.
-   또한 객체 래퍼의 경우 오직 자기 자신하고만 동일하기에, 다른 래핑 객체와는 절대로 동등할 수가 없다.
-   타입스크립트에서는 **기본형** 과 **객체 래퍼 타입** 을 별도로 모델링 하기 때문에 두 개를 절대로 혼용해서는 안된다.

# ✒️ 잉여 속성 체크의 한계 인지하기

### ✏️ 잉여 속성 체크란?

```ts
interface Room {
    numDoors: number;
    celingHeightFt: number;
}

const r: Room = {
    numDoors: 1,
    celingHeightFt: 10,
    isBooked: true, // '{ numDoors: number; celingHeightFt: number; isBooked: boolean; }' 형식은 'Room' 형식에 할당할 수 없습니다.
};

const r2: Room = r; // 정상, 오류가 발생하지 않는다.
```

-   **잉여 속성 체크** 란 타입이 명시된 변수에 객체 리터럴을 할당할 시, 해당 타입의 **속성이 있는지**, 그리고 **그 외의 속성은 없는지** 확인하는 것이다. (객체에만 적용)
-   **구조적 타이핑 관점** 에서는 변수 `r` 에 `isBooked` 속성이 있어도 오류가 없어야 한다. `Room` 인터페이스에서 요구하는 속성들을 모두 갖추었기 때문이다.
-   이 경우 임시 변수 (`r2`) 를 도입하면 해결이 가능한데, 두 구문의 차이는 **잉여 속성 체크** 의 유무이다. 이는 할당 가능 검사와는 **별도로 작동한다.**

### ✏️ 왜 잉여 속성 체크를 진행하는가?

-   가장 큰 이유는 구조적 타입 시스템에서 발생할 수 있는 **오류** 를 잡기 위해서다.

```ts
interface Options {
    title: string;
    darkMode?: boolean;
}

function createWindow(options: Options) {
    if (options.darkMode) {
        setDarkMode();
    }
}

// 개체 리터럴은 알려진 속성만 지정할 수 있지만 'Options' 형식에 'darkmode'이(가) 없습니다. 'darkMode'을(를) 쓰려고 했습니까?
createWindow({ title: 'Spider Solitaire', darkmode: true });

// 심지어는 이런 케이스도 전부 할당이 가능하다. 모두 `title` 속성을 가지고 있기 때문이다.
const o1: Options = document;
const o1: Options = new HTMLAnchorElement();

// 이 경우에는 오류가 발생한다. darkmode 는 Options 타입에 없어 잉여 속성 체크에 걸렸다.
const o3: Options = { title: 'Ski Free', darkmode: true };
```

-   상단의 코드를 실행하면 런타임 과정에서 오류가 발생하지 않지만, 사용자의 의도대로 **동작하지 않을 수 있다.**
-   현재 `Options` 타입은 범위가 매우 넓기 때문에 구조적 타입 체커로는 이런 종류의 오류를 쉽게 잡기 어렵다. 단순히 `title` 속성만 가지고 있다면 전부 허용해버리기 때문이다.
-   따라서 잉여 속성 체크를 통해 TS의 구조적 본질을 해치지 않고, 객체 리터럴에 **알 수 없는 속성** 을 허용하지 않음으로서 문제점을 방지할 수 있다.

```ts
const o = { title: 'Ski Free', darkmode: true }; // darkmode 라는 잘못된 속성이 기입됨.
const o2: Options = o; // 정상
```

-   하지만 잉여 속성 체크에도 한 가지 허점이 있는데, 바로 **임시 변수** 를 만들어 우회하면 타입 체크가 불가능하다는 것이다.
-   잉여 속성 체크는 오직 객체 리터럴만을 대상으로 하기에, 임시 변수에 대해서는 체크를 진행하지 않는다.

```ts
interface Options {
    title: string;
    [otherOptions: string]: unknown; // 인덱스 시그니쳐 사용
}

const o: Options = { title: 'Ski Free', darkMode: true }; // 정상
```

-   또한 **as 키워드** 를 사용한 타입 단언을 진행한다면 잉여 속성 체크가 진행되지 않는다.
-   만약 체크를 원하지 않는다면, **인덱스 시그니처** 를 사용해서 타입 체커가 추가적인 속성을 예상하도록 할 수 있다.

### ✏️ 공통 속성 체크란?

```ts
interface LineChartOptions {
    logscale?: boolean;
    invertedYAxis: boolean;
    areaChart: boolean;
}

const opts = { logScale: true };
// { logScale: boolean; } 형식에 'LineChartOptions' 형식의 invertedYAxis, areaChart 속성이 없습니다.
const o: LineChartOptions = opts; // 임시 변수를 생성해도 또 체크를 진행함.
```

-   **옵셔널 속성** 이 있는 약한 타입에 대해서도 유사한 체크가 동작한다. 이를 **공통 속성 체크** 라고 한다.
-   구조적 관점에서 `LineChartOptions` 는 모든 타입이 optional 함으로 모든 객체를 포함할 수 있다.
-   하지만 타입 체커는 약한 타입에서 값 타입과 선언 타입에 **공통된 속성이 있는지** 를 확인하는 별도의 체크를 수행한다.
-   잉여 속성 체크와 다른 점은, **약한 타입과 관련된 할당문** 마다 시행된다는 것이다. 즉 임시 변수로 우회할 수 없다.

# ✒️ 함수 표현식에 타입 적용하기

### ✏️ 타입스크립트에서는 함수 표현식이 좋다.

-   JS 에서는 함수 선언문과 함수 표현식을 다르게 인식하지만, 타입스크립트에서는 함수 표현식을 사용하는 것이 좋다.
-   함수의 매개변수부터 반환 값 전부를 별도의 **함수 타입** 으로 선언하여 함수 표현식에 재사용이 가능하기 때문이다.

```ts
type CalculateNumber = (a: number, b: number) => number;
const add: CalculateNumber = (a, b) => a + b;
const sum: CalculateNumber = (a, b) => a - b;
const mul: CalculateNumber = (a, b) => a * b;
const div: CalculateNumber = (a, b) => a / b;
```

-   함수 타입의 선언은 타입을 선언하는 과정에서의 **불필요한 코드의 반복을 줄이고**, 하나의 통일된 타입으로 사용할 수 있다.

```ts
// lib.dom.d.ts
declare function fetch(input: RequestInfo, init?: RequestInit): Promise<Response>;

// fetch 함수를 타입으로 사용하여 각 매개변수와 반환 값이 자동으로 추론되었다.
// 매개변수 input, init 에 별도의 타입을 지정하지 않아도 알아서 추론해준다.
const checkFetch: typeof fetch = async (input, init) => {
    const response = await fetch(input, init);
    if (!response.ok) {
        throw new Error(`Request failed : ${response.status}`);
    }
    return response;
};
```

-   또한 타입 구문은 함수의 반환 타입을 반드시 보장하며, 타입스크립트가 매개변수와 반환값을 자동으로 추론할 수 있게 해준다.
-   따라서 함수의 매개변수 각각에 타입 선언을 해주는 것 보다는 함수 표현식 전체 타입을 정의하는 것이 더욱 간결하다.
