---
title: "JAVA로 자료구조 구현해보기"
date: 2024-07-20 00:00:00 +0800
categories: [algorithm, java]
tags: [자료구조]
math: true
render_with_liquid: false
---


JAVA로 직접 자료구조를 구현하면서 공부

## 1. 배열 (Array)

- 동일한 데이터 타입을 순서대로 정리
- 정해진 크기
- jdk 클래스 : ArrayList, Vector


### 예제
생성

```java
타입[] 변수 = { 값0, 값1, 값2, 값3, … };
```

사용

```java
int[] scores = new int[3];
scores[0] = 83;
scores[1] = 90;
scores[2] = 75;
```

2차원 배열

```java
int[][] classStudentNumber;
classStudentNumber = new int[10][30];
```

## 2. 스택

