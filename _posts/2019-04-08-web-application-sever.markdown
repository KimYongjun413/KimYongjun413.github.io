---
layout: post
title:  (Java)회원 관리 시스템
date:   2019-04-08 11:50:00
author: Kim Yongjun
categories: Java
tags: Java
---

# 회원 관리 시스템

안녕하세요!! 김용준입니다.

박재성 님의 [자바 웹 프로그래밍 Next Step](http://www.yes24.com/Product/goods/31869154 "자바 웹 프로그래밍 Next Step")의 <br>
<b>'3장 개발 환경 구축 및 웹 서버 실습 요구 사항'</b><br>
의 요구 사항을 주어진 힌트를 사용하여 <b>회원 관리 시스템</b>을 만들었습니다.<br>

사용한 언어는 Java이고 IDE는 IntelliJ입니다.

개발은 4월 3일부터 시작하였습니다.<br>
<b>5일간</b>퇴근 후 학습 및 개발을 진행하여 4월 8일에 3장을 마칠 수 있었습니다.

아직 실습에 대한 부분만 마친 사항이며 추가 학습 자료에 대한 학습 및 스스로에 대한 실습은 마치치 못한 상태입니다.

처음엔 제가 알고 있는 한도 내에서 ERP를 개발해볼까라는 생각을 했습니다.<br> 
설계를 하다 보니 욕심이 커지기 시작했습니다.<br> 
커지기 시작한 욕심은 결국 개발을 시작하기 막막할 정도로 커졌습니다.<br>
시작 전부터 의지가 꺾여서는 안되기에, 저는 좀 가볍게 시작할 수 있는 프로그램을 개발하기로 마음먹었습니다.<br> 
그때, 아샬님이 추천해주신 박재성 님의 자바 웹 프로그래밍 Next Step이 생각났고
이 책에 있는 실습을 진행해 보기로 했습니다. 

제가 생각한 <b>이 책의 장점은 힌트를 주고 스스로 생각하여 개발하게 만든다는 것</b>입니다.
개발하다 막히는 부분이 생길 때마다 다시 한번 요구 사항의 힌트를 읽어보면 
막힌 부분을 풀 수 있는 실마리가 보였습니다.<br> 
다행히도 라이브 코딩을 보지 않고 스스로 개발을 마쳤습니다.
제대로 코딩을 했다고 생각하진 않습니다. 기능 구현이 우선이었으니까요...<br>
<b>하지만 끝까지 완료했다는데 성취감을 느낄 수 있어 기분이 좋습니다.</b>
<br><br>

<h1 style="margin:0px;"> 코딩전 확인</h1>
<hr style="height:1px; margin:0px;">

### 1. 이 프로그램을 사용하는 상황과 필수 요소, 제약 조건 등을 정합니다.
* 사용하는 상황
질문/답변 게시판 입니다.
필수요소 : 회원가입시 아이디, 비밀번호, 이름, 이메일을 입력받습니다.
제약조건 : 회원가입을 한 사용자만 로그인 하여 질문과 답변을 할 수 있습니다.

### 2. 이 프로그램에 무엇을 입력했을 때 어떤 결과가 나와야 하는지 유형을 분류하고 구체적인 사례를 찾아 봅니다.
* 회원가입 : 
    - 회원정보를 입력했을 때 회원가입이 되어야하고 DataBase에 회원정보가 입력되어야 합니다.<br>
    - (현재는 실제 DB연동하여 개발되지 않았습니다.)<br>
    - 회원가입이 성공하면 질문목록 화면으로 이동합니다.<br>
    - 아이디를 입력하지 않았거나, 비밀번호를 틀리면 회원가입이 되지 않습니다.

* 로그인 : 
    - 회원정보를 입력하여 로그인을 하면 DataBase에 입력된 정보와 비교하여 로그인을 진행합니다.
    - 로그인이 성공하면 질문목록 화면으로 이동합니다.
    - 로그인이 실패하면 로그인 실패화면으로 이동합니다.

* 로그아웃을 하면 로그인 화면으로 이동합니다.

* 회원목록 버튼을 누르면 회원 아이디 이름 이메일주소가 있는 상세화면으로 이동합니다.

* 질문하기 : 
    - 질문하기 버튼을 클릭하여 글쓴이,제목,내용을 입력하여 질문하기 버튼을 누르면 DataBase에 저장됩니다.
    - 글쓴이,제목,내용 모두 필수로 입력해야합니다.

* 질문보기 : 
    - 질문 목록 화면에서 각 질문 제목을 클릭하면 각 질문의 상세보기 화면으로 이동합니다.
    - 상세보기 화면에서 질문과 답변의 수정/삭제가 가능합니다.

### 3. 어디까지 구현할 건지, 무엇이 가장 중요한지, 어떤 순서대로 진행해야 할지 결정합니다.
* 구현 범위 : 회원가입, 로그인, 회원목록까지 구현합니다.
* 중요사항 : 
    1. 웹서버에 배포하여 확인할 수 있어야 합니다.
    2. 요구사항의 기능이 작동되도록 구현해야 합니다.
* 구현 순서 
    1. AWS의 EC2인스턴스를 우분투로 생성하여 배포 실습할 환경을 구축합니다.
    2. Git을 설치하여 소스코드를 재배포 하도록 합니다.
    3. Xshell을 설치하여 서버에 접속하여 서비스와 소스를 관리합니다.
    4. 7가지의 요구사항을 차례로 진행하여 개발합니다.
        1) Index.html 응답하기
        2) GET 방식으로 회원가입하기
        3) POST 방식으로 회원가입하기
        4) 302 status code 적용
        5) 로그인하기
        6) 사용자 목록 출력
        7) CSS 지원하기

