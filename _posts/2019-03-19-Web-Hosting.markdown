---
layout: post
title:  (Javascript)웹 호스팅 등록하여 서버 이용하기
date:   2019-03-15 11:50:00
author: Kim Yongjun
categories: Javascript
tags: Javascript
---

# 웹 호스팅 등록하여 서버 이용하기

안녕하세요!! 김용준입니다.

이번 주 달랩 멘토링 과제는 인터넷으로 접속 가능한 개인 사이트 만들기 입니다.  
과제를 내 주신 의도가 개인 블로그를 만들게 하는데 가깝다고 하셨습니다. 하지만  

"수단과 방법은 가리지 않습니다."

라고 하셨기에 제가 자바스크립트를 공부하면서 웹 호스팅 등록하여 서버를 이용하는 방법을 포스팅하고자 합니다!

해당 내용은 [Do it! 자바스크립트 + 제이쿼리 입문](http://www.yes24.com/Product/Goods/59461086? "Do it! 자바스크립트 + 제이쿼리 입문") 의 313 ~ 316페이지의 내용을 공부하여 포스팅 했습니다.
<br><br>

<h1 style="margin:0px;"> 웹 호스팅 </h1>
<hr style="height:1px; margin:0px;">


### 1. 웹 호스팅이란?  
외부 서버 컴퓨터에 일정한 저장 공간을 임대하여 웹 서비스를 제공해 주는 것을 말합니다.  
여기에서는 [아이비로](http://www.ivyro.net/ "아이비로") 호스팅을 이용할 것입니다.
<br>
<br>
<h1 style="margin:0px;"> 웹 호스팅 등록 </h1>
<hr style="height:1px; margin:0px;">

### 1. [아이비로](http://www.ivyro.net/ "아이비로") 사이트에 방문하여 회원 가입 후 로그인한 후에 무료로 제공하는 호스팅을 신청합니다.

<img src="{{ site.baseurl }}/assets/ivyro_1.PNG" title="아이비로 회원가입" class="ivyro">


### 2. [무료 호스팅]을 누른 후 다음과 같이 계정 정보를 입력합니다.

<img src="{{ site.baseurl }}/assets/ivyro_2.PNG" title="아이비로 무료 호스팅" class="ivyro">

- 서비스 신청 사양  
서버 타입 : UTF-8 / PHP 5.6 / MySQL 5.6  
설치파일 업로드 : Xpress Engine


- 계정 정보 입력  
계정 아이디, 비밀번호 : 파일을 업로드할 때 필요합니다. 아이디는 도메인 주소로 지정됩니다.
MySQL 비밀번호 : 데이터를 저장할 때 필요한 DB 비밀번호 입니다.

### 3. 원격으로 파일을 올릴 수 있는 [FileZilla](http://filezilla-project.org/ "FileZilla") 프로그램을 설치합니다.

<img src="{{ site.baseurl }}/assets/ivyro_3.PNG" title="아이비로 다운로드" class="ivyro">
<img src="{{ site.baseurl }}/assets/ivyro_4.PNG" title="아이비로 다운로드" class="ivyro">

### 4. 내려받은 프로그램을 설치 후 다음과 같이 입력 요소를 채우고 [빠른 연결]버튼을 누릅니다. 이때 '비밀번호를 저장하겠습니까?'라고 물어보는 경고 창이 나타납니다. 비밀번호를 저장하면 다음에 접속할 때는 비밀번호를 입력하지 않아도 됩니다.

<img src="{{ site.baseurl }}/assets/ivyro_5.PNG" title="FileZilla 로그인" class="ivyro">

- 호스트 : 도메인 주소만 입력합니다.(ex: ID.ivyro.net)
- 사용자명 : 아까 만든 아이비로 계정 아이디를 입력합니다.
- 비밀번호 : 아이비로 계정 비밀번호를 입력합니다.
- 포트 : FTP 프로토콜 '21' 또는 SFTP 프로토콜 '22'를 입력합니다.
- 빠른 연결 : [빠른 연결] 버튼을 누르면 웹 호스팅에 원격으로 접속이 완료됩니다.

### 5. 간단한 문서를 작성 후 서버(웹 호스팅) 컴퓨터의 홈 디렉터리에 파일을 업로드합니다.

<img src="{{ site.baseurl }}/assets/ivyro_6.PNG" title="FileZilla 업로드" class="ivyro">

### 6. 브라우저에 앞에서 가입했던 도메인(계정ID.ivyro.net)을 입력한 후 이동하여 정상적으로 페이지가 나타나는지 확인합니다.

<img src="{{ site.baseurl }}/assets/ivyro_7.PNG" title="페이지 확인" class="ivyro">

<h1 style="margin:0px;"> 참고자료 </h1>
<hr style="height:1px; margin:0px;">

[Do it! 자바스크립트 + 제이쿼리 입문](http://www.yes24.com/Product/Goods/59461086? "Do it! 자바스크립트 + 제이쿼리 입문")
