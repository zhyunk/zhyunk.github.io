---
layout: post
title: JPA - @Where 어노테이션 Deprecated
date: 2024-03-16 22:55:51 +0900
author: zhyun
categories:
  - Issue
  - Spring Boot
tags:
  - JPA
  - Hibernate
  - Deprecated
  - Where
  - SQLRestriction
  - Document
mermaid: false
pin: false
---

> 사용 환경   
> Spring Data JPA 3.2.3 (Hibernate Core 6.4.4)   
{: .prompt-info}  

<br>

`@Where` 어노테이션을 사용하려고 보니 hibernate 6.3부터 deprecate 되었다고 나타났다.  
![image](/assets/img/2024-03-16-Where-어노테이션-Deprecated/Pasted-image-20240316224843.png)

<br>

그래서 <https://stackoverflow.com/questions/77178963/is-there-a-replacement-for-the-in-6-3-deprecated-where-and-loader>{: target='\_blank'} 를 참고하여 `@Where` 어노테이션을 사용할 자리에 `@SQLRestriction` 어노테이션을 적용하였다.

(참고한 링크를 타고 [hibernate document](https://docs.jboss.org/hibernate/stable/orm/javadocs/org/hibernate/annotations/Where.html)에 가보면 `@SQLRestriction`을 사용하라고 안내 되어 있다.)

<br>

*Deprecated 전*
```java
@Entity
@Where(clause = "deleted = false")  
public class Article {
```

<br>

*Deprecated 후 @SQLRestriction 적용*
```java
@Entity
@SQLRestriction("deleted = false")  
public class Article {
```

<br>
---

<br>

## @SQLRestriction

<https://docs.jboss.org/hibernate/orm/6.3/javadocs/org/hibernate/annotations/SQLRestriction.html>{: target='\_blank'}  

특징
1. 직접 SQL 제약 조건을 정의하여 엔티티에 적용할 수 있도록 해주는 어노테이션이다.  
2. 데이터베이스에서 필터링을 수행한다.    

<br>

@Where과의 차이점이 있는데, 
@Where은 Hibernate가 엔티티를 로드한 후에 메모리에서 필터링을 수행하고 @SQLRestriction은 데이터베이스에서 필터링 수행 후에 엔티티를 로드한다는 점이다.

<br>

@Where처럼 엔티티에 제약 조건을 정의하면서 메모리에서 동작하는 다른 어노테이션으로 `@Filter`도 있다!  
\> [찾아본 @Filter에 대해 정리가 잘 되어있는 블로그](https://bestasyusuf8.medium.com/hibernates-filter-annotation-e4810a82f1aa){: target='\_blank'}  

<br>

데이터는 변함이 없는데 where 조건이 자주 변하게 되는 상황이 온다면 메모리에서 필터링을 수행하는 `@Filter`를 사용하는게 더 나을 것 같다는 생각을 했다.  

하지만 where 조건이 거의 변하지 않고 JPA를 사용하면서 해당 엔티티의 모든 조회되는 데이터에 적용해야 하는 조건이 있다면(예를 들면 게시판의 논리 삭제된 게시글 필터링) `@SQLRestriction`을 사용하는게 좋을 것 같다는 생각을 하게되었다.    

<br><br><br>


> ## Hibernate Document
> <https://docs.jboss.org/hibernate/stable/orm/javadocs/org/hibernate/annotations/package-summary.html>{: target='_blank'}    
{: .prompt-tip}  
JPA 사용 중 Deprecated 객체를 만나면 위 사이트에 가서 먼저 검색해보면 좋을 것 같다!  




