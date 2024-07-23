---
title: "[회고] 우아한 프리코스 2주차 회고"
date: 2024-10-29 00:00:00 +0800
categories: ["2024", 회고]
tags: [MVC, stream]
math: true
render_with_liquid: false
---
우아한 프리코스 과정 중 공부한 것, 느낀 점을 남긴 회고 입니다.


## 프리코스 1주차

![1](/assets/img/precorse/fox.jpeg)

왜 2주차 회고를 1주차 얘기로 시작하는가...<br>
1주차는 사실 반성문이다.<br>
우선 1주차는 정말 바빳다. 많은 이유들이 있었지만 가장 큰 이유는 정처기 실기였다. 60점만 넘으면 되는 간단한 시험이지만 미루고 미루다 3회차가 되어서 올해 마지막 시험이라는 점이 부담감으로 다가왔다. 마지막 3~4일 정도는 자고 밥먹고 씻는 인간으로써의 존엄성을 지키는 시간 빼곤 모두 정처기 공부 했던 것 같다.

결론적으로 1주차는 제출 당일날 2~3시간만에 급하게 제출했고 사실 프리코스에 대한 안내를 읽고, 환경설정을 하는 시간을 제외하면 코드짜는 시간은 1시간도 안됐던 것 같다. <br>
다행히 코드는 잘 동작했지만 리팩토링을 전혀하지 못했고 가독성, 효율, 예외처리는 개나 줘버렸다...

그래도 후회는 하지 않는다. 다시 시간을 돌린다고 해도 1주차에 시간을 많이 쓰지 못했을거고, 1주차 코드가 동작하지 않는 것도 아니고 2주차 이후에 개선되는 모습을 보여주면 될 거라고 생각했다.

## MVC 패턴

MVC는 Model, View, Controller의 약자로 하나의 애플리케이션, 프로젝트를 구성할 때 그 구성요소를 세가지의 역할로 구분한 패턴

![1](/assets/img/precorse/ModelViewControllerDiagram.png)
출처:XESCHOOL

### Model

애플리케이션의 정보, 데이터를 나타낸다. 데이터베이스, 처음의 정의하는 상수, 초기화값, 변수 등을 뜻. 또한 이러한 DATA, 정보들의 가공을 책임지는 컴포넌트

> 사용자가 편집하길 원하는 모든 데이터를 가지고 있어야 한다.

> 뷰나 컨트롤러에 대해서 어떤 정보도 알지 말아야 한다.

> 변경이 일어나면, 변경 통지에 대한 처리방법을 구현해야만 한다.

### View

input 텍스트, 체크박스 항목 등과 같은 사용자 인터페이스 요소를 나타낸다. 다시 말해 데이터 및 객체의 입력, 그리고 보여주는 출력을 담당. 데이타를 기반으로 사용자들이 볼 수 있는 화면

> 모델이 가지고 있는 정보를 따로 저장해서는 안된다.

> 모델이나 컨트롤러와 같이 다른 구성요소들을 몰라야 된다.

> 변경이 일어나면 변경통지에 대한 처리방법을 구현해야만 한다.

### Controller

데이터와 사용자인터페이스 요소들을 잇는 다리역할 사용자가 데이터를 클릭하고, 수정하는 것에 대한 "이벤트"들을 처리하는 부분

> 모델이나 뷰에 대해서 알고 있어야 한다.

> 모델이나 뷰의 변경을 모니터링 해야 한다.

### 적용

MVC 패턴을 배우긴 했지만 직접 적용해본적은 한번도 없었다. 그러다 보니 각각 무슨 역할을 하는지 머리로는 이해해도 막상 코드를 짜려고 하니 전혀 감이 잡히지 않았다.

우선 ```Model```은 자동차 경주의 핵심 데이터를 관리하고, 비즈니스 로직을 처리한다.<br>
Car 클래스: 각 자동차의 이름과 현재 위치를 저장하고, 이동 조건(랜덤 값에 따라 이동)을 구현한다.<br>
RacingGame 클래스: 자동차 리스트를 받아 경주를 관리하며, 각 라운드에서 모든 자동차가 이동하도록 하고 우승자를 계산하는 기능을 수행한다.

```View```는 사용자에게 데이터를 표시하거나, 사용자로부터 입력을 받는 역할을 담당한다.<br>
InputView 클래스: 사용자에게 입력을 받아 필요한 정보를 제공한다. 여기서 입력된 자동차 이름, 시도 횟수 등의 유효성을 검사하여 예외를 처리한다. <br>
OutputView 클래스: 각 라운드의 결과를 출력하고, 우승자를 표시한다. 자동차의 이름과 위치 정보를 가져와 결과를 콘솔에 표시하는 형식으로 구현되어 있다.

```Controller```는 사용자 입력과 비즈니스 로직(Model)을 연결하고, 결과를 View에 전달하는 역할을 합니다.<br>
GameController 클래스: 경주를 진행하는 전체 과정을 관리한다.


## JAVA Stream

그 전에는 for이 있는데 굳이 Stream을 써야되나? 라는 생각을 했었다. 이번에 많은 사람들의 코드 리뷰를 하고 내 코드를 리뷰 받으면서 Stream을 사용하면 확실히 이점이 있다는 것을 알게되었다.

```Stream```은 컬렉션(리스트, 배열 등)의 데이터 처리를 위한 API로, 함수형 프로그래밍 스타일을 활용하여 데이터 변환, 필터링, 매핑 등을 보다 간결하고 직관적으로 수행할 수 있게 해준다. Java 8에서 처음 도입되었으며, ```Stream API```는 데이터를 효율적으로 다룰 수 있도록 다양한 기능을 제공한다.

