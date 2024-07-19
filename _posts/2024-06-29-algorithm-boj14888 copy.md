---
title: "[python] 백준 14888번: 연산자 끼워넣기"
date: 2024-06-29 00:00:00 +0800
categories: [algorithm, python]
tags: [백트래킹, 브루트포스]
math: true
render_with_liquid: false
---

## 문제
문제 출처 : [https://www.acmicpc.net/problem/14888](https://www.acmicpc.net/problem/14888)

N개의 수로 이루어진 수열 A1, A2, ..., AN이 주어진다. 또, 수와 수 사이에 끼워넣을 수 있는 N-1개의 연산자가 주어진다. 연산자는 덧셈(+), 뺄셈(-), 곱셈(×), 나눗셈(÷)으로만 이루어져 있다.

우리는 수와 수 사이에 연산자를 하나씩 넣어서, 수식을 하나 만들 수 있다. 이때, 주어진 수의 순서를 바꾸면 안 된다.

예를 들어, 6개의 수로 이루어진 수열이 1, 2, 3, 4, 5, 6이고, 주어진 연산자가 덧셈(+) 2개, 뺄셈(-) 1개, 곱셈(×) 1개, 나눗셈(÷) 1개인 경우에는 총 60가지의 식을 만들 수 있다. 예를 들어, 아래와 같은 식을 만들 수 있다.

1+2+3-4×5÷6
1÷2+3+4-5×6
1+2÷3×4-5+6
1÷2×3-4+5+6
식의 계산은 연산자 우선 순위를 무시하고 앞에서부터 진행해야 한다. 또, 나눗셈은 정수 나눗셈으로 몫만 취한다. 음수를 양수로 나눌 때는 C++14의 기준을 따른다. 즉, 양수로 바꾼 뒤 몫을 취하고, 그 몫을 음수로 바꾼 것과 같다. 이에 따라서, 위의 식 4개의 결과를 계산해보면 아래와 같다.

1+2+3-4×5÷6 = 1
1÷2+3+4-5×6 = 12
1+2÷3×4-5+6 = 5
1÷2×3-4+5+6 = 7
N개의 수와 N-1개의 연산자가 주어졌을 때, 만들 수 있는 식의 결과가 최대인 것과 최소인 것을 구하는 프로그램을 작성하시오.

### 입력

첫째 줄에 수의 개수 N(2 ≤ N ≤ 11)가 주어진다. 둘째 줄에는 A1, A2, ..., AN이 주어진다. (1 ≤ Ai ≤ 100) 셋째 줄에는 합이 N-1인 4개의 정수가 주어지는데, 차례대로 덧셈(+)의 개수, 뺄셈(-)의 개수, 곱셈(×)의 개수, 나눗셈(÷)의 개수이다.

### 출력

첫째 줄에 만들 수 있는 식의 결과의 최댓값을, 둘째 줄에는 최솟값을 출력한다. 연산자를 어떻게 끼워넣어도 항상 -10억보다 크거나 같고, 10억보다 작거나 같은 결과가 나오는 입력만 주어진다. 또한, 앞에서부터 계산했을 때, 중간에 계산되는 식의 결과도 항상 -10억보다 크거나 같고, 10억보다 작거나 같다.

## 문제 풀이

### 접근

보통 백트래킹 문제를 Python으로 풀때 permutation이나 combination을 자주 사용한다. 하지만 이 문제를 봤을때 재귀적으로 탐색하는 것이 제일 먼저 떠올랐다.

### 풀이

연산자 배열이 [0, 0, 0, 0]이 될때까지 재귀적으로 탐색한다.

### 코드

```python
def searchMax(ans, idx, calculater):
    tmp = -float('inf')
    if idx == n - 1:
        return ans
    if calculater[0] != 0:
        calculater[0] -= 1
        tmp = max(tmp, searchMax(ans + arr[idx + 1], idx + 1, calculater))
        calculater[0] += 1

    if calculater[1] != 0:
        calculater[1] -= 1
        tmp = max(tmp, searchMax(ans - arr[idx + 1], idx + 1, calculater))
        calculater[1] += 1

    if calculater[2] != 0:
        calculater[2] -= 1
        tmp = max(tmp, searchMax(ans * arr[idx + 1], idx + 1, calculater))
        calculater[2] += 1

    if calculater[3] != 0:
        calculater[3] -= 1
        if ans < 0:
            tmp = max(tmp, searchMax(-(-ans // arr[idx + 1]), idx + 1, calculater))
        else:
            tmp = max(tmp, searchMax(ans // arr[idx + 1], idx + 1, calculater))
        calculater[3] += 1

    return tmp


def searchMin(ans, idx, calculater):
    tmp = float('inf')
    if idx == n - 1:
        return ans

    if calculater[0] != 0:
        calculater[0] -= 1
        tmp = min(tmp, searchMin(ans + arr[idx + 1], idx + 1, calculater))
        calculater[0] += 1

    if calculater[1] != 0:
        calculater[1] -= 1
        tmp = min(tmp, searchMin(ans - arr[idx + 1], idx + 1, calculater))
        calculater[1] += 1

    if calculater[2] != 0:
        calculater[2] -= 1
        tmp = min(tmp, searchMin(ans * arr[idx + 1], idx + 1, calculater))
        calculater[2] += 1

    if calculater[3] != 0:
        calculater[3] -= 1
        if ans < 0:
            tmp = min(tmp, searchMin(-(-ans // arr[idx + 1]), idx + 1, calculater))
        else:
            tmp = min(tmp, searchMin(ans // arr[idx + 1], idx + 1, calculater))
        calculater[3] += 1

    return tmp

n = int(input())
arr = list(map(int, input().split()))
cal = list(map(int, input().split()))

print(searchMax(arr[0], 0, cal))
print(searchMin(arr[0], 0, cal))
```

### 후기

코드가 길어지다보니 다 풀어놓고 간단한 실수를 해서 "틀렸습니다"가 3번이나 나왔다. 좀 더 신중하게 제출하는 습관을 들어야겠다. 문제 자체는 어렵지 않은 백트래킹 문제였던 것 같다.