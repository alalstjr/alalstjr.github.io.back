---
layout:     post
title:      Spring Boot 나만의 웹서버 만들기 3편
author:     쭌프로
tags:       JAVA Spring-Boot Spring-Security aws git mysql Thymeleaf
subtitle:   MappedSuperclass, EntityListeners, AuditingEntityListener 알아보기
category:   JAVA
---

<!-- Start Writing Below in Markdown -->

![Java]({{ site.url }}/img/java_bg.png)

안녕하세요 쭌프로입니다. <br/>
모든 Entity 는 기본적으로 상속받아 사용 받을 수 있는 <br/>
BaseEntity 추상 클래스를 생성하고 중복되는 필드값을 공통으로 관리하도록 합니다.

모든 도메인의 상위 클래스라 생각하고 가장 중요한 작업이라 생각합니다.

## BaseEntity 생성하기

폴더 이름은 Domain 으로 생성합니다.
그 후 BaseEntity abstract 추상 클래스를 생성합니다.

~~~
com.jjunpro.website.domain.BaseEntity

@Getter
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public abstract class BaseEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;

    @CreatedDate
    @Column(updatable = false)
    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yy-MM-dd", timezone = "Asia/Seoul")
    private LocalDateTime createdDate;

    @LastModifiedDate
    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yy-MM-dd", timezone = "Asia/Seoul")
    private LocalDateTime modifiedDate;
}
~~~

### @MappedSuperclass

객체의 입장에서, 공통 맵핑 정보를 관리하고 싶을 때, class 위에 기재함 <br/>
[고유 식별값, 생성 날짜, 수정된 날짜] 가 공통으로 포함하고 있습니다. <br/> 
`BaseEntity` 라는 클래스로 따로 만들어서 이 클래스 내부에 공통으로 사용하는 변수를 선언해 줍니다.

`@MappedSuperclass` 어노테이션을 선언해줌으로 써 BaseEntity 를 상속받는 모든 클래스는 <br/>
공통으로 사용하는 변수를 포함하게 됩니다.

Account 도메인을 생성하여 BaseEntity 상속 후 DB 가 생성되는 것을 확인해 보겠습니다.

![MappedSuperclass BaseEntity 생성하기]({{ site.url }}/img/2021/0210_01.png)

테스트해본 결과 정상적으로 공통 변수를 가져와서 생성하는 것을 확인할 수 있습니다. 

### @EntityListeners

Entity의 데이터가 변경할 때 동작합니다. <br/> 
Entity 를 DB에 적용하기 이전에 Call back 을 요청할 수 있는 어노테이션입니다.

~~~
{} 를 통해서, 복수의 리스너를 등록할 수 있음
@EntityListeners(AccountListener.class)
@Entity
public class Account() {
   
   @Id
   private long id;
   
   ...
}

public class AccountListener {
    @PostLoad
    public void postLoad(DataDto dto) {
        log.info("post load: {}", dto);
    }
    
    @PrePersist
    public void prePersist(DataDto dto) {
        log.info("pre persist: {}", dto);
    }
    
    @PostPersist
    public void postPersist(DataDto dto) {
        log.info("post persist: {}", dto);
    }
    
    @PreUpdate
    public void preUpdate(DataDto dto) {
        log.info("pre update: {}", dto);
    }
    
    @PostUpdate
    public void postUpdate(DataDto dto) {
        log.info("post update: {}", dto);
    }
    
    @PreRemove
    public void preRemove(DataDto dto) {
        log.info("pre remove: {}", dto);
    }
    
    @PostRemove
    public void postRemove(DataDto dto) {
        log.info("post remove: {}", dto);
    }
}
~~~

- EntityListener 클래스에 적용한 이벤트는 다음 시점에 호출된다
    - @PostLoad : 해당 엔티티를 새로 불러오거나 refresh 한 이후
    - @PrePersist : 해당 엔티티를 저장하기 이전
    - @PostPersist: 해당 엔티티를 저장한 이후
    - @PreUpdate : 해당 엔티티를 업데이트 하기 이전
    - @PostUpdate : 해당 엔티티를 업데이트 한 이후
    - @PreRemove : 해당 엔티티를 삭제하기 이전
    - @PostRemove : 해당 엔티티를 삭제한 이후

### AuditingEntityListener

[Auditing](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#auditing){:target="_blank"} : 감시하다라는 뜻

엔티티에 변화가 언제 누구에 의해 발생한것을 기록하는 기능을합니다. <br/>
아쉽지만 이 기능은 스프링 부트가 자동 설정 해주지 않습니다. <br/>
그러므로 직접 설정을 해줘야 합니다.

Entity를 영속성 컨테스트에 저장하거나 조회를 수정한 후 update 하는 경우에 <br/> 
자동으로 auditor, time 을 맵핑하여 데이터 베이스에 넣도록 도와준다. <br/>
스프링 부트의 Run Application 위치에 `@EnableJpaAuditing` 을 적용하여 <br/> 
`JPA Auditing` 을 활성화 해야한다.

~~~
com.jjunpro.website.WebsiteApplication

@EnableJpaAuditing
@SpringBootApplication
public class WebsiteApplication {
    ...
}
~~~

### @CreatedDate, @LastModifiedDate

Entity 생성 & 수정시 특정 필드를 자동으로 DB에 맵핑해주는 어노테이션 <br/>
즉 `Auditing` 기능을 사용하려면, 위의 어노테이션을 사용해야 합니다. <br/>
`@EnableJpaAuditing` 이 필수적으로 `SpringApplication` 시작에 있어야 합니다.

CreatedDate 필드에는 수정날짜가 반영되지 않도록 `@Column(updatable = false)` 어노테이션을 선언해줍니다.

이제 제가 원하는 엔티티를 생성할 준비가 완료되었습니다! <br/>
다음에는 유저를 생성하는 방법 그리고 로그에 JPA DB 생성 정보 출력을 할 수 있도록 설정하는 방법을 <br/>
작성해보는 시간을 가져보도록 하겠습니다.

## 참고한 대단한 개발자의 블로그

[hydroniumion - [BE/1주차] JPA Annotation](https://velog.io/@hydroniumion/BE1%EC%A3%BC%EC%B0%A8-JPA-Annotation#entitylisteners){:target="_blank"} 

## 예제 Git 주소

혹시 제작중인 Spring Boot 프로젝트가 궁금하시면 아래 링크에서 확인하실 수 있습니다.

[https://github.com/alalstjr/jjunpro-blog-ex](https://github.com/alalstjr/jjunpro-blog-ex){:target="_blank"}