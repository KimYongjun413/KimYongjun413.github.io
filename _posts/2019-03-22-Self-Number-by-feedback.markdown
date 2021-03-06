---
layout: post
title:  (Java)피드백 적용한 self number의 합 프로그램
date:   2019-03-22 11:50:00
author: Kim Yongjun
categories: Java
tags: Java
---

# Self Number

안녕하세요!! 김용준입니다.

<b>피드백을 적용한</b> self number의 합을 구하는 프로그램을 만들었습니다.<br>
사용한 언어는 Java이고 IDE는 IntelliJ입니다.
<br><br>

<h1 style="margin:0px;"> 패드백 내용</h1>
<hr style="height:1px; margin:0px;">

- .gitignore를 활용해서 불필요한 파일들을 제거해 주세요.

- 테스트 코드를 먼저 작성하면 인터페이스가 좀 달라질 겁니다. 예를 들어 assertEquals(2, ssn.d(String.valueOf(1))); 같은 경우엔 ssn이란 이름이 불명확해 보이고, 그래서 이 이름을 어떻게 할지 먼저 고민하게 됩니다. 그리고 입력이 String이 되는데, 이것도 또 고민하게 되죠. 여기서 이걸 정리하지 않았기 때문에 코드 전체에 String.valueOf가 퍼져나갑니다.

