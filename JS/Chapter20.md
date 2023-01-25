# ✒️ ES2020

### Rest, Spread Operator in Object

-   객체의 경우에도 내부의 속성 값을 스프레드 연산자를 통해 확장시킬 수 있음.

```javascript
const name = {
    firstName: 'Baik',
    lastName: 'Gwangin',
};

const person = { age: 25, ...name };
console.log(person); // {age: 25, firstName: 'Baik', lastName: 'Gwangin'}
```

-   나머지 연산자의 경우에도 객체에서 충분히 사용될 수 있다.

```javascript
const person = {
    major: 'computer engineering',
    age: 25,
    firstName: 'Baik',
    lastName: 'Gwangin',
};

const { firstName, lastName, ...leftObj } = person;
console.log(leftObj); // {major : 'computer engineering', age: 25}
```

### async 이터레이터와 제너레이터

-   비동기 이터레이터를 사용할 경우, 비동기적으로 들어오는 데이터를 필요에 따라 처리할 수 있다.
-   일반적인 iterable 객체에서는 `[Symbol.iterator]` 속성이 쓰이지만, 비동기적인 경우 `[Symbol.asyncIterator]` 속성을 써야 한다.
-   `next()` 메서드는 반드시 Promise를 반환해야 한다.
-   비동기 이터러블 객체를 대상으로 한 반복 작업은 `for await (let item of iterable)` 반복문을 사용해야 한다.
-   단, 전개 구문의 경우 일반적인 이터레이터가 필요하므로 비동기 상황에서는 작동하지 않음

```javascript
let asyncRange = {
    from: 1,
    to: 5,
    [Symbol.asyncIterator]() {
        return {
            current: this.from,
            last: this.to,

            async next() {
                await new Promise((resolve) => setTimeout(resolve, 1000));
                if (this.current <= this.last) {
                    return { done: false, value: this.current++ };
                }
                return { done: true };
            },
        };
    },
};

// asyncRange의 비동기 이터레이터를 반복시켜 1초 간격으로 숫자 출력
(async () => {
    for await (let num of asyncRange) {
        console.log(num); // 1, 2, 3, 4, 5 (1초 간격으로 출력)
    }
})();
```

-   위의 코드를 제네레이터로 수정하면 아래와 같이 된다.

```javascript
async function* asyncRange(from, to) {
    for (let i = from; i <= to; i++) {
        await new Promise((resolve) => setTimeout(resolve, 1000));
        yield i;
    }
}

// asyncRange의 비동기 이터레이터를 반복시켜 1초 간격으로 숫자 출력
(async () => {
    for await (let num of asyncRange(1, 5)) {
        console.log(num); // 1, 2, 3, 4, 5 (1초 간격으로 출력)
    }
})();
```

### Promise.prototype.finally

-   `finally()` 핸들러로 프로미스의 상태와 관계없이 완료되었을 때 호출할 콜백 함수를 등록할 수 있다.
-   `finally` 핸들러는 어떠한 인수도 전달받지 않는다. Promise가 성공적으로 이행되었는지 거부되었는지 판단할 수 없기 때문이다.
-   `finally()` 핸들러 또한 Promise를 반환하므로 Promise Chaining이 가능하지만, 연결된 Promise는 `finally` 이전에 반환된 값을 전달 받는다.

```javascript
const myPromise = new Promise((resolve, reject) => resolve());
myPromise
    .then(() => {
        return 'still working';
    })
    .finally(() => {
        return 'Done!';
    })
    .then(() => {
        return console.log(res); // still working
    });
```

### RegExp New Feature

1. `s` 플래그 추가

-   `s` 플래그는 표현식이 개행 문자를 포함한 모든 문자를 포함하도록 함.

2. 명명된 캡쳐 그룹

-   기존의 정규식의 결과를 사용할 경우에는, 번호가 매겨진 캡쳐 그룹으로 정규식이 일치하는 문자열의 특정 부분을 참조할 수 있다.
-   하지만 자동으로 할당되는 번호만으로는 정규식을 파악하고 리팩터링 하기가 까다롭다.
-   `?<name>` 문법으로 사용이 가능하며 각 이름은 반드시 고유해야 한다. 또한 식별자 생성 규칙을 따라야 한다.
-   명명된 그룹의 경우 매칭 결과를 담은 객체의 `group` 속성으로 접근할 수 있도록 함.

```javascript
let re = /(\d{4})-(\d{2})-(\d{2})/u;

// 결과로 반환된 배열의 인덱스를 보고 그룹을 파악해야 한다. 상당히 어렵다.
let result = re.exec('2015-01-02'); // ['2015-01-02', '2015', '01', '02', index: 0, input: '2015-01-02', groups: undefined]

let reWithGroup = /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/u;
let resultWithGroup = reWithGroup.exec('2022-01-25');

// 명명된 캡쳐 그룹 문법을 사용할 경우, group 속성에 각 그룹에 해당되는 문자열이 담기게 된다.
console.log(resultWithGroup.group); // {year: '2022', month: '01', day: '25'}
```

3. Look-behind Assertion

-   Look-behind Assertion 의 경우, 패턴 앞에 다른 패턴이 있는지에 대한 여부 확인이 가능하다.
-   Positive Assertion은 `(?<=...)` 이며 Negative Assertion의 경우 `(?<!...)` 으로 표시한다.

```javascript
let re = /(?<=\$)\d+(\.\d*)?/;
let dollarCost = '$10.53';

// $ 기호가 숫자 앞에 있으므로 이를 제외한 나머지를 캡쳐하여 반환한다.
let result = re.exec(dollarCost); // "10.53"
```

4. 유니코드 속성 이스케이스

-   `\p{...}`, `\P{...}` 속성을 통해 유니코드 속성 이스케이스 사용이 가능하다.
-   해당 이스케이프의 경우 `u` 플래그가 설정된 정규식에서 사용 가능하다.

```javascript
let re = /\p{Script=Greek}/i;
re.test('π'); // true
```
