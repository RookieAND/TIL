# 📖 Introduction

# ✒️ 공식 명칭에는 상표를 붙이기

### ✏️ 별칭을 사용할 때는 주의하자

```ts
interface Vector2D {
  x: number;
  y: number;
}

function calculateNorm(p: Vector2D) {
  return Math.sqrt(p.x ** 2 + p.y ** 2);
}

calculateNorm({ x: 3, y: 3 }); // 정상
calculateNorm({ x: 3, y: 3, z: 1 }); // 정상... 이긴 한데...
```

- 구조적 타이핑의 특성 상, 타입에서 요구하는 속성만 충족한다면 그 이후의 나머지 속성에 대해서는 별도로 체크하지 않는다. 이것이 문제가 될 수 있다.
- 상단의 함수가 3차원 벡터를 허용하지 않게 하려면 **공식 명칭** 을 사용하면 되는데, 이는 **값의 관점** 에서 타입을 명시하는 키워드를 추가하는 것이다.

```ts
interface Vector2D {
  x: number;
  y: number;
  dimension: "2D";
}

function calculateNorm(p: Vector2D) {
  return Math.sqrt(p.x ** 2 + p.y ** 2);
}

calculateNorm({ x: 3, y: 3, dimension: "2D" }); // 정상
calculateNorm({ x: 3, y: 3, z: 1 }); // 오류, dimension 속성이 없으므로.
```

- **상표 기법** 은 타입 시스템에서 동작하지만, 런타임 과정에서 타입을 검사하는 것과 동일한 효과를 낼 수 있다. 따라서 런타임 오버헤드가 줄어들고 추가 속성을 붙일 수 없는 원시 타입도 상표화 할 수 있다.
- 따라서 특정 타입에만 존재하는 고유한 **타입** 을 삽입함으로서, 마치 상표를 통해 옷을 구분하듯이 타입을 분류하는 기법을 **상표 기법** 이라 하는구나 라고 필자는 이해했다.
- 하지만 이는 완전한 해결책이 아니다. 사용자가 마음 먹고 **상표에 쓰이는 속성까지 추가해버리면** 더 이상 제 기능을 하지 못하게 된다.

### ✏️ is 연산자를 활용한 Type Narrowing

```ts
type AbsolutePath = string & { _brand: "abs" }; // 타입 영역에서만 가능하다.
function isAbsolutePath(path: string): path is AbsolutePath {
  return path.startsWith("/");
}
```

- `is` 키워드를 사용해서 타입을 정제하는 함수를 선언하고, 이를 활용하여 특정한 타입을 분기 처리하여 걸러낼 수도 있다.
- 결국 `AbsolutePath` 타입임을 증명하기 위해서는 해당 타입을 지정한 변수이거나, `isAbsolutePath` 함수의 결과가 참이 되어야만 한다.
- `string & { _brand: "abs" }` 이라는 타입은 값 영역에서 존재할 수 없지만, 타입으로만 존재하기 때문에 특정한 타입을 걸러내기 위한 기능으로는 제격이다. (string 만으로는 힘드니까)

# ✒️ any 타입은 가능한 좁은 범위에서 사용하기

### ✏️ 별칭을 사용할 때는 주의하자

```ts
// f1 에서 x는 함수가 종료되는 내내 any 타입으로 추론된다.
function f1() {
  const x: any = expressionReturningFoo();
  processBar(x);
}

// f2 에서 x는 processBar의 인자일 때만 any 타입으로 단언된다.
function f2() {
  const x = expressionReturningFoo();
  processBar(x as any);
}
```

- 정 `any` 타입을 써야겠으면 변수에 직접 할당시키지 말고, 피치 못하게 `any` 로 넘겨야 하는 곳에서만 `as` 키워드를 통해 타입 단언을 하자.
- 또한 함수의 반환 타입을 추론할 수 있는 경우에도 가능하면 반환 타입을 사용자가 명시하는 것이 좋다. 내부의 값이 어떻게 되었던 간에 결국은 반환 타입으로 추론되기 때문이다.

