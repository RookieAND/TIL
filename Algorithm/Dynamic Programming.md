# 동적 프로그래밍 (Dynamic Programming)

### 1. 정의

-   메모리를 적절히 사용하여 수행 시간에 따른 효율성을 비약적으로 향상시키는 방법
-   이미 계산된 결과를 별도의 메모리에 저장하여 추후 이를 다시 계산하지 않도록 한다.
-   동적 프로그래밍의 걍우 탑 다운(하향식), 바텀 업(상향식) 과 같이 총 두 가지 방식이 존재한다.
-   다이나믹은 별 다른 의미 없이 사용된 단어임 (동적 할당되는 별다른 관계 없음)

### 2. 사용 조건

동적 프로그래밍 사용 조건은 아래와 같다.

-   최적 부분 구조 : 큰 문제를 작은 문제로 나누어, 작은 문제의 답을 모아 해결 가능한 상황
-   중복되는 부분 문제 : 동일한 작은 문제가 반복적으로 호출되어 이를 해결해야 할 때.

#### 예시: 피보나치 수열

-   점화식 : An = An-1 + An-2 (A1=1, A2 = 2) -> 꼭 구해야 한다!
-   재귀함수로 이를 해결할 경우, 시간 복잡도는 O(2N) 으로 지수 함수 형태를 띄게 된다 (비효율적)
-   따라서, 반복적으로 실행되는 문제를 사전에 저장해두어, 추후 이를 필요로 할 때 꺼내 쓰도록 해야 한다.

1. 사용 조건 만족 확인

    - 최적 부분 구조 : 큰 문제를 작은 문제로 나눌 수 있음 (An = An-1 + An-2, 부분 구조 확인)
    - 중복되는 부분 문제 : f(2) 와 f(1) 같이, 동일한 작은 문제가 계속해서 일어남을 확인.

2. 메모이제이션 : 작은 문제를 계산하여 나온 값은 별도의 List에 저장하여 추후 이를 사용할 수 있도록 함. (캐싱)

### 3. 탑 다운

-   구현 과정에서 재귀 함수를 사용하고, 작은 문제가 모두 해결되었을 때 큰 문제가 해결되도록 유도

#### 예시 : 탑 다운 재귀 함수 방식으로 피보나치 수열 문제를 해결하려 할 경우

```python
import sys

N = int(sys.stdin.readline())
dp = [0] * (N + 1)

# 재귀함수를 통해 해결해나가는 탑 다운 방식 채용

def fibonacci(x): # x가 1 또는 2라면 값이 1이므로 이를 리턴 # 작은 문제에 대한 값을 이미 알고 있으니, 이를 사용하면 됨.
if 1 <= x <= 2:
return 1 # 만일 메모이제이션에 해당되는 값이 있다면 이를 사용함. # 굳이 더 많은 재귀를 진행하지 않아도 기록된 값을 활용하면 됨
if dp[x] != 0:
return dp[x] # 그렇지 않을 경우, 재귀 함수를 통해 결과를 반환해야 함 # 반환된 결과는 생성한 dp 테이블에 새로이 적재되어짐.
dp[x] = fibonacci(x-1) + fibonacci(x-2)
return dp[x]


print(fibonacci(N))
```

### 4. 보텀 업

-   작은 문제를 먼저 계산하고, 이를 활용하여 큰 문제를 단계적으로 해결해 나가는 방식 (전형적인 형태)
-   결과 저장용 리스트는 DP 테이블이라 하며, 이전에 계산한 결과를 일시적으로 기록해 놓는 용도

#### 예시 : 보텀 업 반복문 방식으로 피보나치 수열 문제를 해결하려 할 경우

```python
import sys

N = int(sys.stdin.readline())

# 메모이제이션 생성, 피보나치 수열에 따라 인덱스 1, 2번의 값을 1로 초기화

# 점화식에서의 시작 값을 초기화 한 후, 하단의 반복문을 통해 새롭게 값들을 할당시킴

dp = [0] \* (N + 1)
dp[1] = dp[2] = 1

# 3부터 N까지 반복하여, dp 테이블에 순차적으로 값을 집어넣음

for i in range(3, N+1):
dp[i] = dp[i-1] + dp[i-2]

print(dp[N])
```

-   두 가지 경우를 통해 문제를 해결할 경우, 시간 복잡도는 O(N) 이다.

### 5. DP 문제 접근 방식

1. 해당 문제가 다이나믹 프로그래밍 유형임을 파악하는 것이 중요하다.
2. 그리디, 구현, 완전 탐색 등으로 문제를 해결할 수 있는지를 먼저 검토한다.
3. 그런 후, 점화식을 도출하여 재귀 함수로 비효율적인 완전 탐색 코드를 작성한 후, 메모이제이션을 적용해본다.
4. 탑 다운에서 보텀 업으로 코드를 개선할 수 있는지를 판별하여 이를 추가로 적용해본다.

#### 예시 : 개미 전사 문제 코드

```python
import sys

N = int(sys.stdin.readline())
data = [map(int, sys.stdin.readline().split())]

dp = [0] \* (N + 1)
dp[0] = data[0]
dp[1] = max(data[0], data[1])

for i in range(N + 1):
dp[i] = max(dp[i-1], dp[i-2] + data[i])

print(dp[N-1])
```

#### 예시 : 탐색 문제 코드

```python
import sys
from math import inf

N, M = map(int, sys.stdin.readline().split())
cashs = sorted([int(sys.stdin.readline()) for _ in range(N)])
dp = [inf] * (M + 1)
dp[0] = 0

for cash in cashs:
for price in range(cash, M+1):
if dp[price - cash] != inf:
dp[price] = min(dp[price], dp[price - cash] + 1)

print(dp[M] if dp[M] != inf else -1)
```

#### 예시 : 금광 매설 문제 코드

```python
import sys

cnt = int(sys.stdin.readline())
for _ in range(cnt):
N, M = map(int, sys.stdin.readline().split())
case = list(map(int, sys.stdin.readline.split()))
dp, index = [], 0
for i in range(N):
dp.append(case[index:index + M])
index += M

    # 1행부터 시작해야 함 (0열부터 시작할 수 없음)
    for j in range(1, M):
        # 모든 열을 대상으로 반복문을 통해 최적의 값을 도출
        for i in range(N):
            # i = 0 의 의미는 현재 해당 위치가 테이블 가장 위라는 뜻.
            # 따라서 좌측 상단은 존재하지 않으므로 0으로 설정.
            if i == 0:
                left_up = 0
            else:
                left_up = dp[i - 1][j - 1]
            # i = N-1 이라는 의미는 현재 위치가 테이블 가장 아래라는 뜻
            # 따라서 좌측 하단은 존재하지 않으므로 0으로 설정
            if i == N - 1:
                left_down = 0
            else:
                left_down = dp[i + 1][j - 1]
            # 중단의 경우 열의 위치와 상관 없이 늘 존재하므로 별도의 조건을 두지 않음
            left = dp[i][j-1]
            # 금의 총량은 현재 자리에 매장된 금 + 좌측 상단, 중단, 하단의 금 매설량의 최댓값을 합해야 함.
            dp[i][j] = dp[i][j] + max(left_up, left, left_down)
        result = 0
        # DP 테이블의 가장 오른쪽 행에 모든 경우에 따른 금 총량이 저장될 것
        # 따라서 이를 반복문으로 순회하여 가장 높은 수량의 매설량이 무엇인지를 도출해야 함.
        for i in range(N):
            result = max(result, dp[i][M-1])
        print(result)
```
