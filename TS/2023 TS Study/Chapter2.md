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

```ts
const x: number = 0; // 값 0 은 타입 number 의 원소이다. 즉 number 집합에 속한다.
const y: never = 'never'; // never 집합은 공집합이다. 어떠한 원소도 가지지 않는다.
const z:
```

-   이 집합은 타입의 범위라고 부르며, **최상위 타입** 은 `unknown` 이며 **최하위 타입** 은 `never` 이다. `unknown` 은 어떤 값이던 가질 수 있고, `never` 는 어떤 값도 가질 수 없다.

### ✏️ 리터럴 타입과 유니온 타입

-   **리터럴 집합** 은 오직 한 가지 값만 포함하는 타입을 의미한다. 타입스크립트에서는 **Unit Type** 이라고도 부른다.
-   각각의 집합을 여러 개로 묶기 위해서는 **유니온 타입** 을 사용해야 하며, 타입 사이에 `|` 를 넣어 하나로 묶는다.

```ts
type A = 'A'; // 'A' 라는 값만 포함하는 리터럴 타입.
type B = 'B'; // 'B' 라는 값만 포함하는 리터럴 타입.

type AB = A | B; // 'A' 값 또는 'B' 값을 가질 수 있는 유니온 타입.
```
