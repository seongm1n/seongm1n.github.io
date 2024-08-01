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

## 2. 스택 (Stack)

- 후입선출(LIFO) 구조

JAVA에서 제공하는 Stack 클래스
### 예제

```java
Stack<Element> stack = new Stack<>();
```
위와 같이 생성할 수 있으며 기본적으로 push(), pop(), peek(), empty(), search() 기능을 지원한다.

```java
import java.util.Stack;
import javax.lang.model.element.Element;

public class Main {
    public static void main(String[] args) {
        Stack<Integer> stack = new Stack<>();//push, pop, peek, empty, seach 지원
        for(int i=1; i<=5 ; i++) {
            stack.push(i);
            System.out.println(stack.peek());
        } //1, 2, 3, 4, 5 출력
        stack.pop();
        System.out.println("Pop()");
        System.out.println(stack.peek());    //4출력
        System.out.println(stack.search(3));    //2출력
        System.out.println(stack.empty());    //false출력
    }

}
```

## 3. 큐 (Queue)

- 선입 선출(FIFO) 구조

### 예제

```java
Queue<자료형> q = new LinkedList<>();
```


1. 삽입
```
q.add(삽입할 value);
```

2. 삭제
```
q.remove();
```

3. 큐의 프론트에 위치한 벨류 반환
```
q.peek();
```

## 4. 힙 (heap)