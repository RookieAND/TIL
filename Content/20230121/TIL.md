# 📖 Introduction

> **react-query** 를 쓰라고 해서 쓰긴 쓰는데.. 이걸 왜 써야 하는 걸까?

필자는 프론트엔드 개발을 계속 진행하던 도중, 개발자 커뮤니티에서 무한 스크롤 구현을 쉽게 하시려면 `react query` 와 Intersection Observer를 잘 버무려서 구현하면 좋다는 말을 듣고 무작정 react query를 시작하였다.

물론 리액트 쿼리를 사용한 입장으로서 만족감은 최상이었다, 기존의 `useState` 와 전역 상태관리 라이브러리를 활용하여 서버에서부터 데이터를 받아와 이를 보관하는 과정은 종종 예상치 못한 에러를 내뿜기 마련이었고, 해당 코드를 일관성 있게 작성하는 것은 더더욱 어려웠다. 하지만 리액트 쿼리는 이러한 단점을 많이 상쇄시켜준 고마운 친구였다.

하지만 이 고마운 친구가 정확히 어떤 일을 하는 건지, 그리고 나는 이걸 왜 써야 하는지에 대한 심도 있는 고찰이 필요한 시점이 바로 지금이라고 생각하여, 리액트 쿼리가 어쩌다 만들어졌고 이걸 써야 하는 이유와, 리액트 쿼리를 사용함에 있어 필요한 기본 지식을 정리하고자 한다.

# ✒️ Server State

#### Server State란 무엇인가?

1. state (상태)

- React에서는 렌더링에 영향을 미치는 자바스크립트 Object 라고 정의.
- Global State (전역 상태) 는 어플리케이션 어디에서든 접근이 가능함.
- 또한 전역 상태의 변화는 곧 애플리케이션의 렌더링에 전반적으로 영향을 미침.

2. Client State

- UI 테마, 사이드바, 폼 입력 등과 같이 **클라이언트가 소유하고 제어하는** 데이터를 의미.
- 클라이언트에서 항상 제어가 가능하기에 항상 동기적인 상태를 가짐.
  - local client state : 폼 입력, 사이드바 같이 하나 또는 인접한 컴포넌트에서 사용하는 state.
  - global client state : 언어, UI 테마 (다크 모드) 와 같이 어플리케이션에 전반적으로 사용되는 state.

3. Server State

- 유저 DB 정보 등과 같이 클라이언트가 **서버로부터 받아오는 모든 데이터** 를 의미함.
- 클라이언트가 제어하고 관리할 수 없기에 **특정 시점**으로부터 받아온 데이터를 사용함. 이로 인해 비동기적인 상태를 가짐.
- 서버와 클라이언트 간의 state가 항상 일치한다는 보장이 없으며, 클라이언트에서 더 이상 **유효하지 않은 데이터**를 소유할 가능성이 있음.

# ✒️ Why React Query?

#### 1. React Query 가 대체 뭔가?

> **fetching, caching, synchronizing and updating server state** in your React applications - tanstack

- react-query는 서버에서부터 데이터를 fetching하고, 클라이언트와 동기화하는 라이브러리이다.
- 서버로부터 받아온 데이터를 별도의 키를 통해 관리하고, **캐싱**하는 역할도 한다.
- 사용자의 로직에 따라 필요한 데이터를 서버 쪽에서 받아와 백그라운드에서 업데이트 (refetch) 하는 기능을 한다.

#### 2. 이걸 왜 써야 하는가?

1. Client state와 Server state의 **완전한 분리.**

- 일반적으로 서버에서 받아온 server state의 경우 전역으로 관리하는 것이 합리적임.
- state를 필요로 하는 모든 컴포넌트에서 개별적인 API 호출을 하는 것은 비용 낭비이기 때문.
- 하지만, 여러 컴포넌트에서 state를 공유하려 props drilling 을 사용하는 것도 한계가 있음.
- 따라서 전역 스토어에서 인계받은 server state를 마치 cache 처럼 관리한다면 훨씬 접근이 용이함.
- Redux를 사용할 경우 서버에서 데이터를 받아와 상태 관리를 하기 위해 Redux-Saga가 필요함. 이로 인해 관련 로직 및 모듈이 비대해짐.

2. server state의 비동기적인 특성 때문.

- 전역 상태 관리 라이브러리의 경우, 서버의 데이터와 다른 값을 보유할 수도 있다.
- 그에 대한 이유는 앞서 말한 server state의 비동기적인 특성 때문이다.
- 서버에서 받아온 데이터는 데이터를 로드한 그 시간의 "스냅샷" 에 불과하다.

```
  1. fetch, update를 위한 별도의 비동기 API가 필요함.
  2. 지속적인 관찰이 미비할 경우 오래된 데이터로 취급될 수 있음.
  3. 다른 사용자에 의해 서버 단에서 데이터가 변경될 수 있음.
  4. 기본적으로 클라이언트가 관리할 수 없는 영역에 위치해 있음.
```

- 클라이언트에서 보유 중인 데이터가 서버의 데이터와 같은지를 확인하기도 어렵다.
- 따라서 server state를 원활하게 다루기 위해서는 아래와 같은 조건이 선행되어야 한다.

```
  - 서버로부터 받은 데이터를 캐싱
  - 동일한 데이터에 대한 여러 요청을 단일 요청으로 변환
  - 백그라운드 단에서 오래된 데이터를 자동으로 업데이트
  - 데이터가 언제 오래된 (stale) 상태가 되었는지 확인.
  - 서버로부터 받은 데이터를 관리하고, GC로부터 언제 처리되는지 관리.
```

3. server state를 훨씬 쉽게 관리할 수 있음.

