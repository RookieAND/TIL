# ๐ Introduction

> **Git Hook** ์ด๋ผ๋ ๊ฒ๋ ์ฒ์ ๋ค์ด๋ณด๋๋ฐ, **Husky**๋ ์ ๋ ์ฐ๋ ๊ฑธ๊น?

์ด์  ESLint ๊ด๋ จ ํ์ต์ ๋ง์น๊ณ  ๋์ Husky์ ๊ด๋ จ๋ ํ์ต์ ์ถ๊ฐ์ ์ผ๋ก ์งํํ๋ ค๋๋ฐ, Git hooks ์ด ๋ฌด์์ธ์ง ๋ฌป๋ ์ง๋ฌธ์์ ์ด๋ฏธ ๋ด ์ ์ ์ ํฑ ๋งํ๋ฒ๋ ธ๋ค. ๋๋์ฒด๊ฐ ์ด๋์ ๊ณต๋ถ๋ ํ๋๋ฅผ ์ข ํ๋ ค ํ๋ฉด ์ฐ์์ ์ผ๋ก ๋ชจ๋ฅด๋ ๊ฒ ์ด๋์ ํฐ์ ธ๋์จ๋ค. ์์ฃผ ์ฌ๋์ ํ์ฅํ๊ฒ ๋ง๋๋๋ฐ๋ ๋๊ฐ ํ๋ค.

๊ฒฐ๊ตญ ์ธ๋ฉฐ ๊ฒจ์๋จน๊ธฐ๋ก, ์ด์ฐจํผ ์์์ผ ํ์ ๋ด์ฉ์ด๋ผ ์๊ฐํ๋ฉฐ Git hook์ ๋ํ ๋ด์ฉ๋ถํฐ ๋จผ์  ํ์ตํ๋๋ฐ.. ์ด๋์ผ ์ด๊ฑฐ ๋ด๋ผ. ์๊ฐ๋ณด๋ค ์ ์ฉํ๋ค. ๋ฐ๋ผ์ ์ค๋์ 5์๊ฐ ๋์ ์ฌ๋ฌ ์ฝ์ง๊ณผ ์ ๋ฆฌ๋ฅผ ํตํด ๋ด๊ฐ ์ด๋ป๊ฒ Husky๋ฅผ ์ ์ฉํ์๊ณ  ์์ฒด์ ์ธ Git Hooks Script๋ ์ด๋ค ๋ฐฉ์์ผ๋ก ์์ฑํ๋์ง, ๊ทธ๋ฆฌ๊ณ  ๊ทผ๋ณธ์ ์ผ๋ก **์** Husky๋ฅผ ์ฌ์ฉํด์ผ ํ๋์ง์ ๋ํ ๊ณ ์ฐฐ๊ณผ ๋ต๋ณ์ ์์ฑํ๊ณ ์ ํ๋ค.

# โ๏ธ Git Hook

#### 1. Git Hook ์ด๋?

-   Git๊ณผ ๊ด๋ จ๋ ์ด๋ฒคํธ๊ฐ ๋ฐ์ํ์ ๊ฒฝ์ฐ, ํน์  ์คํฌ๋ฆฝํธ (Shell Script) ๋ฅผ ์คํํ  ์ ์๋๋ก ํ๋ ๊ธฐ๋ฅ์ด๋ค.
-   ํฌ๊ฒ ํด๋ผ์ด์ธํธ ํ๊ณผ ์๋ฒ ํ์ผ๋ก ๋๋๋ค.
    -   ํด๋ผ์ด์ธํธ ํ : commit, merge, push ๋ฐ์ ์  ํด๋ผ์ด์ธํธ์์ ์คํ๋จ.
    -   ์๋ฒ ํ : git repo๋ก push๊ฐ ๋ฐ์๋์์ ๋ ์๋ฒ์์ ์คํ๋จ.

#### 2. Client Hook์ ๋ํด ์์๋ณด์.

