# ✒️ useEffect VS useLayoutEffect

### ✏️ useEffect

- `useEffect` 훅은 컴포넌트가 Commit Phase를 마치고 브라우저가 완전히 **렌더링 된 후**에 실행된다.
- 따라서 `useEffect` 훅은 반드시 브라우저의 Paint 작업을 마친 이후 실행된다.
- React는 렌더링 단계에서 어떠한 Side Effect도 허용하지 않기 때문에, 렌더링 이후에 이러한 작업을 진행하도록 하는 useEffect 사용을 권장한다.

### ✏️ useLayoutEffect

- `useLayoutEffect` 훅은 컴포넌트가 commit Phase에서 DOM이 수정된 이후에 동기적으로 실행된다.

### ✏️ Difference
