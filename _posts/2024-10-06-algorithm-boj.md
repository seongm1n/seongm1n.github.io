---
title: "[java] 백준 30646번: 최대 합 순서쌍의 개수"
date: 2024-10-06 00:00:00 +0800
categories: [algorithm, java]
tags: [누적 합, 해시맵]
math: true
render_with_liquid: false
---

오랜만에 블로그, 간단하게 회고 해보자면 최근 취준을 하면서 자바로 코테를 준비하고 있다. 확실히 취업시장이 어렵기도 하고, 아직 취업을 하기엔 기술과 프로젝트가 많이 부족한 것 같다. 그래서 마음에 드는 부트캠프도 지원하고 있다. ~~(자소서 쓰는데 생각보다 너무 오래걸린다.)~~

정처기 실기도 얼마 안남아서 이젠 진짜 쉬는날이 없을 것 같다.

## 문제
문제 출처 : [https://www.acmicpc.net/problem/30646](https://www.acmicpc.net/problem/30646)

크기가 N인 배열 a가 주어진다. 배열 a의 임의의 위치를 나타내는 두 수 i, j를 골랐을 때, 아래 두 조건을 만족하면 같은 수 순서쌍 (i, j)를 만들 수 있다.

1 ≤ i ≤ j ≤ N
ai = aj
만들어진 같은 수 순서쌍 (i, j)의 합은 ai부터 aj까지의 합 ai + ai+1 + ai+2 + … + aj-1 + aj로 정의된다. 이때 주어진 배열에서 만들 수 있는 같은 수 순서쌍의 최대 합을 찾고, 최대 합을 가지는 같은 수 순서쌍의 개수를 출력하는 프로그램을 작성하시오.

### 입력

첫 번째 배열 a의 크기 N이 주어진다.

두 번째 줄에 배열 a의 원소 a1, a2, …, aN이 주어진다.

### 출력

주어진 배열에서 만들 수 있는 같은 수 순서쌍의 최대 합과 최대 합을 가진 같은 수 순서쌍의 개수를 출력한다.

## 문제 풀이

누적합을 기록해두고 같은값이 나올때마다 max값을 갱신해준다.

### 코드

```java
import java.util.*;
import java.io.*;

public class Main {

    public static void main(String[] args) throws IOException {

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());
        long[] arrSum = new long[n];
        int ansCnt = 1;
        long ansNum;
        HashMap<Long, Integer> map = new HashMap<>();
        StringTokenizer tr = new StringTokenizer(br.readLine());

        arrSum[0] = Integer.parseInt(tr.nextToken());
        ansNum = arrSum[0];
        map.put(arrSum[0], 0);

        for (int i = 1; i < n; i++) {

            long temp = Integer.parseInt(tr.nextToken());
            arrSum[i] = arrSum[i - 1] + temp;

            if (map.containsKey(temp)) {
                int idx = map.get(temp);
                if (idx == 0) temp = arrSum[i];
                else temp = arrSum[i] - arrSum[idx - 1];
            }
            else {
                map.put(temp, i);
            }

            if (temp > ansNum) {
                ansNum = temp;
                ansCnt = 1;
            } else if (temp == ansNum) {
                ansCnt += 1;
            }

        }

        System.out.println(ansNum + " " + ansCnt);

    }

}
```

### 후기

누적합 문제는 쉬운문제는 쉽게 풀리는데 어려운 문제는 아무리봐도 이해가 안가는 느낌이다. 뭔가 중간이 없는 느낌