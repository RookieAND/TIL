# ✒️ Mutation 이란 무엇인가?

- **mutation** 이란, 서버에 Side-Effect 를 일으키도록 하는 함수다. 인계 받은 쿼리 값을 기반으로 새로운 결과를 도출하여 서버에 변경 사항을 적용하도록 요청하는 기능을 한다.
- 보통 **POST, PUT, DELETE** 같이 데이터를 수정하거나 추가하는 요청을 보낼 때 같이 사용되며, react-query 에서는 관련 작업을 위해 useMutation 훅을 지원한다.

### 📒 useQuery 와의 유사점과 차이점

- 일반적으로 쿼리는 자동으로 실행된다. **refetchOnWindowFocus** 옵션을 통해서 창에 포커스를 맞출 경우 백그라운드 단에서 refetch 를 요청하거나, **refetchInterval** 옵션을 사용하여 일정 기간마다 refetch 작업을 수행한다. 굳이 사용자가 refetch 작업을 지정하지 않아도 백그라운드 단에서 진행된다.
- 하지만 mutation 은 사용자가 **선언적으로** 언제 데이터를 업데이트 할지를 직접 지정해줘야 한다. react-query 에서는 **useMutation** 훅의 반환값인 **mutate** 함수를 호출함으로서 mutation 작업을 시행할 수 있게 한다.

```jsx
function AddComment({ id }) {
  // 컴포넌트 내에서 useMutation 훅을 호출하는 것까지는 영향을 미치지 않습니다.
  const addComment = useMutation({
    mutationFn: (newComment) =>
      axios.post(`/posts/${id}/comments`, newComment),
  })

  return (
    <form
      onSubmit={(event) => {
        event.preventDefault()
        // ✅ mutate 함수를 호출함으로서 mutation 작업을 시행합니다.
        addComment.mutate(new FormData(event.currentTarget).get('comment'))
      }}
    >
      <textarea name="comment" />
      <button type="submit">Comment</button>
    </form>
  )
}
```

- 또 다른 차이점은 useQuery 는 하나의 **queryCache** 를 구독하는 여러 **queryObserver** 들을 생성하는 역할이므로 여러 컴포넌트에서 동일한 훅이 호출되더라도 같은 결과를 전달 받는다. (캐싱된 결과 반환)
- 하지만 mutation 의 경우에는 여러 곳에서 훅이 호출되고, 작업이 시행되었다면 실행된 순간마다 각기 다른 값을 반환하므로 같은 결과를 전달 받지 않는다.
- mutation 작업의 결과는 mutationCache 에 저장되고, 이는 **useMutation** 훅을 호출할 때마다 1:1 로 대응된다. 따라서 같은 작업을 하는 **mutateFn** 을 가진 훅이더라도 각기 다른 작업으로 처리될 수밖에 없다.

# ✒️ useMutation 사용법

- mutation 작업도 마찬가지로 Promise 를 반환하는 비동기 함수를 인자로 넣어야 하며, 작업이 성공했을 때와 실패했을 때, 그리고 작업 성공 여부와 관계 없이 항상 실행되는 콜백 함수를 넣을 수 있다.
    - onSuccess, onSettled, onError 옵션을 통해 useMutation 훅의 작업 흐름을 제어할 수 있다.

```jsx
import { useMutation } from "react-query";
const { data, isLoading, mutate } = useMutation(mutationFn, options);

mutate(variables, {
  onError,
  onSettled,
  onSuccess,
});
```

- hook 을 호출하여 나온 결과 값 중, `mutate` 객체를 통해 실질적으로 데이터를 수정하도록 요청하는 작업을 수행할 수 있으며, mutate 객체의 결과에 따라서도 콜백 함수를 추가할 수 있다.
    - **onSuccess, onSettled, onError** 옵션을 통해 useMutation 훅의 작업 흐름을 제어할 수 있다.
    - **onMutate**의 경우 mutation 작업 이전에 실행되는 함수이므로 optimistic update 에 쓰인다. 또한 onMutate 에서 반환된 값은 onSuccess, onSettled, onError 콜백의 context 인자에 담긴다.
    - **useMutation** 내에 정의된 콜백 함수가 먼저 실행되고 이후 **mutate** 에 정의된 콜백 함수가 실행된다.
    - 만약 컴포넌트가 중간에 unmount 되면 더 이상 callback **이 실행되지 않으므로** 이를 유의해야 한다.

```jsx
useMutation(addTodo, {
   onSuccess: (data, variables, context) => {
     // I will fire first
   },
   onError: (error, variables, context) => {
     // I will fire first
   },
   onSettled: (data, error, variables, context) => {
     // I will fire first
   },
  });

mutate(todo, {
   onSuccess: (data, variables, context) => {
     // I will fire second!
   },
   onError: (error, variables, context) => {
     // I will fire second!
   },
   onSettled: (data, error, variables, context) => {
     // I will fire second!
   },
});
```