### 장점

- 코드 간결성: 반복문을 사용하지 않고도 데이터를 처리할 수 있어 코드가 간결해진다.
- 병렬 처리 지원: parallelStream()을 사용하여 간단하게 병렬 처리가 가능하다.
- 유연성: 스트림은 다양한 데이터 연산을 조합하여 사용할 수 있어 유연한 데이터 처리 파이프라인을 만들 수 있다.

### 스트림 생성

**배열스트림 : Arrays.stream()**

```java
String[] arr = new String[]{'a', 'b', 'c'};
Stream<String> stream = Arrays.stream(arr);
```

**컬렉션 스트림: .stream()**
```java
List<String> list = Arrays.asList("a","b","c");
Stream<String> stream = list.stream();
```

**Stream.builder()**
```java
Stream<String> builderStream = Stream.<String>builder()
    .add("a").add("b").add("c")
    .build(); 
```

**람다식 Stream.generate(), iterate()**
```java
Stream<String> generatedStream = Stream.generate(()->"a").limit(3);

Stream<Integer> iteratedStream = Stream.iterate(0, n->n+2).limit(5); //0,2,4,6,8
```

**기본 타입형 스트림**
```java
IntStream intStream = IntStream.range(1, 5);
```

**병렬 스트림: parallelStream()**
```java
Stream<String> parallelStream = list.parallelStream();
```

### 가공

**Filtering**

스트림 내 요소를 하나씩 평가해서 걸러내는 역할 (IF)

```java
List<String> list = Arrays.asList("a","b","c");
Stream<String> stream = list.stream()
	.filter(list -> list.contains("a"));
```

**Mapping**

스트림 내 요소를 하나씩 특정 값으로 변환하는 역할

```java
Stream<String> stream = list.stream()
	.map(String::toUpperCase);
	//[A,B,C]
    
    .map(Integers::parseInt);
    // 문자열 -> 정수로 변환
```

**Sorting**

스트림 내 요소를 정렬하는 역할

```java
Stream<String> stream = list.stream()
	.sorted() // [a,b,c] 오름차순 정렬
    .sorted(Comparator.reverseOrder()) // [c,b,a] (내림차순)
    
List<String> list = Arrays.asList("a","bb","ccc");
Stream<String> stream = list.stream()
	.sorted(Comparator.comparingInt(String::length)) // [ccc,bb,a] //문자열 길이 기준 정렬
```

**기타 연산**

```java
Stream<String> stream = list.stream()
	.distinct() // 중복 제거
    .limit(max) // 최대 크기 제한
    .skip(n)    // 앞에서부터 n개 skip하기
    .peek(System.out::println) // 중간 작업결과 확인
```

### 최종 연산

**Calculating**

```java
IntStream stream = list.stream()
	.count()   //스트림 요소 개수 반환
    .sum()     //스트림 요소의 합 반환
    .min()     //스트림의 최소값 반환
    .max()     //스트림의 최대값 반환
    .average() //스트림의 평균값 반환
```

**Reduction**

```java
IntStream stream = IntStream.range(1,5);
	.reduce(10, (total,num)->total+num);
    //reduce(초기값, (누적 변수,요소)->수행문)
    // 10 + 1+2+3+4+5 = 25
```

**Collecting**

```java
//예시 리스트
List<Person> members = Arrays.asList(new Person("lee",26),
									 new Person("kim", 23),
									 new Person("park", 23));
                    
// toList() - 리스트로 반환
members.stream()
	.map(Person::getLastName)
    .collect(Collectors.toList());
    // [lee, kim, park]
    
// joining() - 작업 결과를 하나의 스트링으로 이어 붙이기
members.stream()
	.map(Person::getLastName)
    .collect(Collectors.joining(delimiter = "+" , prefix = "<", suffix = ">");
    // <lee+kim+park>
    
//groupingBy() - 그룹지어서 Map으로 반환
members.stream()
	.collect(Collectors.groupingBy(Person::getAge));
	// {26 = [Person{lastName="lee",age=26}],
    //  23 = [Person{lastName="kim",age=23},Person{lastName="park",age=23}]}
    
//collectingAndThen() - collecting 이후 추가 작업 수행
members.stream()
	.collect(Collectors.collectingAndThen (Collectors.toSet(),
    									   Collections::unmodifiableSet));
	//Set으로 collect한 후 수정불가한 set으로 변환하는 작업 실행
```

**Matching**

```java
List<String> members = Arrays.asList("Lee", "Park", "Hwang");
boolean matchResult = members.stream()
						.anyMatch(members->members.contains("w")); //w를 포함하는 요소가 있는지, True

boolean matchResult = members.stream()
						.allMatch(members->members.length() >= 4); //모든 요소의 길이가 4 이상인지, False

boolean matchResult = members.stream()
						.noneMatch(members->members.endsWith("t")); //t로 끝나는 요소가 하나도 없는지, True
```

**Iterating**

```java
members.stream()
	.map(Person::getName)
    .forEach(System.out::println);
    //결과를 출력 (peek는 중간, forEach는 최종)
```

**Finding**

```java
Person person = members.stream()
					.findAny()   //먼저 찾은 요소 하나 반환, 병렬 스트림의 경우 첫번째 요소가 보장되지 않음
                    .findFirst() //첫번째 요소 반환
```


아직까지는 stream을 사용하는 것이 익숙하지 않지만 for을 사용할때마다 의식적으로 연습해야 될 것 같다.

## 우아한 아이들