```ts
function f1() {
  const x: any = expressionReturningFoo();
  // @ts-ignore
  processBar(x);
  return x;
}

// 함수 내부에서는 x로 추론되었을지라도, 반환 타입은 number 다.
function f2(): number {
  const x = expressionReturningFoo();
  processBar(x as any);
  return x;
}

const y = f2(); // y 는 number 타입
```

- `@ts-ignore` 를 사용하면 아래 줄의 오류를 무시하지만, 이는 근본적인 해결책이 아니기 때문에 사용을 안하는 게 좋다.
- 객체의 경우에도 객체 전체를 `as any` 로 추론하지 말고, any를 꼭 써야만 하는 속성에만 단언을 하는 것이 바람직하다.
- 결론은 any는 되도록 **쓰지 말고**, 정 써야겠다면 **아주 좁은 범위 내에서만** 국소적으로 사용할 수 있도록 한다.

# ✒️ any를 구체적으로 변형해서 사용하기

### ✏️ 조금이라도 디테일하게 범주를 좁혀보자.

```ts
// array는 정말 넓은 범위의 집합이기 때문에 배열이 아니어도 올 수 있다.
function getLengthBad(array: any) {
  return array.length;
}

// 이렇게 하면 무슨 타입인지는 모르겠으나, 어찌되었던 배열로서 타입이 좁혀진다.
function getLength(array: any[]) {
  return array.length;
}
```

- 아래의 함수는 위에 정의된 함수와 다르게 `array.length` 타입이 체크되며, 반환 타입 또한 any가 아닌 number로 추론된다.
- 또한 함수가 호출될 때 매개변수가 배열인지를 체크하기 때문에, 완전히 any를 쓰는 것보다는 조금이라도 구체화 하는 것이 좋다.

### ✏️ 객체도 any 말고 다르게 선언해보자

```ts
// object 타입과는 다르게 열거된 키를 가지고 속성에 접근할 수도 있다.
function hasTwelveLetterKey(o: { [key: string]: any }) {
  for (const key in o) {
    if (key.length === 12) {
      console.log(key, o[key]);
    }
    return true;
  }
  return false;
}

// object 타입의 경우 키를 열거할 수는 있지만 속성에 접근할 수는 없다.
function hasTwelveLetterKey(o: object) {
  for (const key in o) {
    if (key.length === 12) {
      console.log(key, o[key]); // Element implicitly has an 'any' type because expression of type 'string' can't be used to index type '{}'.
    }
    return true;
  }
  return false;
}
```

### ✏️ 함수도 최대한 디테일하게 any를 사용하자

- 함수의 매개변수가 만약 객체일 경우, 값을 알 수 없더라도 인덱스 시그니쳐를 활용하여 `{[key: string]: any}` 로 선언하자. 적어도 객체라는 것만은 추론할 수 있다.
- `object` 타입의 경우 이와 다르게 키를 열거할 수는 있으나, 인덱싱을 통해 객체의 속성에 접근할 수 없다는 차이점이 있다. (아예 빈 객체로 인식된다)

```ts
type IsItFn = any; // 함수인지.. 원시형인지.. 이렇게 봐서는 모른다
type Fn0 = () => any; // 반환 타입만 any 다. 매개변수는 없다.
type Fn1 = (arg: any) => any; // 매개변수와 반환 타입 모두 any로 받는 "함수" 타입이다.
type Fn2 = (...args: any[]) => any;
```

- 함수 또한 단순한 any 타입이 아니라 **정형화된 형식을 사용하여** 매개변수, 혹은 반환 타입만 any 로 추론되게끔 만드는 것이 좋다.

# ✒️ 함수 안으로 타입 단언문 감추기

### ✏️ 타입 단언을 쓰더라도 함수 안에서만 쓰자

```ts
function shallowObjectEqual<T extends object>(a: T, b: T): boolean {
  for (const [k, aVal] of Object.entries(a)) {
    if (!(k in b) || aVal !== b[k]) {
      // Element implicitly has an 'any' type because expression of type 'string' can't be used to index type '{}'.
      return false;
    }
  }
  return Object.keys(a).length === Object.keys(b).length;
}
```

