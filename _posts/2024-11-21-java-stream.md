---
title: "[JAVA] Stream 사용기"
date: 2024-11-12 00:00:00 +0800
categories: [java, stream]
tags: [java, stream]
math: true
render_with_liquid: false
---

## for VS stream: 왜 stream을 사용할까?

우아한 프리코스를 진행하면서 많은 사람들이 Stream API를 사용하고 있었다. 
사실 처음에는 <b>효율적이지도 않고, 배우기도 까다로운데 굳이 for문을 버리고 Stream을 사용해야되나?</b> 하는 생각이 있었다. <br>
사람들이 말하는 Stream API를 사용하는 이유는 여러가지가 있겠지만 사람들이 말하는 가장 큰 이유는 ```가독성```이다.

### 코드의 간결성과 가독성

```for```문은 명령형 프로그래밍 스타일로, 로직을 단계별로 명시적으로 작성한다. 반면, ```Stream```은 선언형 프로그래밍 스타일로 데이터 처리 과정을 간결하고 직관적으로 표현한다.

예를 들어서 짝수 숫자의 합을 구하는 로직을 만든다고 생각해보자
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
int sum = 0;
for (int number : numbers) {
    if (number % 2 == 0) {
        sum += number;
    }
}
System.out.println(sum); // 출력: 6
```
굉장히 마음에 들지 않는다. 특히 ```sum```을 선언해서 사용하는 부분이 지저분하게 느껴진다.

```java
int sum = numbers.stream()
                .filter(n -> n % 2 == 0)
                .reduce(0, Integer::sum);
System.out.println(sum); // 출력: 6
```
단순히 코드의 길이가 짧아진 것을 넘어서 ```filter```, ```reduce```라는 코드가 명확하고 한눈에 무슨일을 하는지 알 것 같다.


### 지연 연산(Lazy Evaluation)

Stream은 중간 연산을 지연 수행하여 필요할 때만 계산한다.

```java
numbers.stream()
       .filter(n -> {
           System.out.println("필터링: " + n);
           return n % 2 == 0;
       })
       .map(n -> {
           System.out.println("매핑: " + n);
           return n * n;
       })
       .findFirst(); // 첫 번째 짝수만 찾으면 연산 종료
```

### 병렬 처리의 간편함

```for```문에서는 병렬 처리를 구현하기 위해 스레드를 직접 관리하거나 병렬 실행을 위한 코드를 작성해야 한다. 하지만 Stream API는 ```parallelStream()```을 통해 병렬 처리를 쉽게 지원한다.

```java
int parallelSum = numbers.parallelStream()
                         .filter(n -> n % 2 == 0)
                         .reduce(0, Integer::sum);
```

### 체이닝을 통한 데이터 처리 파이프라인

Stream은 필터링, 매핑, 정렬 등의 작업을 메서드 체이닝으로 연결할 수 있다.

```java
List<Integer> evenSquares = new ArrayList<>();
for (int number : numbers) {
    if (number % 2 == 0) {
        evenSquares.add(number * number);
    }
}
Collections.sort(evenSquares);
```
for문을 이용하면 딱 보기에도 복잡하고 한번에 이해하기 힘들다.

```java
List<Integer> evenSquares = numbers.stream()
                                   .filter(n -> n % 2 == 0)
                                   .map(n -> n * n)
                                   .sorted()
                                   .collect(Collectors.toList());
```
Stream은 데이터 처리 과정을 논리적인 단계로 나눠서 표현하므로 이해하기 쉽고 유지보수와 확장에 용이하다.

## Stream 사용법

Java의 Stream API는 데이터를 선언적이고 유연하게 처리할 수 있도록 도와준다.

### 1. Stream 생성
Stream은 다양한 데이터 소스 (컬렉션, 배열, 파일 등)에서 생성할 수 있다.

- 컬렉션에서 생성
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
Stream<String> stream = names.stream();
```

- 배열에서 생성
```java
int[] numbers = {1, 2, 3, 4, 5};
IntStream intStream = Arrays.stream(numbers);
```

- 값 직접 제공
```java
Stream<String> stream = Stream.of("A", "B", "C");
```

- 무한 스트림 생성
```java
Stream<Integer> infiniteStream = Stream.iterate(0, n -> n + 2);
```

### 2. Stream 메서드 사용

<b>a. 중간 연산 (Intermediate Operations)</b>

중간 연산은 스트림을 변환하고, 다음 단계로 넘기는 작업을 수행. 지연 평가(Lazy Evaluation)를 사용하므로 최종 연산이 수행될 때 실행된다.

- filter : 조건에 맞는 요소만 필터링
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
Stream<Integer> evenNumbers = numbers.stream().filter(n -> n % 2 == 0);
```

- map : 요소를 반환
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
Stream<Integer> nameLengths = names.stream().map(String::length);
```

- sorted : 요소를 정렬
```java
List<Integer> numbers = Arrays.asList(5, 2, 3, 1, 4);
Stream<Integer> sortedNumbers = numbers.stream().sorted();
```

<b>b. 최종 연산 (Terminal Operations)</b>

최종 연산은 스트림을 처리하고, 결과를 반환하거나 종료

- forEach : 각 요소에 작업 수행
```java
names.stream().forEach(System.out::println);
```

- collect : 결과를 컬렉션으로 반환
```java
List<String> filteredNames = names.stream()
                                  .filter(name -> name.startsWith("A"))
                                  .collect(Collectors.toList());
```

- reduce : 요소를 누적하거나 하나의 값 생성
```java
int sum = numbers.stream().reduce(0, Integer::sum);
```

