---
layout: article
title: 2.1 정규화
permalink: /notes/kr/sql-professional/chapter-02-01
key: notes
sidebar:
  nav: notes-kr
aside:
  toc: true
mermaid: true
---

# 1. 정규화의 이해
## 1.1 정규화란?

> 관계형 데이터베이스의 설계에서 중복을 최소화하게 데이터를 구조화하는 프로세스를 **정규화(Normalization)**라고 한다. 데이터베이스 정규화의 목표는 이상이 있는 관계를 재구성하여 작고 잘 조직된 관계를 생성하는 것에 있다. 일반적으로 정규화란 크고, 제대로 조직되지 않은 테이블들과 관계들을 작고 잘 조직된 테이블과 관계들로 나누는 것을 포함한다. 정규화의 목적은 하나의 테이블에서의 데이터의 삽입, 삭제, 변경이 정의된 관계들로 인하여 데이터베이스의 나머지 부분들로 전파되게 하는 것이다. [위키백과]

데이터 모델링에서 정규화는 가장 기초적이지만 필수적인 작업입니다.

### 1.1.1 제1정규형

- 모든 속성은 반드시 하나의 값을 가집니다. 다중 값을 허용할 경우 원하는 속성 값을 추출하기 어렵습니다.

- 유사 속성은 제거되어야 합니다. 아래 그림에서 4개 이상의 스킬을 등록하고자 하면 문제가 됩니다. 그리고 스킬1, 스킬2, 스킬3 모두 빠르게 조회하기 위해서는 속성마다 인덱스를 추가해야 합니다.

<figure>
<img src="/notes/assets/sqlp-1nf-01.png" width="700px;" alt="1NF 다중값" />
<figcaption>다중 값</figcaption>
</figure>

<figure>
<img src="/notes/assets/sqlp-1nf-02.png" width="700px;" alt="1NF 유사 속성" />
<figcaption>유사 속성</figcaption>
</figure>

### 1.1.2 제2정규형

엔터티의 일반 속성은 주식별자 전체에 종속적이어야 합니다 (부분 함수종속성 제거). 함수종속성은 데이터들이 어떤 기준값에 종속되는 현상을 말합니다. 기준값을 결정자(Determinant)라 하고, 종속되는 값을 종속자(Dependent)라고 합니다. beverage_nm이 업무적으로 변경되고 반영이 필요할 때 중복된 beverage_nm을 모두 변경해야 하는 부담이 생기게 됩니다. 해당 테이블의 데이터를 변경했다고 해도 특점 시점 또는 특정 테이블에는 변경되지 않은 데이터가 존재할 수도 있습니다 (정합성에 문제). 이는 조회 일관성을 떨어뜨리게 됩니다.

<img src="/notes/assets/sqlp-2nf.png" width="700px;" alt="2NF" />

### 1.1.3 제3정규형

엔터티의 일반속성 간에는 서로 종속적이지 않습니다 (이행적 종속 제거). 만약 부서명(dept_nm)이 변경되었다면 아래 엔터티의 모든 부서명을 갱신해야 합니다. 데이터 중복으로 인한 문제는 성능 부하 및 정합성 오류로 제2정규형과 동일합니다.

<img src="/notes/assets/sqld-3nf.png" width="700px;" alt="3NF" />

# 2. 반정규화와 성능
## 2.1 반정규화란?

