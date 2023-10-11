> 왜 **Suspense** 를 썼는데 데이터가 `undefined` 로 추론되는 걸까..?

## 1. 개요
현재 개발 중인 pureureum (푸르름) 프로젝트의 경우 react-query 를 적극적으로 사용하여 서버에서 받은 여러 데이터를 캐싱하고 있다. 

하지만 Typescript 와 React-Query 를 사용했을 때 불편한 점이 `useQuery` 의 return 값 중, data의 타입이 `T | undefined` 로 잡히는 것이었다.

이게 왜 불편하냐면, 분명 인계된 `data` 의 타입이 확실히 인계되었음이 보장됨에도 불구하고 타입 체커에서는 자꾸만 `data` 의 타입을 좁히게끔 유도한다는 의미이다. 

현재 프로젝트에서는 `suspense: true` 옵션을 켜서 React 의 Suspense 와 함께 이를 사용하고 있음에도 data 의 타입이 undefined 로 추론되고 있다.

```ts
const ProjectApplyTemplate = () => {
  const router = useRouter();
  const projectId = Number(router.query.pid);

  const { data: userProfileData } = useGetUserProfile();
  const { data: projectDetailData } = useGetProjectDetail(projectId);

  // 문제 발생 : userProfileData 는 undefined 일 수도 있습니다.
  const { name, nickname, birthday, gender, phoneNumber } = userProfileData;
  const { projectInformation, projectPayment } = projectDetailData;
  
  // 이런 식으로 타입을 좁힌 후에 사용해야 해서 불편하다.
  if (!userProfileData || !projectDetailData) return null;
```

이러한 문제는 비단 필자 뿐만이 아니라 react-query 를 사용하는 여러 사용자들도 불편함을 호소하고 있다. issue 를 찾아보니 정말 많은 사람들이 Suspense 를 사용했음에도 왜 데이터의 무결성을 한번 더 검증해야 하는지에 대한 의문을 남겼다.

물론 fetching 이 실패했을 경우에 대한 대비라던가, enabled 옵션이 꺼졌다거나 하는 케이스로 인해서 데이터가 항상 원하는 타입으로 반환될 것이라는 보장이 없으므로 undefined 가 나올 수도 있다는 가능성은 늘 존재하지만, 적어도 이 부분은 ErrorBoundary 와 Suspense 의 조합으로 다르게 해결할 수 있다고 생각한다.

다행이도 머지않아 릴리즈 될 v5 의 경우에는 Suspense 옵션이 켜졌을 경우 항상 data 의 타입을 올바르게 추론하는 useSuspensedQuery 훅을 추가할 예정이라고 한다. 추가로 `@suspensive/react-query` 라는 라이브러리에서도 `useSuspenseQuery` 훅을 지원한다.

- https://suspensive.org/docs/react-query/src/useSuspenseQuery.i18n
- https://tanstack.com/query/v5/docs/react/guides/migrating-to-v5

## 2. 라이브러리 쓰면 되는 거 아니냐?

물론 react-query 버전을 화끈하게 v5 로 올려버리는 방법 또한 존재한다. 그리고 뭣하면 `@suspensive/react-query` 라이브러리를 쓰면 된다. 근데 왜 안쓰냐? 고작 요거 하나 잡겠다고 버전 업을 하기에는 v4 기반으로 개발된 코드 베이스를 대대적으로 엎어야 하고, 라이브러리를 설치하기에는 그냥 좀 싫었다.

따라서 `toss/slash` 에서 개발한 useSuspensedQuery 코드를 분석하고, 내 입맛대로 이를 살짝 개조하여 올바르게 타입을 반환하는 훅을 만들어야겠다고 생각했다.

아래는 필자가 개조한 useSuspensedQuery 코드의 전문이다.

```ts
import {
  QueryClient,
  type QueryKey,
  type UseQueryOptions,
  type UseQueryResult,
  useQuery,
} from '@tanstack/react-query';

import type { ApiError } from '@/constants/types';

// UseQueryOption 에서 suspense, networkMode 를 제외한 나머지를 반환하는 타입 SuspendedUseQueryOptions
export type SuspendedUseQueryOptions<
  TQueryFnData = unknown,
  TError = unknown,
  TData = TQueryFnData,
  TQueryKey extends QueryKey = QueryKey,
> = Omit<
  UseQueryOptions<TQueryFnData, TError, TData, TQueryKey>,
  'suspense' | 'networkMode'
>;

// QueryResult 타입을 기반으로, Suspense 상태에서 정상적으로 data가 추론되도록 하는 타입
export interface SuspendedUseQueryResult<TData>
  extends Omit<
    UseQueryResult,
    'data' | 'status' | 'error' | 'isLoading' | 'isError' | 'isFetching'
  > {
  data: TData;
  status: 'success' | 'idle';
}

// 요청이 성공했을 경우 데이터의 추론은 TData 제네릭 타입으로 추론되어야 한다.
type SuspendedUseQueryResultOnSuccess<TData> =
  SuspendedUseQueryResult<TData> & {
    status: 'success';
    isSuccess: true;
    isIdle: false;
  };

// IDLE 상태일 때는 아직 Query 가 trigger 되지 않았으므로, 데이터의 추론이 undefined 가 되어야 함.
type SuspendedUseQueryResultOnIdle = SuspendedUseQueryResult<undefined> & {
  status: 'idle';
  isSuccess: false;
  isIdle: true;
};

// Override Function 1 : enabled 이 true 인 경우, 요청이 성공했다고 가정하고 비동기 호출의 결과 (TData) 로 data를 추론한다.
export function useSuspendedQuery<
  TQueryFnData = unknown,
  TError = unknown,
  TData = TQueryFnData,
  TQueryKey extends QueryKey = QueryKey,
>(
  options: Omit<
    SuspendedUseQueryOptions<TQueryFnData, TError, TData, TQueryKey>,
    'enabled'
  > & {
    enabled?: true;
  },
): SuspendedUseQueryResultOnSuccess<TData>;

// Override Function 2 : enabled 가 false 인 경우, queryState 가 idle 이므로 데이터를 undefined 로 추론한다.
export function useSuspendedQuery<
  TQueryFnData = unknown,
  TError = unknown,
  TData = TQueryFnData,
  TQueryKey extends QueryKey = QueryKey,
>(
  options: Omit<
    SuspendedUseQueryOptions<TQueryFnData, TError, TData, TQueryKey>,
    'enabled'
  > & {
    enabled: false;
  },
): SuspendedUseQueryResultOnIdle;

export function useSuspendedQuery<
  TQueryFnData = unknown,
  TError = ApiError,
  TData = TQueryFnData,
  TQueryKey extends QueryKey = QueryKey,
>(
  options: SuspendedUseQueryOptions<TQueryFnData, TError, TData, TQueryKey>,
): SuspendedUseQueryResult<TData> {
  return useQuery({
    suspense: true,
    networkMode: 'always',
    ...options,
  }) as SuspendedUseQueryResult<TData>; // status : 'error' 에 대한 타입 추론은 하지 않도록 한다 (Type Assertion)
}

export default useSuspendedQuery;
```

