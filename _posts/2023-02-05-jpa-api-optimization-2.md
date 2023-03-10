---
layout: post
comments: false
title: "[JPA] API 성능 최적화 하기 (2) - Fetch Join"
author: "Booki"
tags: "JPA"
---

API 성능 최적화 하기 1편에서 이어지는 내용입니다.

---

첫번째로 엔티티를 직접 노출하여 멤버의 팀명을 호출하는 API를 만들어보았다.

`MemberController.java`

```java
@RestController
@RequiredArgsConstructor
public class MemberController {
    private final MemberJpaRepository memberJpaRepository;

    @GetMapping("/api/v1/members")
    public List<Member> membersV1() {
        List<Member> all = memberJpaRepository.findAll();
        for (Member member : all) {
            member.getTeam().getName(); // Lazy 강제 초기화
        }
        return all;
    }
}
```

아무런 설정을 하지 않고 이대로 API를 호출해보면 다음과 같은 오류 메시지가 나올것이다.

```
com.fasterxml.jackson.databind.exc.InvalidDefinitionException: No serializer found for class org.hibernate.proxy.pojo.bytebuddy.ByteBuddyInterceptor and no properties discovered to create BeanSerializer (to avoid exception, disable SerializationFeature.FAIL_ON_EMPTY_BEANS) (through reference chain: java.util.ArrayList[0]->study.datajpa.entity.Member["team"]->study.datajpa.entity.Team$HibernateProxy$AecgE8ot["hibernateLazyInitializer"])
```

현재 엔티티의 설계는 기본적으로 지연 로딩으로 되어있다. 이는 실제 엔티티 대신에 프록시가 존재한다는 말이 된다.

기본 jackson 라이브러리는 기본적으로 이 프록시 객체를 json으로 어떻게 생성해야 하는지 모르기 때문에 `Hibernate5Module` 을 스프링 빈으로 등록하여 해결해야한다.

---

**Hibernate5Module 등록**

스프링 3.0 이상 기준

`build.gradle`

```groovy
dependencies {
	// Hibernate5Module
	implementation 'com.fasterxml.jackson.datatype:jackson-datatype-hibernate5-jakarta'
}
```

`<프로젝트명>Application.java`

```java
// 기본적으로 초기화 된 프록시 객체만 노출
@Bean
Hibernate5JakartaModule hibernate5Module() {
    return new Hibernate5JakartaModule();
}

// 강제 지연 로딩 설정을 하기 위해서는 이와 같이 추가해주면 된다.
@Bean
Hibernate5JakartaModule hibernate5JakartaModule() {
    Hibernate5JakartaModule hibernate5JakartaModule = new Hibernate5JakartaModule();
    hibernate5JakartaModule.configure(Hibernate5JakartaModule.Feature.FORCE_LAZY_LOADING, true);
    return hibernate5JakartaModule;
}
```

> `FORCE_LAZY_LOADING`을 키게 됐을 때 양방향 연관관계를 계속 로딩하게 되기 때문에 관계 한 곳에 `@JsonIgnore` 옵션을 주어야 한다. 그렇지 않으면 무한 루프가 걸리게 된다.
> 

> 간단한 프로젝트같은 경우에는 직접 엔티티를 노출하며 `Hibernate5Module`을 사용해도 무방하겠지만 대부분의 경우에는 `DTO`를 이용하여 값을 반환해주는 것이 좋은 방법이다.
> 

---

**엔티티를 DTO로 변환**

`MemberController.java`

```java
@GetMapping("api/v2/members")
public List<MemberDto> membersV2() {
    List<Member> members = memberJpaRepository.findAll();
    return members.stream()
            .map(MemberDto::new)
            .collect(Collectors.toList());
}

@Data
static class MemberDto {
    private Long memberId;
    private String username;
    private int age;
    private String teamName;

    public MemberDto(Member member) {
        memberId = member.getId();
        username = member.getUsername();
        age = member.getAge();
        teamName = member.getTeam().getName();
    }
}
```

> 참고로 현재 더미 데이터로 member1부터 member4 그리고 teamA, teamB를 가지고 있다.
> 

> member1과 member2는 teamA에 속하고 member3과 member4는 teamB에 속한 상태이다.
> 

다음은 엔티티를 DTO로만 변환해 호출했을 때의 쿼리 결과이다.

