# 📖 Introduction

> **react-query** 를 쓰라고 해서 쓰긴 쓰는데.. 이걸 왜 써야 하는 걸까?

필자는 프론트엔드 개발을 계속 진행하던 도중, 개발자 커뮤니티에서 무한 스크롤 구현을 쉽게 하시려면 `react query` 와 Intersection Observer를 잘 버무려서 구현하면 좋다는 말을 듣고 무작정 react query를 시작하였다.

물론 리액트 쿼리를 사용한 입장으로서 만족감은 최상이었다, 기존의 `useState` 와 전역 상태관리 라이브러리를 활용하여 서버에서부터 데이터를 받아와 이를 보관하는 과정은 종종 예상치 못한 에러를 내뿜기 마련이었고, 해당 코드를 일관성 있게 작성하는 것은 더더욱 어려웠다. 하지만 리액트 쿼리는 이러한 단점을 많이 상쇄시켜준 고마운 친구였다.

하지만 이 고마운 친구가 정확히 어떤 일을 하는 건지, 그리고 나는 이걸 왜 써야 하는지에 대한 심도 있는 고찰이 필요한 시점이 바로 지금이라고 생각하여, 리액트 쿼리가 어쩌다 만들어졌고 이걸 써야 하는 이유와, 리액트 쿼리를 사용함에 있어 필요한 기본 지식을 정리하고자 한다.

# ✒️ Server State

#### Server State란 무엇인가?

1. state (상태)

-   React에서는 렌더링에 영향을 미치는 자바스크립트 Object 라고 정의.
-   Global State (전역 상태) 는 어플리케이션 어디에서든 접근이 가능함.
-   또한 전역 상태의 변화는 곧 애플리케이션의 렌더링에 전반적으로 영향을 미침.

2. Client State

-   UI 테마, 사이드바, 폼 입력 등과 같이 **클라이언트가 소유하고 제어하는** 데이터를 의미.
-   클라이언트에서 항상 제어가 가능하기에 항상 동기적인 상태를 가짐.
    -   local client state : 폼 입력, 사이드바 같이 하나 또는 인접한 컴포넌트에서 사용하는 state.
    -   global client state : 언어, UI 테마 (다크 모드) 와 같이 어플리케이션에 전반적으로 사용되는 state.

3. Server State

-   유저 DB 정보 등과 같이 클라이언트가 **서버로부터 받아오는 모든 데이터** 를 의미함.
-   클라이언트가 제어하고 관리할 수 없기에 **특정 시점**으로부터 받아온 데이터를 사용함. 이로 인해 비동기적인 상태를 가짐.
-   서버와 클라이언트 간의 state가 항상 일치한다는 보장이 없으며, 클라이언트에서 더 이상 **유효하지 않은 데이터**를 소유할 가능성이 있음.

# ✒️ Why React Query?

#### 1. React Query 가 대체 뭔가?

> **fetching, caching, synchronizing and updating server state** in your React applications - tanstack

-   react-query는 서버에서부터 데이터를 fetching하고, 클라이언트와 동기화하는 라이브러리이다.
-   서버로부터 받아온 데이터를 별도의 키를 통해 관리하고, **캐싱**하는 역할도 한다.
-   사용자의 로직에 따라 필요한 데이터를 서버 쪽에서 받아와 백그라운드에서 업데이트 (refetch) 하는 기능을 한다.

#### 2. 이걸 왜 써야 하는가?

1. React 에서는 데이터를 fetching 하거나 update 하는 방법을 제공하지 않는다.

-   따라서 개발자들은 각자의 방식대로 서버로부터 데이터를 fetching 하는 방식을 설계하였다.
-   보통은 Custom Hook을 활용하여 state에 값을 전달하거나, Redux 같은 전역 상태 관리 라이브러리로 인계받은 데이터를 저장한다.

2. Client state와 Server state의 **완전한 분리** 를 위함이다.

-   일반적으로 서버에서 받아온 server state의 경우 전역으로 관리하는 것이 합리적이다. 왜냐하면 state를 필요로 하는 컴포넌트에서 개별적으로 API 호출을 하는 것은 큰 비용 낭비이기 때문이다.
-   하지만 여러 컴포넌트에서 state를 공유하려 props drilling 을 사용하는 것도 한계가 있기에, 전역 상태 관리 라이브러에서 인계받은 server state를 마치 cache 처럼 관리한다면 훨씬 접근이 용이해진다.
-   그러나 Redux 같은 라이브러리를 사용할 경우 서버에서 데이터를 받아와 상태 관리를 하기 위해 별도의 작업 (Redux-Saga) 이 필요하고, 이로 인해 관련 로직 및 모듈이 비대해진다.
-   react-query의 경우 Server State와 관련된 작업을 완벽히 구분지어 제공하기에, Client State 까지 담당하는 전역 상태 관리 라이브러리로부터 이를 분리해준다.

3. Server State 는 항상 최신의 상태임을 보장하지 않기 때문이다.

-   전역 상태 관리 라이브러리의 경우, 서버의 데이터와 다른 값을 보유할 수도 있다. 그에 대한 이유는 앞서 말한 server state의 비동기적인 특성 때문이다.
-   서버에서 받아온 데이터는 데이터를 로드한 시간의 **"스냅샷"** 에 불과하다. 따라서 명시적으로 fetching을 해줘야 최신의 데이터로 전환되는데, 이러한 작업을 여러번 수행하기에는 비용이 크다.
-   react-query의 경우 state의 상태를 fresh, stale, inactive로 구분지으며 유효하지 않은 상태라면 백그라운드 단위에서 데이터를 refetching 하는 기능을 제공한다.

