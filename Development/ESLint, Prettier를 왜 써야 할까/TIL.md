# 📖 Introduction

> 지금까지 나는 무지성으로 **ESLint**를 사용해 온 것인가... 라는 생각이 들었다.

처음 프론트엔드 개발을 시작했을 때, 내가 알고 지내던 풀스택 개발자 친구에게서 ESLint와 Prettier를 꼭 설정하라는 조언을 들었다. 하지만 그때 당시 웹 개발에 무지했던 나로서는 그게 도대체 뭔지 알 길이 없었고, 그 후 지금까지도 이것들에 대한 필요성을 크게 느끼지 못했다.

하지만 최근 협업 기회가 생겨 다른 개발자 분과 프로젝트를 진행 하려던 중, 그분깨서 husky 사용에 대한 여부를 물으셨다. 당연히 나는 그게 뭐죠? 를 시전했고, 이를 개인적으로 공부해본 결과 ESLint와 밀접한 연관이 있음을 알게 되었다.

따라서 오늘부터 개인적인 시간을 내어 이것들이 대체 뭐고, 어떻게 쓰는 것인지를 공부하고자 한다.

# ✒️ ESLint

#### 1. ESLint란? (ECMAScript + Lint)

- JS 코드에서 발견된 문제 패턴을 식별하기 위한 정적 코드 분석 도구.
- 코딩 컨벤션에 위배되는 코드, 문법적인 오류나 안티 패턴을 찾아줌.
#### 2. 왜 이렇게 많이 쓰이는가?

- 각자의 스타일로 코드를 작성하게 될 경우 추후 리팩터링 과정에서 코드를 분석하기 위한 노력이 많이 든다.
- 따라서 코드를 일정한 기준에 맞추어 작성하도록 스타일을 지정한다면 생산성이 더욱 좋아질 것이다.
- 즉. **코드의 퀄리티** 를 보장하기 위함이다.


- React, Angular와 같은 프레임워크도 일관성 있는 코드 스타일을 지정할 수 있기에 더욱 뛰어나다.
- 다른 사용자들이 사전에 지정한 config를 쓰거나, 필요하다면 새롭게 Rules를 만들어 사용할 수도 있다.
- 다른 Linter Library인 JSHint, JSLint 보다 커스터마이징이 쉽고, 확장성이 뛰어나 많이 쓰이고 있다.

> Lint란?

- 소스코드를 분석하여 문법적인 오류나 스타일적인 오류, 적절하지 않은 구조 등에 표시 (빨간 줄) 를 해주는 작업이다
- 그리고 Linter란 이러한 Lint 작업을 도와주는 툴을 의미한다.
- C 언어 소스 코드를 검사하는 유닉스 유틸리티에서 기원하였다.

# ✒️ How does it Work?

#### 1. ESLint는 어떻게 동작하는가?
> ESLint는 크게 5가지 과정을 거쳐 동작한다.

1. Javascript Code를 parser를 통해 AST (Abstract Syntax Tree) 를 생성한다.
2. Linter가 사전에 정의한 규칙을 생성하고, parser를 통해 AST 노드를 순회한다
3. AST 노드를 순회하면서, AST 노드 타입과 같은 이름을 가진 이벤트를 호출한다.
4. 이벤트 발생 시 Rules의 리스너에게 전달되고, 규칙을 지켰는지에 대한 검사를 진행한다.
5. 규칙이 맞지 않는 경우 Report를 진행하며, 가능한 경우 코드를 수정하는 Fixer도 실행한다.

#### 2. AST (Abstract Syntax Tree) 란?

