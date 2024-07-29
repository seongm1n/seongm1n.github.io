---
title: "[회고] 우아한 프리코스 3주차 회고"
date: 2024-10-29 00:00:00 +0800
categories: ["2024", 회고]
tags: [MVC, stream]
math: true
render_with_liquid: false
---
우아한 프리코스 과정 중 공부한 것, 느낀 점을 남긴 회고 입니다.

## Enum

```Java Enum을 적용하여 프로그램을 구현한다.```

이번 과제에 있는 요구사항이다. enum... OOP 수업에서 분명 배웠던 것 같은데 낯설다.

우아한 테크 블로그의 [Java Enum 활용기](https://techblog.woowahan.com/2527/)를 참고했다. 무려 2017년의 글이다.


### enum 이란?

미리 정의된 상수의 집합. enum에 열거된 상수들은 객체 생성 없이 외부에서 사용 가능하고, ```public static final``` 이다. 그리고 **모두 대문자로 적는 것이 원칙이다.**

```java
public enum Prize {
    FIRST(6, false, 2_000_000_000),
    SECOND(5, true, 30_000_000),
    THIRD(5, false, 1_500_000),
    FOURTH(4, false, 50_000),
    FIFTH(3, false, 5_000),
    NONE(0, false, 0);

    private final int matchCount;
    private final boolean bonusMatch;
    private final int prizeMoney;

    Prize(int matchCount, boolean bonusMatch, int prizeMoney) {
        this.matchCount = matchCount;
        this.bonusMatch = bonusMatch;
        this.prizeMoney = prizeMoney;
    }

    public int getMatchCount() {
        return matchCount;
    }

    public boolean isBonusMatch() {
        return bonusMatch;
    }

    public int getPrizeMoney() {
        return prizeMoney;
    }

    public static Prize valueOf(int matchCount, boolean bonusMatch) {
        return Arrays.stream(values())
                .filter(prize -> prize.matchCount == matchCount)
                .filter(prize -> {
                    if (prize.matchCount == 5) {
                        return prize.bonusMatch == bonusMatch;
                    }
                    return true;
                })
                .findFirst()
                .orElse(NONE);
    }
}
```
과제에서는 Prize를 enum으로 구현했다.

### enum의 장점
- 문자열과 비교해, IDE의 적극적인 지원을 받을 수 있다. 
(자동완성, 오타검증, 텍스트 리팩토링 등등)
- 허용 가능한 값들을 제한할 수 있다.
- 리팩토링시 변경 범위가 최소화 된다. 
(내용의 추가가 필요하더라도, Enum 코드외에 수정할 필요가 없다.)

```enum```을 사용하면 코드 안정성과 가독성이 높아지고 특정 값 집합을 제한할 수 있어서, 정해진 값만을 사용하고자 할 때 매우 유용하다.