# ✒️ Git Hook

#### 1. Git Hook 이란?

-   Git과 관련된 이벤트가 발생했을 경우, 특정 스크립트 (Shell Script) 를 실행할 수 있도록 하는 기능이다.
-   크게 클라이언트 훅과 서버 훅으로 나뉜다.
    -   클라이언트 훅 : commit, merge, push 발생 전 클라이언트에서 실행됨.
    -   서버 훅 : git repo로 push가 발생되었을 때 서버에서 실행됨.

#### 2. Client Hook에 대해 알아보자.

-   Commit Workflow Hook : git commit 명령어를 통해 commit을 진행할 때 실행됨.
    -   pre-commit : commit 메세지를 작성하기 전 실행됨.
    -   prepare-commit-msg : commit 메세지를 생성한 후, 편집기를 실행하기 전 실행
    -   commit-msg : commit 메세지를 완성한 후, commit을 최종 실행하기 전에 실행.
    -   post-commit : commit 을 완료한 후 실행.
-   Email Workflow Hook : `git am` 명령어를 통해 patch 파일을 git에 반영할 경우 실행됨.
    -   applypatch-msg : `git am` 명령어 실행 시 가장 먼저 실행됨,
    -   pre-applypatch : patch 파일을 적용한 후 실행됨, patch 반영 또한 중단시킬 수 있음.
    -   post-applypatch : patch 파일이 commit으로 반영되기 직전에 수행되며, patch 반영은 중단시킬 수 없음.
-   Other Hook : 기타
    -   pre-rebase : git rebase 작업 전에 실행
    -   post-rewrite : `git commit -amend`, `git rebase` 처럼 commit을 변경하는 명령을 실행한 후 실행.
    -   post-merge : merge 작업이 종료된 후 실행.
    -   pre-push : `git push` 명령 실행 시 작동하며, remote 정보를 업데이트 한 후 데이터를 전송하기 전 실행, push 중단 가능.

> git patch란?

git commit을 하나의 파일로 만들 경우 patch 파일이 생성된다. 이는 다른 개발자에게 전달이 가능하다.

`git format-patch` 명령어를 사용할 시, 적용된 각각의 commit에 대응되는 patch 파일이 생성된다.

이렇게 만들어진 파일을 전달받아 다시 commit으로 적용하려면, `git am [patch-file]` 명령어를 사용하여 commit에 반영시킨다.

> git rebase란?

두개의 branch에서, 한 branch의 base를 다른 branch의 최신 커밋으로 적용하는 작업이다.

다른 개발자가 올린 commit의 수정사항을 내가 작업한 branch에 즉각 적용할 수 있다.

또한 rebase는 merge와 달리 commit 이력이 남지 않아 보다 깔끔한 commit history를 가져갈 수 있음.

단, rebase의 경우 merge와 달리 각 commit에 대한 confilct 처리를 일일히 해줘야 한다는 단점이 있지만, 오히려 이것이 장점으로 부각될 수 있음.

> Shell Script란?

Shell에서 사용 가능한 명령어들의 조합을 모아서 만든 배치 (Batch : 데이터를 일괄적으로 모아서 처리하는 작업) 파일이다.

즉, OS의 Shell을 사용하여 각 줄의 명령어를 순차적으로 실행시켜주는 인터프리터 방식의 프로그램이다.

스크립트의 첫 줄에 적힌 `#!/bin/bash` 에서 `#!` 은 Shebang이라고 칭하며, 해당 Shell Script 파일을 해석해줄 인터프리터의 절대 경로를 지정한 것이다.

따라서 `#!/bin/bash` 의 의미는 해당 파일을 여러 종류의 shell 중 `bash` 를 통해 실행시키겠다는 의미이다. `sh` 를 적용하고 싶다면 `#!/bin/sh` 를 작성하면 된다.

#### 3. Git Hook 적용 방식은?

git hook의 경우 `.git/hooks` 디렉토리 안에 저장된다. hook은 실행 가능한 Shell Script이며, 설정하고자 하는 훅을 확장자 없이 파일 명으로만 지정하면 적용이 가능하다.

예시로 `pre-commit` 훅을 적용하여 commit 전에 `Hello Gwangin!` 메세지를 출력시키고 싶다면 아래와 같이 파일을 생성한다.

-   .git/hooks/pre-commit

```
#!/bin/sh

echo 'Hello Gwangin!'

exit 0 # exit이 0이 아닐 경우 commit이 취소된다고 함.
```

`pre-push` 훅을 적용하여 직접적으로 master branch에 push 를 진행하는 것을 막고자 한다면, 아래와 같이 파일을 생성한다.

-   .git/hooks/pre-push