- 프로그래밍 언어로 작성된 소스 코드의 구조를 트리 형태로 표기한 것이다.
- Abstract (추상적) 이 붙은 이유는, 실제 구문에서 나타나는 모든 정보를 표기하지 않기 때문.
- 하단의 JS 코드를 파싱하여 만들어진 AST는 아래와 같다.
```javascript
const a = 1 + "foo"
```
```json
{
  "type": "Program",
  "start": 0,
  "end": 19,
  "body": [
    {
      "type": "VariableDeclaration",
      "start": 0,
      "end": 19,
      "declarations": [
        {
          "type": "VariableDeclarator",
          "start": 6,
          "end": 19,
          "id": {
            "type": "Identifier",
            "start": 6,
            "end": 7,
            "name": "b"
          },
          "init": {
            "type": "BinaryExpression",
            "start": 10,
            "end": 19,
            "left": {
              "type": "Identifier",
              "start": 10,
              "end": 11,
              "name": "a"
            },
            "operator": "+",
            "right": {
              "type": "CallExpression",
              "start": 14,
              "end": 19,
              "callee": {
                "type": "Identifier",
                "start": 14,
                "end": 17,
                "name": "foo"
              },
              "arguments": [],
              "optional": false
            }
          }
        }
      ],
      "kind": "const"
    }
  ],
  "sourceType": "module"
}
```
- 이를 트리 구조로 도식화하면 아래와 같은 이미지가 나오게 된다.
![](https://velog.velcdn.com/images/rookieand/post/fb21d929-df96-43b0-aef8-bb8f2fdffc91/image.PNG)

- 이후 parser를 통해 각 노드를 순회하면서, 해당 노드의 Type과 동일한 이름의 이벤트를 발생시킨다.
- 발생된 이벤트는 Rules의 리스너에게 전달되고, 해당 노드가 규칙을 지키고 있는지를 검사한다.


#### 3. AST의 생성 과정
- Lexical analyzer / Scanner (렉시컬 분석)
    - 코드의 문자를 읽어서 정해진 룰에 따라 이들을 토큰으로 만들어 합쳐주는 역할을 함.
    - 문자를 읽다 공백 혹은 연산자, 특수문자를 발견하면 해당 단어가 끝난 것으로 간주한다.
- Syntax analyzer / parser (신택스 분석)
    - 렉시컬 분석을 통해 나온 토큰 목록을 트리 구조로 만든다.
    - 이 과정에서 구조적 또는 언어적으로 문제가 발생할 경우 에러를 발생시킨다.
    - 트리 빌딩 과정에서, 일부 parser의 경우 불필요한 토큰을 생략하기도 한다.

# ✒️ ESLint's Config

#### 1. .eslintrc.json (ESLint Run Command)

- ESLint의 사용자 설정을 작성하는 컨피그 파일.
- ESLint가 코드를 검사할 프로젝트의 루트 디렉토리에 두어야 함.
- YAML, JSON, JS 등 여러 파일 확장자를 지원함. (보통은 JSON을 많이 씀)

#### 2. eslintrc의 파일 구조

1. env : 코드를 실행시킬 환경을 설정
   - 각 환경마다 미리 정의된 전역 변수를 사용하도록 허용.
   - `brower` 의 경우 window, `node` 의 경우 global 등..
2. extends : 추가한 플러그인에서 사용할 config option 을 설정하는 목록.
   - 여러 기업에서 정의한 ESLint 설정을 한번에 가져올 수 있음 (airbnb, google)
   - 플러그인에서 지원하는 `recommand`, `all`, `strict` config option 을 적용할 수 있음.
   - `eslint-plugin-` 접두사를 가진 패키지의 경우, 이를 제거하고 `plugin:/` 을 붙여 추가.
   - `eslint-config-` 접두사를 가진 패키지의 경우, 이를 제거한 나머지를 적용시키면 됨.

```
{
    "extends": [
        "airbnb-typescript", // eslint-config-airbnb-typescript 패키지 사용.
        "plugin:react/recommended", // eslint-plugin-react 패키지의 recommand 옵션 사용.
        "plugin:@typescript-eslint/eslint-recommended",
        "plugin:@next/next/recommanded" // @next/eslint-plugin-next 패키지의 recommand 옵션 사용.
      ]
}
```

3. root : 설정 파일이 위치한 디렉토리를 루트로 설정하는 옵션

- 기본적으로 ESLint의 경우, 현재 Lint 대상의 파일이 위치한 폴더 안에 설정 파일이 있는지 우선 확인함.
- 만약 없다면 상위 폴더로 거슬러 올라가면서 설정 파일을 찾음.
- 만약 root 옵션이 `true` 로 설정된 설정 파일을 만났다면, 더 이상 상위 폴더로 올라가지 않음.

4. parser : JS의 확장 문법이나 최신 문법으로 작성된 코드를 Lint하기 위해, 이를 파싱하는 옵션.

- ESLint 의 경우 기본적으로 Espree 파서를 사용함.
- 하지만 JSX나 Typescript의 경우 순수 JS가 아니므로 ESLint가 분석하지 못하는 언어임.
- 따라서 구문 분석을 위해서 별도의 parser를 설정하고, 파싱 옵션 또한 별도로 정의할 수 있음.
- babel의 경우 `babel-parser` 가, Typescript의 경우 `@typescript-eslint/parser` 가 쓰인다.
- parser 설정의 경우 플러그인에서 지원하는 `recommended` 설정에 포함되는 경우가 많다.

4-1. parserOption : ESLint 사용을 위해 지원하려는 JS 옵션을 지정할 수 있음.
- sourseType : JS 구문의 export 형태를 설정
- ecmaFeatures : JS 구문을 어떻게 파싱할 것인지를 지정하는 옵션들
    - jsx : JSX 구문 또한 파싱할 것인지를 결정

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

5. plugins : 기본적으로 제공되는 규칙 외에 새로운 규칙을 사용하도록 만든 플러그인을 지정하는 옵션.

- 사용하고자 하는 plugin 패키지를 개발 의존성으로 설치했다면 등록 가능.
- 플러그인을 단순히 추가하는 것에서 끝나는 게 아니라, 플러그인에서 지원하 Rule에 대한 설정도 마쳐야 함.
- 플러그인은 단순히 새롭게 정의한 Rules들을 제공할 뿐이지, 설정까지는 진행하지 않기 때문.

```
// 이렇게 plugins만 설정하면 해당 extension이 작동하지 않는다.
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

- 새롭게 정의한 여러 Rules 를 하나로 묶어 만든 패키지이다.
- 따라서 단순히 플러그인 패키지를 plugin 섹션에 넣기만 한다면 적용이 되지 않는다.
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

- 여러 개의 플러그인에서 지원하는 룰을 조합하여 하나의 설정으로 만든 것.
- 단순히 여러 플러그인의 룰을 조합한 것이므로, 플러그인 패키지는 별도로 설치해야 한다.
- eslint-config-airbnb 는 하단의 패키지에서 지원하는 룰을 조합하였다.
    - eslint
    - eslint-plugin-import
    - eslint-plugin-react
    - eslint-plugin-react-hooks
    - eslint-plugin-jsx-a11y 


# ✒️ Prettier

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
- `eslint-config-prettier` 패키지의 경우, Prettier와 충돌 가능성이 있는 옵션을 전부 Off 해준다.

# 📖 References
- https://tech.kakao.com/2019/12/05/make-better-use-of-eslint/
- https://eslint.org/docs/latest/user-guide/configuring/
- https://yceffort.kr/2021/05/ast-for-javascript
- https://typescript-eslint.io/architecture/parser/