# 📖 Introduction

# ✒️ 일관성 있는 별칭 사용하기

### ✏️ 별칭을 사용할 때는 주의하자

```ts
const borough = {name: 'Brooklyn', location: [40.688, -73.979]};
const loc = borough.location;
```

- 상단의 코드에서는 `borough.location` 내의 배열에 `loc` 이라는 새로운 별칭을 추가하였다. 별칭의 값을 변경하면 원래 속성 값에도 영향을 준다.
- 별칭을 남발하게 되면 제어 흐름을 파악하기 어렵고, 이는 곧 디버깅과 코드 구조를 파악하는 것을 어렵게 한다.

```ts
interface Content {
    name: string,
    desc?: string[],
}

function printContent(content: Content) {
    if (content.desc) {
        console.log(content.desc);
    }
}

function printContent(content: Content) {
    const desc = content.desc;
    if (content.desc) {
        console.log(desc[0]); // 'desc' is possibly 'undefined'.
    }
}
```

- `content.desc` 값을 `desc` 라는 식별자에 넣었으나, 속성 체크 과정에서 `desc` 에 대한 정제가 이루어지지 않았으므로 `undefined` 로 추론될 가능성이 있음을 타입 체커에서 지적한다.
- 만약 별칭을 사용할 것이라면 관련된 모든 코드에 이를 사용하도록 하고, 비구조화 문법을 사용해서 일관된 별칭을 가지도록 하는 것이 좋다.
- 함수 호출의 경우 객체 속성의 타입 정제를 무효화할 수 있다. 왜냐하면 함수를 호출할 때마다 속성 체크를 반복해야 하기 때문이다.

# ✒️ 비동기 코드에는 async 함수를 사용하기

### ✏️ 콜백 지옥에서 벗어나 비동기 처리를 진행하자.

- 과거 JS에서는 비동기 동작을 모델링 하기 위해 콜백 함수를 채용했으며, 이 때문에 여러 콜백 함수가 이어지는 **콜백 지옥** 에 쉽게 맞닥뜨릴 수 밖에 없었다.
- 이후 Promise와 async, await 키워드가 등장하며 비동기 처리를 훨씬 간단하게 처리할 수 있게 되었다.
- TS에서는 콜백보다 Promise가 코드를 작성하기도 쉽고, 타입을 추론하기도 훨씬 쉽다.

### ✏️ async / await을 적극적으로 사용해보자.

```ts
const _cache = {[url: string]: string} = {};
function fetchWithCache(url: string, callback: (text: string) => void) {
    if (url in _cache) {
        callback(_cache[url])
    } else {
        fetchURL(url, text => {
            _cache[url] = text;
            callback(_cache[url])
        })
    }
}

let requestStatus = 'loading' | 'success' | 'error'

// 캐시된 경우 callback 함수는 동기적으로 호출된다.
function getUser(userId: string) {
    fetchWithCache(`/user/${userId}`, profile => {
        requestStatus = 'success';
    });
    requestStatus = 'loading';
}

async function getUser(userId: string) {
    requestStatue = 'loading';
    const profile = await fetchWithCache(`/user/${userId}`);
    requestStatus = 'success';
} 

```

- 콜백 혹은 Promise를 사용할 경우 실수로 반동기 코드를 작성할 수 있다. 하지만 async는 무조건 비동기로 실행되게끔 하기에 이러한 실수를 없앨 수 있다.
- async 함수에서 Promise를 반환하면 또 다른 Promise로 매핑되지 않는다.

# ✒️ 타입 추론에 문맥이 어떻게 사용되는지 이해하기

### ✏️ 타입 추론에 문맥이 어떻게 사용되는가?

```ts
type Language = 'Javascript' | 'Typescript'

function setLanguage(language: Language) {
    /* ... **/
}

let language = 'Javascript'
setLanguage(language); // Argument of type 'string' is not assignable to parameter of type 'Language'.
```

- JS 에서는 코드의 동작과 실행 순서를 바꾸지 않으면서 표현식을 상수로 분리할 수 있다.
- 하지만 TS에서는 할당 시점에 변수의 타입을 추론하므로, JS에서는 허용되었던 방식이 통하지 않을 수 있다.

```ts
const language = 'Javascript' // 'Javascript' 리터럴 타입이 할당됨
setLanguage(language); // 정상


let language: Language = 'Javascript' // Language type에서 할당 가능한 값만 올 수 있음
setLanguage(language); // 정상
```

