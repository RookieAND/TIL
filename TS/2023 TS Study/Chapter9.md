# 📖 Introduction

# ✒️ 정보를 감추는 목적으로 private 사용하지 않기

### ✏️ JS 에서는 private 속성을 지원하지 않는다

- JS 에서는 private 클래스 속성을 지원하지 않기 때문에 언더바를 붙여서 암묵적으로 내부 속성처럼 사용하도록 하는 것이 관례였다.
- TS 에서는 protected, private, public 접근 제어자를 사용할 수 있기 때문에 이들이 런타임 환경에서까지 영향을 미칠 수 있을 거라고 기대하는 경우가 많다.
- 하지만 TS 는 JS로 변환됨을 주의하자, JS로 변환된 코드는 접근 제어자가 모두 사라진 상태이기 때문에 런타임 환경에서는 private 속성에도 언제든 접근할 수 있다.

```ts
class PasswordChecker {
  checkPassword: (password: string) => boolean;
  constructor(passwordHash: number) {
    this.checkPassword = (password: string) => {
      return hash(password) === passwordHash;
    };
  }
}
```

### ✏️ 차라리 Closure를 사용해서 은닉화를 하자.

- 정보의 은닉화를 위해 private 접근 제어자를 맹신하지 말자. 차라리 closure를 사용해서 관리하는 것이 더 낫다.
- 상단의 코드는 PasswordChecker 의 생성자 외부에서 passwordHash 에 접근할 수 없기 때문에 정보를 은닉할 수 있게 되었다.
- 하지만 passwordHash는 생성자 외부에서 접근할 수 없기에 이에 접근 가능한 메서드가 생성자 내부에 정의되어야 한다는 것이다.
- 그리고 이렇게 생성자 내부에 메서드를 정의할 경우, 인스턴스를 생성할 때마다 새로운 메서드가 생성되므로 메모리 낭비로 이어질 수 있다.
- 또한 동일한 클래스로부터 생성된 인스턴스라 하더라도, 상호 간의 비공개 데이터에 접근하는 것이 불가능하기 때문에 불편함이 따른다.

### ✏️ `#` 접두사를 붙여서 비공개 필드를 만들어버리자.

```ts
class PasswordChecker {
  #passwordHash: number;
  constructor(passwordHash: number) {
    this.#passwordHash = passwordHash;
  }

  checkPassword(password: string) {
    return hash(password) === passwordHash;
  }
}
```

- 또 하나의 선택지로 현재 표준화가 진행중인 (이미 되었다) 비공개 필드 기능을 사용할 수도 있다. 접두사로 `#` 를 붙여서 타입 체크와 런타임 환경에서 비공개로 만든다.

# ✒️ 소스맵을 사용하여 타입스크립트 디버깅하기

- 디버거는 런타임 환경에서 동작하며, 현재 동작하는 코드가 어떤 과정을 거쳐 만들어졌는지 알지 못한다.
- 그리고 타입스크립트는 런타임 이전에 전처리기에 의해 자바스크립트로 변환되고, 디버거는 이 코드를 읽기 때문에 디버깅을 하기 매우 어렵다.
- 디버깅 문제를 해결하기 위해 브라우저 제조사들은 서로 협력해서 소스맵이라는 것을 만들었다. 소스맵은 변환된 코드와 위치를 원본 코드의 위치와 심벌들로 매핑한다.

# ✒️ 모던 자바스크립트로 작성하기

### ✏️ ECMAScript 모듈 사용하기

- ES2015 이전에는 코드를 개별 모듈로 분할하는 표준 방법이 없었다.
- 하지만 지금은 여러 개의 script 태그를 사용하거나, CommonJS의 require 구문을 쓰거나, AMD 의 define 콜백까지 매우 다양한 방식이 존재한다.
- ES2015 이후부터는 import, export를 사용하는 ES 모듈이 표준이 되었다. 만약 마이그레이션 대상이 CJS나 다른 비표준 모듈을 사용한다면 웹펙이나 ts-node 같은 도구가 필요할 수 있다.

### ✏️ 프로토타입 대신 클래스 사용하기

- ES2015 이후 도입된 클래스는 프로토타입의 문법적 설탕이라 불릴 정도로 견고하며 안전하게 설계되었기 때문에, 마이그레이션 하려는 코드에서 단순한 객체를 다룰 때 프로토타입을 사용했다면 class로 변환하는 것이 좋다.

