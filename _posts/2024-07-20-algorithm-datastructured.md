---
title: "JAVA로 자료구조"
date: 2024-07-20 00:00:00 +0800
categories: [algorithm, java]
tags: [자료구조]
math: true
render_with_liquid: false
---


JAVA로 자료구조 사용법 공부

## 1. 스택 (Stack)

- '쌓다'라는 의미로, 데이터를 차곡차곡 쌓아 올린 형태의 자료구조이다.
- 후입선출(LIFO) 구조를 가지고 있다.
- 자바에서 제공하는 Stack 클래스를 이용한다.

### 사용법
기본적으로 push(), pop(), peek(), empty(), search() 기능을 지원한다.

```java
import java.util.Stack;
import javax.lang.model.element.Element;

public class Main {
    public static void main(String[] args) {
        Stack<Integer> stack = new Stack<>();

        for(int i=1; i<=5 ; i++) {
            stack.push(i);
            System.out.println(stack.peek());
        }
        stack.pop();
        System.out.println("Pop()");
        System.out.println(stack.peek());
        System.out.println(stack.search(3));
        System.out.println(stack.empty());
    }

}
```

### 예제

BOJ-5297 [키로거](https://www.acmicpc.net/problem/5397)

스택을 2개 이용하여 커서의 왼쪽과 오른쪽을 구분한다.


```java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String s;
        int t = Integer.parseInt(br.readLine());
        while (t-- > 0) {
            s = br.readLine();
            Stack<Character> stack1 = new Stack<>();
            Stack<Character> stack2 = new Stack<>();
            for (char c : s.toCharArray()) {
                if (c == '<') {
                    if (!stack1.isEmpty()) { stack2.push(stack1.pop()); }
                }
                else if (c == '>') {
                    if (!stack2.isEmpty()) { stack1.push(stack2.pop()); }
                }
                else if (c == '-') {
                    if (!stack1.isEmpty()) { stack1.pop(); }
                }
                else { stack1.push(c); }
            }

            StringBuilder sb = new StringBuilder();
            while (!stack1.isEmpty()) { stack2.push(stack1.pop()); }
            while (!stack2.isEmpty()) { sb.append(stack2.pop()); }
            System.out.println(sb.toString());
        }
    }
}
```

몇가지 메소드만 알면 스택을 쉽게 이용할 수 있어서 큰 어려움이 없었다.<br>
출력할때 계속 시간초과가 나서 이유를 한참 찾았다.

## 2. 큐 (Queue)

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

## 3. 힙 (heap)

- 최솟값 또는 최댓값을 빠르게 찾아내기 위해 완전이진트리 형태로 만들어진 구조

### 예제

```java
import java.util.PriorityQueue;

public class HeapExample {
    public static void main(String[] args) {
        // Integer를 저장하는 최소 힙 생성
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();

        // 요소 추가
        minHeap.offer(5);
        minHeap.offer(2);
        minHeap.offer(8);
        minHeap.offer(1);

        // 최소값 확인
        int minValue = minHeap.peek();
        System.out.println("Min value: " + minValue); // Min value: 1

        // 최소값 삭제
        int deletedValue = minHeap.poll();
        System.out.println("Deleted value: " + deletedValue); // Deleted value: 1

        // 최소값 확인
        minValue = minHeap.peek();
        System.out.println("Min value: " + minValue); // Min value: 2
    }
}
```