```
Hibernate: 
    select
        m1_0.member_id,
        m1_0.age,
        m1_0.team_id,
        m1_0.username 
    from
        member m1_0
2023-03-10T20:31:06.936+09:00  INFO 61090 --- [nio-8080-exec-3] p6spy                                    : #1678447866936 | took 0ms | statement | connection 3| url jdbc:mysql://localhost:3306/datajpa?useSSL=false&useUnicode=true&serverTimezone=Asia/Seoul
select m1_0.member_id,m1_0.age,m1_0.team_id,m1_0.username from member m1_0
select m1_0.member_id,m1_0.age,m1_0.team_id,m1_0.username from member m1_0;
Hibernate: 
    select
        t1_0.team_id,
        t1_0.name 
    from
        team t1_0 
    where
        t1_0.team_id=?
2023-03-10T20:31:06.959+09:00  INFO 61090 --- [nio-8080-exec-3] p6spy                                    : #1678447866959 | took 19ms | statement | connection 3| url jdbc:mysql://localhost:3306/datajpa?useSSL=false&useUnicode=true&serverTimezone=Asia/Seoul
select t1_0.team_id,t1_0.name from team t1_0 where t1_0.team_id=?
select t1_0.team_id,t1_0.name from team t1_0 where t1_0.team_id=1;
Hibernate: 
    select
        t1_0.team_id,
        t1_0.name 
    from
        team t1_0 
    where
        t1_0.team_id=?
2023-03-10T20:31:06.963+09:00  INFO 61090 --- [nio-8080-exec-3] p6spy                                    : #1678447866963 | took 0ms | statement | connection 3| url jdbc:mysql://localhost:3306/datajpa?useSSL=false&useUnicode=true&serverTimezone=Asia/Seoul
select t1_0.team_id,t1_0.name from team t1_0 where t1_0.team_id=?
select t1_0.team_id,t1_0.name from team t1_0 where t1_0.team_id=2;
```

> N+1 문제를 일으키며 쿼리가 총 세번 실행된 모습을 볼수있다.
> 
- `member`조회 한 번
- `member` → `team` 지연 로딩 조회 N번
- teamA와 teamB가 있으므로 2번 조회되므로 1+2 총 세번 실행되었다.

이 문제를 해결하기 위해 이제 Fetch Join을 적용해 최적화 해보도록 하겠다.

---

**Fetch Join 최적화**

`MemberController.java`

```java
@GetMapping("/api/v3/members")
public List<MemberDto> membersV3() {
    List<Member> members = memberJpaRepository.findAllMemberWithTeam();
    return members.stream()
            .map(MemberDto::new)
            .collect(Collectors.toList());
}
```

`MemberJpaRepository.java`

```java
public List<Member> findAllMemberWithTeam() {
    return em.createQuery(
                    "select m from Member m" +
                    " join fetch m.team t", Member.class)
            .getResultList();
}
```

해당 API를 호출했을 때 조회되는 쿼리의 개수를 보자.

```
Hibernate: 
    select
        m1_0.member_id,
        m1_0.age,
        t1_0.team_id,
        t1_0.name,
        m1_0.username 
    from
        member m1_0 
    join
        team t1_0 
            on t1_0.team_id=m1_0.team_id
2023-03-10T20:49:54.983+09:00  INFO 61178 --- [nio-8080-exec-3] p6spy                                    : #1678448994983 | took 0ms | statement | connection 3| url jdbc:mysql://localhost:3306/datajpa?useSSL=false&useUnicode=true&serverTimezone=Asia/Seoul
select m1_0.member_id,m1_0.age,t1_0.team_id,t1_0.name,m1_0.username from member m1_0 join team t1_0 on t1_0.team_id=m1_0.team_id
select m1_0.member_id,m1_0.age,t1_0.team_id,t1_0.name,m1_0.username from member m1_0 join team t1_0 on t1_0.team_id=m1_0.team_id;
```

> Fetch Join으로 인해 `member` → `team` 은 이미 조회 된 상태이므로 지연로딩이 일어나지 않아 쿼리 한 번에 모두 조회한 모습을 볼 수 있다.
> 

이로서 X to One 연관 관계에서 API 개발할 시에 마주할 수 있는 문제와 최적화 방법을 알아보았다. 다음 글에서는 X to Many 연관 관계에서의 API 개발 및 최적화에 관해 알아보겠다.