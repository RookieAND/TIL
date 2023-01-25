# ✒️ Set

#### 1. Set 이란?

-   자료형과 관계 없이 모든 원시 값과 참조된 객체를 유일하게 저장할 수 있다.
-   만약 중복된 값이나 같은 주소를 참조하는 객체를 넣는다면, 하나를 제외하고 전부 누락시킨다.
-   `NaN` 과 `undefined` 도 Set에 저장할 수 있다.

#### 2. Set의 인스턴스 메서드

1. Set.prototype.add(value)

-   기존의 Set 에 새로운 값이 추가된 Set 객체를 반환한다.

2. Set.prototype.clear(value)

-   Set 객체에 내장되었던 모든 값들을 제거하고, Set을 빈 상태로 만든다.

3. Set.prototype.delete(value)

-   인자로 받은 값이 Set 객체 내부에 있는지를 확인하고 이를 제거한다.
-   만약 값을 성공적으로 제거했다면 `true` 를 반환하고, 아니라면 `false` 를 반환한다.

4. Set.prototype.has(value)

-   인자로 받은 값이 Set 객체 내부에 있는지를 확인하고, 결과를 `boolean` 으로 반환한다.

> https://velog.io/@dev_shu/%EC%99%9C-set.has%EB%8A%94-O1%EC%9D%B8%EA%B0%80

```javascript
let setObj = new Set([1, 2, 3, 4, 5]);

setObj.add(6); // Set { 1, 2, 3, 4, 5, 6 }
setObj.delete(1); // Set { 2, 3, 4, 5, 6 }
setObj.has(0); // false
```

5. Set.prototype.values() / Set.prototype.keys()

-   Set 객체에 요소가 **삽입된 순서대로** 각 요소의 값을 순회할 수 있는 Iterator 객체를 반환한다.
-   따라서 반환받은 Iterator 객체를 `next()` 메서드를 통해 반복시킬 수 있다.

6. Set.prototype.forEach()

-   Set 객체에 요소가 **삽입된 순서대로** 주어진 함수를 실행시킨다.

#### 3. Loop of Set

-   Set은 iterable 한 객체이기에 `for..of` 로 내부의 값을 순회할 수 있다.
-   또한 `next()` 메소드를 통해 직접 객체를 순회하여 결과값을 받을 수 있다.
-   Set 객체에 내장된 `forEach` 함수를 통해서도 내부를 순회할 수 있다.

```javascript
let setObj = new Set([1, 2, 3, 4, 5]);

const setIter = setObj.values();
console.log(setIter.next().value); // 1

for (const num of setObj) {
    console.log(num); // 1, 2, 3, 4, 5
}

setObj.forEach((num) => console.log(num * 2)); // 2, 4, 6, 8, 10
```

# ✒️ Map

-   Map 객체는 `key` 와 `value` 로 이루어진 한 쌍의 데이터를 보관할 수 있다.
-   또한 본래의 삽입 순서를 기억하기에, 반복 시 먼저 삽입된 `key` 와 `value` 값이 나온다.

> Object 와 다른 점이란?

-   `Object` 는 키에 무조건 문자열 혹은 심볼만 들어갈 수 있으나, `Map` 은 어떠한 자료형이던 전부 허용함. 심지어는 객체도 key 값으로 적용할 수 있다.
-   `Map` 은 순회가 가능한 iterable 객체이나, `Object` 의 경우 그렇지 않다.
-   자료구조에 추가, 삭제가 빈번할수록 `Map` 이 더 뛰어난 성능을 보여준다.

#### 업데이트 예정인 지식 목록

1. SameValueZero (map 이 키를 비교하는 방식)
2. Map Chaining (set 메서드는 map 객체를 반환하므로 연속된 작업이 가능)

#### 2. Map의 인스턴스 메서드

1. Map.prototype.set(key, value)

-   기존의 Map 에 `key` 와 `value` 로 이루어진 한 쌍의 엔트리를 추가한다.
-   만약 인자로 받은 `key` 값과 대응하는 `value` 값이 이미 있다면, 이를 새롭게 업데이트 한다.

2. Map.prototype.get(key)

-   인자로 받은 `key` 값에 대응되는 `value` 값이 Map 객체 안에 있는지를 확인한다.
-   만약 대응되는 `value` 값이 존재하지 않을 경우에는 결과로 `undefined` 를 리턴한다.

3. Map.prototype.clear(value)

-   Map 객체에 내장되었던 모든 값들을 제거하고, Map을 빈 상태로 만든다.

4. Map.prototype.delete(key)

-   인자로 받은 `key` 값이 Map 객체 내부에 있는지를 확인하고 값을 제거한다.

5. Map.prototype.has(value)

-   인자로 받은 값이 Map 객체 내부에 있는지를 확인하고, 결과를 `boolean` 으로 반환한다.

```javascript
let mapObj = new Map();

mapObj.set('name', 'baik gwangin');
mapObj.set('job', 'student');
mapObj.set('age', 30);

map.set('age', 25); // age 값을 25로 업데이트.
mapObj.has('major'); // false
mapObj.get('age'); // 25
```