- `variables` 인자의 경우 **mutate** 함수가 호출되었을 때 인자로 들어간 값이 담긴다.
- `error` 인자의 경우 mutate 함수가 에러를 일으켰을 때 반환된 결과가 담긴다.
- `data` 인자의 경우 mutationFn 이 실행된 결과 값이 담긴다. (onSuccess, onSettled 에 쓰임)
- `context` 의 경우 onMutate 콜백 함수가 반환한 값이 담긴다. (onError, onSettled 에 쓰임)

# ✒️ 결과를 업데이트 하는 방법

- 첫 번째는 mutation 이 성공적으로 수행되었을 때 실행되는 onSuccess 콜백 함수 내에서 특정 쿼리를 invalid 처리 함으로서 백그라운드 단에서 자동으로 refetch 되게끔 하는 기법이 있다. (**Query Invalidation**)
- 두 번째는 mutation 작업의 결과 값을 인계 받아 onSuccess 콜백 함수 내에서 특정 쿼리에 내장된 데이터를 queryClient.setQueryData 메서드로 업데이트 하는 방식이 있다. (****Updates from Mutation Responses****)
- 세 번째는 mutation 성공 여부와 관계 없이, 일단 해당 작업이 성공적으로 수행되었으리라 기대하고 클라이언트 단에서 mutation 작업 수행 전에 쿼리의 데이터를 먼저 변경해버리는 방식이 있다. (****Optimistic Updates)****

### 📒 Query Invalidation

