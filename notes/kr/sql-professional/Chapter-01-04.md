---
layout: article
title: 1.4 관계
permalink: /notes/kr/sql-professional/chapter-01-04
key: notes
sidebar:
  nav: notes-kr
aside:
  toc: true
mermaid: true
---

# 1. 관계의 이해
## 1.1 관계란?

> [관계 모델](https://ko.m.wikipedia.org/wiki/%EA%B4%80%EA%B3%84_%EB%AA%A8%EB%8D%B8)에서 **관계**(relation)는 동일한 구조로 이루어진 [튜플](https://ko.m.wikipedia.org/wiki/%ED%8A%9C%ED%94%8C)의 집합을 말한다. 값으로서의 관계를 ‘관계값’(relation value)라고 하며, 관계값을 값으로 가지는 변수를 ‘[관계변수](https://ko.m.wikipedia.org/w/index.php?title=%EA%B4%80%EA%B3%84%EB%B3%80%EC%88%98&action=edit&redlink=1)’(relvar, relation variable)라고 한다. [위키백과]
> 

<figure>
<img src="/notes/assets/sqlp-terminologies-of-table.png" width="500px;" alt="테이블 명칭">
<figcaption>
  <a href="https://ko.m.wikipedia.org/wiki/관계_(데이터베이스))">wikepedia.com</a>
</figcaption>
</figure>

즉 인스턴스 사이의 논리적 연관성으로 존재적 연관성 또는 행위적 연관성이 부여된 상태라고 할 수 있습니다. 관계는 엔터티 간 연관성을 표현하므로 엔터티 정의, 속성 정의, 관계 정의에 따라서 다양하게 변할 수 있습니다.

## 1.2 관계와 패어링

엔터티 내에 인스턴스와 인스턴스 사이에 관계가 설정된 어커런스를 관계 패어링(Relationship Paring)이라고 합니다. 관계 패어링의 논리적 집합의 표현이 관계(Relation)입니다.
관계의 표현에는 이항 관계(Binary Relationship), 삼항 관계(Ternary Relationship), n항 관계가 존재할 수 있는데 삼항 관계 이상은 잘 나타내지 않습니다.

# 2. 관계의 분류
## 2.1 존재적, 행위적 관계

- **존재적 관계:** 특정 이벤트나 액션, 행위에 의한 것이 아닌 단순 소속에 의한 관계를 말합니다, <sub><i>e.g., 학부/학생</i></sub>.

- **행위적 관계:** 인스턴스의 특정 행위에서 발생하는 관계를 말합니다, <sub><i>e.g., 고객/주문</i></sub>.

## 2.2 연관, 의존 관계

UML(Unified Modeling Language) 클래스다이어그램의 관계 중에는 **연관 관계(Association)**와 **의존 관계(Dependency)**가 있습니다. 연관 관계는 항상 이용하는 관계로 존재적 관계에 해당하고, 의존 관계는 행위에 의해 관계가 형성됨을 의미합니다.

- **연관 관계:** 두 클래스 간 래퍼런스에 기반한 관계입니다. 소스코드에서 멤버변수로 선언되어 관계가 발생합니다. 실선 화살표(→)로 표현합니다.

- **의존 관계:** 주로 특정 오퍼레이션(메소드)의 일부로 클래스 래퍼런스를 받았을 때 발생하는 관계입니다. 소스코드에서 메소드의 파라미터 등으로 클래스 래퍼런스를 전달받게 됩니다. 점선 화살표로 표현합니다.

# 3. 관계 표기법

- **관계명(Membership):** 관계의 이름

- **관계차수(Cardinality):** 1:1, 1:M, M:N

- **관계선택사양(Optionality):** 필수, 선택

## 3.1 관계명

엔터티가 관계에 참여하는 형태를 현재형 동사로 작성합니다.  관계가 시작되는 편을 관계시작점, 관계를 받는 편을 관계끝점이라 합니다. 그리고 참여 엔터티의 관점에 따라  능동적이거나 수동적인 관계명을 사용합니다.

## 3.2 관계차수(Degree/Cardinality)

두 엔터티 간의 관계에서 참여자의 수를 표현합니다.

### 3.2.1 관계 표시 방법(1:1, 1:M, M:N)

<img src="/notes/assets/sqlp-notation-of-relationship.jpeg" width="500px;" alt="관계 표기법">

## 3.3 관계선택사항

필수참여관계는 모든 참여자가 반드시 관계를 가져야 합니다. 주문서는 반드시 주문목록을 가져야 하는 것과 같은 관계입니다. 반대로 목록은 주문되지 않은 목록이 있을 수 있으므로 선택참여관계가 됩니다. 즉 하나의 주문목록에는 한 개의 목록을 항상 포함하고, 한 개의 목록은 여러 개의 주문목록에 포함될 수 있습니다.

<figure>
<img src="/notes/assets/sqlp-optionality-of-relationship.jpeg" width="500px;" alt="관계선택사항">
<figcaption>
  <a href="https://dataonair.or.kr/db-tech-reference/d-guide/sql/?mod=document&uid=328">dataonair.or.kr</a>
</figcaption>
</figure>

선택참여관계는 물리속성에서 FK로 연결될 경우 Null 값을 허용할 수 있습니다. 만약 선택참여관계로 지정해야 할 관계를 필수참여관계로 지정하게 된다면 문제가 발생할 수 있습니다. 관계선택사항의 경우 개발 시점의 업무 로직과 직접 연관되므로 신중하게 설정하는 것이 좋습니다.

# 4. 관계 정의 확인사항

- 두 개의 엔터티 사이에 관심 있는 연관규칙이 존재하는가?

- 두 개의 엔터티 사이에 정보의 조합이 발생하는가?

- 업무기술서, 장표에 관계연결에 대한 규칙이 존재하는가?

- 업무기술서, 장표에 관계연결을 가능하게 하는 동사가 있는가?

<br>
<br>
<span style="color: grey; font-weight: 700;">References</span>   
SQL 전문가 가이드(2020 개정판)   
[https://dataonair.or.kr](https://dataonair.or.kr/)   
[https://www.wikipedia.org](https://www.wikipedia.org/)