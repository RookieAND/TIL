# 📖 Introduction

> React 에서 state 를 쓸 줄 모른다는 건 React를 다룰 줄 모른다는 뜻이 아닐까.

state, React를 처음 입문했을 당시 나에게는 너무나 생소한 개념이었다. 굳이 지역 함수가 아닌 별도의 state 라는 개념을 채용한 이유가 무엇인가? 이런 나의 의문은 React의 생태계를 공부하면서 하나씩 풀렸다.
하지만 아직도 나는 state를 완벽히 이해했다고 말하기 어렵다. 함수형 업데이트를 진행하면 동기적으로 작동하는 게 아닌가? 라는 잘못된 생각을 최근에야 바로잡게 된 이후 이러한 생각은 더욱 심화되었다.
따라서 날을 잡아서 state와 관련된 개념을 최대한 깨부수고, 디테일한 설명의 경우에는 추가적으로 React 코드를 좀 뜯어보면서 작동 원리를 이해하고자 한다. 하지만 React 소스 코드의 복잡도란.. 이하 생략..

# ✒️ state in React

### ✏️ What is State?

- 컴포넌트는 사용자와의 상호작용 결과로서 화면을 변경해줘야 할 때가 종종 있다. 예를 들면 사용자가 Form에 정보를 입력했다던가, 버튼을 클릭하여 캐러셀에서 다음 이미지를 보여주게 한다던가..
- 이렇게 컴포넌트는 로직의 처리를 위해 **특정한 값들을 저장해야 할 필요** 가 있으며, React 에서는 이를 `state` 라고 정의하였다.
- `state` 값이 변경되었을 경우 React 는 해당 컴포넌트를 리렌더링 한다. 따라서 `state` 의 변화는 컴포넌트의 리렌더링을 유발시킨다고 볼 수 있다.

### ✏️ local Variable doesn't trigger re-render

- 하단의 코드는 사용자가 `+1`, `-1` 버튼을 눌렀을 경우 이에 맞춰서 count 변수의 값을 변경시키는 이벤트 핸들러를 버튼에 할당시켰다.
- 하지만 해당 코드를 실행시키고 버튼을 누르면 우리가 기대했던 결과가 나오지 않게 된다. 버튼을 눌러도 Count는 계속 `0` 이 찍힌다.

```jsx
function Counter() {
  let count = 0;

  function increaseCount() {
    count += 1;
    console.log(count);
  }

  function decreaseCount() {
    count -= 1;
    console.log(count);
  }

  return (
    <div>
      <button onClick={increaseCount}>+1</button>
      <button onClick={decreaseCount}>-1</button>
      <p>Count : {count}</p>
    </div>
  );
}

export default Counter;
```

- 지역 변수 `count` 가 컴포넌트가 새롭게 리렌더링 될 때마다 `0` 으로 초기화 되기 때문이다. React는 지역 변수의 값이 변경되어도 이를 리렌더링에 반영하지 않는다.
- 사실 더 근본적인 이유는 우리가 컴포넌트 내부에 정의한 지역 변수의 값이 변경되었다 하더라도 React에서는 컴포넌트를 **리렌더링 하지 않는다** 는 것이다.
- 실제로 `console.log` 에는 count의 변경 값이 잘 찍히지만 **리렌더링이 되지 않았기 때문에** 화면에는 컴포넌트가 초기에 렌더링 되었을 때의 `count` 값인 0을 그대로 유지한다.
- 따라서 React 에서는 값이 변경되었을 때 리렌더링을 진행하도록 하면서, 컴포넌트가 새롭게 렌더링이 되었더라도 변경된 값을 그대로 유지하도록 하는 장치를 `state` 로 구현하였다.

### ✏️ useState Hook