- `QueryClient.invalidateQueries` 메서드를 시용하면 특정 쿼리의 status를 즉시 stale로 변경하며 백그라운드 단에서 refetch가 일어나도록 유도한다.
- 해당 메서드의 경우 각 쿼리 별로 지정된 staleTime 설정을 모두 무시하며, useQuery 혹은 다른 hook 에 의해 렌더링 된 상태이더라도 백그라운드 단에서 refetch 작업이 이루어진다.
- **Query Filter** 를 사용하여 배열 형태의 query key 중 특정 범주의 쿼리 상태만을 stale 하게 만들 수도 있다. 혹은 exact, predict 옵션을 사용하여 특정 조건에 부합하는 쿼리만 stale 하게 처리할 수도 있다.
- [https://tanstack.com/query/v4/docs/react/guides/filters#query-filters](https://tanstack.com/query/v4/docs/react/guides/filters#query-filters) : Query Filter 사용법

```jsx
import { useQuery, useQueryClient } from '@tanstack/react-query'

const queryClient = useQueryClient()

// 1번 쿼리, (key : ['todos'])
const todoListQuery = useQuery({
  queryKey: ['todos'],
  queryFn: fetchTodoList,
})
// 2번 쿼리, (key : ['todos', { page: 1 }])
const todoListQuery = useQuery({
  queryKey: ['todos', { page: 1 }],
  queryFn: fetchTodoList,
})

// 1번, 2번 쿼리 둘 다 invalid 처리 되어 stale 상태가 된다.
queryClient.invalidateQueries({ queryKey: ['todos'] })

// 2번 쿼리만 invalid 처리 된다.
queryClient.invalidateQueries({ queryKey: ['todos', { page: 1 }] })

// 1번 쿼리만 invalid 처리 된다. (exact 옵션을 키면 하위 쿼리들까지 invalid 처리 되지 않는다)
queryClient.invalidateQueries({ queryKey: ['todos'], exact: true })

// 1, 2번 쿼리 중 inactive 상태인 쿼리만 invalid 처리 된다.
queryClient.invalidateQueries({ queryKey: ['todos'], type: 'inactive' })

// 1, 2번 쿼리 중 fresh 상태인 쿼리만 invalid 처리 된다.
queryClient.invalidateQueries({ queryKey: ['todos'], stale: false })

// 2번 외에도, page 값이 1 이상인 query 는 모두 invalid 처리 된다. (predict 로 filter 함수 생성)
queryClient.invalidateQueries({
  predicate: (query) =>
    query.queryKey[0] === 'todos' && query.queryKey[1]?.page >= 1,
})
```

- 이를 mutation 작업과 연관지으면, mutate 작업이 **성공적으로 수행되었을 때** 업데이트 해야 하는 특정 쿼리를 invalid 하게 만들어 refetch 를 유도할 수 있다.

```jsx
import { useMutation, useQueryClient } from '@tanstack/react-query'
const queryClient = useQueryClient()

// mutate 작업이 성공적으로 끝났다면, ['todos'] 키 값을 가진 하위 쿼리들이 모두 invalid 처리 된다.
// 이후 해당 쿼리들은 refetch 작업이 수행되어 업데이트된 결과 값을 컴포넌트에 전달하게 된다.
const mutation = useMutation({
  mutationFn: addTodo,
  onSuccess: () => {
    queryClient.invalidateQueries({ queryKey: ['todos'] })
  },
})
```

### 📒 Updates from Mutation Responses

- mutation 작업의 결과를 인계 받은 후, 이를 직접 쿼리에 적용할 수도 있다.
- invalidation 방식의 경우 쿼리의 상태를 stale 하게 만들기 때문에 이후 refetch 작업이 필연적으로 발생하는데, 굳이 불필요하게 API 를 한번 더 요청하고 싶지 않다면 이 방식을 쓰자.
- 즉, mutateFn 의 반환 값이 존재하고, 이것이 유효한 값임을 보장할 때는 굳이 refetch 를 유도하지 않고 서버로부터 반환된 결과 값을 즉시 업데이트 하는 것이 더욱 바람직하다.

```jsx
const useMutateTodo = () => {
  const queryClient = useQueryClient()
  return useMutation({
    mutationFn: editTodo,
		// mutate 실행 결과로 받은 값인 data를 query 에 직접 업데이트 해준다.
		// cache 값이 변경되었으므로, 이를 관측한 queryObserver 를 구독하는 컴포넌트들은 모두 업데이트 된다.
		// 불필요한 refetch 없이도 캐싱된 값을 업데이트 할 수 있어 더욱 효율적이다.
    onSuccess: (data, variables) => {
      queryClient.setQueryData(['todo', { id: variables.id }], data)
    },
  })
}
```

- `QueryClient.setQueryData` 메서드를 시용하여 특정 쿼리에 저장된 캐싱 데이터를 수동으로 변경해준다.
- 단 캐싱된 데이터가 참조형일 경우에는 불변성을 지켜서 새로운 객체, 혹은 배열로 수정된 값을 넣어줘야 한다. 그렇지 않을 경우 동작은 잘 되지만 내부적으로 버그가 발생할 수 있다고 한다. (공식 문서 참조)

<aside>
💡 Updates via **`setQueryData`** must be performed in an *immutable* way. **DO NOT** attempt to write directly to the cache by mutating data (that you retrieved via from the cache) in place. It might work at first but can lead to subtle bugs along the way.

</aside>

```jsx
ueryClient.setQueryData(
  ['posts', { id }],
  (oldData) => {
    if (oldData) {
			// ❌ 이렇게 직접적으로 객체 내부의 속성을 수정하는 것은 좋지 않다.
      oldData.title = 'my new post title'
    }
    return oldData
  })
queryClient.setQueryData(
  ['posts', { id }],
	// ✅ 불변성을 지켜서 업데이트 하는 것이 권장된다.
  (oldData) => oldData ? {
    ...oldData,
    title: 'my new post title'
  } : oldData
)

```

### 📒 Optimistic Update

- mutation 작업이 시행되기 이전에 해당 작업이 이미 성공했다고 가정하고, **미리 쿼리에 캐싱된 값을 예상된 결과로 즉시 업데이트** 하는 기법이다.
- 해당 API 가 무조건 성공한다는 것을 전제로 수행하기 때문에 만약 실패했을 경우 mutation 작업이 진행되기 전의 값으로 쿼리 데이터를 되돌린다.
- 값이 변경되었다가 다시 되돌아가는 과정은 **UX 적으로 매우 좋지 않기 때문에** 거의 확실하게 요청이 성공할 것 같은 케이스에 대해서만 적용하자.

```jsx
const queryClient = useQueryClient()
useMutation({
  mutationFn: updateTodo,
  // mutate 가 호출되었을 때, api 콜 이전에 미리 쿼리 데이터를 수정한다.
  onMutate: async (newTodo) => {
		// mutationFn 이 성공적으로 실행될 것이라 가정하고 현재 쿼리 데이터를 수정했기 때문에
		// 만약 해당 queryKey 에 대해서 다른 refetch 작업이 수행된다면 데이터가 덮어 씌워지므로 이를 방지하기 위함이다.
    await queryClient.cancelQueries({ queryKey: ['todos'] })
		// mutation 작업 이전의 값을 가져와 context 로 이전시킨다.
    const previousTodos = queryClient.getQueryData(['todos'])
	  // 쿼리 데이터를 수정 이후의 값으로 직접, 명시적으로 업데이트 해준다. 
    queryClient.setQueryData(['todos'], (old) => [...old, newTodo])
			// 업데이트 이전의 값이 담긴 context 객체를 return 한다.
    return { previousTodos }
  },
	// 만약 mutate 작업이 실패했을 경우, 이전에 저장했던 값으로 다시 롤백 시킨다.
  onError: (err, newTodo, context) => {
    queryClient.setQueryData(['todos'], context.previousTodos)
  },
	// 실패 혹은 성공 여부와 관계 없이 mutation 작업 이후에는 쿼리를 stale 하게 만들어 refetch를 유도한다. 
  onSettled: () => {
    queryClient.invalidateQueries({ queryKey: ['todos'] })
  },
})

```

- `onMutate` 콜백 내에서 `cancelQueries` 를 호출하는 이유는, Optimistic Update 을 진행하면서 외부에 refetch 작업이 발생할 경우 값이 재수정 되기 때문이다.
- 이와 관련된 자세한 내용은 [https://velog.io/@mskwon/react-query-cancel-queries](https://velog.io/@mskwon/react-query-cancel-queries) 에서 상세히 다뤄주셨다. 실제 문제가 발생한 케이스를 들어 설명해주셨다