-   Commit Workflow Hook : git commit ๋ช๋ น์ด๋ฅผ ํตํด commit์ ์งํํ  ๋ ์คํ๋จ.
    -   pre-commit : commit ๋ฉ์ธ์ง๋ฅผ ์์ฑํ๊ธฐ ์  ์คํ๋จ.
    -   prepare-commit-msg : commit ๋ฉ์ธ์ง๋ฅผ ์์ฑํ ํ, ํธ์ง๊ธฐ๋ฅผ ์คํํ๊ธฐ ์  ์คํ
    -   commit-msg : commit ๋ฉ์ธ์ง๋ฅผ ์์ฑํ ํ, commit์ ์ต์ข ์คํํ๊ธฐ ์ ์ ์คํ.
    -   post-commit : commit ์ ์๋ฃํ ํ ์คํ.
-   Email Workflow Hook : `git am` ๋ช๋ น์ด๋ฅผ ํตํด patch ํ์ผ์ git์ ๋ฐ์ํ  ๊ฒฝ์ฐ ์คํ๋จ.
    -   applypatch-msg : `git am` ๋ช๋ น์ด ์คํ ์ ๊ฐ์ฅ ๋จผ์  ์คํ๋จ,
    -   pre-applypatch : patch ํ์ผ์ ์ ์ฉํ ํ ์คํ๋จ, patch ๋ฐ์ ๋ํ ์ค๋จ์ํฌ ์ ์์.
    -   post-applypatch : patch ํ์ผ์ด commit์ผ๋ก ๋ฐ์๋๊ธฐ ์ง์ ์ ์ํ๋๋ฉฐ, patch ๋ฐ์์ ์ค๋จ์ํฌ ์ ์์.
-   Other Hook : ๊ธฐํ
    -   pre-rebase : git rebase ์์ ์ ์ ์คํ
    -   post-rewrite : `git commit -amend`, `git rebase` ์ฒ๋ผ commit์ ๋ณ๊ฒฝํ๋ ๋ช๋ น์ ์คํํ ํ ์คํ.
    -   post-merge : merge ์์์ด ์ข๋ฃ๋ ํ ์คํ.
    -   pre-push : `git push` ๋ช๋ น ์คํ ์ ์๋ํ๋ฉฐ, remote ์ ๋ณด๋ฅผ ์๋ฐ์ดํธ ํ ํ ๋ฐ์ดํฐ๋ฅผ ์ ์กํ๊ธฐ ์  ์คํ, push ์ค๋จ ๊ฐ๋ฅ.

> git patch๋?

- git commit์ ํ๋์ ํ์ผ๋ก ๋ง๋ค ๊ฒฝ์ฐ patch ํ์ผ์ด ์์ฑ๋๋ค. ์ด๋ ๋ค๋ฅธ ๊ฐ๋ฐ์์๊ฒ ์ ๋ฌ์ด ๊ฐ๋ฅํ๋ค.
- `git format-patch` ๋ช๋ น์ด๋ฅผ ์ฌ์ฉํ  ์, ์ ์ฉ๋ ๊ฐ๊ฐ์ commit์ ๋์๋๋ patch ํ์ผ์ด ์์ฑ๋๋ค.
- ์ด๋ ๊ฒ ๋ง๋ค์ด์ง ํ์ผ์ ์ ๋ฌ๋ฐ์ ๋ค์ commit์ผ๋ก ์ ์ฉํ๋ ค๋ฉด, `git am [patch-file]` ๋ช๋ น์ด๋ฅผ ์ฌ์ฉํ์ฌ commit์ ๋ฐ์์ํจ๋ค.

> git rebase๋?

