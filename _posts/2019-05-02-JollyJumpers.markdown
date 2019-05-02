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
<br><br>

<h1 style="margin:0px;"> 참고자료 </h1>
<hr style="height:1px; margin:0px;">