- 상단의 코드에서는 타입 체커가 오류를 지적했지만, 이는 실제 오류가 아닌 객체를 핸들링하는 과정에서 벌어진 "실수" 이므로 타입 단언을 하는 게 더 빠르다.
- `b as any` 로 타입을 단언하더라도 이는 안전하며 (k in b 체크가 되었으므로), 정확한 타입으로 정의된다.
- 타입 선언문은 일반적으로 쓰면 안되지만, 상황에 따라서 확실한 타입 체크가 되었다고 판단할 경우에는 **정확한 정의를 가진 함수 안으로** 숨겨서 사용하는 것이 현실적인 해결책이다.

# ✒️ any의 진화를 이해하기

### ✏️ 타입의 진화에 대해 알아보자

```ts
function range(start: number, limit: number) {
  const out = []; // 초기값을 통해 any[] 로 추론되었다.
  for (let i = start; i < limit; i++) {
    out.push(i);
  }
  return out; // out은 이제 number[] 로 추론된다.
}

const arr = []; // any[]
arr.push(1); // number[]
arr.push("a"); // (string | number)[]
```

- 분명 상단의 함수 내에서 변수 `out` 의 타입은 `never[]` 이었는데, 이후 return 에서는 `number[]` 로 추론되었다.
- 이는 for 문 내에서 number 원소를 넣었기 때문이다. 배열에 다양한 타입의 요소를 넣을 경우 타입이 확장되며 진화한다.

### ✏️ null 이었던 타입도 진화가 가능하다.

```ts
let val = null; // null 타입으로 추론되었다.
try {
  somethingDangerous();
  val = 12; // number 타입으로 추론되었다.
} catch (e) {
  console.log("alert!");
}

let val2: any; // any 타입으로 추론되었다.
val1 = "hello"; // string을 넣었음에도 any 타입이 고정된다.
```

- 상단의 코드와 같이, 처음에는 초기값이 null 이었음에도 이후 다른 값이 들어옴에 따라 타입이 진화하는 모습을 보인다.
- 단, 사용자가 명시적으로 타입을 **지정한 경우** 에는 절대로 타입이 진화하지 않고 명시된 타입을 그대로 따른다.

### ✏️ any 타입의 진화를 믿지 말자

- 일반적인 타입들은 Type Narrowing을 통해 좁혀지기만 하나, `any` 타입의 경우에는 이후 값에 따라서 진화할 수 있다.
- any 타입의 진화는 **암시적 any 타입에** 어떤 값을 할당하려고 하는 경우에만 발생하며, 함수 호출을 거쳐도 진화하지 않는다.
- 하지만 타입을 안전하게 지키기 위해서는 any 진화 방식보다 명시적 타입 구문을 쓰는 것이 좋다. 실수로 다른 타입을 넣어서 진화시켰을 경우에 대한 부작용이 존재한다.

# ✒️ 모르는 타입에는 any 보다 unknown 을 쓰자.

### ✏️ unknown 은 any랑 어떤 점이 다를까?

- any 타입이 강력하면서도 위험한 이유는 두 가지가 있다. 바로 **어떠한 타입이던** any 타입에 할당이 가능하다는 것과 **어떠한 타입으로도** 할당될 수 있다는 점이다.
- 타입 체커는 집합 기반이기 때문에, 모든 집합과 관계를 맺는 any 타입을 사용하는 것은 타입 체커의 무력화를 유발한다.

```ts
let a: unknown = 1; // number 타입이지만 unknown 타입으로 할당이 가능하다.
let b: never = 1; // number 타입은 never 타입에 할당될 수 없다.
```

- `unknown` 속성은 어떠한 타입이던 할당이 가능하지만, 반대로 어떠한 타입으로도 **할당될 수는 없다**. unknown은 오직 자기 자신과 any 타입으로만 할당될 수 있다.
- `never` 은 반대로 `unknown` 과 반대되는 개념이다. (전체 집합 vs 공집합) `never` 의 경우 어떤 타입에도 할당될 수 없지만, 어떠한 타입으로도 할당될 수 있다.

### ✏️ unknown 은 any랑 어떤 점이 다를까