- ๋๊ฐ์ branch์์, ํ branch์ base๋ฅผ ๋ค๋ฅธ branch์ ์ต์  ์ปค๋ฐ์ผ๋ก ์ ์ฉํ๋ ์์์ด๋ค.
- ๋ค๋ฅธ ๊ฐ๋ฐ์๊ฐ ์ฌ๋ฆฐ commit์ ์์ ์ฌํญ์ ๋ด๊ฐ ์์ํ branch์ ์ฆ๊ฐ ์ ์ฉํ  ์ ์๋ค.
- ๋ํ rebase๋ merge์ ๋ฌ๋ฆฌ commit ์ด๋ ฅ์ด ๋จ์ง ์์ ๋ณด๋ค ๊น๋ํ commit history๋ฅผ ๊ฐ์ ธ๊ฐ ์ ์์.
- ๋จ, rebase์ ๊ฒฝ์ฐ merge์ ๋ฌ๋ฆฌ ๊ฐ commit์ ๋ํ confilct ์ฒ๋ฆฌ๋ฅผ ์ผ์ผํ ํด์ค์ผ ํ์ง๋ง, ์คํ๋ ค ์ด๊ฒ์ด ์ฅ์ ์ผ๋ก ๋ถ๊ฐ๋  ์ ์์.

> Shell Script๋?

- Shell์์ ์ฌ์ฉ ๊ฐ๋ฅํ ๋ช๋ น์ด๋ค์ ์กฐํฉ์ ๋ชจ์์ ๋ง๋  ๋ฐฐ์น (Batch : ๋ฐ์ดํฐ๋ฅผ ์ผ๊ด์ ์ผ๋ก ๋ชจ์์ ์ฒ๋ฆฌํ๋ ์์) ํ์ผ์ด๋ค.
- ์ฆ, OS์ Shell์ ์ฌ์ฉํ์ฌ ๊ฐ ์ค์ ๋ช๋ น์ด๋ฅผ ์์ฐจ์ ์ผ๋ก ์คํ์์ผ์ฃผ๋ ์ธํฐํ๋ฆฌํฐ ๋ฐฉ์์ ํ๋ก๊ทธ๋จ์ด๋ค.
- ์คํฌ๋ฆฝํธ์ ์ฒซ ์ค์ ์ ํ `#!/bin/bash` ์์ `#!` ์ Shebang์ด๋ผ๊ณ  ์นญํ๋ฉฐ, ํด๋น Shell Script ํ์ผ์ ํด์ํด์ค ์ธํฐํ๋ฆฌํฐ์ ์ ๋ ๊ฒฝ๋ก๋ฅผ ์ง์ ํ ๊ฒ์ด๋ค.
- ๋ฐ๋ผ์ `#!/bin/bash` ์ ์๋ฏธ๋ ํด๋น ํ์ผ์ ์ฌ๋ฌ ์ข๋ฅ์ shell ์ค `bash` ๋ฅผ ํตํด ์คํ์ํค๊ฒ ๋ค๋ ์๋ฏธ์ด๋ค. `sh` ๋ฅผ ์ ์ฉํ๊ณ  ์ถ๋ค๋ฉด `#!/bin/sh` ๋ฅผ ์์ฑํ๋ฉด ๋๋ค.

#### 3. Git Hook ์ ์ฉ ๋ฐฉ์์?

- git hook์ ๊ฒฝ์ฐ `.git/hooks` ๋๋ ํ ๋ฆฌ ์์ ์ ์ฅ๋๋ค. 
- hook์ ์คํ ๊ฐ๋ฅํ Shell Script์ด๋ฉฐ, ์ค์ ํ๊ณ ์ ํ๋ ํ์ ํ์ฅ์ ์์ด ํ์ผ ๋ช์ผ๋ก๋ง ์ง์ ํ๋ฉด ์ ์ฉ์ด ๊ฐ๋ฅํ๋ค.

์์๋ก `pre-commit` ํ์ ์ ์ฉํ์ฌ commit ์ ์ `Hello Gwangin!` ๋ฉ์ธ์ง๋ฅผ ์ถ๋ ฅ์ํค๊ณ  ์ถ๋ค๋ฉด ์๋์ ๊ฐ์ด ํ์ผ์ ์์ฑํ๋ค.

-   .git/hooks/pre-commit

```
#!/bin/sh

echo 'Hello Gwangin!'

exit 0 # exit์ด 0์ด ์๋ ๊ฒฝ์ฐ commit์ด ์ทจ์๋๋ค๊ณ  ํจ.
```

