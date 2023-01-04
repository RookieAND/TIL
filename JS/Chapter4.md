# ✒️ Template literal

#### 1. 문자열 삽입

-   ES5 까지는 문자열을 삽입하기 위해 `+` 연산자를 주로 사용했다.
-   ES6 부터는 백틱 `\`` 을 사용하여 쉽게 문자열을 삽입할 수 있다.

```javascript
const oldIntroduce = function (name) {
    return 'Hello my name is' + name;
};
const newIntroduce = (name) => `Hello my name is ${name}`;
```

#### 2. 여러 줄 문자열 생성

-   ES5 까지는 문자열의 각 행마다 백슬래시 `\` 를 삽입해야 했다.
-   ES6 부터는 백틱 `\`` 을 사용하여 문자열 전체를 감싸주면 된다.

```javascript
const oldIntroduce =
    'Hello, \
my name is \
Baik Gwangin. ';

const newIntroduce = `Hello
my name is
Baik Gwangin`;
```

#### 3. 템플릿 리터럴의 여러 기능

-   템플릿 내부에 또 다른 템플릿을 넣을 수도 있다.
-   템플릿 리터럴 내부에 삼항 연산자를 사용할 수 있다.
-   템플릿 리터럴 내부에 함수를 전달할 수도 있다.
-   함수를 태그하여 템플릿 리터럴을 수행할 경우, 내부의 항목이 태그된 함수의 인수로 제공된다.

```javascript
export const LikeBox = styled.div`
    ${({ theme, isLike }) => {
        const { colors } = theme;
        return css`
            width: 100%;
            padding: 4px 44px;
            margin: 8px auto;

            display: flex;
            justify-content: space-between;

            background-color: ${isLike ? colors.main.pressed : colors.main.opacity30};
            color: ${isLike ? colors.mono.white : colors.main.normal};
            border-radius: 8px;
            cursor: pointer;

            svg,
            path {
                fill: ${isLike ? colors.mono.white : colors.main.normal};
                margin: auto 0px;
            }
        `;
    }}
`;
```
