# 그리디 알고리즘 (Greedy)

### 1. 정의

-   현재 상황에서 지금 당장 좋은 것만 고르는 방법을 의미하며, 그리디 해법은 정당성의 분석이 중요하다.
-   단순히 가장 종하 보이는 것을 반복적으로 선택하더라도 최적의 해를 구할 수 있는지를 꼭 검토해야 함.
-   문제를 풀기 위한 최소한의 아이디어를 생각해야 함. 사고력이 많이 요구되는 스타일.

#### 예시 : 거스름 돈 문제

```python
import sys

# 존재하는 동전들을 사전에 list에 담아둠 (내림차순으로)
coins = [500, 100, 50, 10, 5, 1]
n, result = int(sys.stdin.readline()), 0
left_money = 1000 - n

# 동전은 가치가 큰 순서부터 낮은 순서대로 진행
for coin in coins:
    # 아직 잔돈이 남아있다면, 해당 동전을 최대로 사용하여 잔돈에서 제외
    if left_money > 0:
        result += left_money // coin
        left_money -= left_money % coin
    else:
        break
print(result)

```
