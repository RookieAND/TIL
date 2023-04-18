# 📖 Introduction

# ✒️ 문서에 타입 정보를 쓰지 않기

### ✏️ 코드 상단에 주석으로 타입 정보 쓰지 않기

- TS의 타입 구문은 이미 체계적이며 쉽게 읽을 수 있도록 설계 되었으며, 컴파일러가 이를 체크해주기 때문에 구현체와의 정합성이 어긋나지 않는다.
- 주석은 코드와 **동기화 되지 않으며** 추후 코드가 변경되었다 해도 주석이 변경되지 않으면 문제가 생긴다.
- 만약 특정 매개변수나 반환 값에 대한 명시를 해주고 싶다면 JSDocs를 사용하면 된다.
- 또한 타입이 명확하지 않은 경우는 변수명에 단위에 대한 정보를 기술해주는 것도 좋다.

```ts
/**
 * 신규 유저의 회원가입을 처리하는 함수 registerAsync
 * @param email 유저의 이메일
 * @param password 유저의 비밀번호
 * @param name 유저의 실명
 * @param phoneNumber 유저의 전화번호
 * @param birthday 유저의 생일
 * @param gender 유저의 성별 (MALE, FEMALE)
 * @returns 가입 성공 시 201, 실패 시 에러 반환 (400 등)
 */
static async registerAsync(
email: string,
password: string,
name: string,
phoneNumber: string,
birthday: number,
gender: GenderType,
): ApiResponse<undefined> {
    const response = await postAsync<undefined, undefined>(
        '/users/register',
        undefined,
        {
        params: { email, password, name, phoneNumber, birthday, gender },
        },
    );
    return response;
}
```

# ✒️ 타입 주변에 null 값 배치하기

### ✏️ 값이 전부 null 이거나 아닌 경우로만 나누자.

```ts
function extent(nums: number[]) {
  let min, max;
  for (const num of nums) {
    if (!min) {
      min = num;
      max = num;
    } else {
      min = Math.min(min, num);
      max = Math.max(max, num);
    }
  }
  return [min, max];
}
```

- 상단의 코드를 보면, strictNullChecks 설정이 켜졌을 경우 함수의 반환 타입이 `(number | undefined)[]` 로 추론된다.
- 이유는 `nums` 배열이 비게 되면 초기값이 할당되지 않아 `undefined` 상태를 그대로 유지하기 때문이다.

```ts
function extent(nums: number[]) {
  let result: [number, number] | null = null;
  for (const num of nums) {
    if (!result) {
      result = [num, num];
    } else {
      result = [Math.min(min, result[0]), Math.max(max, result[1])];
    }
  }
  return result;
}
```

- 따라서 이 경우에는 두 개의 변수를 동시에 초기화 하지 말고, 두 값을 **하나의 객체** 에 넣어 null check를 시행하자.
- extents 의 결과 값으로서 단일 객체를 채용하였고, 하나의 객체에 대해서만 null check를 시행하면 되기에 이전 케이스보다 더 명료한 타입 관계가 생성되었다.

```ts
// 두 차례의 비동기 처리 동안, 두 속성은 null일 가능성을 내포하고 있어 클래스의 메서드에 악영향을 준다.
class UserPosts {
    user: UserInfo | null;
    posts: Post[]  | null;

    constructor(user: UserInfo, posts: Post[]) {
        this.user = null
        this.posts = null;
    }

    async init(userId: string) {
        return Promise.all([
            async () => this.user = await.fetchUser(userId);
            async () => this.posts = await fetchPostsForUser(userId);
        ])
    }
}

// 차라리 클래스 인스턴스를 생성할 때, 값에 대한 확실성을 심어주는 것이 더 좋다.
class UserPosts {
    user: UserInfo;
    posts: Post[]

    constructor(user: UserInfo, posts: Post[]) {
        this.user = user;
        this.posts = posts;
    }

    async init(userId: string) {
        return Promise.all([
            async () => this.user = await.fetchUser(userId);
            async () => this.posts = await fetchPostsForUser(userId);
        ])
    }
}
```

- 클래스를 생성할 때 또한, 필요한 모든 값이 준비되었을 때만이 생성하여 인스턴스를 생성하는 것이 좋다.

# ✒️ 유니온의 인터페이스보다는 인터페이스의 유니온을 사용하기

### ✏️ 유니온 타입의 속성을 여러개 가지면 실수가 생긴다

```ts
// 이렇게 선언하면 의도치 않은 조합을 가진 객체를 허용할 수 있다.
interface Layer {
  layout: FillLayout | LineLayout | PointLayout;
  paint: FillPaint | LinePaint | PointPaint;
}

// 각각의 인터페이스를 유니온으로 묶어서 정리하면, 잘못된 조합을 방지할 수 있습니다.
interface FillLayout {
    layout: FillLayout;
    paint: FillPaint;
}

interface LineLayout {
    layout: LineLayout;
    paint: LinePaint;
}

interface PointLayout {
    layout: PointLayout;
    paint: PointPaint;
}

interface Layer = FillLayout | LineLayout | PointLayout;
```

- 만약 속성에 대한 유니온을 처리하게 되면, 의도한 조합과는 다르게 다른 속성을 가진 객체가 허용될 수 있다.
- 따라서 각각의 케이스에 대한 인터페이스를 선언하고, 이를 유니온으로 묶는다면 이러한 결함을 줄일 수 있다.

### ✏️ 태그된 유니온 타입을 사용할 때도 인터페이스의 유니온을 쓰자.

```ts
interface FillLayout {
    type: 'fill',
    layout: FillLayout;
    paint: FillPaint;
}

interface LineLayout {
    type: 'line',
    layout: LineLayout;
    paint: LinePaint;
}

interface PointLayout {
    type: 'point',
    layout: PointLayout;
    paint: PointPaint;
}

interface Layer = FillLayout | LineLayout | PointLayout;
```

- type 속성은 '태그' 이며, 런타입 과정에서 타입에 대한 Layer가 사용되는지 판단하는 데 쓰인다.
- 타입스크립트는 태그를 참고하여, Layer 의 타입의 범위를

```ts
const a = [] + 1;
```