> **역정규화**(denormalization)는 이전에 [정규화](https://ko.m.wikipedia.org/wiki/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4_%EC%A0%95%EA%B7%9C%ED%99%94)된 데이터베이스에서 성능을 개선하기 위해 사용되는 전략이다. [컴퓨팅](https://ko.m.wikipedia.org/wiki/%EC%BB%B4%ED%93%A8%ED%8C%85)에서 역정규화는 일부 쓰기 성능의 손실을 감수하고 데이터를 묶거나 데이터의 복제 사본을 추가함으로써 [데이터베이스](https://ko.m.wikipedia.org/wiki/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4)의 읽기 성능을 개선하려고 시도하는 과정이다.[[1]](https://ko.m.wikipedia.org/wiki/%EC%97%AD%EC%A0%95%EA%B7%9C%ED%99%94#cite_note-1)[[2]](https://ko.m.wikipedia.org/wiki/%EC%97%AD%EC%A0%95%EA%B7%9C%ED%99%94#cite_note-2) 아주 많은 수의 읽기 작업을 처리할 필요가 있는 [관계형](https://ko.m.wikipedia.org/wiki/%EA%B4%80%EA%B3%84%ED%98%95_%EB%AA%A8%EB%8D%B8) [데이터베이스 소프트웨어](https://ko.m.wikipedia.org/wiki/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4)의 [성능](https://ko.m.wikipedia.org/wiki/%EC%BB%B4%ED%93%A8%ED%84%B0_%EC%84%B1%EB%8A%A5)이나 스케일링에서 고려된다. 역정규화는 [비정규화](https://ko.m.wikipedia.org/w/index.php?title=%EB%B9%84%EC%A0%95%EA%B7%9C%ED%99%94&action=edit&redlink=1)(unnormalized form)와는 구별한다. 데이터베이스/테이블은 이들을 효율적으로 역정규화하기 위해 우선 정규화되어야 한다. [위키백과]

즉 정규화를 통해 데이터 중복을 줄여나갔다면, 반정규화는 성능 향상 목적(조회 성능)으로 데이터 중복을 허용하는 것을 의미합니다.

### 2.1.1 반정규화를 적용할 수 있는 모델

주문 엔터티(# 주문번호, 고객번호)와 결제 엔터티(# 결제번호, 주문번호, 결제수단구분코드, 결제수단번호, 결제일시)가 비식별자 관계를 맺고 있다고 합시다. 고객의 편의를 위해 결제 단계에서 직전 결제 정보를 불러와 결제의 편의성을 제공하고 싶습니다. 결제 엔터티를 조인하여 검색할 경우 주문내역 증가에 따라 부하가 커지게 됩니다. 이때 결제 엔터티에 고객번호 속성을 추가하여 성능을 개선할 수 있습니다.

```sql
SELECT 결제수단번호
FROM (
    SELECT MAX(결제일시) AS 결제일시
          ,결제수단구분코드
          ,결제수단번호
    FROM 결제
    WHERE 고객번호 = 1000000801
      AND 결제수단구분코드 = 'CRD'
    ORDER BY 결제일시 DESC
) WHERE ROWNUM = 1;
```

### 2.1.2 반정규화를 적용할 경우 성능 저하가 우려되는 모델

주문 엔터티(# 주문번호, 고객번호, 주문상태코드)와 배송 엔터티(# 배송번호, 주문번호, 배송구분코드, 송장번호, 배송일시)가 있다고 할 때 다음과 같이 반정규화를 수행했습니다. 주문 엔터티(# 주문번호, …, 송장번호). 해당 케이스의 경우 주문 시점에는 송장번호에 NULL 값이 들어가며, 배송 준비가 되었을 때 송장번호를 갱신(UPDATE)할 수 있습니다. 반정규화 전에는 없었던 갱신 로직이 추가된 것입니다. 반정규화는 데이터 불일치로 인한 정합성 문제뿐 아니라, 불필요한 트랜잭션으로 인한 성능 문제(조회 성능에서 미미한 이점을 취하고 불필요한 갱신 발생)도 만들어 낼 수 있으므로 주의해야 합니다.

## 2.2 반정규화 절차

1. **반정규화 대상 조사**

    - 범위 처리 빈도수 조사

    - 대량 범위 처리 조사

    - 통계성 프로세스 조사

    - 테이블 조인 개수 조사

2. **반정규화를 제외한 방법 검토**

    - View, 클러스터링, 인덱스 조정, 응용 애플리케이션

3. **반정규화 적용**

    - 테이블, 속성, 관계

<br>
<br>
<span style="color: grey; font-weight: 700;">References</span>   
SQL 전문가 가이드(2020 개정판)   
[https://dataonair.or.kr](https://dataonair.or.kr/)   
[https://www.wikipedia.org](https://www.wikipedia.org/)