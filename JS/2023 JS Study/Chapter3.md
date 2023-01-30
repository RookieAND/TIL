# ✒️ Default Function Parameter

#### 1. 함수 파라미터 기본 값

-   기본적으로 JS에서는 함수의 인자의 기본값을 `undefined` 로 설정한다. (호이스팅)
-   ES6 이전에는 함수의 인자를 각각 조건문으로 파악하여 값이 할당되지 않은 인자를 파악해야 했다.

```javascript
function getLocadtion(city, country, continent) {
    if (typeof city === undefined) {
        country = 'Milan'
    }

    if (typeof continent === undefined) {
        continent = 'Europe'
    }


    if (typeof country === undefined) {
        continent = 'Italy'
    }
    ...
}
```

-   ES6 부터는 함수의 파라미터에 기본값을 쉽게 지정할 수 있다.

```javascript
function getLocadtion(city = 'Milan', country = 'Italy', continent = 'Europe') {
    ...
}
```

-   기본값을 설정하려는 파라미터의 경우, 항상 그렇지 않은 파라미터들의 이후에 와야 한다.

```javascript
// Correct Case
function getLocadtion(city, country, continent = 'Europe') {
    ...
}

// Wrong Case
function getLocadtion(city = 'Milan', country, continent) {
    ...
}
```

-   만약 실제로 함수에 전달된 인자값들의 정보에 대해서 알고 싶다면, `arguments` 객체를 사용하자.

```javascript
function addNum() {
    var sum = 0; // 합을 저장할 변수 sum을 선언함.
    for (var i = 0; i < arguments.length; i++) {
        // 전달받은 인수의 총 수만큼 반복함.
        sum += arguments[i]; // 전달받은 각각의 인수를 sum에 더함.
    }
    return sum;
}

addNum(1, 2, 3); // 6
addNum(1, 2); // 3
addNum(1); // 1
addNum(); // 0
addNum(1, 2, 3, 4, 5, 6, 7, 8, 9, 10); // 55
```
