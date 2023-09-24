---
layout: article
title: 2.2 관계와 조인
permalink: /notes/kr/sql-professional/chapter-02-02
key: notes
sidebar:
  nav: notes-kr
aside:
  toc: true
mermaid: true
---

# 1. 조인의 이해
## 1.1 조인이란?

> **join(조인)** 또는 **결합 구문**은 한 [데이터베이스](https://ko.m.wikipedia.org/wiki/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4) 내의 여러 [테이블](https://ko.m.wikipedia.org/wiki/%ED%85%8C%EC%9D%B4%EB%B8%94_(%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4))의 레코드를 조합하여 하나의 열로 표현한 것이다. 따라서 조인은 테이블로서 저장되거나, 그 자체로 이용할 수 있는 결과 셋을 만들어 낸다. JOIN은 2개의 테이블에서 각각의 공통값을 이용함으로써 필드를 조합하는 수단이 된다. ANSI 표준 SQL은 네 가지 유형의 JOIN을 규정한다. [위키백과]
> 
> - INNER JOIN
> - OUTER JOIN
> - LEFT JOIN
> - RIGHT JOIN

1과목에서 배운 관계를 다시 한 번 떠올려 봅시다. 관계를 맺는다는 것은 부모의 식별자를 자식이 상속하고, 상속된 속성을 매핑키로 활용하여 데이터를 결합할 수 있다는 의미입니다. SQL에서는 이런 형태의 결합을 조인(Join)이라고 합니다. 신입 개발자 면접 시 조인과 관련한 질문은 단골 소재입니다.

## 1.2 JOIN SQL

다음은 데이터 표준화 과정의 하나인 표준 도메인 사전을 정의할 때 꼭 필요한 정보인 도메인 그룹과 인포타입의 샘플 데이터입니다. 표준 사전을 만들 때는  표준 단어 → 표준 도메인 → 표준 용어 순으로 정의하게 되며 도메인은 속성이 가질 수 있는 값의 범위입니다. 도메인은 하나의 인포타입과 매핑됩니다. 앞으로 표준화 관련 예제를 사용하여 표준화에 대한 내용도 함께 학습하려고 합니다. 아래 링크를 클릭하시면 더 많은 정보를 확인할 수 있습니다.

[dataonair.or.kr](https://dataonair.or.kr/) 👈 클릭

- 도메인그룹

| 도메인그룹ID | 그룹명 | 코드도메인여부 |
| ------------ | ------ | -------------- |
| 1100001      | 수량   | N              |
| 1100002      | 코드   | N              |

- 인포타입

| 인포타입ID | 타입명     | 논리데이터타입 | 데이터길이 | 소수점자릿수 | 도메인그룹ID |
| ---------- | ---------- | -------------- | ---------- | ------------ | ------------ |
| 1100003    | CD_VAR0001 | VARCHAR        | 1          | NULL         | 1100002      |
| 1100004    | CD_VAR0002 | VARCHAR        | 2          | NULL         | 1100002      |
| 1100005    | CD_VAR0003 | VARCHAR        | 3          | NULL         | 1100002      |
| 1100006    | QN_NUM0005 | NUMBER         | 5          | NULL         | 1100001      |

```sql
SELECT A.그룹명
      ,B.타입명
      ,B.논리데이터타입
FROM 도메인그룹 A, 인포타입 B
WHERE A.도메인그룹ID = B.도메인그룹ID;
```

# 2. 계층형 데이터 모델

자기 자신과 관계가 발생하는 경우도 있습니다. 계층형 데이터 모델이란 계층 구조를 가진 모델을 의미합니다. 아래의 테이블과 쿼리를 보면 데이터 간에 계층 구조가 있음을 알 수 있습니다. 이렇게 데이터 간에 계층 구조가 존재할 때 계층형 데이터 모델이 발생합니다.

- 도메인

| 도메인ID | 도메인명         | 도메인그룹ID | 인포타입ID | 부모도메인ID |
| -------- | ---------------- | ------------ | ---------- | ------------ |
| 1100007  | 엑터상세유형코드 | 1100002      | 1100005    | NULL         |
| 1100008  | 개인상세유형코드 | 1100002      | 1100005    | 1100007      |
| 1100009  | 법인상세유형코드 | 1100002      | 1100005    | 1100007      |

```sql
-- Self-Join
SELECT B.도메인명 AS 부모도메인명
      ,A.도메인명
FROM 도메인 A, 도메인 B
WHERE A.도메인ID = '1100008'
  AND A.부모도메인ID = B.도메인Id;
```

# 3. 상호배타적 관계

상호배타적 관계는 개념만 숙지하고 가겠습니다. 아래 모델을 보면 괄호가 있습니다. 이는 상호배타적 관계를 의미합니다. 주문 엔터티의 개인/법인번호 속성은 개인번호 또는 법인번호 하나만 상속할 수 있습니다.

<figure>
<img src="/notes/assets/sqlp-mutually-exlusive-relationship.png" width="700px;" alt="상호배타적 관계">
<figcaption>SQL 전문가 가이드</figcaption>
</figure>

상품구매내역 엔터티의 특정 데이터를 고객명과 함께 보여주고 싶다면 아래와 같이 작성해 볼 수 있습니다.

```sql
-- JOIN 결과가 없다면 공집합(NO ROWS)
SELECT B.개인고객명 AS 고객명
      ,A.주문번호
FROM 주문 A, 개인고객 B
WHERE A.주문번호     = '1100012'
  AND A.고객구분코드  = '01' -- 개인: 01
  AND A.개인/법인번호 = B.개인번호
UNION ALL
SELECT B.법인명 AS 고객명
      ,A.주문번호
FROM 주문 A, 법인고객 B
WHERE A.주문번호     = '1100012'
  AND A.고객구분코드  = '02' -- 법인: 02
  AND A.개인/법인번호 = B.법인번호;

-- JOIN 결과가 없다면 NULL값을 가진 한 건의 ROW
SELECT COALESCE(B.개인고객명, C.법인명) AS 고객명
      ,A.주문번호
FROM 주문 A
LEFT OUTER JOIN 개인고객 B
     ON A.개인/법인번호 = B.개인번호
LEFT OUTER JOIN 법인고객 C
     ON A.개인/법인번호 = C.법인번호
WHERE A.주문번호 = '1100012';
```

<br>
<br>
<span style="color: grey; font-weight: 700;">References</span>   
SQL 전문가 가이드(2020 개정판)   
[https://dataonair.or.kr](https://dataonair.or.kr/)   
[https://www.wikipedia.org](https://www.wikipedia.org/)