- 테스트 코드는 가능하면 주석이 없게 하고, 각 단위를 메서드로 분리해 보세요.<br> [아샬님 - 테스트 코드 작성 템플릿](https://github.com/ahastudio/til/blob/master/blog/2018/12-08-given-when-then.md?fbclid=IwAR3gKIWCiVBsbca9R16owI6CIz53bL6NSz6RTf1x0VsgcpaJ1HjKDmYPqls "테스트 코드 작성 템플릿")

- generators 테스트 코드를 훨씬 단순하게 만들려면 어떻게 해야 할까요?<br>
[JUnit4 - Assert](https://junit.org/junit4/javadoc/latest/org/junit/Assert.html?fbclid=IwAR0aw1F8zlQgm-txEPAljubFDhk4-JXzYUssR6dwHrY4VS10AE75Lg_i0ZE#assertArrayEquals(java.lang.Object[],%20java.lang.Object[]) "JUnit4 - Assert")<br>
[Oracle - Class ArrayList](https://docs.oracle.com/javase/9/docs/api/java/util/ArrayList.html?fbclid=IwAR3lSla0vKojvHduTZeoaFMffrhASfi2MjwQmRyKU6U6UvhYBHT1NOxXXEk#toArray-T:A- "Oracle - Class ArrayList")


<h1 style="margin:0px;"> 패드백 적용</h1>
<hr style="height:1px; margin:0px;">

### 1. [피드백 내용] 테스트 코드를 먼저 작성하면 인터페이스가 좀 달라질 겁니다. 예를 들어 assertEquals(2, ssn.d(String.valueOf(1))); 같은 경우엔 ssn이란 이름이 불명확해 보이고, 그래서 이 이름을 어떻게 할지 먼저 고민하게 됩니다. 

SumOfSelfNumber의 약자인 ssn는 의미를 파악하기 힘든 이름이었습니다.<br>
그래서 어떤 이름이 잘 어울릴까 생각하다가 문제에서 힌트를 얻어 <br>
자연수 <b>'naturalNumbers'</b>라고 지었습니다.

```java
naturalNumbers.generators(n)
naturalNumbers.selfNumbers(n)
naturalNumbers.sum(n)
```
의미 파악이 한결 쉬워진 느낌입니다.

### 2. [피드백 내용] 입력이 String이 되는데, 이것도 또 고민하게 되죠. 여기서 이걸 정리하지 않았기 때문에 코드 전체에 String.valueOf가 퍼져나갑니다.

String.valueOf가 퍼져 나가는걸 막기 위해 정리할 방법을 생각해 보았습니다.<br>
d(n)메서드의 파리미터 타입을 int로 변경하고<br> n의 각 자리수들을 구할 방법을 생각해 보기로 했습니다.

한자리 숫자들의 합이기 때문에 char 형태로 잘라야겠다고 생각했습니다.<br>
그래서 다음과 같은 d메서드가 만들어졌습니다.<br>
```java
public static int d(int n) {
    int result = n;
    char[] charNumbers = String.valueOf(n).toCharArray();
    for(int i = 0; i < charNumbers.length; i++) {
        result += charNumbers[i] - '0';
    }
    return result;
}
```
코드를 더 줄일 수 없을까 생각하다가 <br>
몫과 나머지로 자연수를 자리수 별로 자르던 알고리즘 문제가 생각났습니다. <br>
그래서 재귀와 반복문을 가진 d 메서드를 만들었습니다.<br>
재귀 메서드는 최초 자기 자신의 값을 더할 수 없어 <br>
<b>반복문을 가진 d메서드를 사용</b>하기로 했습니다!

```java
public static int recursiveD(int n) {
    if(n < 1) return 0;
    else return n%10 + recursiveD(n/10);
}

public static int whileD(int n) {
    int result = n;
    while(n > 0) {
        result += n % 10;
        n = n/10;
    }
    return result;
    }
```
### 3. [피드백 내용] 테스트 코드는 가능하면 주석이 없게 하고, 각 단위를 메서드로 분리해 보세요.

- 주석을 사용한 부분들을 살펴봅니다.
```java
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
```
- generators()메서드에서 주석 처리한 "5까지"는 d(5)의 generator를 뜻했습니다.<br>
generators(5)가 반환하게 될 리스트와 직접 비교하는 assertArrayEquals를 사용하여<br>
테스트 코드가 직관적으로 보이기 시작했습니다.
```java
@Test
    public void generators() {
        assertArrayEquals(new int[]{2}, naturalNumbers.generators(1));
        assertArrayEquals(new int[]{2, 4, 6, 8}, naturalNumbers.generators(4));
        assertArrayEquals(new int[]{2, 4, 6, 8, 10}, naturalNumbers.generators(5));
    }
```

전체 소스는 [여기](https://github.com/KimYongjun413/DalLab-Mentoring/tree/master/SelfNumber "Self Number GitHub")를 가시면 볼 수 있습니다.

<h1 style="margin:0px;"> 느낀점 </h1>
<hr style="height:1px; margin:0px;"/>

- '단위 테스트'를 먼저 작성하는 게 어떤 차이를 가지고 오는지 직접 느낀 적이 없었는데<br>
그 차이점을 느끼게 되었습니다. 
- '단위 테스트'와 메서드 둘 다 소스코드만 보고도 의미를 파악할 줄 알아야 하는 걸 알 수 있었습니다.

<h1 style="margin:0px;"> 배운점 </h1>
<hr style="height:1px; margin:0px;">

- 테스크 코드 작성 템플릿인 'GIVEN-WHEN-TEHN'을 배웠습니다.
- ArrayList 클래스를 자세히 들여다볼 수 있었습니다.
- JUnit4의 Assert메서드를 자세히 들여다볼 수 있었습니다.

<h1 style="margin:0px;"> 앞으로의 계획 </h1>
<hr style="height:1px; margin:0px;">

- 추가적으로 피드백을 받는다면 지속적으로 수정할 계획입니다.

<h1 style="margin:0px;"> 추가 피드백 </h1>
<hr style="height:1px; margin:0px;">

- naturalNumbers는 너무 과한 느낌인데 d, generator 이런 거에 엄청난 맥락이 숨어있지 않거든요. 제가 이런 곳에서 제가 쓰는 이름은 math 같은 거에요.

- IntelliJ를 쓰신다면 (맥 기준으로) option+command+L을 눌러보세요. 정적분석 삼대장을 쓰시면 손쉽게 관리 가능합니다.https://youtu.be/PT2RtnVeMNo (39분 41초부터)

<br>
<b>피드백 적용</b><br>
1. 피드백을 바탕으로 테스트코드의 변수명을 naturalNumbers에서 math로 변경하였습니다.<br>
2. 정적분석 삼대장 plugin을 설치하였고, checkstyle 통해 오탈자를 수정할 수 있었습니다.

적용된 소스는 [여기](https://github.com/KimYongjun413/DalLab-Mentoring/commit/9afa2bed2c588e71e4805bad7d52722e1b0b6003 "Self Number GitHub")를 가시면 볼 수 있습니다.

<h1 style="margin:0px;"> 참고자료 </h1>
<hr style="height:1px; margin:0px;">

[아샬님 - 테스트 코드 작성 템플릿](https://github.com/ahastudio/til/blob/master/blog/2018/12-08-given-when-then.md?fbclid=IwAR3gKIWCiVBsbca9R16owI6CIz53bL6NSz6RTf1x0VsgcpaJ1HjKDmYPqls "테스트 코드 작성 템플릿")<br>
[JUnit4 - Assert](https://junit.org/junit4/javadoc/latest/org/junit/Assert.html?fbclid=IwAR0aw1F8zlQgm-txEPAljubFDhk4-JXzYUssR6dwHrY4VS10AE75Lg_i0ZE#assertArrayEquals(java.lang.Object[],%20java.lang.Object[]) "JUnit4 - Assert")<br>
[Oracle - Class ArrayList](https://docs.oracle.com/javase/9/docs/api/java/util/ArrayList.html?fbclid=IwAR3lSla0vKojvHduTZeoaFMffrhASfi2MjwQmRyKU6U6UvhYBHT1NOxXXEk#toArray-T:A- "Oracle - Class ArrayList")
