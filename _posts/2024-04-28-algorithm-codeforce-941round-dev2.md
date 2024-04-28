---
title: "Codeforces #941 (div.2) 후기"
date: 2024-04-28 00:00:00 +0800
categories: [contest, codeforces]
tags: ["#941", div2]
math: true
render_with_liquid: false
---
### 코드포스 첫번째 회고
코드포스를 시작한지는 얼마되지 않았다. contest 기준으로 3번정도 참가해봤던 것 같다.<br> 아무래도 영어에 능숙하지 않은 점이 제일 어렵다. 모르는 표현들이 많이 나와서 아직까진 변역기에 도움을 많이 받고 있다. 그리고 div2 기준으로 a, b 많으면 c, d까지가 그리디 문제이다. 개인적으로 그리디가 약하다고 생각하기에 그리디를 연습하는데는 좋지만 아무래도 초반에 잘 스퍼트가 나지 않는다.

기존에는 백준을 가장 많이 풀었었다. 아무래도 수준별로 잘 나눠져 있는 레이팅 시스템과 다른 사람과 직접적으로 비교할 수 있는 경쟁 시스템이 매력적이었다. 하지만 국내에서 유명한 만큼 다른 사람의 코드를 참고하기도 너무 쉽고 문제수로 랭킹을 나누는것도 실력순은 아니라고 생각했다.

그에 비해 코드포스는 상당히 매력적이다. 주1회이상 대회가 열리고 대회 결과로 레이팅이 정해진다. 당연히 다른사람의 코드 참고도 불가능하다. 그리고 대회가 끝나면 틀린 문제의 반례도 확인이 가능하고 테스트케이스도 비교적 다양해서 업솔빙이 편리하다.

다만 아직 영어가 미숙하고, 문제에도 적응하고 있는 단계라서 노력이 많이 필요해 보인다. ~~ps 괴물이 너무 많다~~ 꾸준이 contest 만이라도 참가해서 실력을 끌어 올려보고 싶다.

## 문제. A
문제 출처 : [https://codeforces.com/contest/1966/problem/A](https://codeforces.com/contest/1966/problem/A)
<center>
A. Card Exchange<br>
time limit per test1 second<br>
memory limit per test256 megabytes<br>
inputstandard input<br>
outputstandard output
</center><br>

You have a hand of 𝑛 cards, where each card has a number written on it, and a fixed integer 𝑘. You can perform the following operation any number of times:

Choose any 𝑘 cards from your hand that all have the same number. Exchange these cards for 𝑘−1 cards, each of which can have any number you choose (including the number written on the cards you just exchanged).
Here is one possible sequence of operations for the first example case, which has 𝑘=3:

![A941](/assets/img/A941.png)

What is the minimum number of cards you can have in your hand at the end of this process?

Input<br>
The first line of the input contains a single integer 𝑡
 (1≤𝑡≤500
) — the number of test cases. The description of the test cases follows.

The first line of each test case contains two integers 𝑛 and 𝑘 (1≤𝑛≤100, 2≤𝑘≤100) — the number of cards you have, and the number of cards you exchange during each operation, respectively.

The next line of each test case contains 𝑛 integers 𝑐1,𝑐2,…𝑐𝑛 (1≤𝑐𝑖≤100) — the numbers written on your cards.

Output<br>
For each test case, output a single integer — the minimum number of cards you can have left in your hand after any number of operations.
<br>

### 접근
코드포스 A 문제는 거의 그리디 문제가 나오는 것 같다. 당연히 그리디부터 생각했다.<br>
조금만 생각해보면 같은 숫자가 k개 이상 있는 카드라 한 그룹이라도 있으면 원하는 카드를 k-1장 얻을 수 있기 때문에 어떤 카드는 소거가 가능하다.<br>

### 풀이
각 숫자 카드가 몇개씩 있는지 확인하고 k개가 있는 숫자가 1그룹이라도 있으면 답은 k-1 없다면 답은 n 이다.

### 코드

```python
t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    cards = list(map(int, input().split()))
    bit = [0] * 101
    for card in cards:
        bit[card] += 1
    if max(bit) >= k:
        print(k - 1)
    else:
        print(n)
```

### 후기
div2 기준 A문제는 평균 10분에서 20분 정도 걸리는 것 같다. 영어라서 내가 이해 하는데 시간이 걸리는건지 코드포스에서 문제를 이해하기 어렵게 내는건지는 잘 모르겠다.