`pre-push` ํ์ ์ ์ฉํ์ฌ ์ง์ ์ ์ผ๋ก master branch์ push ๋ฅผ ์งํํ๋ ๊ฒ์ ๋ง๊ณ ์ ํ๋ค๋ฉด, ์๋์ ๊ฐ์ด ํ์ผ์ ์์ฑํ๋ค.

-   .git/hooks/pre-push

```
#!/bin/sh

PROHIBITED_REF="refs/heads/master"
โ
if read local_ref local_sha remote_ref remote_sha
then
    if [ "$remote_ref" = "$PROHIBITED_REF" ]
    then
        echo "prevent to push master"
        exit 1
    fi
fi

exit 0
```

# โ๏ธ Husky

#### 1. husky๋?

-   Husky๋ git hooks ๋ฅผ ์ ์ฉํ๊ฒ ํด์ฃผ๋ ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ค.
-   ์ฌ์ฉ์๊ฐ `./git/hooks` ๋๋ ํ ๋ฆฌ์ ์คํฌ๋ฆฝํธ๋ฅผ ๋ฃ์ ๊ฒฝ์ฐ git์ ์ฌ๋ผ๊ฐ์ง ์๋๋ค.
-   ํ์ง๋ง`.husky` ํด๋ ๋ด์ ์คํฌ๋ฆฝํธ๋ฅผ ์ ์ฅํ๋ฉด git hook์ ๋ค๋ฅธ ๊ฐ๋ฐ์์ ๊ณต์ ํ  ์ ์๋ค.
-   ๋จ, `./git` ์ด ์กด์ฌํ๋ ๋๋ ํ ๋ฆฌ์ `./husky` ํ์ผ์ด ์์ด์ผ ์ ์์ ์ผ๋ก ์๋์ด ๊ฐ๋ฅํ๋ค.

#### 2. ์ Husky๋ฅผ ์จ์ผ ํ ๊น?

-   `./git` ๋๋ ํ ๋ฆฌ๋ ๊ธฐ๋ณธ์ ์ผ๋ก git ignore ๋์์ด๋ค, ๋ฐ๋ผ์ hook์ ๊ณต์ ํ๊ธฐ ์ํด์๋ ๋ณ๋์ ์์์ด ํ์ํ๋ค.
-   git hooks๋ฅผ ์ค์ ํ๋ ์คํฌ๋ฆฝํธ ์์ฒด๋ฅผ ๋ณ๋์ ๋๋ ํฐ๋ฆฌ์ ์ ์ฅํ๊ฑฐ๋, git template์ ์ฌ์ฉํ์ฌ ๊ณต์ ๊ฐ ๊ฐ๋ฅํ๋ค.

1. git hook ์คํฌ๋ฆฝํธ๋ฅผ ๋ณ๋์ ๋๋ ํฐ๋ฆฌ์ ์ ์ฅํ๊ณ , ์ด๋ฅผ ์ถํ ์ ์ฉํ๋๋ก ํ๋ค.

-   ํ๋ก์ ํธ ๋ด์ `git_hooks` ํด๋๋ฅผ ๋ง๋ค๊ณ , ํด๋น ํด๋ ๋ด์ ์ฌ์ ์ ์์ฑํ hook script ํ์ผ์ ๋ฃ์ด๋๋ค.
-   ์ดํ ํ๋จ์ ์คํฌ๋ฆฝํธ๋ฅผ ๊ณต์ ํ๊ฑฐ๋, ๋ค๋ฅธ ์ฌ์ฉ์๊ฐ ์๋์ผ๋ก ์คํฌ๋ฆฝํธ ํ์ผ์ `.git/hooks` ํด๋์ ๋ฃ์ผ๋ฉด ๋๋ค.
-   ํ๋จ์ ์คํฌ๋ฆฝํธ๋ git_hooks ๋ด์ ๋ชจ๋  ํ์ผ์ .git/hooks ๋๋ ํ ๋ฆฌ๋ก ๋ณต์ฌ (cp) ํ๋ ๋ช๋ น์ด๋ฅผ ๋ด๊ณ  ์๋ค.

