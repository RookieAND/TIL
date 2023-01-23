# ✒️ Iterable? Iterator? 이것들은 대체 뭘까?

#### Iterable

1. 정의 및 특징

-   ES6에서 새롭게 규정된 것으로 "배열을 일반화한 객체" 를 의미한다.
-   JS에서는 Iteration Protocol 을 통해 해당 객체가 Iterable한지를 평가한다.
-   Iterable 한 객체는 항상 `for..of` 반복문을 통해 컬렉션을 순회할 수 있다.
-   대표적으로 Iterable한 객체는 Array, String, Set 이 있다.

> Iteration Protocol?

-   해당 객체가 Iterable 한지를 판별하기 위한 일련의 기준.
-   객체 내에 `[Symbol.iterator] (= @@iterator)` 메소드가 존재해야 한다.
-   `[Symbol.iterator]` 메소드는 반드시 **Iterator** 객체를 반환해야 한다.

#### Iterator

-   1. 정의

    -   Itrable 한 객체가 반복하면서 어떠한 값을 반환할지를 결정하는 객체이다.

-   2. 조건

    -   Iterator 객체 내에는 반드시 `next` 메서드가 존재해야 한다.
    -   `next` 메소드의 경우 반드시 `IteratorResult` 객체를 반환해야 한다.
    -   `IteratorResult` 객체는 `done: boolean` 과 `value: any` 속성을 가진다.
    -   done 속성이 `true` 라면 반복이 끝났음을 의미하여, 이후 `false` 로 전환될 수 없다.

#### 한번 Iterable한 객체를 구현해보자.

-   Iterable한 객체가 `for..of` 구문에 의해 반복이 진행된다면, 아래의 매커니즘을 따른다.

```
1. `for..of` 구문은 시작과 동시에 `Symbol.iterator` 메서드를 호출하여 `iterator` 객체를 받는다.
2. 이후 `for..of` 구문은 `iterator` 에서 반환된 객체를 대상으로만 동작한다.
3. `for..of` 구문에서 다음으로 올 값이 필요하다면, `next()` 메소드를 호출한다.
4. 이후 `done` 속성에 true 가 올 때까지 반복을 진행하며, 반복이 끝났다면 구문을 종료한다.
```

-   하단의 예제는 Python의 `range` 함수를 JS에서 구현한 코드이다.

```javascript
// 함수 range는 반드시 iterable 한 객체를 반환하도록 함.
const range = (from, to) => {
    return {
        from,
        to,
        // [Symbol.iterator] 메소드는 Iterator 객체를 반환함.
        [Symbol.iterator]() {
            return {
                current: this.from,
                end: this.to,
                // Iterator 객체 내에는 반드시 next 메소드가 존재해야 함.
                // next 메소드의 경우 반드시 IteratorResult 객체를 리턴해야 함.
                next() {
                    while (this.current < this.end) {
                        return { value: this.current++, done: false };
                    }
                    // 반복이 끝났을 경우 done 속성의 값은 true, value는 생략 가능.
                    return { done: true };
                },
            };
        },
    };
};

// 실행 시 1 부터 10 사이의 자연수가 여러 줄에 걸쳐 출력됨.
for (const num of range(1, 11)) {
    console.log(num);
}
```

#### Well - formed Iterable

-   Iterator면서 동시에 Iterable한 객체를 Well - formed Iterable 라고 한다.
-   즉, `iter[Symbol.iterator] === iter` 인 경우를 Well - formed Iterable 라고 한다.
-   Well - formed Iterable의 장점은 기존에 비해 코드의 길이가 훨씬 짧아진다는 점이다.
-   또한 Iterable 객체가 현재 자신이 어디까지 반복을 진행했는지도 기억할 수 있다.

```javascript
const range = (from, to) => {
    return {
        from,
        to,
        // 현재 반복이 어디까지 진행되었는지를 저장하는 프로퍼티 current.
        current: from,
        // Well-formed Iterable 객체이므로, 내부에 next 메소드를 지원해야 함.
        // 또한 next 메소드는 반드시 IteratorResult 객체를 반환해야 함.
        next() {
            while (this.current < this.to) {
                return { value: this.current++, done: false };
            }
            return { done: true };
        },

        // iterable 객체에 Symbol.iterator 메서드가 존재하며 자기 자신을 반환한다.
        // 왜냐면 iterable 객체의 경우 iterator가 되기 위한 조건을 모두 갖추었기 때문이다.
        [Symbol.iterator]() {
            return this;
        },
    };
};

const rangeObj = range(1, 11);
console.log(rangeObj.next()); // {value: 1, done: false}
console.log(rangeObj.next()); // {value: 2, done: false}
console.log(rangeObj.next()); // {value: 3, done: false}

// 앞에서 next 메서드를 호출했기 때문에, 반복 시 4부터 시작된다.
for (const num of rangeObj) {
    console.log(num); // 4, 5, ... 10
}
```

-   Itrable 객체를 구현할 경우 **제너레이터** 를 활용하여 구현의 번거로움을 줄일 수 있다.
-   제너레이터 함수를 실행할 경우 반환되는 객체의 경우 Well - formed Iterable로 평가된다.

```javascript
function* iterator(numbers) {
    for (const num of numbers) {
        yield num;
    }
}

const iterable = iterator([...Array(10).keys()]);
for (const number of iterable) {
    console.log(number);
}
```
