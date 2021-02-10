---
layout:     post
title:      Spring Boot 나만의 웹서버 만들기 2편
author:     쭌프로
tags:       JAVA Spring-Boot Spring-Security aws git mysql Thymeleaf
subtitle:   로그창에 나만의 이름을 남겨보자
category:   JAVA
---

<!-- Start Writing Below in Markdown -->

![Java]({{ site.url }}/img/java_bg.png)

안녕하세요 쭌프로입니다. <br/>
오늘은 Spring Application 을 구동 시 출력되는 로그의 Spring 문구를 <br/>
자신만의 이름, 개발 버전이 출력되도록 수정해 보도록 하겠습니다.

## Banner Custom 나만의 개발환경 만들기

우선은 최상위에 폴더를 하나 생성합니다. <br/>
폴더 이름은 공통으로 사용하는 Class 모음을 의미하는 Common 으로 생성합니다. <br/>
그리고 Common 폴더에 CustomBanner Class 를 생성합니다.

~~~
com.jjunpro.website.common.CustomBanner

/**
 * Application 구동시 출력되는 기본 정보
 */
public class CustomBanner implements Banner {

    @Override
    public void printBanner(
            Environment environment,
            Class<?> sourceClass,
            PrintStream out
    ) {
        out.println("  __      ____.    ____.                                  __");
        out.println(" / /     |    |   |    |__ __  ____ _____________  ____   \\ \\  ");
        out.println("/ /      |    |   |    |  |  \\/    \\\\____ \\_  __ \\/  _ \\   \\ \\ ");
        out.println("\\ \\  /\\__|    /\\__|    |  |  /   |  \\  |_> >  | \\(  <_> )  / / ");
        out.println(" \\_\\ \\________\\________|____/|___|  /   __/|__|   \\____/  /_/  ");
        out.println("                                  \\/|__|");
        out.println(":: Project :: " + environment.getProperty("project.name"));
        out.println(":: Version :: " + environment.getProperty("project.version"));
        out.println(":: State   :: " + environment.getProperty("project.state"));
        out.println(" ");
    }
}
~~~

[Banner Class](https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/Banner.html){:target="_blank"} <br/>
프로그래밍 방식으로 배너를 작성하기위한 인터페이스 클래스입니다.

Banner 상속을 받을 수 있도록 하고 Override Method printBanner 를 불러옵니다. <br/>
매개변수의 [Environment environment] 는 Spring Boot properties 의 작성한 정보를 불러와 표기할 수 있도록 정보를 가져오며 <br/>
[PrintStream out] 변수는 문구 출력을 도와주는 역할을 합니다.

[아스키코드로 변환해주는 사이트](https://wepplication.github.io/tools/asciiArtGen/){:target="_blank"} 에서 출력을 원하는 문구를 작성 후 <br/> 
[PrintStream out] println 값에 적절하게 넣어줍니다.

하단에는 프로젝트명 버전 상태를 표시하는 정보도 같이 작성해 줍니다. <br/>
마지막으로 프로젝트 정보를 `src/main/resources/application.properties` 위치에 작성해 줍니다.

~~~
src/main/resources/application.properties

#--------------------------------------------------
# COMMON
#--------------------------------------------------
project.name    = JJunproSite
project.version = 0.0.0
project.state   = Prod

...
~~~

마지막으로 커스텀한 Banner 를 Application 에 적용하도록 하겠습니다.

~~~
com.jjunpro.website.WebsiteApplication

@SpringBootApplication
public class WebsiteApplication {

    public static void main(String[] args) {
        SpringApplication application = new SpringApplication(WebsiteApplication.class);
        application.setBanner(new CustomBanner());
        application.run(args);
    }
}
~~~

단순하게 run 으로 Application 을 실행해주는 코드에 <br/>
CustomBanner() Class 가 실행될 수 있도록 추가하였습니다.

![Spring Boot Application 커스텀 로그 설정]({{ site.url }}/img/2021/0208_01.png)

정상적으로 커스텀 로그가 출력되는 것을 확인하실 수 있습니다. <br/>
이제 진짜 나만의 웹사이트 개발 코드가 되는거 같은 기분이 들지 않을까요!?

## Prod or Development 상태 구분하여 실행하기

여기서 끝나면 아쉬운 부분이 있으니 한 가지 더 작성하고 갑니다. <br/>
개발을 하다 보면 개발환경 따로 실제 운용하는 서버 따로 존재합니다.

예를 들어서 개발하는데 실제 서버의 DB를 가지고 개발하면 현재 운용 중인 홈페이지에 영향이 가기 때문에 <br/>
문제가 생길 수 도 있습니다. 그래서 local DB 를 가지고 개발을 합니다. <br/>
하지만 Prod, Development 상태의 Spring Boot 환경설정을 상황에 따라서 주석을 풀어주고 교체해주고 하면 너무나 귀찮은 일입니다. <br/>
그래서 활용하는 것이 Program arguments 입니다.

자세한것은 제 [Git 프로파일을 정리한 글](https://github.com/alalstjr/Java-spring-boot#%ED%94%84%EB%A1%9C%ED%8C%8C%EC%9D%BC){:target="_blank"} 을 확인해 주세요.

`src/main/resources/application-development.properties` <br/> 
Spring Boot 로컬 개발전용 설정 Properties 를 생성해줍니다.

그 후 로컬환경정보를 입력합니다.

~~~
src/main/resources/application-development.properties

#--------------------------------------------------
# COMMON
#--------------------------------------------------
project.name    = JJunproSite
project.version = 0.0.0
project.state   = Development

#--------------------------------------------------
# DB
#--------------------------------------------------
# Test DB
spring.datasource.url      = 로컬 MySQL 정보
spring.datasource.username = 로컬 MySQL 정보
spring.datasource.password = 로컬 MySQL 정보
~~~

또 실제 운용하려는 사이트의 Prod 정보도 입력해 줍니다.

~~~
src/main/resources/application.properties

#--------------------------------------------------
# COMMON
#--------------------------------------------------
project.name    = JJunproSite
project.version = 0.0.0
project.state   = Prod

#--------------------------------------------------
# DB
#--------------------------------------------------
# Test DB
spring.datasource.url      = 실제 운용 MySQL 정보
spring.datasource.username = 실제 운용 MySQL 정보
spring.datasource.password = 실제 운용 MySQL 정보
~~~

이제 로컬 개발환경과 실제 운용하는 서버의 개발 환경을 나누어 주었습니다. <br/>
마지막으로 실행할 때 Development 가 실행될 수 있도록 Run Application 정보를 수정하겠습니다.

![Spring Boot Application Development 설정]({{ site.url }}/img/2021/0208_02.png)

Environment variables 값을 `--spring.profiles.active=development` 으로 넣어주고 Run 해줍니다. <br/>
그러면 개발환경으로 프로젝트가 실행되는 것을 확인하실 수 있습니다.

완벽하게 자신만의 웹사이트라고 도장을 콱 찍어주었습니다. <br/>
이제 자신만의 웹사이트를 제작하는 일만 남았습니다.

다음에는 Domain 을 생성하여 DB를 만드는 작업을 해보도록 하겠습니다.

## 예제 Git 주소

혹시 제작중인 Spring Boot 프로젝트가 궁금하시면 아래 링크에서 확인하실 수 있습니다.

[https://github.com/alalstjr/jjunpro-blog-ex](https://github.com/alalstjr/jjunpro-blog-ex){:target="_blank"}