```
#!/bin/bash

cp git_hooks/* .git/hooks
```

2. git template ๋ฅผ ํ์ฉํ์ฌ `git clone` ์ `--template` ์ต์์ ํตํด `.git` ๋๋ ํ ๋ฆฌ๋ฅผ ์ฌ์ ์ ์ง์ ํ ์์๋๋ก ์ด๊ธฐํํ  ์ ์๋ค.

-   ํ๋ก์ ํธ ๋ด `git_template` ํด๋๋ฅผ ๋ง๋ค๊ณ , ์ค์  `.git` ํด๋ ๊ตฌ์กฐ์ฒ๋ผ `hooks` ํด๋๋ฅผ ์์ฑํ ํ ๋ด๋ถ์ ์คํฌ๋ฆฝํธ ํ์ผ์ ์ถ๊ฐํ๋ค.

```
/main/git_templates
    โโ /hooks
        โโ pre-push
        โโ pre-commit
        โโ etc...
```

-   ์ดํ ํ๋ก์ ํธ๋ฅผ clone ํ  ๋ `--template` ์ต์์ ์ฌ์ ์ ์์ฑํ template ๋๋ ํ ๋ฆฌ๋ฅผ ๊ฒฝ๋ก๋ก ์ง์ ํ๋ค.
-   ํ๋จ์ ๋ช๋ น์ด๋ ํ๋ก์ ํธ๋ฅผ clone ํ๋ฉด์, template ์ต์์ ์ ํ ๋๋ ํ ๋ฆฌ๋ฅผ ๊ธฐ๋ฐ์ผ๋ก `.git` ๋๋ ํ ๋ฆฌ๋ฅผ ์ด๊ธฐํํ๋ค.

```
 git clone --template=/main/git_templates https://github.com/RookieAND/test-git-hooks.git
```

๋จ, 1๋ฒ๊ณผ 2๋ฒ์ ๊ฒฝ์ฐ ๊ฒฐ๊ตญ ๋ค๋ฅธ ์ฌ์ฉ์๊ฐ git hooks์ ์๋์ผ๋ก ์ ์ฉํด์ผ๋ง ํ๋ค. 
๊ณ ๋ก ๋ค๋ฅธ ์ฌ์ฉ์๊ฐ ์ด๋ฅผ ๊น๋จน์์ ๊ฒฝ์ฐ hook์ด ์ ์์ ์ผ๋ก ์ ์ฉ๋์ง ์์ ๊ฐ๋ฅ์ฑ์ด ๋๋ค.
๋ฐ๋ผ์ ์ด๋ฌํ ๋ฌธ์ ๋ฅผ ํด๊ฒฐํ๊ธฐ ์ํด ๋์๋ library๊ฐ ๋ฐ๋ก **husky** ๋ค.

#### 3. husky๋ฅผ ์ด๋ป๊ฒ ์ ์ฉํด์ผ ํ ๊น?

`npm` ์ ์ฌ์ฉํ๋ค๋ฉด, ํ๋จ์ ๋ช๋ น์ด ํ ์ค๋ก ๊ธฐ์ด ์ธํ์ด ์๋ฃ๋๋ค.

```
npx husky-init && npm install
```

`yarn` ์ ๊ฒฝ์ฐ, ๋ ๋ฒ์ ๊ฑธ์ณ ์ธํ์ ์๋ฃํ๋ค.

```
yarn add husky --dev
yarn husky install
```

์ค์ ์ด ์๋ฃ๋๋ฉด ํ๋ก์ ํธ์ `.husky` ๋๋ ํ ๋ฆฌ๊ฐ ์์ฑ๋๊ณ , ์ด ์์์ hook ์คํฌ๋ฆฝํธ๋ฅผ ์ถ๊ฐํ๋ฉด ๋๋ค.