4. react-query를 통해 Server state를 훨씬 쉽게 관리할 수 있다.

-   react query의 경우 아래의 기능을 지원하는 라이브러리이며, 로직 또한 단순한 축에 속한다.
-   현재 데이터가 fetching 되었는지, 아니면 중간에 에러가 발생했는지를 쉽게 체크할 수 있다.

1. 서버로부터 받은 데이터를 캐싱
2. 동일한 데이터에 대한 여러 요청을 단일 요청으로 변환
3. 백그라운드 단에서 오래된 데이터를 자동으로 업데이트
4. 데이터가 현재 오래된 (stale) 상태로 변했는지를 확인.
5. 서버로부터 받은 데이터를 관리하고, GC에 의해 소거되는 과정도 관찰.

# ✒️ Query of React Query

#### 1. React Query 에서 쿼리는 무엇을 의미하는가?

> A query is a declarative dependency on an asynchronous source of data that is tied to a unique key

-   Query는 Server state를 요청하는 함수 (QueryFn) 과 함께 고유한 키 (QueryKey) 로 매핑된다.
-   서버로부터 데이터를 가져올 경우에는 `useQuery`, `useInfiniteQuery` Hook을 쓴다.
-   단, 서버의 데이터를 수정할 경우에는 `useMutation` Hook을 사용해야 한다.

#### 2. queryKey와 queryFn에 대하여.

```javascript
function Todos() {
    const { isLoading, isError, data, error } = useQuery({
        queryKey: ['todos'],
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

-   `queryKey` 의 경우, refetch, caching 등과 같은 작업을 할때 쓰이는 고유한 키다. v4 부터는 무조건 배열로만 key를 선언해야 한다.
-   `queryFn` 의 경우 서버로부터 데이터를 가져오기 위해 쓰이는 비동기 함수를 의미하며, 반드시 Promise를 리턴해야 한다.

#### 3. 쿼리가 데이터를 가져오는데 실패했다면?

-   만약 쿼리가 데이터를 가져오는 데 실패했다면, react query의 경우 기본적으로 세 차례 재요청을 진행한다.
-   만약 재요청 횟수와 인터벌을 수정하고 싶다면, `retry` 옵션과 `retryDelay` 옵션을 별도로 설정하자.

#### 4. 쿼리의 상태는 어떻게 구분짓는가?

-   `useQuery` 가 리턴한 결과 객체의 `state` 프로퍼티로 쿼리의 상태를 파악한다.
-   `state` 속성의 경우 현재 mount 된 쿼리 인스턴스의 상태를 파악할 때 쓰인다.

1. loading : 쿼리가 아직 **데이터를 가지지 않음** 을 의미.
2. error : 쿼리 실행 도중 예상치 못한 **오류가 발생했음** 을 의미.
3. success: 쿼리가 성공적으로 작동되었으며, **데이터가 유효함** 을 의미.

-   `fetchState` 속성의 경우 쿼리가 사용한 `queryFn` 의 동작 결과를 나타낸다.

1. idle: 현재 해당 쿼리의 `queryFn` 이 동작하지 않고 있음을 의미.
2. fetching: 현재 쿼리가 서버로부터 데이터를 fetching 중임을 의미.
3. paused: 쿼리가 데이터를 불러오던 중에 중지되었음을 의미. (네트워크 이슈 등)

-   따라서 `state` 와 `fetchState` 를 모두 확인하여 쿼리의 상태를 유추할 수 있다.

#### 5. 쿼리가 가리키는 데이터의 상태는 어떻게 구분하는가?

1. fresh
    - 해당 쿼리가 보관한 데이터가 아직 **유효**함을 의미.
    - 새로운 쿼리 인스턴스가 mount 되었을 때나, 창이 포커싱 되었을 때와 같이 refetch를 유발하는 이벤트가 trigger 되더라도 이를 진행하지 않는다.
2. stale:
    - 해당 쿼리의 유효 기간이 지나 **오래된 데이터**가 되었음을 의미.
    - 기본적으로 fresh한 데이터는 fetching이 완료되는 즉시 stale한 상태로 변경됨.
    - `staleTime` 옵션을 통해 fresh한 데이터가 stale 한 상태가 되기까지 걸리는 시간을 지정할 수 있음.
3. inactive
    - 해당 쿼리가 `unmount` 되어 현재 비활성화 되었음을 의미.
    - 기본적으로 `inactive` 한 상태가 된 후 5분이 지나면 해당 쿼리는 자동으로 GC에 수집됨.
    - `cacheTime` 옵션을 통해 inactive 된 쿼리 데이터가 GC에게 수집되기까지 걸리는 시간을 지정할 수 있음.

#### 6. 캐싱된 데이터는 어떻게 관리되는가?

-   `useQuery` 혹은 `useInfiniteQuery` 로 생성된 쿼리 인스턴스는 디폴트로 캐싱된 데이터를 `stale` 상태로 본다.
-   단, `staleTime` 속성을 통해 해당 쿼리에 종속된 데이터의 유효 시간을 설정할 수 있다. React Query 에서는 다음과 같은 현상이 발생할 경우 `stale` 한 쿼리를 refetch 한다.
-   하단의 속성은 모두 QueryClient 인스턴스를 생성할 때 옵션으로 on / off 를 설정할 수 있다.

    1. 새로운 쿼리 인스턴스가 mount 되었을 경우. (refetchOnMount)
    2. 사용자가 창을 클릭하여 focus 하였을 경우. (refetchOnWindowFocus)
    3. 네트워크가 재연결 되었을 경우. (refetchOnReconnect)
    4. 사용자가 특정 주기마다 refetch를 진행하도록 한 경우 (refetchInterval)