6.  Map.prototype.keys()

-   Map 객체의 `key` 값을 **삽입된 순서대로** 순회할 수 있는 Iterator 객체를 반환한다.
-   따라서 반환받은 Iterator 객체를 `next()` 메서드를 통해 반복시킬 수 있다.

7.  Map.prototype.values()

-   Map 객체의 `value` 값을 **삽입된 순서대로** 순회할 수 있는 Iterator 객체를 반환한다.
-   따라서 반환받은 Iterator 객체를 `next()` 메서드를 통해 반복시킬 수 있다.

8. Map.prototype.entries()

-   Map 객체의 `key`, `value` 값을 한 쌍으로 하는 iterator 객체를 반환한다.
-   따라서 반환받은 Iterator 객체를 `next()` 메서드를 통해 반복시킬 수 있다.

9. Map.prototype.forEach()

-   Map 객체에 요소가 **삽입된 순서대로** 주어진 함수를 실행시킨다.

#### 3. Loop of Map

-   Map은 iterable 한 객체이기에 `for..of` 로 내부의 값을 순회할 수 있다.
-   Map 객체에 내장된 `forEach` 함수를 통해서도 내부를 순회할 수 있다.

```javascript
let MapObj = new Map([
    ['foo', 3],
    ['bar', {}],
    ['baz', undefined],
]);

const MapIter = MapObj.values();
console.log(MapIter.next()); // {value: 3, done: false}

for (const [key, value] of MapObj) {
    console.log(`${key} ${value}`); // foo 3, bar [object Object], baz undefined
}

for (const key of MapObj.keys()) {
    console.log(key); // foo, bar, baz
}

for (const value of MapObj.values()) {
    console.log(value); // 3, {}, undefined
}
```

# ✒️ WeakSet

> https://ui.toast.com/posts/ko_20210901 참조하여 더 정리해보자.

#### 1. WeakSet 이란?

-   Set과 유사하게 동작하지만 오직 객체만을 요소로 넣을 수 있다.
-   WeakSet은 iterable한 객체가 아니므로 `for..of` 루프 사용이 불가하다.
-   WeakSet의 요소로 쓰인 객체는 Garbage Collector 의 수거 대상이 된다.
-   WeakSet이 포함한 객체가 GC에 의해 삭제되면 WeakSet에서도 자동 삭제된다.

#### 2. WeakSet의 인스턴스 메서드

1. WeakSet.prototype.add(key)

-   WeakSet 인스턴스 객체 내에 새로운 객체를 추가하고, 변경된 WeakSet 인스턴스를 반환한다.

2. WeakSet.prototype.delete(key)

-   인자로 받은 객체를 WeakSet 인스턴스 객체 내부에서 제거한다.

3. WeakSet.prototype.has(key)

-   WeakSet 인스턴스 내에 인자로 받은 객체가 존재하는지를 단언하는 `boolean` 값을 리턴한다.

```javascript
const weakSet = new WeakSet();

const obj1 = {};
const obj2 = function () {};

weakSet.add(window);
weakSet.add(obj1);
weakSet.add(obj2);

weakSet.has(obj1); // true
weakSet.delete(obj1);
weakSet.has(obj1); // false
```

# ✒️ WeakMap

-   Map과 유사하게 동작하지만 오직 객체 (함수) 만을 `key` 로 넣을 수 있다.
-   만약 원시 값을 키로 넣으려 한다면 `TypeError` 에러가 발생한다.
-   Map 객체와는 다르게 키 목록을 얻을 수 없으며, 보유한 객체들을 열거할 수 없다.
-   WeakMap의 `key` 로 쓰인 객체는 Garbage Collector 의 수거 대상이 된다.
-   해당 객체에 대한 참조가 WeakMap 외에 없다면, 자동으로 GC에 의해 수거된다.

```javascript
const weakMap = new WeakMap();

const key1 = {};
const key2 = function () {};
const key3 = window;

weakMap.set(key1, 11);
weakMap.set(key2, 'test');
weakMap.set(key3, undefined);
weakMap.set('key4', 'key'); // Uncaught TypeError: Invalid value used as weak map key
```

#### 2. WeakMap의 인스턴스 메서드

1. WeakMap.prototype.set(key, value)

-   기존의 WeakMap 에 `key` 와 `value` 로 이루어진 한 쌍의 엔트리를 추가한다.
-   만약 인자로 받은 `key` 가 객체가 아닐 경우, `TypeError` 를 발생시킨다.

2. WeakMap.prototype.get(key)

-   인자로 받은 `key` 값에 대응되는 `value` 값이 WeakMap 객체 안에 있는지를 확인한다.
-   만약 대응되는 `value` 값이 존재하지 않을 경우에는 결과로 `undefined` 를 리턴한다.

4. WeakMap.prototype.delete(key)

-   인자로 받은 `key` 값이 WeakMap 객체 내부에 있는지를 확인하고 값을 제거한다.
-   요소가 성공적으로 제거된 경우 `true` 를, 키를 찾을 수 없거나 키가 객체가 아닌 경우는 `false` 를 반환한다.