```
/.husky
    โโ .gitignore
    โโ husky.sh
    โโ ๋ด๊ฐ ์ถ๊ฐํ  script...
```

# โ๏ธ Use Husky with..

#### 1. ESLint + lint-staged + Husky ์กฐํฉ์ผ๋ก lint ์ ์ฉ

-   lint-staged : staged ์ํ์ ๋์ธ ํ์ผ์ ๊ฒ์ฌํ์ฌ lint๋ฅผ ํต๊ณผํ์ง ๋ชปํ ํ์ผ์ commit failed ์ฒ๋ฆฌ ์ํค๋ ๋ผ์ด๋ธ๋ฌ๋ฆฌ.
-   lint-staged๋ฅผ ์ ์ฉํ๊ธฐ ์ํด์๋ `lint-staged` ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ฅผ ๊ฐ๋ณ ์์กด์ฑ์ผ๋ก ์ค์นํด์ผ ํ๋ค.
-   `.lintstagedrc` ํ์ผ์ ์์ฑํ๊ณ  ๋ค์๊ณผ ๊ฐ์ด ์ฝ๋๋ฅผ ์ ๋๋ค. (glob pattern)

```
// js, jsx, ts, tsx ํ์ผ์ ๋ํ eslint ๊ฒ์ฌ ์คํ.
{
  "./src/**/*.[jt]s?(x)": "eslint --cache --fix"
}
```

-   ์ดํ `./husky` ๋ด์ pre-commit ํ ์คํฌ๋ฆฝํธ๋ฅผ ์ถ๊ฐํ๊ณ , ํ๋จ๊ณผ ๊ฐ์ด ๋ด์ฉ์ ์์ฑํ๋ค.

```
/.husky/pre-commit
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npx lint-staged
```

#### 2. commitlint + Husky ์กฐํฉ์ผ๋ก ์ปค๋ฐ ์ปจ๋ฒค์ ์ ์ฉ

-   commitlint๋, commit์ ์งํํ  ๊ฒฝ์ฐ ์ปค๋ฐ ์ปจ๋ฒค์ ์ค์ ์ฌ๋ถ๋ฅผ ์ฒดํฌํด ์ฑ๊ณต, ์คํจ๋ฅผ ๋ฆฌํดํ๋ Linter Library ๋ค.
-   commitlint๋ฅผ ์ ์ฉํ๊ธฐ ์ํด์๋ ํ๋จ์ ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ฅผ ๊ฐ๋ฐ ์์กด์ฑ์ผ๋ก ์ค์นํด์ผ ํ๋ค.
    -   @commitlint/cli
    -   @commitlint/config-conventional
-   ์ดํ commitlint์ ์ปจํผ๊ทธ์ธ `commitlint.config.js` ํ์ผ์ ์์ฑํ๊ณ  ๋ค์๊ณผ ๊ฐ์ด ์ฝ๋๋ฅผ ์ ๋๋ค.

```
module.exports = { extends: ['@commitlint/config-conventional'] };
```

-   ์ดํ `./husky` ๋ด์ commit-msg ํ ์คํฌ๋ฆฝํธ๋ฅผ ์ถ๊ฐํ๊ณ , ํ๋จ๊ณผ ๊ฐ์ด ๋ด์ฉ์ ์์ฑํ๋ค.
-   npm์ ๊ฒฝ์ฐ `-edit` ์ต์์ ์ฌ์ฉํ๊ณ , yarn์ ๊ฒฝ์ฐ `--edit` ์ต์์ ์ฌ์ฉํ์ฌ ์์ฑํ๋ฉด ๋๋ค.

```
/.husky/commit-msg
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npx --no-install commitlint --edit
```
![](https://velog.velcdn.com/images/rookieand/post/35388536-bfa6-49ef-95a4-a4a309562334/image.PNG)

~~fix ๊ณผ ์ฝ๋ก  ์ฌ์ด์ ๊ณต๋ฐฑ์ ๋ฃ์ด์ ๋น ๊พธ ๋นํ ๋ชจ์ต์ด๋ค.~~