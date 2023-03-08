---
layout: post
comments: false
title: "[JPA] 쿼리 로그 설정"
author: "Booki"
tags: "JPA"
---

JPA를 사용하며 쿼리 로그를 출력해야 할 때의 설정은 다음과 같다.

`application.yml`

```yaml
spring:
  jpa:
    properties:
      hibernate:
        show_sql: true
        format_sql: true
logging:
  level:
    org.hibernate.sql: debug
    org.hibernate.type: trace # parameter 값 보기
```

> `show_sql` 옵션은 `System.out` 에 Hibernate 실행 SQL을 남기고,
> 

> `org.hibernate.sql` 옵션은 logger를 통해 Hibernate 실행 SQL을 남긴다.
> 

*가급적 쿼리 출력은 로거를 통해 남기자!*

위 설정으로 parameter 값을 볼 수 있지만 쿼리문 안에 parameter 값이 적용되어있지 않아 보기 불편할 수 있다.

---

이 때 p6spy를 적용해 쿼리 로그를 확인해보자.

`build.gradle`

```groovy
dependencies {
	// p6spy
	implementation 'com.github.gavlyukovskiy:p6spy-spring-boot-starter:1.9.0'
}
```

`application.yml`

```yaml
decorator:
  datasource:
    # p6spy 설정 적용
    p6spy:
      enable-logging: true
      multiline: true
      logging: slf4j
```

p6spy 설정을 통해 log 파일을 남기거나 log 포맷을 바꾸는 등 여러 설정을 할 수 있다.

*2023년 3월 8일 기준 SpringBoot 3.0 이상에서도 잘 작동한다.*

> 참고: <https://github.com/gavlyukovskiy/spring-boot-data-source-decorator>

---

p6spy가 적용 된 로그를 확인해보자.

```
2023-03-08T23:59:58.355+09:00  INFO 51289 --- [           main] p6spy
insert into member (username, id) values (?, ?)
insert into member (username, id) values ('member', 1);
2023-03-08T23:59:58.356+09:00  INFO 51289 --- [           main] p6spy
```

*바인딩 된 parameter 값이 쿼리 문 안에 잘 적용된 모습이다.*