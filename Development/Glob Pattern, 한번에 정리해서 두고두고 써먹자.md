# 📖 Introduction

> 도대체 이 괴상망측한 기호들과 애스터리스크는 뭘 의미하는 걸까?

# ✒️ Glob Pattern

#### 1. Glob Pattern 이란?

- 여러 와일드카드 문자를 사용하여 다수의 파일 집합을 지정하는 방식
- 리눅스 뿐만 아니라 Python, Javascript, Go, Ruby 등의 언어에 쓰인다.

```
// tsconfig.json의 include option.
{
  "include": ["next-env.d.ts", "**/*.ts", "**/*.tsx"],
}

```

#### 2. 왜 이렇게 많이 쓰이는가?

- 사실, 비슷한 기능을 하는 정규식 (RegExp) 이 더 정교하게 작동할 수도 있다.
- 하지만 그보다 더 심플한 glob 패턴이 특정한 경우에는 더 좋게 쓰인다.

# ✒️ How does it Work?

#### 1. Wild Card

> Glob Pattern에서 사용하는 특수 기호를 와일드 카드라고 한다.

1. - : 길이와 상관 없이 어떤 문자열이 와도 허용 (/ 제외)
   * `*.tsx` 는 이름과 상관없이 모든 tsx 파일을 의미함.
   * 단, 현재 디렉토리에 있는 파일만 찾고 하위 디렉토리는 찾지 않음.
2. \*\* : 길이와 상관 없이 어떤 문자열이 와도 허용 (/ 포함)
   - 현재 디렉토리를 기준으로 0개 이상의 하위 디렉토리를 매칭시킴.
   - `**/*.ts` 는 모든 하위 디렉토리에 존재하는 ts 파일을 의미함.

# 📖 References

- https://tech.kakao.com/2019/12/05/make-better-use-of-eslint/
- https://eslint.org/docs/latest/user-guide/configuring/
- https://yceffort.kr/2021/05/ast-for-javascript
- https://typescript-eslint.io/architecture/parser/