- `useState` 훅은 컴포넌트가 기억하고자 하는 값, 즉 `state` 를 컴포넌트에 추가할 수 있도록 해주는 React Hook 이다.
- 인자로는 state의 초기 값 (initialValue) 을 넘겨줄 수 있으며 원시 값, 객체 뿐만 아니라 콜백 함수도 초기 값으로 넣어줄 수 있다.
- `useState` 은 state와 setter 함수를 하나의 배열로 반환하여 넘겨주며, 일반적으로 비구조화 할당을 통해 두 개의 값을 인계 받는다.

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  function increaseCount() {
    setCount(count + 1);
    console.log(count);
  }

  function decreaseCount() {
    setCount(count + 1);
    console.log(count);
  }

  return (
    <div>
      <button onClick={increaseCount}>+1</button>
      <button onClick={decreaseCount}>-1</button>
      <p>Count : {count}</p>
    </div>
  );
}

export default Counter;
```

- 지역 변수로 구현되었던 코드를 `useState` 훅으로 재구성한 코드이다. 이제 정상적으로 카운터가 작동된다!

### ✏️ State is private, and isolated

- state 는 자신이 속한 컴포넌트에 종속적이다. 만약 같은 컴포넌트를 두 차례 렌더링 하더라도, 각 컴포넌트에 속한 state는 서로 독립적으로 작동한다.

```jsx
import Counter from "./Counter";

function App() {
  return (
    <div>
      <Counter />
      <Counter />
    </div>
  );
}

export default App;
```

- 사전에 제작한 Counter 컴포넌트를 두 차례 렌더링하고, 두 컴포넌트 내에 있는 버튼을 누르게 되면, state가 서로 **간섭하지 않고 독립적으로 작동함**을 알 수 있다.
- 이로서 state는 자신이 속한 컴포넌트에 독립적으로 관리되며, 이로 인해 Counter 컴포넌트를 렌더링하는 상위 컴포넌트인 App 에도 Counter 내의 state가 어떻게 작동하는지 알 수 없다.
- 만약 부모 컴포넌트의 state 가 변경되었을 때 자식 컴포넌트에도 이를 적용시키기 위해서는 **props drilling**, **Context API** 와 같은 방법들을 택해야 한다.

# ✒️ Update state

### ✏️ React batches state updates

- `state` 값이 변경되었을 경우 React 에서는 해당 컴포넌트를 리렌더링 하며, 불필요한 리렌더링을 방지하기 위해 state를 변경하는 작업을 **일괄적으로 처리**한다.
- 이렇게 `state` 의 업데이트 작업을 모아 일괄 처리하는 방식을 **Batching** 이라고 하며, 이 덕에 React 에서는 불필요한 리렌더링을 방지할 수 있게 되었다.
- 또한 Batching 작업을 통해 컴포넌트 내부의 일부 state update 작업이 실행되지 않은 상태로 리렌더링이 진행되는 **half-finished** 상태를 막을 수도 있다.

```jsx
function changeState() {
  const [flag, setFlag] = useState(false);
  // setFlag (setter) 함수가 세 차례 실행되었지만, 리렌더링은 단 한번만 발생하게 된다.
  setFlag((prevFlag) => !prevFlag); // false => true
  setFlag((prevFlag) => !prevFlag); // true => false
  setFlag((prevFlag) => !prevFlag); // false => true
}
```

> **Batching** 에 대해 더 자세히 기술한 포스팅은 아래에 있다.

https://velog.io/@rookieand/React-18%EC%97%90%EC%84%9C-%EC%B6%94%EA%B0%80%EB%90%9C-Auto-Batching-%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80

### ✏️ State is a snapshot

- React 에서 컴포넌트를 렌더링한다는 의미는 함수형 컴포넌트를 호출한다는 것와 동일하다. 또한 함수로부터 반환된 JSX 는 컴포넌트가 렌더링된 순간의 **스냅샷** 임을 유의하자.
- 또한 React는 컴포넌트 내부의 props, 이벤트 핸들러, 지역 변수를 렌더링이 진행된 순간의 state를 활용하여 계산하고, 이를 최종적으로 반환하게 된다.
- state는 컴포넌트 외부에서 Closure 형태로 관리되는 변수이다. React 는 컴포넌트 함수가 호출된 순간의 state 값 (스냅샷) 을 제공하며 이는 다음 렌더링 이전까지 변하지 않는다.

```jsx
import { useState } from "react";

