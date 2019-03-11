---
layout: post
title:  (C#) 괄호없는 다항 사칙연산 계산기
date:   2019-03-11 10:28:00
author: Kim Yongjun
categories: C#
tags: C#
---

# (C#) 괄호없는 다항 사칙연산 계산기

안녕하세요!! 김용준입니다.

달랩 멘토링 첫 과제는 "사칙연산 계산기 만들기" 입니다.  

현재 윈도우 표준 계산기는 사칙연산 우선순위에 의해 계산되지 않는걸 보고   
우선순위가 적용되는 사칙연산 계산기를 만들어 보기로 했습니다!!  
(공학용 계산로 하면 물론 사칙연산 우선순위에 맞게 계산이 됩니다.)


저는 C#으로 계산기를 만들어 봤습니다.  
IDE는 visual studio 2008를 사용하였습니다.  
대상 프레임워크는 .NET Framework 3.5 이고 Windows Form을 사용하여 개발했습니다.
<br><br>

# 선행과정
<hr style="height:0px">


## 1. 계산기를 사용하는 상황과 사용자
- 사용하는 상황 : 윈도우의 표준 계산기와 같이 간단한 계산을 하고 싶을 때 사용합니다.
- 조작 방법
  - 윈도우의 표준 계산기처럼 키보드 입력과 버튼입력으로 식을 입력하게 됩니다.
  - 식을 완성하면 바로 계산된 답이 뜹니다.("="버튼이 없습니다)
- 사용자 : 사칙연산 우선순위를 지키는 간단한 계산을 하고싶은 사용자.

## 2. 이 프로그램에 무엇을 입력했을 때 어떤 결과가 나올지 여러 가지로 탐구
- 키보드로 나머지(%)가 입력되지 않도록 개발
- 0으로 나누엇을 때 infinity로 표현
- 식 과 답이 표현해야할 총 길이는?
  - decimal형식의 전체 자릿수는 28-29개의 유효 자릿수 그럼으로 총 입력길이는 28개로 제한합니다.
- 답의 소수점은 몇쨰자리까지 표현해야 할까?
  - 28자리
- 괄호 및 연산 우선순위는 어떻게 인식해야 할까?
  - 괄호를 사용한 연산 우선순위가 있는 계산기는 다음 수정시에 반영할 예정입니다.

## 3. 이 프로그램이 제대로 됐음을 확인할 수 있는 방법
- 아무키나 입력하여 문자가 입력되는지 확인합니다.
- 입력가능한 최대길이의 숫자를 입력하여 연산해봅니다.
- 연산기호를 입력해본다. 식의 처음이 숫자가 아닌 연산자가 오면 어떻게 계산 되는지 확인합니다.
- 괄호를 사용한 우선순위 연산이 되는지 확인해봅니다.
  - 해당기능을 가진 계산기 구현 후 해야할 확인할 예정입니다.

## 4. 프로그램을 구현합니다.
- [괄호없는 다항 4칙연산 계산기] Calculator_C# 폴더에 있습니다.<br>
https://github.com/KimYongjun413/DalLab-Mentoring.git

# 개발과정
<hr style="height:0px">

## 문제발생
학부생 때 스택으로 계산기를 만들던게 생각났습니다. 그래서 스택을 사용하여 개발을 합니다.  
하지만 도저히 스택으로 괄호없는 우선수위 연산로직을 만들수 없었습니다.

<b>이유는 다음과 같습니다.</b>  
1. 입력제한 덕분에 연산자를 필터조건으로 걸어 피연산자와 연산자를 구분할 수 있었습니다.  
2. 하지만, 어떻게 곱과 나눗셈을 먼저 해줄지에 대한 로직이 생각나지 않았습니다. 

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


## 해결방법
1. 인덱스를 통해 데이터에 접근하기 위해 리스트를 사용했습니다.  
2. 피연산자, 연산자 리스트를 만들고,   
중위표기법 순서를 가진 식 리스트를 두 리스트를 사용해 만들었습니다.  
곱과 나눗셈을 먼저 계산한뒤 덧샘과 뺄샘을 해주어 최종 답을 만들었습니다.

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

//숫자와 연산자가 중위표기법 순으로 담겨있습니다.
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


# 느낀점
<hr style="height:0px">
- 낯설음을 느꼈습니다.
C#, .Net Framework3.5 기본으로 그동안 개발을 해왔지만, 특정 외산 프레임워크에 익숙한 저 였습니다.
익숙함을 벗어나 무언가를 개발해본게 오랜만이라 상당히 낯설었습니다.
- 그럼에도 만들어보고자 하는 방향대로 만들 수 있어 다행이라고 생각했습니다.

# 배운점
<hr style="height:0px">
- 낯설게 개발할 때 제가 깊게 몰입하여 집중하는 것을 느꼈습니다.
- 개발에 대한 재미를 더욱 더 깊게 느꼈습니다. 이래서 다들 토이프로젝트를 하는가 봅니다.
- 유저컨트롤을 만드는 법을 이번에 알았습니다. 일 할 때 써먹어도 좋을 것 같습니다.

# 앞으로의 계획
<hr style="height:0px">
- 괄호를 입력가능하게 하여 "괄호있는 다항 4칙연산 계산기"를 만들어 볼 계획입니다.
- 연사자를 두번이상 연속으로 입력 불가능 하도록 개선해야 합니다.

# 피드백
<hr style="height:0px">
1. 피드백을 받으면 여기에 올리도록 하겠습니다.

# 참고자료
<hr style="height:0px">
[C# TextBox 사이즈 변경] https://www.dazzii.com/c-textbox-%EC%82%AC%EC%9D%B4%EC%A6%88-%EB%B3%80%EA%B2%BD/

[C# TextBox에 숫자만 입력받기] https://terrorjang.tistory.com/39