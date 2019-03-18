---
layout: post
title:  (Java)1 이상, 5000 미만의 self number의 합
date:   2019-03-15 11:50:00
author: Kim Yongjun
categories: Java
tags: Java
---

# Self Number

안녕하세요!! 김용준입니다.

1 이상, 5000 미만의 self number의 합을 구하는 프로그램을 만들었습니다.
사용한 언어는 Java이고 IDE는 IntelliJ입니다.
<br><br>

<h1 style="margin:0px;"> 코딩전 확인 </h1>
<hr style="height:1px; margin:0px;">


### 1. input과 ouput을 충분히 모아보세요. 처음부터 완벽할 필요는 없고, 코딩 도중에 추가하셔도 됩니다.

        * 1자리 수
        d(1) = 1 + 1 = 2
        d(2) = 2 + 2 = 4
        d(3) = 3 + 3 = 6
        d(4) = 4 + 4 = 8
        d(5) = 5 + 5 = 10
        d(6) = 6 + 6 = 12
        d(7) = 7 + 7 = 14
        d(8) = 8 + 8 = 16
        d(9) = 9 + 9 = 18

        * 2자리 수
        d(10) = 1 + 0 + 10 = 11
        d(11) = 1 + 1 + 11 = 13
        d(12) = 1 + 2 + 12 = 15
        d(13) = 1 + 3 + 13 = 17
        d(14) = 1 + 4 + 14 = 19
        d(15) = 1 + 5 + 15 = 21
        d(16) = 1 + 6 + 16 = 23
        d(17) = 1 + 7 + 17 = 25
        d(18) = 1 + 8 + 18 = 27
        d(19) = 1 + 9 + 19 = 29
        d(20) = 2 + 0 + 20 = 22

        * 3자리 수
        d(100) = 1 + 0 + 0 + 100 = 101
        d(101) = 1 + 0 + 1 + 101 = 103
        d(102) = 1 + 0 + 2 + 102 = 105
        d(103) = 1 + 0 + 3 + 103 = 107

        * 4자리 수
        d(1000) = 1 + 0 + 0 + 0 + 1000 = 1001
        d(1001) = 1 + 0 + 0 + 1 + 1001 = 1003

### 2. 어떻게 풀어볼지 간단히 생각하고, 곰인형 등을 앞 또는 옆에 두고 친절하게 설명해 보세요.

1차적으로 생각한 해결 법은 다음과 같습니다.
1. 반복문을 사용하여 1 이상 5000 미만의 자연수 n을 넣어 나온 값들을 리스트에 저장합니다.
2. 다시 한번 반복문을 사용하여 1 이상 5000 미만의 자연수가 리스트에 저장된 값에 속하지 않는 값들을 누적합으로 구합니다.

(친절하지 못하여 죄송합니다ㅠㅠ)

### 3. 어떻게 하면 1번에서 모은 input과 output을 항상 순식간에 확인할 수 있을지 고민해 보세요(사실 그냥 자동화된 테스트 코드를 작성하면 됩니다).

