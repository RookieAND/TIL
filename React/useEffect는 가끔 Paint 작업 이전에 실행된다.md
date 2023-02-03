# 📖 Introduction

> useEffect는 많이 썼다만 useLayoutEffect는 대체 뭘까?

시니어 프론트엔드 개발자 테오님의 카카오톡 그룹 채팅방을 보던 도중, useEffect는 가끔 Paint 작업 이전에도 실행될 수 있느냐는 질문을 보았다. 처음에는 엥? useEffect는 항상 브라우저가 완전히 렌더링 된 이후에 실행되는 거 아닌가? 일부러 React 에서 약간의 timeout 까지 줘가며 Passive Effect를 실행시키는데, 이게 말이 되나 싶어 그렇지 않다는 답글을 달았다.

하지만 질문자 분께서 제공해주신 포스팅을 보니, 내가 알고 있던 상식이 뒤바뀌는 결과를 낳게 되어 이 번뜩이는 지식을 재빨리 글로서 정리하고자 한다. 우리가 알고 있었던 useEffect의 당연한 공식이 어쩌면 가끔은 비정상적으로 동작할 수 있다는 유익한 정보를 여러분들께서도 알기 바란다.

# ✒️ useEffect VS useLayoutEffect

### ✏️ useEffect

![](https://velog.velcdn.com/images/rookieand/post/27c02182-4bea-48ee-a80d-76099d4f2b80/image.png)

- `useEffect` 는 일반적으로 React의 Commit Phase를 마친 후, 브라우저가 **렌더링 된 후**에 실행된다.
- 따라서 `useEffect` 훅은 대부분의 경우 브라우저의 Layout과 Paint 작업을 마친 이후 실행된다.
- React는 렌더링 단계에서 어떠한 Side Effect도 허용하지 않기 때문에, 렌더링 이후에 작업을 수행하는 useEffect에 Side Effect를 시행하는 것을 추천한다.
- `useEffect` 훅 내부에 DOM을 건드리는 요소가 존재할 경우 리렌더링을 발생시켜 화면의 깜빡임을 유발시킨다.

### ✏️ useLayoutEffect

- `useLayoutEffect` 훅은 컴포넌트가 commit Phase에서 DOM이 수정된 이후에 동기적으로 실행된다.
- 이후 `useLayoutEffect` 의 작업이 완료되면, React는 제어권을 브라우저에게 위임하고 Paint 작업을 진행하게끔 한다.
- 이는 곧 React에서 Paint 작업을 의도적으로 늦추는 결과를 야기할 가능성이 존재한다는 의미이다.
- `useLayoutEffect` 내부에 DOM을 건드리는 요소가 있더라도 Paint 작업 이전에 모두 완료되므로 깜빡임이 없다.

### ✏️ 언제 써야 하는가?

- Paint 이전에 DOM 요소의 값을 읽고 처리해야할 때는 `useLayoutEffect` 사용이 바람직하다.
- DOM 과의 인터렉션이 없고 비동기적으로 처리될 Side Effect의 경우 `useEffect` 사용이 좋다.
- 대부분의 케이스에서는 `useEffect` 를 거의 사용한다. `useLayoutEffect` 는 사용을 자주 하지 않는다.

### ✏️ useEffect는 가끔 Paint 이전에 실행될 수 있다.

- 이상적인 케이스에서 React는 항상 `useEffect` 훅이 Paint 작업 이후에 실행되도록 보장한다.
- 하지만 `useLayoutEffect` 에서 리렌더링이 발생할 경우, `useEffect` 의 작업인 Passive Effect가 Paint 작업 이전에 실행된다.

```jsx
const ResponsiveInput = ({ onClear, ...props }) => {
	const el = useRef();
	const [w, setW] = useState(0);
	// measure 함수에서 setState 함수가 호출된다. state update
	const measure = () => setW(el.current.offsetWidth);
	// Paint 작업 이전에 state가 업데이트 되었으므로 re-render 발생.
	useLayoutEffect(() => measure(), []);
	useEffect(() => {
		window.addEventListener("resize", measure);
		return () => window.removeEventListener("resize", measure);
	}, []);
	return (
		<label>
			<input {...props} ref={el} />
			{w > 200 && <button onClick={onClear}>clear</button>}
		</label>
	);
};
```

- 상단의 코드는 `useLayoutEffect` 훅에서 setState 함수를 호출시켜 리렌더링을 발생시킨다.
- 따라서 React는 Commit Phase 이후 `useLayoutEffect` 훅을 호출하며 state를 업데이트 한다.
- 이후 새로운 리렌더링 작업이 예약되고, **기존의 렌더링 작업에 포함되었던** `useEffect` 훅을 호출한다.
- 이후 React 가 해당 컴포넌트를 다시 렌더링하고, 이후 `useLayoutEffect` 훅을 재호출한다.
- state 가 변동되지 않았으므로 정상적으로 React는 제어권을 브라우저에게 위임한다.
- 이후 Paint 작업이 진행되며 브라우저의 렌더링 작업이 완료되면 `useEffect` 훅이 호출된다.

### ✏️ 왜 이런 결과가 나오게 되었는가?

> React will **always flush a previous render’s effects** before starting a new update.

![](https://velog.velcdn.com/images/rookieand/post/c1be3947-3af7-400b-bda6-e0eef883e2c9/image.png)

- React 는 기본적으로 새로운 렌더링을 진행하기 전에, 기존의 렌더링 작업에 묶였던 Effect들을 모두 실행시키고, 재수집하는 과정을 거치기 때문이다.
- 따라서 `useEffect` 훅이 관할하는 Passive Effect의 경우에도 예외없이 실행되고, 이는 Paint 작업 이전에 `useEffect` 가 호출되게끔 한다.

### ✏️ 두 개의 훅을 쓰면서 지양해야 할 것들

- 늘 React가 이상적으로 동작한다는 보장이 없으니 Paint 이후의 작업들을 `useEffect` 에 의존하지 말자.
- `useLayoutEffect` 훅에 리렌더링을 유발시키는 요소를 넣는 행위는 이상적인 케이스가 아니기에 (de-opt) 되도록이면 사용을 피하자.
- 만약 피치 못할 경우로 Paint 이전에 DOM 요소를 핸들링 해야 할 경우, ref를 통해 직접 DOM을 수정하자.

```javascript
const clearRef = useRef();
const measure = () => {
	// React의 state를 사용하는 대신, DOM을 useLayoutEffect 에서 직접 핸들링.
	clearRef.current.display = el.current.offsetWidth > 200 ? null : none;
};
useLayoutEffect(() => measure(), []);
useEffect(() => {
	window.addEventListener("resize", measure);
	return () => window.removeEventListener("resize", measure);
}, []);
return (
	<label>
		<input {...props} ref={el} />
		<button ref={clearRef} onClick={onClear}>
			clear
		</button>
	</label>
);
```

### ✏️ 번외 : React 에서 Side Effect 란?

1. Pure Function

- 순수 함수는 함수의 인자가 동일할 경우 리턴 값도 동일한 함수를 의미한다.
- 해당 함수의 스코프에 묶이지 않은 외부의 상태를 변경하는 Side Effect가 없다.
- 순수 함수는 예측이 가능하며 테스트하기가 용이하다. 같은 input이면 동일한 output을 보장받기 때문이다.

2. Side Effect in React

- React 의 컴포넌트 외부에서 작동되는 모든 작업이 Side Effect 이다.
- 일반적인 React 의 Side Effect 로 취급되는 요소는 아래와 같다.

  - 외부 API를 호출하여 비동기적으로 데이터를 가져오는 작업
  - setTimeout(), setInterval() 등과 같은 타이머 함수를 작동
  - 사용자가 직접 컴포넌트 내부의 DOM 요소를 수정하는 작업
  - 함수형 컴포넌트가 매개변수로 받은 값들을 수정하는 작업

- React에서는 `useEffect` 훅을 통해 컴포넌트 내부에서 이러한 Side Effect를 처리하도록 한다.
- 왜냐하면 Side Effect는 렌더링 과정에서 영향을 주면 안되므로, 렌더링 이후에 실행되는 `useEffect` 가 적합하기 때문이다.