### 4. 구현합니다.

전체 소스는 [여기](https://github.com/KimYongjun413/web-application-server.git "web-application-server")를 가시면 볼 수 있습니다.

<h1 style="margin:0px;"> 개발과정 </h1>
<hr style="height:1px; margin:0px;">

각 요구사항 구현 과정을 통해 학습한 내용을 인식하며 정리한 내용입니다.

### 요구사항 1 - http://localhost:8080/index.html로 접속시 응답
* 잊지말자 상대경로! ( ./ = 현재 디렉토리 참조, ../ = 현재 디렉토리의 부모 디렉토리 참조)
* The try-with-resources Statement ( try( ) { } )<br>InputStream, OutputStream 은 Closeable 인터페이스를 사용하고 있어서
    () 안에 생성하게되면 try가 끝나고 따로 메모리를 해제하지 않아도 해제가 됩니다!(자바7부터 가능)<br>
    참고 : [The try-with-resources Statement](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html "The try-with-resources Statement")
* InputStream을 BufferedReader로 바꾸는 법<br>
  BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream,"UTF-8"));

### 요구사항 2 - get 방식으로 회원가입
* Map, HasMap<br>
  참고 : [Map, HasMap (wikidocs)](https://wikidocs.net/208 "Map, HasMap 참고1")<br>
 [Map, HasMap (Onsil's blog)](https://onsil-thegreenhouse.github.io/programming/java/2018/02/22/java_tutorial_1-24/ "Map, HasMap (Onsil's blog)")
* String [] url2 = url.split("?"); 은 왜 안될까요??<br>
  정규표현식의 특수문자로 '?' 가 쓰이기 때문에 문자로써 인식을 시켜야 구분자로써 사용할 수 있습니다.
  이스케이프 처리를 하여 다음과 같이 하면 됩니다.<br> String [] url2 = url.split("\\?");

### 요구사항 3 - post 방식으로 회원가입
* POST로 데이터를 전달할 경우 전달하는 데이터는 HTTP 본문에 담깁니다.
* 본문의 길이는 HTTP 헤더의 Content-Length의 값입니다.

### 요구사항 4 - redirect 방식으로 이동
* [List of HTTP status codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes, "List of HTTP status codes")
* [HTTP 302 status code](https://en.wikipedia.org/wiki/HTTP_302, "HTTP 302 status code")<br>
 302 리다이렉트의 의미는 요청한 리소스가 임시적으로 새로운 URL로 이동했음(Temporarily Moved)을 나타냅니다.
  
  

### 요구사항 5 - cookie
* [HTTP 쿠키](https://developer.mozilla.org/ko/docs/Web/HTTP/Cookies, "HTTP 쿠키")
  
* 쿠키값은 어떻게 생성할까요?<br>
  응답 헤더에 Set-cookie를 추가해주면 됩니다!!<br>
  
* 생성된 쿠키는 어디서 봐야 하는 것일까요?<br>
참고 : [[HTTP] 쿠키와 세션 (될성부른떡잎)](https://ledgku.tistory.com/72 "[HTTP] 쿠키와 세션")

### 요구사항 6 - stylesheet 적용
* [Content-Type 이란?](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Content-Type "Content-Type")

<h1 style="margin:0px;"> 느낀점 </h1>
<hr style="height:1px; margin:0px;">

* 방법이 맞던 틀리던 요구 사항대로 개발하려고 노력을 했습니다. <br>
그중 가장 생소하고 고민을 많이 한 부분이 요청과 응답을 주고받는 과정이었습니다.<br>
* [HTTP 메시지(요청,응답 헤더 설명)](https://developer.mozilla.org/ko/docs/Web/HTTP/Messages#HTTP_%EC%9D%91%EB%8B%B5 "HTTP 메시지")을 보고 개념을 잡았습니다.
구글 신의 도움을 받아 힌트를 사용해 요구 사항을 진행할 수 있었는데요...<br>
 기능 중심으로 구현한 거라 잘못된 게 얼마나 많을지 상상도 안됩니다.
* 하지만 리팩토링과 단위 테스트에 중점을 두어 수정을 하다 보면 재밌는 부분들이 많을 것 같아 기대가 됩니다!!

<h1 style="margin:0px;"> 앞으로의 계획 </h1>
<hr style="height:1px; margin:0px;">

- 리팩토링과 단위 테스트를 적용하여 프로그램을 수정 할 계획입니다.

<h1 style="margin:0px;"> 피드백 </h1>
<hr style="height:1px; margin:0px;">

- 



<h1 style="margin:0px;"> 참고자료 </h1>
<hr style="height:1px; margin:0px;">

- [The try-with-resources Statement](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html "The try-with-resources Statement")
- [Map, HasMap (wikidocs)](https://wikidocs.net/208 "Map, HasMap 참고1")
- [Map, HasMap (Onsil's blog)](https://onsil-thegreenhouse.github.io/programming/java/2018/02/22/java_tutorial_1-24/ "Map, HasMap (Onsil's blog)")
- [List of HTTP status codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes, "List of HTTP status codes")
- [HTTP 302 status code](https://en.wikipedia.org/wiki/HTTP_302, "HTTP 302 status code")
- [[HTTP] 쿠키와 세션 (될성부른떡잎)](https://ledgku.tistory.com/72 "[HTTP] 쿠키와 세션")
- [Content-Type 이란?](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Content-Type "Content-Type")
- [HTTP 메시지(요청,응답 헤더 설명)](https://developer.mozilla.org/ko/docs/Web/HTTP/Messages#HTTP_%EC%9D%91%EB%8B%B5 "HTTP 메시지")