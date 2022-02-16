---
layout:     post
title:      Spring Boot 나만의 웹서버 만들기 1편
author:     쭌프로
tags:       JAVA Spring-Boot Spring-Security aws git mysql Thymeleaf
subtitle:   Spring Boot 로 나만의 웹 페이지, 블로그 구현하기
category:   JAVA
---

<!-- Start Writing Below in Markdown -->

![Java]({{ site.url }}/img/java_bg.png)

안녕하세요 쭌프로입니다. <br/>
오랜만에 블로그에 글을 올리려고 합니다. <br/>
회사에서 일하면서 틈틈이 개인적으로 홈페이지를 만들었는데 다 만들고 보니 <br/>
내가 만든 홈페이지가 어떻게 만들어졌으며 <br/> 
어떤 상황에서 문제가 있었는데 어떻게 해결했는지를 <br/>
기록하려고 글을 쓰려고 합니다. 

제가 만든 홈페이지는 하루 50명씩 꾸준히 들어오는 홈페이지로 <br/> 
나름 성공적으로 만들어졌기에 제 블로그의 글을 보고 따라 만들어도 <br/> 
충분히 홈페이지 운영이 되겠다 해서 제가 만드는 방법을 공유하려고 합니다.

Spring Boot으로 웹사이트 만들기 어느 정도 웹 공부 하신 분 <br/>
혹은 개인적으로 공부했는데 어렵다 하시는 분 <br/>
따라 만드는 거 추천합니다.

기본적으로 Git 사용하는 방법은 알고 있어야 합니다. 

## 웹사이트 구축에 필요한 준비

