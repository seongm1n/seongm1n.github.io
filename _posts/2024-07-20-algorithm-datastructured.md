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

### 사용
기본적으로 push(), pop(), peek(), empty(), search() 기능을 지원한다.

```java
import java.util.Stack;

public class Main {
    public static void main(String[] args) {
        // Stack 객체 생성
        Stack<Integer> stack = new Stack<>();

        // 스택에 값 추가
        for (int i = 1; i <= 5; i++) {
            stack.push(i);  // 스택에 i 값을 추가
            System.out.println("Pushed: " + i + ", Top: " + stack.peek());  // 스택의 최상단 값 확인
        }

        // 스택에서 값 제거
        stack.pop();  // 스택의 최상단 값을 제거
        System.out.println("After pop()");
        System.out.println("Top after pop: " + stack.peek());  // 제거 후의 최상단 값 확인

        // 스택에서 특정 값의 위치 찾기
        System.out.println("Position of 3 in stack: " + stack.search(3));  // 3이 스택에서 몇 번째에 위치하는지 반환

        // 스택이 비어있는지 확인
        System.out.println("Is stack empty? " + stack.empty());
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

- 표를 사러 일렬로 늘어선 사람들로 이루어진 줄
- 선입 선출(FIFO) 구조

### 사용

```java
import java.util.Queue;
import java.util.LinkedList;

public class Main {
    public static void main(String[] args) {
        // Queue 인터페이스를 LinkedList로 구현
        Queue<Integer> q = new LinkedList<>();

        // 큐에 값 추가
        for (int i = 1; i <= 5; i++) {
            q.add(i);  // 큐에 i를 추가
            System.out.println("Added: " + i + ", Peek: " + q.peek());  // 큐의 첫 번째 값을 확인
        }

        // 큐에서 값 제거
        q.remove();  // 큐에서 첫 번째 값을 제거
        System.out.println("After remove()");

        // 큐에서 다시 첫 번째 값 확인
        System.out.println("Peek after remove: " + q.peek());  // 큐에서 제거 후의 첫 번째 값

        // 큐가 비어있는지 확인
        System.out.println("Is queue empty? " + q.isEmpty());
    }
}
```

### 예제

BOJ - 15828 [Router](https://www.acmicpc.net/problem/15828)

q를 이용하여 정보 처리를 구현한다.

```java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());
        int num;
        Queue<Integer> q = new LinkedList<>();
        while (true) {
            num = Integer.parseInt(br.readLine());
            if (num == -1) {
                break;
            } else if (num != 0 && q.size() != n) {
                q.add(num);
            } else if (num == 0 && q.size() != 0) {
                q.remove();
            }
        }
        n = q.size();
        for (int i = 0; i < n; i++) {
            System.out.print(q.poll() + " ");
        }
    }
}
```

## 3. 힙 (heap)

- 최솟값 또는 최댓값을 빠르게 찾아내기 위해 완전이진트리 형태로 만들어진 구조

### 사용

```java
import java.util.PriorityQueue;

public class MinHeapExample {
    public static void main(String[] args) {
        // PriorityQueue는 기본적으로 최소 힙으로 동작
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();

        // 데이터 삽입
        minHeap.offer(5);
        minHeap.offer(2);
        minHeap.offer(8);
        minHeap.offer(1);

        // 최소값 조회
        int minValue = minHeap.peek();  // 최소값 조회
        System.out.println("Min value: " + minValue);

        // 최소값 삭제
        int deletedValue = minHeap.poll();  // 최소값 삭제
        System.out.println("Deleted value: " + deletedValue);

        // 최소값 다시 조회
        minValue = minHeap.peek();  // 삭제 후 새로운 최소값 조회
        System.out.println("Min value: " + minValue);
    }
}
```

### 예제

BOJ - 1374 [강의실](https://www.acmicpc.net/problem/1374)

```java
import java.io.*;
import java.util.*;

public class Main {

    private static class lesson implements Comparable<lesson> {
        int id, start, end;

        public lesson(int id, int start, int end) {
            this.id = id;
            this.start = start;
            this.end = end;
        }

        @Override
        public int compareTo(lesson o) {
            return start - o.start;
        }
    }

    public static void main(String[] args) throws Exception {

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st;
        int n = Integer.parseInt(br.readLine());

        List<lesson> lessons = new ArrayList<>();

        for (int i = 0; i < n; i++) {
            st = new StringTokenizer(br.readLine());
            int id = Integer.parseInt(st.nextToken());
            int start = Integer.parseInt(st.nextToken());
            int end = Integer.parseInt(st.nextToken());
            lessons.add(new lesson(id, start, end));
        }
        Collections.sort(lessons);

        PriorityQueue<Integer> pq = new PriorityQueue<>();
        int max = 1;

        for (int i = 0; i < n; i++) {
            while (!pq.isEmpty() && pq.peek() <= lessons.get(i).start) {
                pq.poll();
            }
            pq.offer(lessons.get(i).end);
            max = Math.max(max, pq.size());
        }

        System.out.println(max);
    }
}
```
class를 만들어 강의실을 처리하는 과정이 생소했다.

## 4. 해시테이블

- (key, value)로 데이터를 저장하는 자료구조
- 시간복잡도 O(1)로 매우 빠르다.

### 사용

```java
import java.util.Hashtable;

public class HashTableExample {
    public static void main(String[] args) {
        // Hashtable 객체 생성 (키와 값은 모두 Integer 타입)
        Hashtable<Integer, String> hashTable = new Hashtable<>();

        // 데이터 삽입
        hashTable.put(1, "Apple");
        hashTable.put(2, "Banana");
        hashTable.put(3, "Cherry");
        hashTable.put(4, "Date");

        // 값 조회
        String value = hashTable.get(2); // 키 2에 해당하는 값을 가져옴
        System.out.println("Value for key 2: " + value);

        // 특정 키가 존재하는지 확인
        boolean containsKey = hashTable.containsKey(3);
        System.out.println("Does key 3 exist? " + containsKey);

        // 데이터 삭제
        String removedValue = hashTable.remove(4); // 키 4에 해당하는 값을 삭제
        System.out.println("Removed value for key 4: " + removedValue);

        // 남아있는 모든 값 출력
        System.out.println("Current HashTable contents:");
        hashTable.forEach((key, val) -> {
            System.out.println("Key: " + key + ", Value: " + val);
        });
    }
}
```