```
#!/bin/sh

PROHIBITED_REF="refs/heads/master"
​
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

# ✒️ Husky

#### 1. husky란?

-   Husky는 git hooks 를 적용하게 해주는 라이브러리다.
-   사용자가 `./git/hooks` 디렉토리에 스크립트를 넣을 경우 git에 올라가지 않지만, `.husky` 폴더 내에 스크립트를 하면 올라가므로 git hook을 다른 개발자와 공유할 수 있다.
-   단, `./git` 이 존재하는 디렉토리에 `./husky` 파일이 있어야 정상적으로 작동이 가능하다.
    -   나의 경우 하나의 레포지토리에 클라이언트와 서버 프로젝트가 담겨 있어, 이에 대한 해결이 필요했다.
    -   앞으로는 서버와 클라이언트를 별도의 레포로 분리해서 관리하는 게 좋아 보인다...

#### 2. 왜 Husky를 써야 할까?

-   `./git` 디렉토리는 기본적으로 git ignore 대상이다, 따라서 hook을 공유하기 위해서는 별도의 작업이 필요하다.
-   git hooks를 설정하는 스크립트 자체를 별도의 디렉터리에 저장하거나, git template을 사용하여 공유가 가능하다.

1. git hook 스크립트를 별도의 디렉터리에 저장하고, 이를 추후 적용하도록 한다.

-   프로젝트 내에 `git_hooks` 폴더를 만들고, 해당 폴더 내에 사전에 작성한 hook script 파일을 넣어둔다.
-   이후 하단의 스크립트를 공유하거나, 다른 사용자가 수동으로 스크립트 파일을 `.git/hooks` 폴더에 넣으면 된다.
-   하단의 스크립트는 git_hooks 내의 모든 파일을 .git/hooks 디렉토리로 복사 (cp) 하는 명령어를 담고 있다.

```
#!/bin/bash

cp git_hooks/* .git/hooks
```

2. git template 를 활용하여 `git clone` 시 `--template` 옵션을 통해 `.git` 디렉토리를 사전에 지정한 양식대로 초기화할 수 있다.

-   프로젝트 내 `git_template` 폴더를 만들고, 실제 `.git` 폴더 구조처럼 `hooks` 폴더를 생성한 후 내부에 스크립트 파일을 추가한다.

```
/main/git_templates
    └─ /hooks
        └─ pre-push
        └─ pre-commit
        └─ etc...
```

-   이후 프로젝트를 clone 할 때 `--template` 옵션에 사전에 생성한 template 디렉토리를 경로로 지정한다.
-   하단의 명령어는 프로젝트를 clone 하면서, template 옵션에 적힌 디렉토리를 기반으로 `.git` 디렉토리를 초기화한다.

```
 git clone --template=/main/git_templates https://github.com/RookieAND/test-git-hooks.git
```

단, 1번과 2번의 경우 결국 다른 사용자가 git hooks을 수동으로 적용해야만 한다. 고로 다른 사용자가 이를 까먹었을 경우 hook이 정상적으로 적용되지 않을 가능성이 높다.

따라서 이러한 문제를 해결하기 위해 도입된 library가 바로 **husky** 다.

#### 3. husky를 어떻게 적용해야 할까?

`npm` 을 사용한다면, 하단의 명령어 한 줄로 기초 세팅이 완료된다.

```
npx husky-init && npm install
```

`yarn` 의 경우, 두 번에 걸쳐 세팅을 완료한다.

```
yarn add husky --dev
yarn husky install
```

설정이 완료되면 프로젝트에 `.husky` 디렉토리가 생성되고, 이 안에서 hook 스크립트를 추가하면 된다.

```
.husky/
    └─ .gitignore
    └─ husky.sh
    └─ 내가 추가할 script...
```

#### 4. ESLint + lint-staged + Husky 조합으로 lint 적용

-   lint-staged : staged 상태에 놓인 파일을 검사하여 lint를 통과하지 못한 파일은 commit failed 처리 시키는 라이브러리.
-   lint-staged를 적용하기 위해서는 `lint-staged` 라이브러리를 개별 의존성으로 설치해야 한다.
-   `.lintstagedrc` 파일을 생성하고 다음과 같이 코드를 적는다. (glob pattern)

```
// js, jsx, ts, tsx 파일에 대한 eslint 검사 실행.
{
  "./src/**/*.[jt]s?(x)": "eslint --cache --fix"
}
```

-   이후 `./husky` 내에 pre-commit 훅 스크립트를 추가하고, 하단과 같이 내용을 작성한다.

```
/.husky/pre-commit
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npx lint-staged
```

#### 5. commitlint + Husky 조합으로 커밋 컨벤션 적용

-   commitlint란, commit을 진행할 경우 커밋 컨벤션 준수 여부를 체크해 성공, 실패를 리턴하는 Linter Library 다.
-   commitlint를 적용하기 위해서는 하단의 라이브러리를 개발 의존성으로 설치해야 한다.
    -   @commitlint/cli
    -   @commitlint/config-conventional
-   이후 commitlint의 컨피그인 `commitlint.config.js` 파일을 생성하고 다음과 같이 코드를 적는다.

```
module.exports = { extends: ['@commitlint/config-conventional'] };
```

-   이후 `./husky` 내에 commit-msg 훅 스크립트를 추가하고, 하단과 같이 내용을 작성한다.
-   npm의 경우 `-edit` 옵션을 사용하고, yarn의 경우 `--edit` 옵션을 사용하여 작성하면 된다.

```
/.husky/commit-msg
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npx --no-install commitlint --edit
```
