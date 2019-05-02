---
layout: post
title:  (Java)Jolly Jumpers
date:   2019-05-02 11:50:00
author: Kim Yongjun
categories: Java
tags: Java
---

# Jolly Jumpers

안녕하세요!! 김용준입니다.

Jolly Jumpers를 구하는 프로그램을 만들었습니다.<br>
사용한 언어는 Java이고 IDE는 IntelliJ입니다.
<br><br>

<h1 style="margin:0px;"> 코드 레벨에서의 제약 </h1>
<hr style="height:1px; margin:0px;">

아샬님과의 고민 상담을 통해 설정된 제 스스로의 '훈련 목표'는 다음과 같습니다.

#### 1. 메서드 하나가 8줄이 넘지 않게 하자.
#### 2. else를 쓰지 말자.
<br>

<h1 style="margin:0px;"> 개발과정 </h1>
<hr style="height:1px; margin:0px;">

[단위 테스트 코드](https://github.com/KimYongjun413/DalLab-Mentoring/blob/master/JollyJumpers/test/JollyJumpersTest.java "단위 테스트 코드 깃허브 링크")는 다음과 같이 작성했습니다.
```java
    @Test
    public void IsJolly() {
        assertEquals("Jolly", math.IsJolly("4 1 4 2 3"));
        assertEquals("Jolly", math.IsJolly("1 1"));
        assertEquals("Not jolly", math.IsJolly("2 1 4"));
        assertEquals("Jolly", math.IsJolly("3 -6 -5 -7"));
        assertEquals("Not jolly", math.IsJolly("4 1 1 3 2"));
        assertEquals("Not jolly", math.IsJolly("5 1 4 2 -1 6"));
        assertEquals("Jolly", math.IsJolly("6 11 7 4 2 1 6"));
    }
```

전체 소스는 [여기](https://github.com/KimYongjun413/DalLab-Mentoring/tree/master/JollyJumpers "Self Number GitHub")를 가시면 볼 수 있습니다.
<br><br>

<h1 style="margin:0px;"> 피드백 반영 </h1>
<hr style="height:1px; margin:0px;">

if 문을 쓰면 최소 3줄이 된다고 생각해야 한다는 피드백을 받았습니다.<br>
이 9줄짜리 메서드를 '훈련 목표'에 맞게 수정할 것입니다.
```java
private static int[] setDiff(int[] numbers, int[] diff) {
        int absSub = 0;
        for(int i=0; i < numbers.length - 1; i++) {
            absSub = Math.abs(numbers[i] - numbers[i+1]);
            if(absSub > numbers.length) {
                continue;
            }
            diff[absSub]++;
        }
        return diff;
    }
```
SRP(단일 책임 원칙)을 생각해 보았을 때 setDiff 메서드는 2가지 책임이 있다고 생각했습니다.<br>
1. 인접한 수의 절대 차이 값이 Jolly Jumper를 만족하는 유효한 수 인지 확인하는 책임
2. 인접한 수의 절대 차이 값이 인덱스가 되는 diff배열에 그 수를 담는 책임

1번 책임 + 직접 유효한 수를 반환하는 메서드를 만들었습니다.
```java
    private static int[] setDiff(int[] numbers, int[] diff) {
        for(int i=0; i < numbers.length - 1; i++) {
            diff[getValidNumber(numbers.length, Math.abs(numbers[i] - numbers[i+1]))]++;
        }
        return diff;
    }

    private static int getValidNumber(int numbersLength, int absSub) {
        if(absSub <= numbersLength)
            return absSub;
        return 0;
    }
```
<h1 style="margin:0px;"> 느낀점 </h1>
<hr style="height:1px; margin:0px;">

- 단위 테스트를 사용하여 개발할수록 여러 예외 케이스를 테스트하기 정말 편하다는 생각을 했습니다. 
- 좀 더 편하게 개발하거나 테스트하기 위해서 어떻게 만들어야 할지 고민하는 시간을 가질 수 있어 좋았습니다. 
- 보기 좋은 소스코드를 만드는 게 그만큼 힘들다는 것 또한 느꼈습니다.
- <b>GIT에 메서드 구현 시마다 commit을 했어야 했는데 못했습니다.<br> 할 생각조차 못 하고 다 완성하고 나서야 생각났습니다. 반성해야 할 부분입니다.</b>
<br><br>

<h1 style="margin:0px;"> 배운점 </h1>
<hr style="height:1px; margin:0px;">
- 
<br><br>

<h1 style="margin:0px;"> 앞으로의 계획 </h1>
<hr style="height:1px; margin:0px;">
- 피드백을 받는대로 로직을 수정할 계획입니다.
<br><br>

<h1 style="margin:0px;"> 피드백 </h1>
<hr style="height:1px; margin:0px;">

 - 아샬님 피드백 : 라인을 줄이기 위해 if문을 한 줄로 만드는 건 좋지 않습니다.<br> 
 if문을 쓰면 최소 3줄이 된다고 생각하셔야 합니다.<br>
 ```java
    if (조건) {
        처리;
    }
```
- 이 상황에서 라인을 줄이려면 근본적으로 다른 전략(이라고 해도 대부분 extract method)이 필요합니다.
<br><br>

<h1 style="margin:0px;"> 참고자료 </h1>
<hr style="height:1px; margin:0px;">
