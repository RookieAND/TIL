# 재귀 함수 (Recursive Function)

### 1. 정의

-   자기 자신을 다시 호출하는 함수를 재귀 함수라고 함.
-   파이썬에서는 재귀에 대한 최대 깊이 제한이 정해져 있어, 이를 해제하기 위해 sys 라이브러리를 사용해야 함.
-   재귀 함수를 사용할 경우에는 종료 조건을 무조건 걸어두어야 함. (하단의 코드에는 n이 1이나 2일 경우 종료)
-   종료 조건을 기술해두지 않으면 함수가 무한히 호출되어 예기치 못한 오류를 발생시킬 수 있음.
-   실행 후 마지막 함수가 종료된 시점에서, 역으로 첫 번째 호출된 함수까지 역순으로 함수들이 종료된다.
-   재귀함수가 반복보다 유리할 경우도 있고, 불리한 경우도 있으니 이를 잘 판별해서 사용할 것.

#### 예시 : 재귀함수로 피보나치 수열 문제를 해결하려 할 경우

```python
import sys
sys.setrecursionlimit(10 ** 6)

def fibonacci(n):
    if n == 1 or n == 2:
        return 1
    else:
        return fibonacci(n-1) + fibonacci(n-2)
print(fibonacci(6))
```

#### 예시 : 유클리드 호제법

-   A를 B로 나눈 나머지 R에 대해서, A와 B의 최대 공약수는 B와 R의 최대 공약수와 같다.

```python
def gcd(a, b):
    if a % b == 0:
        return b
    else:
        return gcd(b, a % b)
```
