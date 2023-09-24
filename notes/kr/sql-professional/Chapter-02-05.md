---
layout: article
title: 2.5 본질식별자와 인조식별자
permalink: /notes/kr/sql-professional/chapter-02-05
key: notes
sidebar:
  nav: notes-kr
aside:
  toc: true
mermaid: true
---

# 1. 본질식별자 vs 인조식별자

최근 개발 트랜드를 보면 애플리케이션과 서비스를 빠른 속도로 제공하려는 모습을 보이고 있습니다. 빠른 속도에 맞추기 위해서 개발 편의성을 높이는 과정에서 문제가 발생하고는 합니다. 개발자는 개발 편의성을 위해서 인조식별자를 남발해서는 안되며, 본질식별자에 대한 고민이 필요합니다.

## 1.1 본질식별자와 인조식별자란?

- 본질식별자: 업무에 의해 만들어지는 가공되지 않은 식별자입니다.

- 인조식별자: 주식별자의 속성이 두 개 이상인 경우 속성들을 하나로 묶어서 사용하는 식별자입니다, *e.g., 복합식별자의 구성이 🤔복잡할 경우 생성*.

# 2. 인조식별자 사용 방식의 문제점
## 2.1 중복 데이터로 인한 품질 문제

다음과 같은 모델이 있을 때 로직 오류로 인해 중복 데이터가 발생한다면 해당 데이터의 저장을 막을 수 없습니다.

<img src="/notes/assets/sqlp-duplicate-data.png" width="300px;" alt="중복 데이터 발생 가능">

```sql
INSERT INTO ORDER_PRODUCT VALUES (DEFAULT, 110001, 310001, 'CPU', 'SEOUL');
INSERT INTO ORDER_PRODUCT VALUES (DEFAULT, 110001, 310001, 'CPU', 'SEOUL'); -- 중복데이능
INSERT INTO ORDER_PRODUCT VALUES (DEFAULT, 110001, 310001, 'CPU', 'BUSAN');
INSERT INTO ORDER_PRODUCT VALUES (DEFAULT, 110001, 310001, 'CPU', 'INCHEON');
```

## 2.2 불필요한 인덱스 생성

<img src="/notes/assets/sqlp-unnecessary-index.png" width="700px;" alt="불필요한 인덱스 발생">

- **ORDER_PRODUCT_ORIGIN**

    - **PK:** ORDER_NO, PRDT_NO

- **ORDER_PRODUCT_ARTIFACT**

    - **PK:** ORDER_DTL_NO

    - **IDX:** ORDER_NO, PRDT_NO (불필요)

<br>
<br>
<span style="color: grey; font-weight: 700;">References</span>   
SQL 전문가 가이드(2020 개정판)   
[https://dataonair.or.kr](https://dataonair.or.kr/)   
[https://www.wikipedia.org](https://www.wikipedia.org/)