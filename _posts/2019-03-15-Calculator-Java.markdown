---
layout: post
title:  (Java) 괄호있는 다항 사칙연산 계산기
date:   2019-03-15 11:50:00
author: Kim Yongjun
categories: Java
tags: Java
---

# (Java) 괄호있는 다항 사칙연산 계산기

안녕하세요!! 김용준입니다.

C#,Javascript로 만든 계산기의 피드백 내용을 적용하여 새로운 계산기를 만들었습니다.
사용한 언어는 Java이고 IDE는 IntelliJ 입니다.
<br><br>

<h1 style="margin:0px;"> 피드백 내용 </h1>
<hr style="height:1px; margin:0px;">


### 1. “이 프로그램이 제대로 됐음을 확인할 수 있는 방법”을 좀더 구체적으로 할 수 있을까요? 누구나 별 생각 없이 시키는대로 해서 확인할 수 있도록요. 만약 이게 가능하다면 컴퓨터가 혼자서 점검하는 것도 가능할 겁니다.

'구체적', '컴퓨터가 혼자서 점검' 을 키 포인트로 잡고 생각해 보았습니다.  
구글링으로 방법을 찾아 보다가 '단위 테스트'라는 걸 찾았습니다.  
Java의 대표적인 Testing Framework인 JUnit을 으로 '단위 테스트'를 할 수 있다는 걸 알게되어 자바로 만드는 문자열 계산기에 적용했습니다.

### 2. 트리 자료구조를 활용할 수 있을까요? 각 요소를 얼마나 독립적으로 만들어야 이 작업이 수월할까요?

