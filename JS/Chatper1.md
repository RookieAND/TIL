# ✒️ var, const, let

#### 1. var

-   var 키워드로 선언된 변수는 **Functional Scope**에 종속된다.
-   따라서 함수 내에서 선언된 var 의 경우, 함수를 벗어날 경우 스코프를 벗어나 사용이 불가능해진다.
-   var 키워드로 선언된 변수는 언제든지 재선언이 가능하다.

#### 2. let / const

-   let / const 키워드로 선언된 변수의 경우 **Block Scope**에 종속된다.
-   따라서 선언된 블록 밖을 벗어나게 되면 사용이 불가능해진다.
-   let, const 키워드로 선언된 변수의 경우 재선언이 불가능하다.
-   let은 선언 및 초기화 후에 변수의 값을 재할당할 수 있지만, const는 불가하다.

#### 2-1. Object as const

-   const 키워드로 선언된 변수에 객체가 담겼다면, 객체 내부의 속성을 재할당할 수 있다.
-   JS에서 객체 자체를 고정시키는 방법은 `Object.freeze` 메소드를 사용하면 된다. 이는 객체를 동결시켜 immutable 하게 만드는 목적을 가진다.

```javascript
const person = {
    name: 'Alberto',
    age: 25,
};
person.age = 26;
console.log(person.age); // 26

const freezePerson = {
    name: 'Alberto',
    age: 25,
};
Object.freeze(freezePerson);
freezePerson.age = 30;
console.log(freezePerson.age); // 25
```

> 이게 왜 되는가?

-   const 선언은 변수의 식별자가 가리키는 메모리 주소를 변경시키지 않아야 한다.
-   원시 타입의 값이 담긴 변수를 변경할 시, 식별자가 다른 메모리 주소값을 가리키게 되므로 불가하다.
-   하지만 객체의 경우 객체 내부의 값이 변경되더라도 참조된 메모리 주소는 동일하므로 변경이 허용된다.

```javascript
const person = 'Gwangin';
person = 'Not Gwangin'; // TypeError: Assignment to constant variable.

const freezePerson = {
    name: 'Alberto',
    age: 25,
};
freezePerson.age = 30;
console.log(freezePerson.age); // 25
```

# ✒️ TDZ (Temporal Dead Zone)

#### 1. 호이스팅의 원리

-   JS 엔진은 컴파일 단계에서 실행 컨텍스트 내의 Environment Record (환경 레코드) 를 순차적으로 수집한다.
-   Environment Record (환경 레코드) 는 Lexical Environment 내의 식별자 바인딩을 기록하는 객체이다.
-   그중에서도 Declarative Environment Record (선언적 환경 레코드) 에는 함수, 변수, this, super 같은 식별자 바인딩이 저장된다.
-   따라서 JS 엔진은 각 실행 컨텍스트 내의 함수 및 변수의 정보를 컴파일 단계에서 수집한 상태이다.
-   그렇기 때문에 마치 선언부가 **위로 끌어 올려진 것처럼** 보인다. 이러한 현상을 바로 **호이스팅** 이라고 한다.

> 함수 호이스팅과 변수 호이스팅

-   함수 선언문의 경우 컴파일 단계에서 선언된 함수가 메모리에 매핑되며 함수 전체가 메모리에 할당된다.
-   따라서 함수가 선언되지 않은 구역에서도 언제든지 해당 함수를 사용할 수 있게 된다.
-   하지만 함수 표현식을 사용하게 될 경우, 변수는 호이스팅 되더라도 아직 함수를 할당하지 않았기 때문에 사용이 불가하다.

```javascript
console.log(add(1, 10)); // 11
console.log(del(1, 10)); // Uncaught TypeError: del is not a function

function add(a, b) {
    return a + b;
}

var del = function (a, b) {
    return a - b;
};
```

> Variable Environment와 Lexical Environment는 어떤 차이점이 있나?

