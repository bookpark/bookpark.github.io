---
layout: post
comments: false
title: "[JPA] API 성능 최적화 하기 (1) - X to One 연관 관계"
author: "Booki"
tags: "JPA"
---

이 글은 김영한 님의 실전! 스프링 부트와 JPA 활용2 - API 개발과 성능 최적화 편을 보고 남기는 정리글 입니다.

---

X to One 연관 관계의 성능 최적화를 하기위해 먼저 알아야 할 것은 즉시(Eager) 로딩과 지연(Lazy) 로딩의 차이이다.

`Member.java`

```java
@Entity
@Getter
@Setter
public class Member {
	@Id
	@GeneratedValue
	private Long id;

	private String name;
	
	@ManyToOne(fetch = FetchType.EAGER)
	private Team team;
}
```

`Team.java`

```java
@Entity
@Getter
@Setter
public class Team {
	@Id
	@GeneratedValue
	private Long id;

	private String name;

	@OneToMany(mappedBy = "team")
	private List<Member> members = new ArrayList<>();
}
```

간단하게 두 엔티티의 연관 관계를 만들어주고, `FetchType.EAGER`로 설정을 해두었다.

간단한 엔티티 테스트 코드를 통해 쿼리문을 확인해보도록 하자.

`MemberTest.java`

```java
@SpringBootTest
@Transactional
class MemberTest {
    @PersistenceContext
    EntityManager em;

    @Test
    public void testEntity() {
        Team teamA = new Team("teamA");
        em.persist(teamA);

        Member member1 = new Member("member1", 10, teamA);
        em.persist(member1);

        // 초기화
        em.flush();
        em.clear();

        // 확인
        em.find(Member.class, member1.getId());
    }
}
```

`결과`

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
    left join
        team t1_0 
            on t1_0.team_id=m1_0.team_id 
    where
        m1_0.member_id=?
```

> `FetchType.EAGER` 로 설정되어 있을 때는 이와 같이 member만 조회했음에도 team까지 함께 join되어 결과 값을 가져온다.
> 

---

다음은 `FetchType.LAZY` 로 설정했을 때의 결과 값이다.

`결과`

```
Hibernate: 
    select
        m1_0.member_id,
        m1_0.age,
        m1_0.team_id,
        m1_0.username 
    from
        member m1_0 
    where
        m1_0.member_id=?
```

> `FetchType.LAZY` 로 설정되어 있을 때는 team까지는 가져오지 않고 조회한 member만 가져오게 된다.
> 

---

JPA에서 X to One 연관 관계의 기본 값은 `FetchType.EAGER` 로 설정되어 있다. 이로 인해 필요치 않은 데이터가 같이 조회되며 N+1 문제를 일으킬 수 있다.

지연 로딩을 피하기 위해 `FetchType.EAGER` 로 설정하는 것은 성능 튜닝을 어렵게 만드는 지름길이다.

성능 최적화가 필요한 경우에는 `Fetch Join` 을 사용하면 된다. 다음 편에서는 이 fetch join을 사용하여 성능 최적화하는 방법을 알아보도록 하자.