function Counter() {
  // 초기 렌더링 시 count 값은 0으로 고정된다. 이는 다음 렌더링 이전까지 절대 변경되지 않는다.
  const [count, setCount] = useState(0);

  function increaseCount() {
    // 초기 렌더링 이후 count 값은 0으로 고정된다.
    setCount(count + 1); // 0 + 1 = 1
    setCount(count + 1); // 0 + 1 = 1, 이전의 결과에 영향을 받지 않고 현재 count 값을 적용한다.
    setCount(count + 1); // 0 + 1 = 1, 이전의 결과에 영향을 받지 않고 현재 count 값을 적용한다.
  }

  return (
    <div>
      <button onClick={increaseCount}>+3</button>
      <p>Count : {count}</p>
    </div>
  );
}

export default Counter;
```

- 상단의 코드는 총 세 번의 setState 작업을 수행했지만 리렌더링 후 count 값은 기대와 달리 `0` 에서 `1` 로 변경된다.
- 현재 `count` state 값은 초기 렌더링을 진행한 후 0으로 고정되었다. 따라서 `setCount(count + 1)` 작업의 결과는 0에 1을 더한 값으로 state를 업데이트 할 것이다.
- 상단의 코드에서는 이 작업을 세 차례 수행하였으나, `count` 값은 현재 0으로 고정된 상태기에 결국 여러번 작업을 수행해도 리렌더링 이후 결과는 1로 변경될 것이다.
- 중요한 것은 각각의 state update 작업이 하나로 합쳐지지 않고 엄연히 순차적으로 실행되었다는 점이다. Batching 은 업데이트 작업을 일괄적으로 **처리하는 것** 이지, 동일한 작업을 하나로 합치는 것은 아니다.

```jsx
import { useState } from "react";

function Counter() {
  // 초기 렌더링 시 count 값은 0으로 고정된다. 이는 다음 렌더링 이전까지 절대 변경되지 않는다.
  const [count, setCount] = useState(0);

  function increaseCount() {
    // 리렌더링 이후 count 값은 3으로 변경된다. 현재 count 값은 0임에 유의
    setCount(count + 100); // 0 + 100 = 100
    setCount(count - 200); // 0 - 200 = -200
    setCount(count + 1); // 0 + 1 = 1, 최종적으로 count 값은 1로 수정되어 다음 렌더링에 반영됨
  }

  return (
    <div>
      <button onClick={increaseCount}>+1</button>
      <p>Count : {count}</p>
    </div>
  );
}

export default Counter;
```

- 상단의 코드는 총 세 번의 setState 작업을 수행하였고, 리렌더링 이후 count 값은 `1` 로 변경되었다.
- 여기서도 초기 렌더링 시 count 값이 `0` 임을 유의해야 한다. 세 번의 `setCount` 함수가 호출되었으나 인자로 받은 count 값은 `0` 으로 고정된다.
- 따라서 **가장 마지막에 실행된** `setCount` 의 결과가 최종적으로 변경된 count 값이 되고, 리렌더링 이후 count 값은 `1` 으로 수정되는 것이다.

```jsx
import { useState } from "react";