-   실행 컨텍스트를 구성하면서 생성되는 두 환경에는 Environment Record (환경 식별자) 와 외부 상위 스코프를 참조하는 포인터인 outerReferenceEnvironment (외부 환경 참조) 가 모두 존재한다.
-   Variable Environment의 경우, 실행 컨텍스트가 호출되었을 때의 스냅샷을 유지한다. 즉 초기 상태만을 가진다.
-   하지만 Lexical Environment의 경우 실행 이후의 변경 사항을 전부 반영한다.
-   Variable Environment는 Lexical Environment를 상속하는 관계이기 때문에 둘 다 Lexical Environment 라고 볼 수 있다.

#### 2. 언제 변수를 참조할 수 있는가?

-   변수의 값이 아직 **할당되지 않은** 위치에서, 해당 변수를 참조하려 할 경우 문제가 발생한다.
-   var 키워드의 경우 선언 시 메모리를 할당받으며, 초기 값으로 `undefined` 가 저장된다. 선언과 초기화가 동시에 진행된다.
-   하지만 let, const의 경우 해당 코드의 위치를 의미하는 position만 지정된다. 즉 선언은 되었지만 초기화가 되지 않았다.
-   var 의 경우 선언과 동시에 메모리를 할당 받은 상태이며, let과 const 의 경우 선언은 되었지만 **초기화가 필요한 상태임을 명시**하고 메모리를 할당받지 못한 상태이다.

> 변수와 식별자의 차이점

-   변수는 **데이터가 담길 수 있는 메모리 공간** 을 의미한다.
-   식별자는 데이터를 식별하기 위해 사용되는 이름, 즉 **변수명**을 의미한다.
-   따라서 식별자는 변수의 데이터가 담긴 메모리 주소를 기억한다.

> 실행 컨텍스트의 작동 단계

1. Creation Phase (생성 단계)

-   실행 컨텍스트를 이루는 두 환경의 정의가 이루어짐.
-   This Binding과 Outer Reference (어떤 외부 환경을 참조할지) 를 결정함.
-   Environment Record 에 각 식별자에 대한 메모리가 매핑됨 (어떤 메모리 공간을 가리킬지를 지정)

2. Execution Phase (실행 단계)

-   코드를 위에서부터 순차적으로 읽으며 실행.
-   변수 값이 할당되는 코드가 실행될 경우, Environment Record 에 저장된 식별자 메모리에 값을 수정 또한 할당함.

> JS 인터프리터 내부에서 변수를 처리하는 단계

1. 선언 : 실행 컨텍스트의 Creation Phase (실행 단계) 에서 발생하며, 각 환경에 함수와 변수를 등록한다.

-   Variable Environment 에는 var 키워드로 선언된 변수의 정보가 담긴다.
-   Lexical Environment 에는 const, let 키워드로 선언된 변수의 정보가 담긴다.

2. 초기화 : 등록된 변수에 값을 저장하기 위한 메모리 공간을 확보한다.

-   var 키워드의 경우 메모리를 할당 받은 후 암묵적으로 `undefined` 를 할당시킨다.
-   const, let 키워드의 경우 이 과정에서 아무런 작업도 진행하지 않는다.

3. 할당 : 변수 선언 이후 대입 연산자 (=) 를 사용하여 값을 할당시키는 과정이다.

-   var 키워드의 경우 이미 메모리 공간을 받았으므로 새로운 값을 할당시키면 된다.
-   const, let 키워드의 경우 이 과정에서 메모리 공간을 받고, 새로운 값을 할당시킨다.

#### 3. TDZ의 정의

-   따라서 **TDZ (Temporal Dead Zone)** 란, 변수의 선언 구역부터 초기화 구역 사이의 공간을 의미한다.
-   TDZ에서 변수를 참조할 경우 var 키워드는 `undefined` 를 리턴하며, const와 let의 경우 `ReferenceError` 를 발생시킨다.

```javascript
console.log(name); // undefined
var name = 'Gwangin';

console.log(not_defined); // Uncaught ReferenceError: not_defined is not defined

console.log(name); // Uncaught ReferenceError: Cannot access 'name' before initialization
let name = 'Evan';
```

#### 4. var, let, const를 조화롭게 사용하는 방법

-   var 키워드로 선언된 변수의 경우 TDZ 내부에서도 이를 참조할 수 있으므로 사용을 지양해야 한다.
-   상수의 경우 const 키워드로, 변수의 경우 let 키워드를 사용하는 것이 가장 일반적인 방식이다.