- 이를 해결하기 위해서는 타입 선언에서 변수 자체에 **할당이 가능한 타입을 제한**하는 것이다. 
- 혹은 `const` 를 사용하여 변수 자체를 **상수로 만들어** 변경이 불가능함을 타입 체커에게 알린다.

### ✏️ 튜플 타입에서 문맥의 소실을 해결하는 방법

```ts
function panTo(where: [number, number]) {
    /* ... **/
}

panTo([10, 20]) // 정상

const loc = [10, 20];
panTo(loc) // Argument of type 'number[]' is not assignable to parameter of type '[number, number]'.
```

- 첫 번째 케이스에서는 `[10, 20]` 값을 TS에서 `[number, number]` 튜플 타입으로 추론하였다.
- 하지만 두 번째 케이스에서는 `loc` 에 할당된 배열을 토대로 해당 변수의 타입을 `number[]` 으로 추론한다.

```ts
const loc: [number, number] = [10, 20];
panTo(loc) // 정상


function panTo(where: readonly [number, number]) {
    /* ... **/
}
const loc = [10, 20] as const; // readonly [10, 20] 으로 추론
panTo(loc) // 정상


const loc = [10, 20, 30] as const; // readonly [10, 20, 30] 으로 추론, 실제 문제는 여기서 발생되었음.
panTo(loc) //Argument of type 'readonly [10, 20, 30]' is not assignable to parameter of type '[number, number]'.
```

- 첫 번째 해결 방안은 타입 체커에게 **타입 선언** 을 제공하여 해당 변수의 타입을 사용자의 의도대로 추론하게끔 한다.
- 두 번째 해결 방안은 **상수 문맥** 을 제공하여, `as const` 키워드를 통해 배열 내부의 값이 상수라는 사실을 타입 체커에게 전달한다.
- 하지만 이렇게 할 경우 `loc` 변수의 타입은 `readonly [10, 20]` 로 추론되는데, 함수의 매개변수에도 `readonly` 타입을 추가하여 해결할 수 있다.
- `as const` 의 경우 타입 정의에 실수가 있다면 정의된 부분이 아닌 **사용된 부분** 에서 오류를 발생시키기에 근본적인 원인을 파악하기가 어렵다.

### ✏️ 객체 및 콜백 사용 시 주의점

```ts
type Language = 'Javascript' | 'Typescript'

interface GovernedLanguage {
    language: Language,
    organization: string;
}

function complain(language: GovernedLanguage) {
    /* ... **/
}

complain({language: 'Typescript', organization: 'Microsoft'});

const ts = {
    language: 'Typescript', // 선언 과정에서 string 으로 추론되었음.
    organization: 'Microsoft',
}

complain(ts); // Argument of type '{ language: string; organization: string; }' is not assignable to parameter of type 'GovernedLanguage'.
```

- `ts` 객체에서 `language` 의 타입은 `string` 으로 추론되었다. 이 경우에도 타입 선언을 추가하거나 `as const` 상수 단언을 통해서 해결이 가능하다.


# ✒️ 유효한 상태만 표현하는 타입 지향하기

### ✏️ 타입을 잘 설계하지 않았을 때의 문제점

```ts
interface State {
    pageText: string;
    isLoading: boolean;
    error?: string;
}

function renderPage(state: State) {
    if (state.error) {
        return 'Error!'
    } else if (state.isLoading) {
        return 'Loading!'
    }
    return `${state.pageText}`
}
```

- 상단의 코드는 분기 조건이 명확하게 명시되지 않았다, isLoading이 true 면서 error 값이 동시에 존재할 경우에는 로딩 중인지, 에러가 났는지를 파악하기 어렵다.
- 이렇게 타입에 대한 정보가 부족하거나 충돌할 수 있게끔 설계할 경우 구현에 대한 어려움이 매우 커진다.
- 따라서 반드시 **유효한 상태만** 표현하는 타입을 지향해야 하며, 코드가 길어지면 표현하기 어렵지만 시간을 절약하고 원활한 유지보수가 가능해진다.


# ✒️ 사용할 때는 너그럽게, 생성할 때는 엄격하게

### ✏️ 함수의 시그니처에 적용해야 하는 규칙

- 함수의 매개변수는 타입의 범위가 넓어도 되지만, 결과를 반환할 때는 타입의 범위가 더 구체적이야 한다.
- 반환 타입의 넓을수록 코드를 분기 처리하고 정확한 결과를 얻기 위한 부수적인 코드를 작성해야 하므로 로직이 필요 이상으로 비대해진다.