### ✏️ var 대신 const, let 사용하기

- var 키워드의 경우 함수형 스코프를 사용하고 있기 때문에 예상치 못한 상황을 맞이할 가능성이 높으며 호이스팅으로 인해 변수를 초기화 하기 전에 접근할 수 있다는 문제가 있다.

### ✏️ 그 외 최신 자바스크립트 문법으로 변환하는 방법

- `for 문` 혹은 `for - in` 구문을 `for - of` 문이나 배열 메서드를 사용하여 변환해주는 것이 좋다.
- 함수 표현식보다는 화살표 함수를 사용하면 코드가 훨씬 직관적으로 변하며 간결해지기 때문에 좋다.
- 단축 객체 표현, 그리고 구조 분해 할당을 사용하여 객체 내부의 속성에 접근하는 코드를 훨씬 간결하게 사용할 수 있고, default parameter를 사용하여 기본값을 지정할 수도 있다. 함수 매개변수 또한 기본값을 사용할 수 있다.
- 저수준 Promise나 콜백 대신 async / await 를 사용하므로서 코드를 간결하게 줄이고, 타입 정보를 전달하여 비동기 코드를 동기적으로 작성할 수 있다.
- 연관 배열에 객체 대신 Map 과 Set 을 사용하여 훨씬 효율적으로 데이터를 관리할 수 있다.
- 타입스크립트에 `use strict` 키워드를 사용한다 해도 TS에서 수행되는 안전성 검사가 더 엄격한 체크를 하기 때문에 무의미하다.

# ✒️ TS 도입 전에 @ts-check와 JSDoc으로 시험해보기

### ✏️ @ts-check 를 사용하여 사전 점검하기

- `@ts-check` 지시자를 사용할 경우, TS 마이그레이션 시 어떤 문제가 발생하는지 미리 시험해볼 수 있다.
- `@ts-check` 지시자를 사용하여 타입 체커가 파일을 분석하고, 발견된 오류를 보고하도록 지시할 수 있다. 하지만 매우 느슨한 수준으로 타입 체크를 시행하기 때문에 주의해야 한다.
- 선언되지 않은 전역 변수, 알 수 없는 라이브러리, DOM 문제, 부정확한 JSDoc 를 잡아낼 수 있다.

### ✏️ JSDoc 주석에 너무 공들이지 말자

- JSDoc 주석의 경우 중간 단계이기 때문에 공들여서 작성할 이유는 없다. 타입스크립트에서 타입을 명확히 다 지정해준다.
- JSDoc 는 변경 사항이 생길때마다 수동으로 변경해줘야 하지만 타입 체커는 그렇지 않기 때문에 나중에는 불필요해진다.

# ✒️ allowJS로 타입스크립트와 자바스크립트 같이 사용하기

### ✏️ allowJS로 TS, JS 를 동시에 쓸 수 있다.

- `tsconfig.json` 내 allowJS 옵션을 킴으로서 타입스크립트와 자바스크립트를 동시에 사용할 수 있고 import 도 가능하게 할 수 있다.
- 자바스크립트 파일의 경우 문법 오류 외에는 다른 오류, 타입이나 그 외 타입스크립트와 관련된 오류는 뜨지 않는다.

### ✏️ 빌드 체인에 적용하기 위한 밑작업이 필요하다.

- 프레임워크 없이 빌드 체인을 직접 구성했을 경우 outDir 옵션을 통해 타입스크립트가 지정된 디렉터리에 자바스크립트 코드를 생성하고, 이를 대상으로 빌드 체인을 실행시키면 된다.
- 그 외 Webpack 이나 Rollup 같은 번들러를 사용했다면 플러그인을 설치하여 간편하게 allowJS 옵션을 적용할 수 있다.

# ✒️ 의존성 관계에 따라 모듈 단위로 전환하기

# ✒️ 마이그레이션의 완성을 위해 noImplicitAny 설정하기

- 프로젝트 전체를 .ts로 설정했다면 noImplicitAny 옵션을 활성화 하여 any 타입을 최대한 걷어내고, 타입 체크가 명확해질 때까지 점검해보는 것이 좋다.
- noImplicitAny 옵션을 적용하기 전에 타입 오류를 점진적으로 수정하고, 타입 체크의 강도를 높혀 나가기 위해서는 팀 내 구성원들이 타입스크립트에 익숙해진 후에 조금씩 높히는 것이 좋다.