- react query의 경우 상단의 기능을 지원하는 라이브러리이며, 로직 또한 단순한 축에 속한다.
- 현재 데이터가 fetching 되었는지, 아니면 중간에 에러가 발생했는지를 쉽게 체크할 수 있다.

# ✒️ Query of React Query

#### 1. React Query 에서 쿼리는 무엇을 의미하는가?

> A query is a declarative dependency on an asynchronous source of data that is tied to a unique key

- Query란, 고유한 키에 묶인 비동기적인 데이터에 대한 선언적 종속성 (무슨 뜻일까?) 이다.
- Query는 서버에서 데이터를 가져오기 위해 Promise를 반환하는 함수와 같이 쓰인다.
- 서버로부터 데이터를 가져올 경우에는 `useQuery`, `useInfiniteQuery` Hook을 쓴다.
- 단, 서버의 데이터를 수정할 경우에는 `useMutation` Hook을 사용해야 한다.

#### 2. queryKey와 queryFn에 대하여.

```javascript
function Todos() {
	const { isLoading, isError, data, error } = useQuery({
		queryKey: ["todos"],
		queryFn: fetchTodoList,
	});

	if (isLoading) {
		// if (state === 'loading')
		return <span>Loading...</span>;
	}

	if (isError) {
		// if (state === 'error')
		return <span>Error: {error.message}</span>;
	}

	// 이 지점까지 코드가 도달했다면, 정상적으로 쿼리가 success state를 가짐.
	return (
		<ul>
			{data.map((todo) => (
				<li key={todo.id}>{todo.title}</li>
			))}
		</ul>
	);
}
```

- `queryKey` 의 경우, 쿼리를 refetch 하거나 캐싱하거나, 다른 곳에서 사용하고자 할때 쓰이는 고유한 키다.
- `useQuery` 로 리턴된 결과값은 쿼리에 대한 모든 정보들을 한데 모아 보관한 객체다.
- `queryFn` 의 경우 서버로부터 데이터를 가져오기 위해 쓰이는 비동기 함수를 의미하며, 반드시 Promise를 리턴해야 한다.

#### 3. 쿼리가 데이터를 가져오는데 실패했다면?

- 만약 쿼리가 데이터를 가져오는 데 실패했다면, react query의 경우 기본적으로 세 차례 재요청을 진행한다.
- 만약 재요청 횟수와 인터벌을 수정하고 싶다면, `retry` 옵션과 `retryDelay` 옵션을 별도로 설정하자.

#### 4. 쿼리의 상태는 어떻게 구분짓는가?

- `useQuery` 가 리턴한 결과 객체에서, `state` 속성으로 데이터 상태를 파악한다.
- `state` 속성의 경우 쿼리가 가진 데이터에 대한 상태를 파악하고자 할때 쓰인다.

1. loading : 쿼리가 아직 **데이터를 가지지 않음** 을 의미.
2. error : 쿼리에 특정한 **오류가 발생했음** 을 의미.
3. success: 쿼리가 성공적으로 작동되었으며, 데이터가 유효함을 의미.

- `fetchState` 속성의 경우 쿼리가 사용한 `queryFn` 의 동작 결과를 나타낸다.

1. idle: 현재 해당 쿼리의 `queryFn` 이 동작하지 않고 있음을 의미.
2. fetching: 현재 쿼리가 서버로부터 데이터를 fetching 중임을 의미.
3. paused: 쿼리가 데이터를 불러오던 중에 중지되었음을 의미. (네트워크 이슈 등)

- 따라서 `state` 와 `fetchState` 를 모두 확인하여 쿼리의 상태를 유추할 수 있다.

#### 5. 쿼리가 가리키는 데이터의 상태는 어떻게 구분하는가?

1. fresh
   - 해당 쿼리가 아직 **유효**함을 의미.
   - 인스턴스가 mount 되거나, 창이 focused 되거나 하는 경우에도 refetch를 진행하지 않음.
2. stale:
   - 해당 쿼리의 유효 기간이 지나 **오래된 데이터**가 되었음을 의미.
   - 기본적으로 fresh한 데이터는 fetching이 완료되는 즉시 stale한 상태로 변경됨.
   - `staleTime` 옵션을 통해 fresh한 데이터가 stale 한 상태가 되기까지 걸리는 시간을 지정할 수 있음.
3. inactive
   - 해당 쿼리가 `unmount` 되어 현재 비활성화 되었음을 의미.
   - 기본적으로 `inactive` 한 상태가 된 후 5분이 지나면 해당 쿼리는 자동으로 GC에 수집됨.
   - `cacheTime` 옵션을 통해 inactive 된 쿼리 데이터가 GC에게 수집되기까지 걸리는 시간을 지정할 수 있음.

#### 6. 캐싱된 데이터는 어떻게 관리되는가?

- `useQuery` 혹은 `useInfiniteQuery` 로 생성된 쿼리 인스턴스는 디폴트로 캐싱된 데이터를 `stale` 상태로 본다.
- 단, `staleTime` 속성을 수정함으로서 해당 쿼리에 종속된 데이터의 유효 시간을 설정할 수 있다.
- React Query 에서는 다음과 같은 현상이 발생할 경우 `stale` 한 쿼리를 refetch 한다.
  1. 새로운 쿼리 인스턴스가 mount 되었을 경우. (refetchOnMount)
  2. 사용자가 창을 클릭하여 focus 하였을 경우. (refetchOnWindowFocus)
  3. 네트워크가 재연결 되었을 경우. (refetchOnReconnect)
  4. 사용자가 특정 주기마다 refetch를 진행하도록 한 경우 (refetchInterval)
