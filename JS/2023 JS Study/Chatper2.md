# ✒️ Arror Function

#### 1. 화살표 함수란?

-   ES6에서 새롭게 등장한 함수 선언 방식이다.
-   기존의 함수 선언 방식과는 다르게 명시적인 반환을 생략할 수 있다.
-   화살표 함수는 익명 함수이며, 참조할 이름이 필요할 경우 변수에 할당할 수 있다.

```javascript
function oldFunction(name) {
    return `hello ${name}`;
}

const newFunction = (name) => `hello ${name}`;
```

-   매개변수가 1개가 있을 경우에만 소괄호 생략이 가능하며, 그 외에는 생략이 불가능하다.

```javascript
    () => { ... } // 매개변수가 없을 경우
     x => { ... } // 매개변수가 한 개인 경우, 소괄호를 생략할 수 있다.
(x, y) => { ... } // 매개변수가 여러 개인 경우, 소괄호를 생략할 수 없다.
```

-   함수의 몸체가 한 줄의 구문으로 이루어진 경우 중괄호를 생략할 수 있다.

```javascript
(x) => x * x; // 함수 몸체가 한줄의 구문이라면 중괄호를 생략할 수 있으며 암묵적으로 return된다. 위 표현과 동일하다.
() => ({ a: 1 }); // 객체 반환시 소괄호를 사용한다.
```

-   화살표 함수로 선언된 함수는 무조건 상위 스코프 (외부 참조 스코프) 를 this로 바인딩 한다.
-   따라서 bind, call, apply로 this를 변경시킬 수 없다.
-   일반적인 함수 선언의 경우, 호출 방식에 따라 동적으로 this에 바인딩 될 객체가 결정된다.

#### 2. 화살표 함수 사용을 지양해야 하는 경우

1. 객체의 메소드를 정의하는 경우. this는 `Global Object` 에 바인딩 된다.
2. 이벤트 리스터의 콜백함수에 화살표 함수를 사용할 경우, this는 `Global Object` 에 바인딩 된다.
3. 화살표 함수는 생성자 함수로서 사용될 수 없다. prototype 프로퍼티를 가지고 있지 않기 때문이다.
4. 함수에 전달된 인수의 값을 담은 `arguments` 객체에 접근할 경우, 아래 상황에서 문제가 발생한다.

-   화살표 함수 내에서 arguments 객체는 부모 스코프의 값을 상속한다.
-   해당 코드에서 부모 스코프는 전역 스코프기에, 해당 객체가 정의되지 않아 에러를 일으킨다.
-   이를 해결하는 방법은 spread 문법을 사용하여 화살표 함수의 매개변수를 추가하는 것이다.

```javascript
const showWinner = () => {
    const winner = arguments[0];
    console.log(`${winner} was the winner`); // ReferenceError : arguments is not defined
};
showWinner('Usain Bolt', 'Justin Gatlin', 'Asafa Powell');

const showWinner = (...arguments) => {
    const winner = arguments[0];
    console.log(`${winner} was the winner`); // Usain Bolt
};
showWinner('Usain Bolt', 'Justin Gatlin', 'Asafa Powell');
```
