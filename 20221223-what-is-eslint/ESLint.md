# ESLint?

## Introduction

#### 1. ESLint란? (ECMAScript + Lint)

- JS 코드에서 발견된 문제 패턴을 식별하기 위한 정적 코드 분석 도구.
- 코딩 컨벤션에 위배되는 코드, 문법적인 오류나 안티 패턴을 찾아줌.
- 코드 스타일 가이드를 편리하게 적용할 수 있는데, Airbnb와 Google Style Guide로 보통 나뉨.

#### 2. 왜 이렇게 많이 쓰이는가?

- 기본적으로 프로그래밍 언어는 컴파일 과정에서 수행되는 Linter 가 있는데, JS에는 이것이 없기 때문.
- 다른 Linter Library인 JSHint, JSLint 보다 커스터마이징이 쉽고, 확장성이 뛰어나 많이 쓰이고 있다.
- JS에서 함수나 배열을 선언할 수 있는 방법은 정말 많지만, 이를 각자의 스타일이 아니라 일정한 기준에 맞추어 작성하도록 스타일을 지정한다면 생산성이 더욱 좋아질 것.
- 여기서 더 나아가 React, Angular와 같은 프레임워크 사용 방식도 일관성 있는 코드 스타일을 지정할 수 있기에 더더욱 채용하는 것.
- 즉. **코드의 퀄리티** 를 보장하기 위함이다.

> Lint란?

- 소스코드를 분석하여 문법적인 오류나 스타일적인 오류, 적절하지 않은 구조 등에 표시 (빨간 줄) 를 달아주는 행위이다.
- 그리고 Linter란 Lint에서 행하는 동작을 도와주는 툴을 의미한다.
- C 언어 소스 코드를 검사하는 유닉스 유틸리티에서 기원한 것이다.

## Config

#### 1. .eslintrc.json (ESLint Run Command)

- ESLint의 사용자 설정을 작성하는 컨피그 파일.
- ESLint가 코드를 검사할 프로젝트의 루트 디렉토리에 두어야 함.
- YAML, JSON, JS 등 여러 파일 확장자를 지원함. (보통은 JSON을 많이 씀)

#### 2. eslintrc의 파일 구조0

1. env : 코드를 실행시킬 환경을 설정
   - 각 환경마다 미리 정의된 전역 변수를 사용하도록 허용.
   - brower의 경우 window, node의 경우 global.
2. extends : 추가한 플러그인에서 사용할 config option 을 설정하는 목록.
   - 여러 기업에서 정의한 ESLint 설정을 한번에 가져올 수 있음 (airbnb, google)
   - 플러그인에서 지원하는 `recommand`, `all`, `strict` config option 을 적용할 수 있음.
   - `eslint-plugin-` 접두사를 가진 패키지의 경우, 기존의 접두사를 제거하고 `plugin:/` 접두사를 붙인 후 옵션을 추가하면 적용이 완료됨.
   - `eslint-config-` 접두사를 가진 패키지의 경우, 접두사를 제거한 나머지를 적용시키면 됨.

```
{
    "extends": [
        "airbnb-typescript", // eslint-config-airbnb-typescript 패키지 사용.
            "plugin:react/recommended",
        "plugin:@typescript-eslint/eslint-recommended"
      ]
}
```

3. root : 설정 파일이 위치한 디렉토리를 루트로 설정하는 옵션

- 기본적으로 ESLint의 경우, 현재 Lint 대상의 파일이 위치한 폴더 안에 설정 파일이 있는지 우선 확인함.
- 만약 없다면 상위 폴더로 거슬러 올라가면서 설정 파일을 찾음.
- 만약 root 옵션이 `true` 로 설정된 설정 파일을 만났다면, 더 이상 상위 폴더로 올라가지 않음.

4. parser : JS의 확장 문법이나 최신 문법으로 작성된 코드를 Lint하기 위해, 이를 파싱하는 옵션.

- ESLint 의 경우 기본적으로 Espree 파서를 사용함.
- 하지만 JSX나 Typescript의 경우 순수 JS가 아니므로 ESLint가 인식하지 못하는 언어임.
- 따라서 이를 이해하기 위해서 별도로 필요한 parser를 설정해야 하고, 파싱 옵션 또한 별도로 정의내릴 수 있음.

```
{
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "sourceType": "module",
    "ecmaFeatures": {
      "jsx": true
    },
    "ecmaVersion": 11,
  }
}
```

5. plugins : 기본적으로 제공되는 규칙 외에 새로운 규칙을 사용하도록 만들어진 패키지 목록.

- 사용하고자 하는 plugin 패키지를 개발 의존성으로 설치했다면 등록 가능.
- 플러그인을 단순히 추가하는 것에서 끝나는 게 아니라, 플러그인에서 지원하는 여러 Rule들에 대한 설정도 마쳐야 적용이 완료됨.
- 플러그인 패키지는 단순히 새롭게 정의한 Rules들을 제공할 뿐이지, 설정까지는 진행하지 않기 때문.

```
{
  "plugins": ["import", "react"]
}
```

6. rules : 플러그인 별로 지정된 세부적인 룰을 설정할 수 있는 옵션.

- 하단의 설정은 airbnb에서 기본적으로 설정한 rule 값을 변경하여 재설정.
- 너무 많은 Rules를 직접 사용하려 한다면 관리의 비중도 높아져 불편을 초래함.

