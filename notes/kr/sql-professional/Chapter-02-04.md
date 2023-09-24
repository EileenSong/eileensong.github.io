---
layout: article
title: 2.4 NULL 속성
permalink: /notes/kr/sql-professional/chapter-02-04
key: notes
sidebar:
  nav: notes-kr
aside:
  toc: true
mermaid: true
---

# 1. NULL 값의 연산
## 1.1 NULL 값이란?

> **Null** 또는 **NULL**은 [구조적 질의 언어](https://ko.m.wikipedia.org/wiki/%EA%B5%AC%EC%A1%B0%EC%A0%81_%EC%A7%88%EC%9D%98_%EC%96%B8%EC%96%B4) (SQL)에서 [데이터베이스](https://ko.m.wikipedia.org/wiki/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4) 내의 데이터 값이 존재하지 않는다는 것을 지시하는데 사용되는 특별한 표시어(special marker)이다. 데이터베이스 [관계 모델](https://ko.m.wikipedia.org/wiki/%EA%B4%80%EA%B3%84_%EB%AA%A8%EB%8D%B8)의 창시자인 E. F. 콧(E. F. Codd)에 만들어진 개념으로, SQL NULL은 모든 true RDBMS가 빠진 정보와 적용할 수 없는 정보를 지원해야 한다는 요구사항을 충족시키는 역할을 한다. 콧은 또한 데이터베이스 이론에서 Null을 표현하기 위해 [그리스어](https://ko.m.wikipedia.org/wiki/%EA%B7%B8%EB%A6%AC%EC%8A%A4%EC%96%B4)의 [오메가](https://ko.m.wikipedia.org/wiki/%EC%98%A4%EB%A9%94%EA%B0%80)의 소문자(ω)를 이용할 것을 도입했다. NULL은 또한 SQL에서 Null 특수 표시를 구분하기 위해 사용되는 유보된 키워드이다. [위키백과]

IE 표기법에서는 Null 허용여부를 알 수 없지만, 바커 표기법에서는 속성 앞에 동그라미 기호를 통해서 Null 허용 속성임을 표기할 수 있습니다.

<figure>
<img src="/notes/assets/sqlp-attribute-notation.png" width="700px;" alt="">
<figcaption>SQL 전문가 가이드</figcaption>
</figure>

## 1.2 NULL 값은 연산은 NULL

```sql
SELECT 1000 - NULL FROM DUAL;
-- ret: NULL
SELECT NVL(1000 - NULL, 0) FROM DUAL;
-- ret: 0
```

NULL 값으로 가능한 연산은 `IS NULL`, `IS NOT NULL` 밖에 없습니다.

## 1.3 집계함수와 NULL

- 집계함수는 Null 값을 제외하고 연산합니다.

```sql
SELECT SUM(A)
FROM (
    SELECT 1000 AS A FROM DUAL
    UNION ALL
    SELECT NULL AS A FROM DUAL
);
-- ret: 1,000
```

<br>
<br>
<span style="color: grey; font-weight: 700;">References</span>   
SQL 전문가 가이드(2020 개정판)   
[https://dataonair.or.kr](https://dataonair.or.kr/)   
[https://www.wikipedia.org](https://www.wikipedia.org/)