5. WeakMap.prototype.has(value)

-   인자로 받은 값이 WeakMap 객체 내부에 있는지를 확인하고, 결과를 `boolean` 으로 반환한다.

# WeakSet / WeakMap은 Set / Map과 어떤 차이가 있나?

> https://ui.toast.com/posts/ko_20210901 : 참고 자료

-   WeakSet과 WeakMap의 경우 iterable 하지 않아 열거 메서드를 지원해주지는 못한다.
-   하지만 객체의 참조를 **약하게 하여** 메모리 누수 관리에 큰 메리트가 있는 자료구조이다.

#### 참조를 강하게? 약하게? 무슨 의미일까?

-   기본적으로 JS에서 객체의 참조는 강하게 유지된다. 즉, 객체에 대한 참조가 존재하는 한 GC가 일어나지 않는다.

```javascript
// 하단의 객체는 obj 라는 식별자가 참조하여 접근이 가능하다.
let obj = { a: 10, b: 20 };

// 하지만 참조 값을 null 로 덮어씌울 경우 더 이상 객체에 접근이 불가하다.
// 따라서 해당 객체 ({a: 10, b: 20}) 는 자동으로 메모리에서 삭제된다.
obj = null;
```

-   자료구조를 구성하는 요소 또한 자신이 속한 자료구조가 메모리에 잔존한다면, 도달 가능한 값으로 취급한다.

```javascript
let obj = { a: 10, b: 20 };
let arr = [obj];

// obj 식별자의 참조 값을 기존의 객체에서 null로 덮어 씌웠다.
obj = null;

// 하지만 객체를 참조하는 배열이 메모리 상에 남았기에 GC의 대상이 되지 않음.
// array[0] 을 통해 해당 객체의 값을 얻는 것 또한 가능하다.
console.log(arr[0]); // { a: 10, b: 20 }
```

-   하지만 WeakMap이나 WeakSet의 경우 JS에서 객체의 약한 참조를 만드는 유일한 방법이다.
-   WeakSet의 요소나 WeakMap의 `key` 값에 객체를 키로 추가한다면 더 이상 GC를 막지 않게 된다.

```javascript
let obj = { a: 10, b: 20 };
let wm = new WeakMap();
let ws = new WeakSet();

// obj가 참조한 객체를 key로 가지는 한 쌍의 엔트리를 생성.
wm.set(obj, 'foo');
console.log(wm.get(obj)); // foo

ws.add(obj);

// obj 식별자의 참조 값을 null로 덮어씌워 더 이상 객체에 접근하지 못하게 함.
// 이로 인해 WeakMap 안에 저장되었던 기존의 객체 또한 자동으로 소멸됨.
obj = null;
console.log(wm.get(obj)); // undefined
```

#### 왜 WeakMap과 WeakSet은 컬렉션 내부를 순회할 수 없을까?

-   WeakMap과 WeakSet은 기존의 Map과 Set에서 지원하는 `keys()`, `values()` 같은 메서드가 없다
-   그 이유는 바로 Garbage Collection 때문인데, 객체는 모든 참조를 잃게 되면 자동으로 GC의 대상이 된다.
-   하지만 GC가 언제 일어나는지는 오직 JS 엔진만이 결정한다. 따라서 객체가 참조를 잃게 된 후 즉시 삭제될 수 있지만, **대기 후 다른 삭제 작업과 같이 소거될 수 있다.**
-   따라서 현재 WeakMap 혹은 WeakSet 내부에 요소가 몇 개인지를 정확히 체크할 수 없기에 두 자료구조의 요소를 대상으로 한 메서드는 동작이 불가능하다.

# 언제 WeakSet과 WeakMap을 사용하는가?

#### 캐싱

-   WeakMap과 WeakSet의 경우 캐싱이 필요할 때, 즉 **메모이제이션** 에 용이하다.
-   Map과 Set의 경우 더 이상 특정 키가 필요 없어지더라도 수동으로 이를 제거해주어야 한다.
-   하지만 WeakMap과 WeakSet의 경우 객체가 메모리에서 삭제될 경우, 자료구조에서도 자동으로 해당 값이 삭제되기 때문에 관리가 용이하다.

```javascript
function createLargeClosure() {
    const largeObj = {
        a: 1,
        b: 2,
        str: new Array(1000000).join('x'),
    };

    const lc = function largeClosure() {
        return largeObj;
    };

    return lc;
}

const memo = new WeakMap();

function memoize(obj) {
    if (memo.has(obj)) {
        return memo.get(obj);
    }
    const result = obj.a + obj.b;
    memo.set(obj, result);
    return result;
}

function start() {
    const lcObj = createLargeClosure();
    const timer = setInterval(() => {
        memoize(lcObj);
    }, 1000);

    setTimeout(function () {
        clearInterval(timer), 5001;
    });
}
```

-   상단의 코드에서 memo 를 Map으로 했다면, 코드가 실행된 이후에도 lcObj가 남았을 것이다.
-   하지만 WeakMap을 사용한 결과 더 이상 해당 객체를 참조하는 곳이 없기에 자동으로 GC 대상이 되어 소거된다.