```
{
  "extends": ["airbnb"],
  "rules": {
    "no-console": "error",
    "import/prefer-default-export": "off"
  }
}
```

7. settings: 일부 ESLint 플러그인이 지원하는 추가적인 설정을 하기 위한 옵션.

- 하단의 설정은 react 플러그인이 프로젝트에 설치된 react의 버전을 자동으로 탐지하게끔 설정.

```
{
  "settings": {
    "react": {
      "version": "detect"
    }
  }
}
```

#### 3. .eslintignore

- ESLint는 Lint 작업을 수행할 시 `node_module` 폴더나 `.` 으로 시작하는 설정 파일을 기본적으로 무시함.
- 하지만 추가적으로 다른 폴더나 파일의 Lint 작업을 무시하고 싶다면 이를 eslintignore 파일에 별도로 정의해야 함.
- .gitignore 또한 같은 포맷이기에 이를 사용하고 싶다면 명령어 옵션 중 `--ignore-path` 를 사용하면 됨.

```
eslint --ignore-path .gitignore
```

#### 4. eslintrc's plugin/config

1. eslint-plugin 이란?

- 해당 패키지에서 설정한 여러 Rule을 정의한 패키지이다.
- 따라서 단순히 해당 패키지를 plugin 섹션에 넣기만 한다면 적용이 되지 않는다.
- 해당 패키지가 제공하는 여러 Rules 중에서 자신이 사용할 것들을 별도로 Rules 영역에 정의해야 한다.

```
{
  "plugins": ["react"],
  "rules": {
    "react/jsx-uses-react": "error",
    "react/jsx-uses-vars": "error"
  }
}
```

2. Rule을 하나하나 다 설정해야 하나?

- 물론 본인이 필요한 룰을 일일히 설정하는 것이 좋다.
- 하지만 이런 식으로 일일히 Rule을 지정하는 것은 매우 번거롭기에, 플러그인은 `recommand` 나 `all`, `strict` 같은 자체적인 config를 지원한다.
- 플러그인에서 지원하는 config의 경우, 내부에 plugins에 대한 선언을 포함하고 있기에 그 외의 옵션은 따로 적지 않아도 된다.

```
{
  "extends": ["plugin:react/recommended"]
}
```

- https://github.com/jsx-eslint/eslint-plugin-react/blob/master/index.js
- 해당 패키지의 `all`, `recommand` 옵션에 대한 plugin과 rules 목록을 사전에 적용하였다.

```
module.exports = {
  deprecatedRules,
  rules: allRules,
  configs: {
    recommended: Object.assign({}, configRecommended, {
      plugins: eslintrcPlugins,
    }),
    all: Object.assign({}, configAll, {
      plugins: eslintrcPlugins,
      rules: activeRulesConfig,
    }),
    'jsx-runtime': Object.assign({}, configRuntime, {
      plugins: eslintrcPlugins,
    }),
  },
};
```

2. eslint-config 란?

- 여러 개의 eslint-plugins 패키지를 하나로 모아서 설정으로 만든 것.
- eslint-config-airbnb-typescript 의 경우 하단의 패키지들로 구성되어 있다.
  - eslint-config-airbnb-base
  - @typescript-eslint/eslint-plugin
  - @typescript-eslint/parser
- 이렇게 config 패키지를 만들면

# Prettier?

#### 1. Prettier란?

- 작성된 JS 코드의 스타일을 중점적으로 수정해주는 코드 스타일링 도구
- 줄 바꿈, 띄어쓰기, 따옴표, 공백과 같은 스타일 요소들을 주로 변경해줌.

#### 2. 왜 이렇게 많이 쓰이는가?

- VS Code 내에서 사용 시, 파일을 저장할 때마다 자동으로 코드 스타일을 변경해주는 장점이 있음.
- 협업을 위해 코드 스타일을 지정하고, 이를 일관성 있게 맞추기 위해서는 필수적으로 Prettier가 필요.
- 만약 저마다 다른 코드 스타일을 가지고 협업을 진행할 경우, 코드의 일관성이 떨어지고 유지 보수 측면에서도 좋지 않은 결과를 초래할 것이다.
- 즉, **작성된 코드의 스타일** 을 일관성 있게 지정하기 위함이다.

#### 3. ESLint와는 어떤 차이점이 있는가?

- ESLint는 코드의 퀄리티를 일관되게 보장하고, 에러가 발생할 가능성이 높은 패턴을 찾아주는 역할의 Linter지만, Prettier는 말 그대로 코드의 스타일링에만 집중하는 Formatter의 역할을 한다.
- Linter Rule은 크게 Formatting과 Code Quality로 나뉘는데, 코드 퀄리티 룰의 경우 Prettier가 대체할 수 없다.
- 하지만 ESLint보다 Prettier가 코드 포맷팅 기능에 더 특화되어 있으므로, 최대 글자 길이에 맞춘 자동 포맷팅과 같은 기능을 시행할 수 있다.

#### 4. ESLint와 사용시 주의할 점

- 어찌되었던 ESLint도 코드를 스타일링해주는 기능이 있고, Prettier 또한 코드 스타일링이 주된 기능이다보니 상호 간의 충돌이 발생한다.
- 보통 ESLint와 Prettier를 동시에 사용할 경우, ESLint의 style rule을 전부 Disabled 한다.
- eslint-config-prettier Extension 의 경우, Prettier와 충돌 가능성이 있는 옵션을 전부 Off 해준다.