function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <div>
      <h1>{number}</h1>
      <button
        onClick={() => {
          setNumber(number + 5);
          // setTimeout 함수를 통해 비동기적으로 alert 함수를 3초 후 호출시켰음
          // 하지만 해당 작업이 스케줄러에 등록된 당시의 state 값인 0이 적용된다.
          setTimeout(() => {
            alert(number);
          }, 3000);
        }}
      >
        +5
      </button>
    </div>
  );
}
export default Counter;
```

- 상단의 코드는 버튼을 클릭할 경우 `number` state 에 5를 더하고, setTimeout 함수를 사용하여 3초 후에 `number` state를 alert 창에 띄우도록 하였다.
- 하지만 3초 후 alert 창에는 5가 아닌 `0` 이 띄워진다. 왜냐하면 alert 함수에 인자로 들어간 number는 사용자가 **버튼을 클릭했을 때의 state 값**을 담았기 때문이다.
- 설령 alert 함수가 리렌더링 이후에 진행되더라도, 해당 작업이 스케줄러에 등록될 당시의 state 값은 **리렌더링이 일어나기 전의 값**이기에 이런 현상이 발생하는 것이다.
- 결국 state 는 다음 렌더링이 발생하지 않는 이상 **현재의 값을 유지한다.** (불변하다) 설령 이벤트 핸들러 내부의 코드가 비동기적으로 작동하더라도 말이다.

### ✏️ updater function

- 만약 setter 함수의 인자에 값이 아닌 함수를 넣게 된다면, React 에서는 이를 **updater function** 으로 인식하게 된다.
- React 는 Batching 작업을 통해 state update 작업을 일괄 수행하는데, 만약 updater function 이 인자로 들어왔다면 해당 함수의 return 값을 다음 update 작업에 인계한다.

```jsx
import { useState } from "react";

