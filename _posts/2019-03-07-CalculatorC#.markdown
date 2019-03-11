---
layout: post
title:  (C#) 괄호없는 다항 사칙연산 계산기
date:   2019-03-07 10:28:00
author: Kim Yongjun
categories: C#
tags: C#
---

# (C#) 괄호없는 다항 사칙연산 계산기

안녕하세요!! 김용준입니다.

달랩 멘토링 첫 과제는 "사칙연산 계산기 만들기" 입니다.  

현재 윈도우 표준 계산기는 사칙연산 우선순위에 의해 계산되지 않는걸 보고   
우선순위가 적용되는 사칙연산 계산기를 만들어 보기로 했습니다!!  
(공학용 계산기로 하면 물론 사칙연산 우선순위에 맞게 계산이 됩니다.)


저는 C#으로 계산기를 만들어 봤습니다.  
IDE는 visual studio 2008를 사용하였습니다.  
대상 프레임워크는 .NET Framework 3.5 이고 Windows Form을 사용하여 개발했습니다.
<br><br>

<h1 style="margin:0px;"> 선행과정 </h1>
<hr style="height:1px; margin:0px;">


### 1. 계산기를 사용하는 상황과 사용자
- 사용하는 상황 : 윈도우의 표준 계산기와 같이 간단한 계산을 하고 싶을 때 사용합니다.
- 조작 방법
  - 윈도우의 표준 계산기처럼 키보드 입력과 버튼입력으로 식을 입력합니다.
  - 식을 완성하면 바로 계산된 답이 뜹니다.("="버튼이 없습니다)
- 사용자 : 사칙연산 우선순위를 지키는 간단한 계산을 하고싶은 사용자.

### 2. 이 프로그램에 무엇을 입력했을 때 어떤 결과가 나올지 여러 가지로 탐구
- 키보드로 나머지(%)가 입력되지 않도록 개발해야 합니다.
- 0으로 나누었을 때 infinity로 표현합니다.
- 식 과 답이 표현해야할 총 길이는 어떻게 해야 할까요?
  - decimal형식의 전체 자릿수는 28-29개의 유효 자릿수 그럼으로 총 입력길이는 28개로 제한합니다.
- 답의 소수점은 몇쨰자리까지 표현해야 할까요?
  - 28자리
- 괄호 및 연산 우선순위는 어떻게 인식해야 할까요?
  - 괄호를 사용한 연산 우선순위가 있는 계산기는 다음 수정시에 반영할 예정입니다.

### 3. 이 프로그램이 제대로 됐음을 확인할 수 있는 방법
- 아무키나 입력하여 문자가 입력되는지 확인합니다.
- 입력가능한 최대길이의 숫자를 입력하여 연산해봅니다.
- 연산기호를 입력해본다. 식의 처음이 숫자가 아닌 연산자가 오면 어떻게 계산 되는지 확인합니다.
- 괄호를 사용한 우선순위 연산이 되는지 확인해봅니다.
  - 괄호있는 우선순위 사칙연산 계산기는 개발예정입니다.

### 4. 프로그램을 구현합니다.
- [괄호없는 다항 4칙연산 계산기] Calculator_C# 폴더에 있습니다.<br>
https://github.com/KimYongjun413/DalLab-Mentoring.git

<h1 style="margin:0px;"> 개발과정 </h1>
<hr style="height:1px; margin:0px;">

### 문제발생
학부생 때 스택으로 계산기를 만들던게 생각났습니다. 그래서 스택을 사용하여 개발을 합니다.  
하지만 스택으로 괄호없는 우선수위 연산로직을 만들 방법이 생각나지 않았습니다.

<b>어려워 한 부분은 다음과 같습니다.</b>  
1. 어떻게 곱과 나눗셈을 먼저 해줄까요?  
 1-1. Push와 Pop으로 곱셈을 먼저 하게만들 스택을 어떻게 만들까요? 

그래서 일단 입력받은 2개의 숫자와 1개의 연산자만 계산하는 로직을 만들었습니다.

```C#
//연산자를 스택(stOperator)에 담고, 피연산자를 스택(stNumbers)에 담습니다.
Stack<decimal> stNumbers = new Stack<decimal>();
Stack<char> stOperator = new Stack<char>();
for (int n = 0; n < txtFormula.Text.Length; n++){
    if (cFormula[n] == '*'){ stOperator.Push('*'); }
    else if (cFormula[n] == '/') { stOperator.Push('/'); }
    else if (cFormula[n] == '+') { stOperator.Push('+'); }
    else if (cFormula[n] == '-') { stOperator.Push('-'); }   
    else { stNumbers.Push(Convert.ToDecimal(strNumber[n])); }

//2항 사칙연산만 가능한 상황입니다.
x = stNumbers.Pop();
y = stNumbers.Pop();
cOper = stOperator.Pop();
if (cOper == '*'){ txtResult.Text = (x * y).ToString(); }
else if (cOper == '/'){ if (y == 0) txtResult.Text = "Infinity";
                        else txtResult.Text = (x / y).ToString(); }
else if (cOper == '+') { txtResult.Text = (x + y).ToString(); }
else if (cOper == '-') { txtResult.Text = (x - y).ToString(); }
```


### 해결방법
연산자와 피연산자를 가진 각각의 리스트를 만들어서, 
이 두개의 리스트를 이용하여 중위표기법 순서를 가지는 식 리스트를 만들어 해결하기로 했습니다.  
로직의 순서는 다음과 같습니다.
1. 인덱스를 통해 데이터에 접근하기 위해 피연산자와 연산자 리스트 생성합니다.
2. 중위 표기법 순서를 가진 식 리스트를 두 리스트를 사용해 생성 후
곱과 나눗셈을 먼저 계산한 뒤 덧셈과 뺄셈을 해주어 최종 답 도출합니다.

다음은 중위표기법 순서를 가진 리스트를 만드는 소스코드 입니다.
```C#
string strFormula = txtFormula.Text;
char[] cSeparator = { '+', '-', '*', '/' };
string[] strNumber = txtFormula.Text.Split(cSeparator, StringSplitOptions.RemoveEmptyEntries);

//연산자만 리스트에 따로 담습니다.
for (int c = 0; c < strFormula.Length; c++)
{
    if (strFormula[c].ToString() == "*" || strFormula[c].ToString() == "/"
        || strFormula[c].ToString() == "+" || strFormula[c].ToString() == "-")
    {
        oprList.Add(strFormula[c].ToString());
    }
}

//입력한 수들을 리스트에 담습니다.
for (int n = 0; n < strNumber.Length; n++)
{
    numlist.Add(Convert.ToDecimal(strNumber[n]));
}                       

//식 리스트에 피연산자를 담습니다.
for (int i = 0; i < numlist.Count; i++)
{
    formulaList.Add(numlist[i].ToString());
}

//연산자를 숫자 사이에 끼워 넣습니다.(중위표기법 순서를 가진 리스트가 됩니다.)
if (formulaList.Count != 0 && oprList.Count < formulaList.Count)
{
    for (int i = 0; i < oprList.Count; i++)
    {
        formulaList.Insert(2 * i + 1, oprList[i].ToString());
    }
}
```


<h1 style="margin:0px;"> 느낀점 </h1>
<hr style="height:1px; margin:0px;">
- 낯설음을 느꼈습니다.
C#, .Net Framework3.5 기본으로 그동안 개발을 해왔지만, 특정 외산 프레임워크에 익숙해저 있었습니다.
익숙함을 벗어나 무언가를 개발해본게 오랜만이라 상당히 낯설었습니다.
- 그럼에도 만들어보고자 하는 방향대로 만들 수 있어 다행이라고 생각했습니다.
- 개발하면서 느낀것은 아니지만 블로그에 포스팅하기 위한 글쓰기가 일렇게 시간이 많이들고 신경쓸게 많은지 처음 알았습니다.

<h1 style="margin:0px;"> 배운점 </h1>
<hr style="height:1px; margin:0px;">
- 낯설게 개발할 때 제가 깊게 몰입하여 집중하는 것을 느꼈습니다.
- 개발에 대한 재미를 더욱 더 깊게 느꼈습니다. 이래서 다들 토이프로젝트를 하는가 봅니다.
- 유저컨트롤을 만드는 법을 이번에 알았습니다. 일 할 때 써먹어도 좋을 것 같습니다.

<h1 style="margin:0px;"> 앞으로의 계획 </h1>
<hr style="height:1px; margin:0px;">
- 괄호를 입력가능하게 하여 "괄호있는 다항 4칙연산 계산기"를 만들어 볼 계획입니다.
- 연산자를 두번이상 연속으로 입력 불가능 하도록 개선해야 합니다.

<h1 style="margin:0px;"> 피드백 </h1>
<hr style="height:1px; margin:0px;">
1. 피드백을 받으면 여기에 올리도록 하겠습니다.

<h1 style="margin:0px;"> 참고자료 </h1>
<hr style="height:1px; margin:0px;">

[C# TextBox 사이즈 변경](https://www.dazzii.com/c-textbox-%EC%82%AC%EC%9D%B4%EC%A6%88-%EB%B3%80%EA%B2%BD/ "")

[C# TextBox에 숫자만 입력받기](https://terrorjang.tistory.com/39 "C# TextBox에 숫자만 입력받기")