내부 구성은 상당히 단촐한데, 쉽게 정리하자면 아래 케이스에 따른 return 타입을 추론하도록 하였다.

1. enabled 옵션이 `false` 인 경우, 현재 query 의 state 는 `idle` 이므로 data 가 `undefined` 가 되도록 타입을 추론시킨다.

```ts
// Override Function 2 : enabled 가 false 인 경우, queryState 가 idle 이므로 데이터를 undefined 로 추론한다.
export function useSuspendedQuery<
  TQueryFnData = unknown,
  TError = unknown,
  TData = TQueryFnData,
  TQueryKey extends QueryKey = QueryKey,
>(
  options: Omit<
    SuspendedUseQueryOptions<TQueryFnData, TError, TData, TQueryKey>,
    'enabled'
  > & {
    enabled: false;
  },
): SuspendedUseQueryResultOnIdle;
```

2. enabled 옵션이 `true` 인 경우, 현재 query 가 무조건 성공했다고 가정하고 data 를 `TData` Generic 타입으로 추론되도록 설정한다.

```ts
// Override Function 1 : enabled 이 true 인 경우, 요청이 성공했다고 가정하고 비동기 호출의 결과 (TData) 로 data를 추론한다.
export function useSuspendedQuery<
  TQueryFnData = unknown,
  TError = unknown,
  TData = TQueryFnData,
  TQueryKey extends QueryKey = QueryKey,
>(
  options: Omit<
    SuspendedUseQueryOptions<TQueryFnData, TError, TData, TQueryKey>,
    'enabled'
  > & {
    enabled?: true;
  },
): SuspendedUseQueryResultOnSuccess<TData>;
```

3. 나머지 케이스의 경우에도 query 가 무조건 성공했다고 가정하고 data 를 `TData` Generic 타입으로 추론되도록 설정한다.

결국 정리하자면 `enabled: false` 가 아닌 나머지 케이스에 대해서는 해당 queryFn 이 무조건 성공했다고 가정하고, 사용자가 인자로 넣은 `TData` Generic Type 을 data 의 타입으로서 추론되도록 한 것이다.

이렇게 하니 코드 상에서 data 가 존재하는지를 한번 더 체크하지 않아도 되었기에 불필요한 유효성 검사를 진행했던 코드를 많이 줄일 수 있었다.

중요한 점은 해당 훅은 `useQuery` 훅에 들어갈 인자를 **객체로 넣는 케이스에만** 통용된다는 점이다. 만약 훅을 아래와 같이 사용한다면 타입 에러가 100% 날 것이다.

```ts
// 요런 케이스는 된다.
export const useGetProjectList = ({
  searchType,
  category,
}: ProjectReqParams['main']) =>
  useSuspendedQuery<MainProjectListOutput>({
    queryFn: () =>
      ProjectRepository.getMainProjectListAsync({ searchType, category }),
    queryKey: QUERY_KEY.PROJECT.main({ searchType, category }),
    staleTime: 1000 * 60 * 5,
    useErrorBoundary: true,
  });

// 이렇게 인자를 여러 개 넣어서 실행하는 케이스는 안된다. (별도의 함수 override 가 필요)
export const useGetProjectList = ({
  searchType,
  category,
}: ProjectReqParams['main']) =>
  useSuspendedQuery<MainProjectListOutput>(
	ProjectRepository.getMainProjectListAsync({ searchType, category }),
    QUERY_KEY.PROJECT.main({ searchType, category }),
    {
        staleTime: 1000 * 60 * 5,
    	useErrorBoundary: true,
    }
  });

```

## 3. 결론

`useSuspensedQuery` 도입 이후 react-query 를 사용하기가 한결 더 편해졌다고 생각한다. 타입 체커가 의도한대로 데이터 타입을 추론하는 것만으로도 솔직히 이 훅의 존재 의의는 달성했다고 생각한다.

만약 `TData | undefined` 로 인해 react-query 에 고통받는 분이 있다면