- 코드 편집기
    - 완전 무료 편집기 [Visual Studio Code](https://code.visualstudio.com/download){:target="_blank"}
    - 일부 무료 편집기 [IntelliJ IDEA](https://www.jetbrains.com/ko-kr/idea/){:target="_blank"}
    - ....기타 등등 자신이 쓰는것이 있으면 사용하면 됩니다.
- [Git](https://github.com/){:target="_blank"}
    - 웹 사이트를 만들면서 기록을 남기기 위해서 사용합니다.
    - 제작 중인 코드를 안전한 곳에 저장하기 위해서 사용합니다.
- [AWS](https://aws.amazon.com/ko/?nc2=h_lg){:target="_blank"} 회원가입
    - 웹사이트를 만들고 서버에 올려서 실시간으로 구동 시기키 위해서 필요합니다.
    - 도메인 연결 그리고 HTTPS 연동 이미지 저장소 등등... 모든 서비스를 사용하기 위해서 필요합니다.

## 구현하려는 기능

- Back-End
    - 관리자 페이지 구현
    - 회원가입, 로그인, 로그아웃, 회원탈퇴 기능구현
    - 회원가입시 정상적인 접근인지 유효성 검사기능 넣기 (중복이메일 검사 등등...)
    - OAuth2 카카오, 페이스북, 트위터 로그인 기능 추가
    - 간단한 게시글 쓰기 구현
    - 게시글의 파일 업로드
    - 게시글의 댓글 구현
    - 게시글의 좋아요 기능
    - 게시글 태그기능 추가하기
    - 게시글의 평점 부여하기
- Front-end
    - 게시글 슬라이드
    - 게시글 평점 그래프 구현
    - 간단한 위젯 구현

지금부터 천천히 따라오시면 충분히 구현하실 수 있습니다. <br/>
AWS 를 사용하면 HTTPS 는 무료로 사용하실 수 있습니다. <br/>
다만 도메인은 구매해야 합니다 1년 만원 정도입니다. <br/>
간단하게 웹 구현테스트만 하는 경우 서버비용은 <br/> 
AWS 1년 무료를 사용하면 0원으로 구현하실 수 있습니다. <br/>
다만 이미지가 많다던가 트래픽이 많아지면 1년 무료가 끝나기 전에도 비용이 청구될 수 있습니다.

## 개발 초기 세팅하기

우선은 Spring Boot 초기 설정을 해주기 위해서 <br/>
[Spring Initializr](https://start.spring.io/){:target="_blank"} 사이트에 접속하여 프로젝트를 설정한 후 다운로드 해줍니다.

![spring initializr 셋팅]({{ site.url }}/img/2021/0207_01.png)

- DEVELOPER TOOLS
    - Lombok 
        - 상용구 코드를 줄이는 데 도움이 되는 Java 주석 라이브러리.
    - Spring Boot DevTools 
        - 향상된 개발 환경을 위해 빠른 애플리케이션 재시작, LiveRoad 및 구성을 제공합니다.
- WEB
    - Spring Web WEB
        - 스프링 MVC를 사용하여 RESTful을 포함한 웹을 구축합니다. Apache Tomcat을 기본 내장형 컨테이너로 사용합니다.
- TEMPLATE ENGINES
    - Thymeleaf
        - 웹 및 독립 실행형 환경 모두를 위한 최신 서버측 Java 템플릿 엔진입니다. HTML을 브라우저 및 정적 프로토타입으로 올바르게 표시할 수 있습니다.
- SECURITY
    - Spring Security
        - Spring 애플리케이션을 위한 사용자 지정이 가능한 높은 인증 및 액세스 제어 프레임워크.
- SQL
    - Spring Data JPA
        - SpringData 및 Hibernate를 사용하여 Java Persistence API를 사용하여 SQL 저장소에 데이터를 유지합니다.
    - MySQL Driver
- I/O
    - Validation 
        - Bean 유효성 검사기를 사용한 검증입니다.

## 설정한 프로젝트 실행하기

내려받은 알집을 풀어서 프로젝트를 열어줍니다. <br/>
본인은 인텔리제이를 사용하여 오픈했습니다.

유료 버전인 인텔리제이를 사용할 경우 기본적으로 프로젝트를 열어주면 자동으로 실행 클래스를 잡아주지만 <br/>
저와 같은 무료 버전은 따로 찾아주지 않습니다. <br/>
고로 실행 애플리케이션을 직접 잡아주도록 하겠습니다.

기본적으로 실행 클래스 위치는

~~~
com.jjunpro.website.WebsiteApplication
~~~ 

입니다.

![Spring Boot Application 설정-1]({{ site.url }}/img/2021/0207_02.png)
![Spring Boot Application 설정-2]({{ site.url }}/img/2021/0207_03.png)

이미지의 순서대로 시작 클래스를 맞춰 주시고 실행버튼을 클릭하기 이전에 MySQL 설정먼저 하도록 하겠습니다. <br/>
(MySQL 설정없이 실행하려고 하니 'Failed to determine a suitable driver class' 에러가 발생하였습니다.)

꼭 MySQL을 사용하지 않아도 됩니다. <br/>
저는 딱히 이유는 없이 무료라서 사용하는 것입니다.

- MySQL 설치
    - [Windows](https://www.mysql.com/){:target="_blank"} 설치방법
    - [Mac OS](https://whitepaek.tistory.com/16){:target="_blank"} 설치방법
    
정상적으로 MySQL 이 설치되었다면 DB driver 를 properties 에 설정해주도록 하겠습니다.

~~~
src/main/resources/application.properties

#--------------------------------------------------
# DB
#--------------------------------------------------
# Test DB
spring.datasource.url      = jdbc:mysql://localhost:3306/test?serverTimezone=UTC
spring.datasource.username = root
spring.datasource.password = password
~~~

이제 준비는 끝났습니다. <br/>
어플리케이션을 실행하도록 하겠습니다.
 
Mac Os 단축키로는 ctrl + R 단축키를 눌러서 실행합니다.

![spring boot 초기 실행 완료]({{ site.url }}/img/2021/0207_04.png)

위 이미지처럼 로그가 나온다면 성공적으로 실행이 되었습니다!

다음에는 별로 대단한 건 아니지만 로그 창에 출력되는 큰 Spring 문구를 나만의 커스텀 로그로 변경해보는 시간을 가져보도록 하겠습니다.

## 예제 Git 주소

혹시 제작중인 Spring Boot 프로젝트가 궁금하시면 아래 링크에서 확인하실 수 있습니다.

[https://github.com/alalstjr/jjunpro-blog-ex](https://github.com/alalstjr/jjunpro-blog-ex){:target="_blank"}