[단위 테스트 코드](https://github.com/KimYongjun413/DalLab-Mentoring/blob/master/SelfNumber/test/SumOfSelfNumberTest.java "단위 테스트 코드 깃허브 링크")를 작성했습니다.
```java
@Test
public void d() {
    //1자리 수
    assertEquals(2,ssn.d(String.valueOf(1)));
    assertEquals(4,ssn.d(String.valueOf(2)));
    assertEquals(18,ssn.d(String.valueOf(9)));
    //2자리 수
    assertEquals(87,ssn.d(String.valueOf(75)));
    assertEquals(22,ssn.d(String.valueOf(20)));
    //3자리 수
    assertEquals(101,ssn.d(String.valueOf(100)));
    assertEquals(105,ssn.d(String.valueOf(102)));
    assertEquals(107,ssn.d(String.valueOf(103)));
    //4자리 수
    assertEquals(1001,ssn.d(String.valueOf(1000)));
    assertEquals(5005,ssn.d(String.valueOf(5000)));
    assertEquals(1244,ssn.d(String.valueOf(1234)));

}

@Test
public void generators() {
    ArrayList test1 = new ArrayList();
    ArrayList test2 = new ArrayList();
    ArrayList test3 = new ArrayList();
    int[] gnrtNum1 = new int[]{2, 4, 6, 8, 10};//5까지
    int[] gnrtNum2 = new int[]{2, 4, 6, 8, 10, 11, 12, 14, 16, 18};//10까지
    int[] gnrtNum3 = new int[]{2, 4, 6, 8, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 21, 22, 23, 25, 27, 29};//20까지
    for(int i = 0; i < gnrtNum1.length; i++){ test1.add(gnrtNum1[i]);}
    for(int i = 0; i < gnrtNum2.length; i++){ test2.add(gnrtNum2[i]);}
    for(int i = 0; i < gnrtNum3.length; i++){ test3.add(gnrtNum3[i]);}
    assertEquals(test1, ssn.generator(1,5));
    assertEquals(test2, ssn.generator(1,10));
    assertEquals(test3, ssn.generator(1,20));
}

@Test
public void selfNumber() {
    ArrayList exPectedResult = new ArrayList();
    int[] sfNum = {1, 3, 5, 7, 9, 20, 31, 42, 53, 64, 75, 86, 97};
    for(int i = 0; i < sfNum.length; i++){ exPectedResult.add(sfNum[i]);}

    ArrayList generator = ssn.generator(1, 100);
    ArrayList selfNumber = ssn.selfNumber(generator, 1, 100);

    assertEquals(exPectedResult, selfNumber);
}
```

### 4. 구현합니다.

전체 소스는 [여기](https://github.com/KimYongjun413/DalLab-Mentoring/tree/master/SelfNumber "Self Number GitHub")를 가시면 볼 수 있습니다.


<h1 style="margin:0px;"> 개발과정 </h1>
<hr style="height:1px; margin:0px;">



처음 생각한 <b>개발 순서</b>는 다음과 같습니다.

1. self number가 아닌 값을 구하는 메서드 d를 개발합니다.
2. self number가 아닌 값을 리스트에 저장할 메서드를 개발합니다.
3. self number들의 합을 구할 메서드를 개발합니다.

그런데, generator와 self-number를 맞게 구하는지
확인할 단위 테스트가 필요했습니다. 그래서 개발 순서를 변경하게 되었습니다.

1. generator를 구하는 메서드 d를 개발합니다.
2. generator를 리스트에 저장할 메서드를 개발합니다.
3. self-number를 리스트에 저장할 메서드를 개발합니다.
4. generator를 출력하는 메서드를 개발합니다.
5. self-number를 출력하는 메서드를 개발합니다.
6. self-number의 합을 계산하는 메서드를 개발합니다.

### 문제발생
<b>구현에는 문제가 없었지만, 제가 생각한 로직이 최적의 답이라고 생각하지 않습니다.</b>    
<b>하지만 어떻게 최적화 시켜야 할지 아이디어가 떠오르지 않습니다.</b>


### 해결방법
1. 피드백을 받는대로 로직을 수정할 계획입니다.


<h1 style="margin:0px;"> 느낀점 </h1>
<hr style="height:1px; margin:0px;">
- '단위 테스트'를 어떻게 쓰냐에 따라 복잡하게 테스트할 수도 있고<br>
간단하게 테스트할 수도 있다는 것을 이번 개발을 통해 느끼게 되었습니다.
- 어떻게 하면 효율적으로 '단위 테스트'를 할 수 있을지 공부해야겠다고 느꼈습니다.

<h1 style="margin:0px;"> 배운점 </h1>
<hr style="height:1px; margin:0px;">
- 느낀점과 비슷하지만 '단위 테스트'의 중요성 및 

<h1 style="margin:0px;"> 앞으로의 계획 </h1>
<hr style="height:1px; margin:0px;">
- 피드백을 받는대로 로직을 수정할 계획입니다.

<h1 style="margin:0px;"> 피드백 </h1>
<hr style="height:1px; margin:0px;">
1. 추가 피드백을 받으면 여기에 올리도록 하겠습니다.

<h1 style="margin:0px;"> 참고자료 </h1>
<hr style="height:1px; margin:0px;">

[Self Number의 개념 참고](http://poj.org/problem?id=1316&fbclid=IwAR0UIU0Oftr01hw8vx_i_Dz6d11QMWoldtuUN9MYlofJv0C6laXdutoKGS0 "이곳에서 1부터 100까지의 self number의 수를 알 수 있었습니다.")
