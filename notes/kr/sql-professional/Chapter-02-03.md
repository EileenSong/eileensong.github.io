---
layout: article
title: 2.3 모델이 표현하는 트랜잭션
permalink: /notes/kr/sql-professional/chapter-02-03
key: notes
sidebar:
  nav: notes-kr
aside:
  toc: true
mermaid: true
---

## 1. 트랜잭션이란?

> **데이터베이스 트랜잭션**(Database Transaction)은 데이터베이스 관리 시스템 또는 유사한 시스템에서 상호작용의 단위이다. 여기서 유사한 시스템이란 트랜잭션이 성공과 실패가 분명하고 상호 독립적이며, 일관되고 믿을 수 있는 시스템을 의미한다. [위키백과]


## 2. 데이터 모델의 트랜잭션

주문 엔터티(# 주문번호, 고객번호)와 주문상세 엔터티(# 주문번호, # 주문순번, 상품번호)가 필수적인 (Mandatory) 관계로 데이터는 동시에 발생합니다. 개발자는 원자성을 보장할 수 있도록 커밋의 단위를 하나로 묶어 처리해야 합니다.

일반적으로 모델링 툴에서 선택적 관계가 default입니다. 이로 인해 모델링 단계에서 필수/선택 관계 설정에 실수하는 모델러가 많습니다. 이를 꼭 인지하고 주문 엔터티와 주문상세 엔터티가 선택적 관계를 맺는 일이 없도록 주의해야 합니다.

업무적 트랜잭션보다 데이터 모델의 트랜잭션은 태생 자체가 함께 발생하는 데이터입니다. 재사용성의 이점도 없으므로 반드시 하나의 트랜잭션으로 처리해야 합니다, *i.e., API 개발 패턴으로 모든 트랜잭션의 SQL을 낱개로 뜯어내고 있지 않은지 체크해야 합니다*.

<br>
<br>
<span style="color: grey; font-weight: 700;">References</span>   
SQL 전문가 가이드(2020 개정판)   
[https://dataonair.or.kr](https://dataonair.or.kr/)   
[https://www.wikipedia.org](https://www.wikipedia.org/)