'트리 자료구조'가 무엇이고 어떻게 작동하는지를 다시한번 공부한 후 개발을 시작했습니다.  
개발 과정은 [제 깃허브](https://cordelia273.space/32 "Calculator_Java 폴더 참조")의 히스토리를 보시면 알수있습니다.

### 3. 메서드 하나를 10줄 이하로 만들고, 주석 없이 이해가 쉬운 코드를 만들려면 무엇을 하면 좋을까요?
제가 생각한 것은 다음과 같습니다.  
- 1.하나의 메서드 안에서 수행하고 있는 기능들을 또 다른 하나의 메서드로 분리할 수 있어야 합니다. 
- 2.메서드 명만 보고 어떤 기능을 하는지 알 수 있어야 합니다.  
- 3.마치 하나의 문장을 읽듯 메서드명을 보고 전체 소스 코드를 해석 할 수 있어야 합니다.  

### 4. 만약 OpenGL 기반으로 직접 버튼을 구현하거나 웹 API 등으로 서비스가 바뀌더라도 코드를 전혀 고칠 필요가 없게 하려면 어떻게 해야 할까요?

김수보 소장님의 블로그 - IT중심에서 의 ['API의 정의'](https://subokim.wordpress.com/2011/12/13/api-definition/ "API의 정의")를 통해 API의 정의를 알아보았습니다.  
API의 설계에 대한 글 중 "그래서 API를 설계할 때는 사용자가 해당 모듈에 기대하는 기능만 포함하는 것이 좋다."라는 문장에서 피드백의 의미를 어렴풋이 알 수 있었습니다. 그래서 문자열일 입력받아 답을 내는 '기능'만 가진 계산기를 만들기로 하였습니다.


<h1 style="margin:0px;"> 개발과정 </h1>
<hr style="height:1px; margin:0px;">

앞으로의 계획에서 밝힌 '괄호있는' 계산기를 개발 하였습니다.  
그리고 '정수' 가 아닌 '실수'를 계산하는 계산기를 만들었습니다.

<b>개발순서</b>는 다음과 같습니다.

1. Junit Test 개발
2. 후위표기법 메서드 개발
3. 이진트리 메서드 개발
4. 이진트리 순회하여 식을 계산하는 메서드 개발

### 문제발생
<b>어려워 한 부분은 다음과 같습니다.</b>  
1. JUnit이 무엇인지 몰랐습니다. 하지만 사용하고 싶었습니다.
2. 학부생 때 배운 이진트리가 생각나지 않았습니다. 개념을 다시 잡아야 했습니다.
3. 2.0 - 1.1 이 0.8999999999999999 로 계산되었습니다.
4. 입력한 계산식을 후위표기법 으로 변형시키는 메서드(이하 postFix)를 기능 단위로 분리하는 작업이 제일 머리 아팠습니다.

### 해결방법
1. 박재성님의 [junit4 테스트 라이브러리 기본 사용법](https://www.youtube.com/watch?v=tyZMdwT3rIY "junit4 테스트 라이브러리 기본 사용법")을 통해 기초를 배웠습니다.  
Kamang님의 [IT Blog](https://kamang-it.tistory.com/entry/JUnitJUnit-IDE%EC%97%90%EC%84%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B01 "")에서 IntelliJ에서 Test 코드를 생성하는 법을 배웠습니다.
2. 이지영 님이 쓰신 [C로 배우는 쉬운 자료구조](http://www.yes24.com/Product/Goods/9333165?scode=029 "C로 배우는 쉬운 자료구조")라는 책을 오랜만에 펼쳐 이진트리를 다시 학습했습니다.
3. 2.0 - 1.1 의 문제는 카이 호스트만 님의 [가장 빨리 만나는 코어자바9](http://www.yes24.com/Product/Goods/59417581?Acode=101 "가장 빨리 만나는 코어자바9")에서 답을 찾았습니다.  
4. postFix 의 기능을 한글로 적어 본 후 구분지어지는 '기능'들의 순서를 적어보았습니다.  
 1)입력받은 문자열을 char단위로 분리하기.  
 2)122+를 12 2 + 인지 1 22 + 인지를 구분하기 위한 정렬 로직.  
 3)연산자를 만났을 때 stack에 푸쉬하는 로직.  
 4)나머지 스택 팝업하기.  
그런다음 '기능'을 분리 해 보았습니다.


다음은 <b>'기능'</b>단위로 메서드를 분리하기 <b>전</b> postFix 메서드 입니다. 전체 소스는 [이 곳](https://github.com/KimYongjun413/DalLab-Mentoring/blob/33b91bc6b21ff250f520ee50ccdf598660eae0a5/Calculator_Java/src/main/Calculator.java "자바 계산기 히스토리")에서 확인 하실 수 있습니다.
```java
static String postFix(String formula) {
        Stack<String> stack = new Stack<>();
        char[] charFormula = formula.toCharArray();
        StringBuffer result = new StringBuffer();
        char values;
        String preValues = "";
        for (int i = 0; i < charFormula.length; i++) {
            values = charFormula[i];
            if(i != 0) preValues = String.valueOf(charFormula[i-1]);
            switch (values) {
                case '0':   case '1':   case '2':   case '3':   case '4':
                case '5':   case '6':   case '7':   case '8':   case '9':
                case '.':
                    if(stack.empty() || (stack.peek().equals("(") && stack.size() == 1)) result.append(values);
                    else if(i != 0 && isDouble(preValues)) result.append(values);
                    else result.append(" " + values);

                    break;
                case '+':   case '-':   case '/':   case '*':
                    if (stack.isEmpty()) stack.push(String.valueOf(values));
                    else {
                        if (getWeight(stack.peek().charAt(0)) >= getWeight(values)) {
                            result.append(" " + stack.pop());
                            stack.push(String.valueOf(values));
                        } else {
                            stack.push(String.valueOf(values));
                        }
                    }
                    break;
                case '(':
                    stack.push(String.valueOf(values));
                    break;
                case ')':
                    while (!stack.peek().equals("(")) result.append(" " + stack.pop());
                    stack.pop();
                    break;
                default:
            }
        }
        while (!stack.isEmpty()) {
            String peek = stack.peek();
            if (!peek.equals("(")) {
                if (peek.equals("+") || peek.equals("-") || peek.equals("*") || peek.equals("/")) {
                    result.append(" " + stack.pop());
                } else {
                    result.append(stack.pop());
                }
            }
            else stack.pop();
        }
        return result.toString();
    }
```


다음은 <b>'기능'</b>단위로 분리한 메서드 입니다. 전체 소스는 [이 곳](https://github.com/KimYongjun413/DalLab-Mentoring/blob/900e62754bc39f601836cb53c9817664e7a5f4fc/Calculator_Java/src/main/Calculator.java "자바 계산기 히스토리")에서 확인 하실 수 있습니다.
```java
public static String getPostFixResult (char[] charFormula) {
        Stack<String> stack = new Stack<>();
        StringBuffer result = new StringBuffer();

        for (int i = 0; i < charFormula.length; i++) {
            switch (charFormula[i]) {
                case '0':   case '1':   case '2':   case '3':   case '4':
                case '5':   case '6':   case '7':   case '8':   case '9':
                case '.':
                    result = resultAppend(stack, charFormula, i, result);
                    break;
                case '+':   case '-':   case '/':   case '*': case '(':
                    result = pushByWeight(stack, charFormula, i, result);
                    break;
                case ')':
                    result = popByWeight(stack, charFormula, i, result);
                    break;
                case ' ': case ',':
                    break;
                default:
                    return "0";
            }
        }
        return popRemainAll(stack, result).toString();
    }
```

<h1 style="margin:0px;"> 느낀점 </h1>
<hr style="height:1px; margin:0px;">
- '단위 테스트'의 중요성을 느꼈습니다. 그 전에는 디버깅을 통해 테스트를 하였지만, 디버깅 할 때 마다 다수의 Test 데이터를 입력하는 것이 귀찮고도 힘든 일이었습니다. 
 피드백 대로 메서드를 '기능'단위로 분리하여 테스트를 하다보니 오류를 해결하는 복잡도가 많이 낮아지는 것을 알 수 있었습니다.

<h1 style="margin:0px;"> 배운점 </h1>
<hr style="height:1px; margin:0px;">
- 코드를 잘 때, 어떠한 관점으로 소스코드를 작성해야하는지 눈을 뜨게 되었습니다. 변수명,메서드명 어느 것 하나 중요치 않은 요소가 없었습니다.

<h1 style="margin:0px;"> 앞으로의 계획 </h1>
<hr style="height:1px; margin:0px;">
- 문자열 계산기 API를 만드려고 합니다.

<h1 style="margin:0px;"> 피드백 </h1>
<hr style="height:1px; margin:0px;">
1. 추가 피드백을 받으면 여기에 올리도록 하겠습니다.

<h1 style="margin:0px;"> 참고자료 </h1>
<hr style="height:1px; margin:0px;">

API  
[김수보 소장님의 블로그 - IT중심에서 의 'API의 정의'](https://subokim.wordpress.com/2011/12/13/api-definition/ "API의 정의")

JUnit  
[박재성님의 junit4 테스트 라이브러리 기본 사용법](https://www.youtube.com/watch?v=tyZMdwT3rIY "junit4 테스트 라이브러리 기본 사용법")  
[junit4 테스트 라이브러리 기본 사용법](https://www.youtube.com/watch?v=tyZMdwT3rIY "junit4 테스트 라이브러리 기본 사용법")

Java  
[가장 빨리 만나는 코어자바9](http://www.yes24.com/Product/Goods/59417581?Acode=101 "가장 빨리 만나는 코어자바9")