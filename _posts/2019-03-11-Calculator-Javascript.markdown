---
layout: post
title:  (HTML) 괄호없는 다항 사칙연산 계산기
date:   2019-03-11 11:35:00
author: Kim Yongjun
categories: Javascript
tags: Javascript
---

# (HTML) 괄호없는 다항 사칙연산 계산기

안녕하세요!! 김용준입니다.

달랩 멘토링 첫 과제는 "사칙연산 계산기 만들기" 입니다.  

현재 윈도우 표준 계산기는 사칙연산 우선순위에 의해 계산되지 않는걸 보고   
우선순위가 적용되는 사칙연산 계산기를 만들어 보기로 했습니다!!  
(공학용 계산기로 하면 물론 사칙연산 우선순위에 맞게 계산이 됩니다.)


C#에 이어 HTML-CSS, Javascript로 계산기를 만들어 봤습니다.  
IDE는 visual studio code를 사용하였습니다.
<br><br>

<h1 style="margin:0px;"> 선행과정 </h1>
<hr style="height:1px; margin:0px;">


### 1. 계산기를 사용하는 상황과 사용자
- 사용하는 상황 : 윈도우의 표준 계산기와 같이 간단한 계산을 하고 싶을 때 사용합니다.
- 조작 방법
  - 윈도우의 표준 계산기처럼 키보드 입력과 버튼입력으로 식을 입력합니다.
  - 키보드로 식을 완성하면 바로 계산된 답이 뜹니다.
  - 마우스로 피연산자와 연산자를 클릭 후 "="버튼을 누르면 계산이 됩니다.
- 사용자 : 사칙연산 우선순위를 지키는 간단한 계산을 하고싶은 사용자.

### 2. 이 프로그램에 무엇을 입력했을 때 어떤 결과가 나올지 여러 가지로 탐구
- 키보드로 나머지(%)가 입력되지 않도록 개발
- 문자를 입력하면 식입력 창에서 지워지도록 개발
- 계산식의 유효자리 자리수 이상으로 입력이 불가능 하도록 계산.
  - 계산식 최대 표현 자릿수를 고정할 방법을 찾아보고 수정하여 반영할 예정입니다.
- 0으로 나누었을 때 infinity로 표현
- 괄호 및 연산 우선순위는 어떻게 인식해야 할까?
  - 괄호를 사용한 연산 우선순위가 있는 계산기는 다음 수정시에 반영할 예정입니다.

### 3. 이 프로그램이 제대로 됐음을 확인할 수 있는 방법
- 아무키나 입력하여 문자가 입력되는지 확인합니다.
- 입력가능한 최대길이의 숫자를 입력하여 연산해봅니다.
- 연산기호를 입력해본다. 식의 처음이 숫자가 아닌 연산자가 오면 어떻게 계산 되는지 확인합니다.
- 괄호를 사용한 우선순위 연산이 되는지 확인해봅니다.
  - 괄호있는 우선순위 사칙연산 계산기는 개발예정입니다.

### 4. 프로그램을 구현합니다.
- [괄호없는 다항 4칙연산 계산기] Calculator_Javascript 폴더에 있습니다.<br>
https://github.com/KimYongjun413/DalLab-Mentoring.git

<h1 style="margin:0px;"> 개발과정 </h1>
<hr style="height:1px; margin:0px;">


올해 1월, 2월 동안 퇴근 후 공부한 HTML-CSS, Javascript를 이번 기회를 통해 사용해보기로 했습니다.

<b>개발순서</b>는 다음과 같습니다.

1.계산기 모양으로 테이블을 그립니다.
2.cell을 클릭하면 cell에 해당하는 값이 입력창에 표현되도록 합니다.  
3.계산식을 계산하는 메소드를 만듭니다.  
4.결과값을 도출합니다.

### 문제발생
<b>어려워 한 부분은 다음과 같습니다.</b>  
1. 개발순서 2번에서 막혔습니다. 어떤 메소드가 어떤 때에 동작하는지 익숙치 않아 헤맸습니다.  
 1-1. 놀랍게도 onClick을 생각해내지 못했습니다...
2. 계산식에 숫자와 연산자만 입력하고 싶었는데 이 부분을 구현하기가 어려웠습니다.

### 해결방법
1. 참고자료에 링크해 두었지만,  [JuniorEinstein님의 [별별 웹 특강] HTML + javascript 계산기 만들기 프로젝트!](https://cordelia273.space/32 "JuniorEinstein님의 [별별 웹 특강] HTML + javascript 계산기 만들기 프로젝트!")를 통해 계산기 모양과 cell을 클릭하여 입력창에 값을 표현하는 법을 배웠습니다.
2. [생활코딩 - 정규표현식](https://opentutorials.org/course/743/6580 "생활코딩 - 정규표현식")을 듣고 정규식 사용법을 익힌다음, 한글,영문,특수문자(연산자 제외)한 정규표현식을 작성했습니다.

다음은 계산식을 계산하는 소스코드 입니다.
```javascript
function calculate() {            
    var display = document.getElementById('display');            
    var re = /[ㄱ-ㅎ|ㅏ-ㅣ|가-힣]|[a-z]|[\{\}\[\]?.,;:|\)~`!^_<>@\#$%&\\\=\(\'\"]/gi;
    var newFormula = display.value.replace(re,'');
    document.getElementById('display').value = newFormula;

    if(newFormula == ""){
        reset();
    }

    var result = eval(display.value);
    if(typeof result != "undefined") {
        document.getElementById('result').value = result;
    }
}
```


<h1 style="margin:0px;"> 느낀점 </h1>
<hr style="height:1px; margin:0px;">
- HTML-CSS, Javascript 실습 예제를 직접 타이핑해가면서 공부했지만, 역시 직접 만들면서 깨달아야 한다는 걸 다시 한번 느꼈습니다.
- 제가 모르는 걸 배워가며 개발하는 걸 재미있어 한다는 걸 다시 한번 알게 되었습니다.

<h1 style="margin:0px;"> 배운점 </h1>
<hr style="height:1px; margin:0px;">
- 정규 표현식이라는 존재를 알게 되었고, 이것 역시 공부해야 할 필요성을 느꼈습니다.
- [코딩의 신 아샬](https://www.youtube.com/channel/UCLLncfeIYljE0o_yUw7MkcA "코딩의 신 아샬 Youtube")님이 말씀해주신 "손이 익숙해지면 이해가 그 뒤를 따라옵니다."에 적극 동의하며 계속 실천해야겠다고 다시 한번 다짐하는 계기가 되었습니다.

<h1 style="margin:0px;"> 앞으로의 계획 </h1>
<hr style="height:1px; margin:0px;">
- 괄호를 입력가능하게 하여 "괄호있는 다항 4칙연산 계산기"를 만들어 볼 계획입니다.
- 연산자를 두번이상 연속으로 입력 불가능 하도록 개선해야 합니다.

<h1 style="margin:0px;"> 피드백 </h1>
<hr style="height:1px; margin:0px;">
1. 피드백을 받으면 여기에 올리도록 하겠습니다.

<h1 style="margin:0px;"> 참고자료 </h1>
<hr style="height:1px; margin:0px;">


[JuniorEinstein님의 [별별 웹 특강] HTML + javascript 계산기 만들기 프로젝트!](https://cordelia273.space/32 "JuniorEinstein님의 [별별 웹 특강] HTML + javascript 계산기 만들기 프로젝트!")

[egoing님의 생활코딩 - 정규표현식과 JavaScript](https://opentutorials.org/course/743/6580 "정규표현식과 JavaScript")