function Counter() {
	// 초기 렌더링 시 count 값은 0으로 고정된다. 이는 다음 렌더링 이전까지 절대 변경되지 않는다.
	const [count, setCount] = useState(0);

	function increaseCount() {
		setCount(prev => prev + 1); // 0 + 1 = 1, 초기 렌더링 이후 count 값은 0으로 고정된다.
		setCount(prev => prev + 1); // 1 + 1 = 2, 상단의 updater function의 return 값인 1을 인계 받았다.
		setCount(prev => prev + 1); // 2 + 1 = 3, 상단의 updater function의 return 값인 2을 인계 받았다.

	return (
		<div>
			<button onClick={increaseCount}>+3</button>
			<p>Count : {count}</p>
		</div>
	);
}

export default Counter;
```

- 상단의 코드는 setter 함수에 update function 을 인자로 넘겨, 이전 state update 작업의 결과를 기반으로 함수를 실행시킨 뒤 나온 반환 값을 다음 update function 으로 인계하였다.
- 주의할 점은, update function 은 반드시 **순수 함수** 여야 하고, 오직 변경된 state 값만을 return 해야 한다. 함수 내부에 state를 재설정하거나 다른 Side Effect를 실행시켜서는 안된다.

```jsx
import { useState } from "react";

function App() {
  // 초기 렌더링 시 count 값은 0으로 고정된다. 이는 다음 렌더링 이전까지 절대 변경되지 않는다.
  const [count, setCount] = useState(0);

  function increaseCount() {
    // 리렌더링 이후 count 값은 1으로 변경된다. 현재 count 값은 0임에 유의
    setCount(count + 100); // 0 + 100 = 100
    setCount((prevCount) => prevCount + 1); // 100 + 1 = 101
    setCount((prevCount) => prevCount + 1); // 101 + 1 = 102
    setCount(count + 1); // 0 + 1 = 1, 최종 count 값은 1으로 변경
  }

  return (
    <div>
      <button onClick={increaseCount}>+1</button>
      <p>Count : {count}</p>
    </div>
  );
}

export default App;
```

- 한 가지 유의할 점은, updater function은 앞선 작업의 결과를 반영하여 함수를 실행하지만, 단순히 값을 전달했을 경우에는 **앞선 작업의 결과를 무시** 하고 현재 state 의 값만을 고려한다는 것이다.
- 상단의 코드 내부에도 두 차례 update function 이 실행되며 state 값을 3으로 변경했지만, 이후의 작업에서는 이를 고려하지 않고 기존의 state 값을 기반으로 새롭게 count state 값을 업데이트 한다.
- 따라서 update function 의 경우 이전 작업의 결과를 기반으로 새로운 값을 업데이트 하지만, 단순히 값을 인자로 넘겼을 경우에는 이전 작업의 결과를 고려하지 않는다는 점을 유의해야 한다.

> React 에서 추천하는 updater function 의 Name Convention

```js
setEnabled((e) => !e);
setLastName((ln) => ln.reverse());
setFriendCount((fc) => fc * 2);
```

- React 에서는 현재 setter 함수가 업데이트 하려는 state 변수의 앞 글자만을 따서 인자를 작성하는 것을 추천한다.
- 만약 조금 더 구체적인 네이밍을 쓰고 싶다면, `prev` 같은 접두사를 붙이거나 아니면 state 변수의 이름을 그대로 사용해도 된다고 한다.

# ✒️ Initial Value Lazy Loading

### ✏️ Avoiding recreating the initial state

- `useState` 훅을 사용할 때 인자로 넣어준 값은 초기 렌더링 시에 React 에 저장되고, 이후의 렌더링에서는 초기에 저장했던 값을 그대로 사용한다.
- 하지만 초기 값을 함수의 실행으로 넣어줄 경우 매 렌더링마다 해당 함수는 호출될 것이며, 해당 함수의 연산이 비싸질수록 비용의 낭비는 더욱 심해질 것이다.

```jsx
function MyApp({ Component, pageProps }: AppProps) {
  const [queryClient] = useState(
    new QueryClient({
      defaultOptions: {
        queries: {
          refetchOnReconnect: false,
          refetchOnWindowFocus: false,
        },
      },
    })
  );
  return (
    <QueryClientProvider client={queryClient}>
      <ReactQueryDevtools initialIsOpen={false} />
      <Provider>
        <GlobalStyle />
        <ThemeProvider theme={theme}>
          <ModalPortal />
          <Component {...pageProps} />
        </ThemeProvider>
      </Provider>
    </QueryClientProvider>
  );
}

export default MyApp;
```

- 상단의 코드는 react-query의 queryClient 인스턴스를 생성하고 이를 Provider에 적용하는 로직을 담고 있다. 하지만 매번 해당 컴포넌트가 실행될 경우 새로운 인스턴스를 생성하여 비용이 낭비될 수 있다.
- 솔직히 엄청나게 적절한 예시는 아니라고 생각한다. 하지만 필자가 useState의 initialValue 에 콜백 함수를 집어넣는 코드를 처음 접한 것이 상단의 코드와 같은 상황이었기에 예시로서 채택을 하였다.

```jsx
function MyApp({ Component, pageProps }: AppProps) {
  const [queryClient] = useState(
    () =>
      new QueryClient({
        defaultOptions: {
          queries: {
            refetchOnReconnect: false,
            refetchOnWindowFocus: false,
          },
        },
      })
  );
  return (
    <QueryClientProvider client={queryClient}>
      <ReactQueryDevtools initialIsOpen={false} />
      <Provider>
        <GlobalStyle />
        <ThemeProvider theme={theme}>
          <ModalPortal />
          <Component {...pageProps} />
        </ThemeProvider>
      </Provider>
    </QueryClientProvider>
  );
}

export default MyApp;
```

- 하지만 다음과 같이 콜백 함수로서 인자를 넣어주게 되면, React에서는 초기 렌더링 시에만 해당 함수를 호출하고 반환된 값을 초기화 한다.
- 이후의 렌더링에 대해서는 함수를 호출하기 전에 초기화가 되었는지 체크하고, 초기화가 된 상태라면 함수를 실행하지 않음으로서 불필요한 함수 호출을 막을 수 있다.
- 만약 state 의 초기 값을 생성하기 위한 함수의 비용이 크다면, useState 훅의 `initialValue` 에 함수를 실행시키지 말고 콜백 함수를 인자로 넣어보자. 초기 렌더링 시에만 함수가 호출되고 이후에는 반환된 값을 저장하여 활용하니 훨씬 좋다.

# 📒 References

- https://beta.reactjs.org/learn/state-a-components-memory
- https://beta.reactjs.org/learn/state-as-a-snapshot#rendering-takes-a-snapshot-in-time
- https://beta.reactjs.org/learn/queueing-a-series-of-state-updates
- https://beta.reactjs.org/reference/react/useState#avoiding-recreating-the-initial-state
- https://github.com/reactwg/react-18/discussions/5
- https://seokzin.tistory.com/entry/React-useState%EC%9D%98-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC%EC%99%80-%ED%81%B4%EB%A1%9C%EC%A0%80
