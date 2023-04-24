# 📖 Introduction

# ✒️ 몽키 패치보다는 안전한 타입을 사용하자

### ✏️ 객체와 클래스에 임의의 속성을 추가할 수 있지만 위험하다.

```js
window.monkey = 'Tamarin';
document.human = 'Baik Gwangin';

// 만약 정규식 자체에 monkey라는 속성을 추가한다면..?
Regexp.prototype.monkey // 'Capuchin';
/123/.monkey // '/123/' 도 정규식이므로 'Capuchin' 이 담겨 있다;
```

- JS에서는 객체와 클래스에 임의의 속성을 자유롭게 추가할 수 있다. 심지어는 내장 기능의 프로토타입에도 속성 추가가 가능하다.
- 하지만 이 경우 Side Effect가 발생할 확률이 높다. 전혀 연관이 없는 곳에서도 다른 곳에서 추가한 속성이 들어올 수 있기 때문이다.
- 예를 들면 A 라이브러리에서 `window` 객체에 특정한 속성을 집어 넣었는데, 이를 B 라이브러리에서도 사용할 수 있는 문제가 그 예시다. (서로 분리된 로직임에도 전역 변수이기 때문에 의존성을 가지게 된다.)
- window 전역 객체에 임의의 속성을 추가한다면 해당 속성은 전역적으로 관리되고, 이를 사용할 경우 **의존성 문제**가 발생한다.

```ts
document.human = "Baik Gwangin"; // 오류, Document 속성에 human 속성이 없습니다.

(document as any).human = "Baik Gwangin"; // 오류는 나지 않지만, 이제 어떤 값이 오던 간에 타입 체커가 작동하지 않는다.
```

- 게다가 TS 에서는 특정 객체의 내장 속성을 알고 있을지라도, 이후에 임의로 추가한 속성에 대해서는 알 길이 없다.
- 이를 가장 빠르게 해결하는 방법은 **any** 키워드를 사용하는 것이지만, 타입 안정성을 해치기 때문에 비추천한다.

### ✏️ 보강과 사용자 정의 인터페이스

- 최선의 해결책은 추가된 데이터를 분리하는 것이지만, 만약 그럴 수 없는 경우 두 개의 방법이 존재한다.

```ts
interface MonkeyDocument {
  monkey: string;
}

(document as MonkeyDocument).human = "Baik Gwangin"; // 정상
```

- 특정 객체 타입을 상속하는 인터페이스를 만들고, 이를 타입 단언으로 부여하는 방식이 바로 **보강 방식** 이다.
- 보강 방식은 any를 쓰는 것보다 타입이 안전하고, 타입 체커가 인지할 수 있기에 속성에 주석을 붙일 수 있으며 자동 완성도 가능하다.

```ts
export {};

declare global {
  interface Document {
    monkey: string; // string | undefined 로 해도 되지만, 매번 undefined가 아닌지를 Type Narrowing으로 검사해야 한다.
  }
}
```

- 모듈의 관점에서 의도한 바를 동작하게 하기 위해서는 global 선언을 해야 하지만, 해당 방식은 모듈 전체에 걸쳐 전역적으로 적용된다.
- 또한 런타임 환경에서 속성을 할당하면 실행 시점에서 보강을 적용할 방법이 없다. 따라서 어떤 객체가 **속성이 있고 없는지를 파악하기가 어렵다**.
- 이를 방지하기 위해 유니온 타입으로 `undefined` 를 붙여 보다 정확한 타입을 파악할 수는 있지만, 매번 타입을 좁혀야 하기 때문에 사용이 불편하다.

```ts
interface MonkeyDocument extends Document {
  monkey: string;
}

(document as MonkeyDocument).human = "Baik Gwangin"; // 정상
```

- 두 번째 방식은 사용자 정의 인터페이스를 선언하고, 타입 단언을 사용하는 것이다.
- `MonkeyDocument` 는 `Document` 를 확장하기에 타입 단언은 합당하며 안전하다.
- 또한 Document 타입 자체를 건드리지 않고 이를 확장하는 타입으로 선언했기 때문에 앞선 모듈 스코프 문제도 해결이 가능하다.
