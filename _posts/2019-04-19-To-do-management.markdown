---
layout: post
title:  (C#)작업 관리 프로그램
date:   2019-04-19 16:00:00
author: Kim Yongjun
categories: C#
tags: C#
---

# 작업 관리 프로그램

안녕하세요!! 김용준입니다.

이번 주 달랩 멘토링 과제는 '작업 관리 프로그램' 입니다.  

이 프로그램은 제가 최종 완성하게 될 프로그램의 '프로토타입' 이라고 생각해 주세요!  

이 프로그램에 사용된 언어는 C# 이고  
IDE는 visual studio 2010를 사용하였습니다.  
대상 프레임워크는 .NET Framework 3.5 이고 Windows Form을 사용하여 개발했습니다.



<h1 style="margin:0px;"> 코딩전 확인</h1>
<hr style="height:1px; margin:0px;">

### 1. 이 프로그램을 사용하는 상황과 필수 요소, 제약 조건 등을 정합니다. “내가 이 프로그램을 쓴다면?”이라고 가정하면 더 쉬울 수 있습니다.

* 가장 먼저 든 생각!!
현재 저는 일주일 5일 운동 한 시간 하기를 실천 중입니다만...말처럼 쉽지가 않죠?<br>
'강제성'이 필요합니다.<br>
제가 운동을 하지 않을 때 무엇을 하나 생각해 봤습니다.<br>
'유튜브'죠... 그렇습니다. 운동을 안 하면 '유튜브'를 못 보게 막아 버리면  
운동을 할 수밖에 없을것입니다...ㅎㅎㅎ<br>
(스스로 무덤을 파는 거 같지만...)

* 사용하는 상황 : 운동도 하기 전에 유튜브를 보려 하는 나를 다그치기 위한 용도!!
* 필수요소 : 운동 데이터
* 제약조건 : 운동을 하지 않았으면 유튜브를 볼 수 없다!!

### 2. 이 프로그램에 무엇을 입력했을 때 어떤 결과가 나와야 하는지 유형을 분류하고 구체적인 사례를 찾아 봅니다.
* 운동 데이터 가져오기 :
    - 갤럭시 워치를 통해 입력된 실시간 운동 정보를 해당 프로그램에 입력합니다.

* 결과 : 
    - 총 운동 시간을 합산하여 50분이 넘지 못하면 '유튜브'에 접속 불가능 하도록 설정합니다.

### 3. 어디까지 구현할 건지, 무엇이 가장 중요한지, 어떤 순서대로 진행해야 할지 결정합니다.
* 구현 범위 : 운동 데이터 결과를 통해 hosts 파일을 수정하여 유튜브 접속을 차단하는 기능까지 개발합니다.
* 중요사항 : 
    1. 갤럭시 워치에 입력되어 있는 실시간 운동 데이터를 구할 수 있어야 합니다.
    2. 특정 사이트 접속을 차단하는 기능이 반드시 구현되어야 합니다.
* 구현 순서 
    1. 갤럭시 워치에 입력된 운동 데이터를 확인할 방법을 찾습니다.
    2. 특정 사이트 접속을 차단하는 방법을 찾아 개발합니다.
    3. 운동 데이터를 입력받아 운동 종류, 시작 시간, 종료시간을 그리드에 보여주는 걸 개발합니다.
    4. 제가 선택한 운동의 총 운동시간을 합산하여 특정 사이트 차단 여부를 판단하는 걸 개발합니다.

### 4. 구현합니다.

전체 소스는 [여기](https://github.com/KimYongjun413/DalLab-Mentoring/tree/master/To-do-management_CSharp "To-do-management_CSharp")를 가시면 볼 수 있습니다.

<h1 style="margin:0px;"> 주요 개발과정 </h1>
<hr style="height:1px; margin:0px;">

### 갤럭시 워치에 입력된 운동 데이터를 확인할 방법을 찾습니다.
* 운동을 했다는 것을 '증명'할 방법을 찾는 것입니다.
  
  저는 갤럭시 워치 액티브를 사용하고 있고, 운동시간 및 강도 등을 기록하고 있습니다.<br>
  삼성 헬스에서 운동정보를 가져올 수 있는 방법을 찾아봅니다.
  
  삼성 헬스 어플 내에 데이터를 다운로드할 수 있는 기능이 있어서<br>
  csv 및 json 형태로 정보를 다운로드할 수 있습니다.
  
* csv 파일을 보면 Exercise type가 있는데 어떤 운동인지(풀업,푸쉬업 등등)을 나타내는 코드입니다.
* [이 곳](https://developer.samsung.com/onlinedocs/health/EXERCISE_TYPE.html "운동 타입 코드")을 보시면  Exercise type 코드가 어떤 운동을 의미하는지를 나태내는 표가 있습니다.<br>
제가 할 운동의 운동명과 코드 입니다.  
  1. Plank			10025
  2. Push-ups (Press-ups)	10004
  3. Pull-ups (Chin-up)		10005
  4. Squats			10012
  
  
### 특정 사이트 접속을 차단하는 방법을 찾아 개발합니다.
* C:\Windows\System32\drivers\etc 경로의 hosts 파일을 수정하여 특정 사이트 접속을 차단하는 방법이 있습니다.
* hosts [위키백과](https://ko.wikipedia.org/wiki/Hosts "hosts")에 따르면 'hosts 파일은 악성 소프트웨어의 공격 벡터로 악용될 수 있다.'라고 합니다.<br>
그래서 수정 시 백신에 감지되어 다음과 같은 알림이 뜹니다. <br>특정 사이트의 접속을 차단하는 다른 방법을 찾아봐야 할 것 같습니다.

<center><img src="assets\TodoImage\hostsChanged.JPG" width="400" height="200"></center>

<h1 style="margin:0px;"> 프로그램 미리보기 </h1>

### 초기화면
<center><img src="assets\TodoImage\init.JPG" width="400" height="200"></center>

### 파일 불러오기
<center><img src="assets\TodoImage\open1.JPG" width="400" height="200"></center>
<center><img src="assets\TodoImage\open2.JPG" width="400" height="200"></center>

### 결과 확인하기
#### 통과
<center><img src="assets\TodoImage\success.JPG" width="400" height="200"></center>

#### 실패
<center><img src="assets\TodoImage\fail1.JPG" width="400" height="200"></center>
<center><img src="assets\TodoImage\fail2.JPG" width="400" height="200"></center>


<hr style="height:1px; margin:0px;">

<h1 style="margin:0px;"> 앞으로의 계획 </h1>
<hr style="height:1px; margin:0px;">

- 다른 언어로 개발 또는 기능 추가 및 개선을 통해 점차 업그레이드할 예정입니다.

<h1 style="margin:0px;"> 피드백 </h1>
<hr style="height:1px; margin:0px;">

- 



<h1 style="margin:0px;"> 참고자료 </h1>
<hr style="height:1px; margin:0px;">

