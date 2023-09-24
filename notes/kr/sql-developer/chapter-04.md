---
layout: article
title: 4. SQL 활용
permalink: /notes/kr/sql-developer/chapter-04
key: notes
sidebar:
  nav: notes-kr
aside:
  toc: true
---

<span style="color: grey;">참고 도서: 2022 유선배 SQL개발자 과외노트</span><br>
<span style="color: grey;">참고 자료: Wikipedia, oracle.com, oracletutorial.com</span><br>
<span style="color: blue;">** SQLD 자격 시험을 준비하는 분들께 도움이 되고자 정리한 내용을 올리고 있습니다.</span><br>
<span style="color: blue;">** 암기가 필요한 부분은 계속 보면서 외워주세요.</span><br>
<span style="color: blue;">** 실습 환경 구성이 어려운 분들은 아래 링크를 이용하세요.</span><br>
[Oracle Live SQL 👈 클릭](https://livesql.oracle.com/apex/f?p=590:1000)<br>
<span style="color: blue;">** 오류가 있다면 댓글 주세요!</span><br>

## 4.1 서브쿼리(Subquery)

- 하나의 쿼리 안에 존재하는 또 다른 쿼리이다.

    | SELECT 절 | 스칼라 서브쿼리(Scalar Subquery) |
    | --- | --- |
    | FROM 절 | 인라인 뷰(Inline View) |
    | WHERE, HAVING 절 | 중첩 서브쿼리(Nested Subquery) |

### 4.1.1 스칼라 서브쿼리(Scalar Subquery)

- 컬럼이 올 수 있는 대부분 위치에 사용이 가능하다. 컬럼 대신 사용되므로 반드시 하나의 값을 출력해야 한다.

    ``` sql
    -- scott schema
    SELECT ENAME
          ,(SELECT DNAME FROM DEPT B WHERE A.DEPTNO = B.DEPTNO)
    FROM EMP;
    ```

### 4.1.2 인라인 뷰(Inline View)

- FROM 절 등 테이블명이 올 수 있는 위치에 사용한다.

    ``` sql
    -- scott schema
    SELECT *
    FROM (
        SELECT EMPNO
              ,ENAME
        FROM EMP
    );
    ```

### 4.1.3 중첩 서브쿼리(Nested Subquery)

- WHERE 절과 HAVING 절에 사용한다.
    - 비연관 서브쿼리: 메인 쿼리와 관계를 맺고 있지 않음

    ``` sql
    -- scott schema
    -- 비연관 서브쿼리
    SELECT EMPNO
          ,ENAME
    FROM EMP
    WHERE DEPTNO = (
        SELECT DEPTNO
        FROM DEPT
        WHERE DNAME = 'SALES'
    )
    ```

    - 연관 서브쿼리: 메인 쿼리와 관계를 맺고 있음

    ``` sql
    -- scott schema
    -- 연관 서브쿼리
    SELECT EMPNO
          ,ENAME
    FROM EMP A
    WHERE ENAME = (
        SELECT ENAME
        FROM BONUS B
        WHERE A.ENAME = B.ENAME
    );
    ```

- 출력 값에 따라 단일 행, 다중 행, 다중 컬럼 서브쿼리로 나눌 수 있다.

    | 단일 행 서브쿼리 | 서브쿼리가 1건 이하의 데이터를 출력한다. 단일 행 비교 연산자와 함께 사용할 수 있다. e.g. =, <, > , … |
    | --- | --- |
    | 다중 행 서브쿼리 | 서브쿼리가 여러 건의 데이터를 출력한다. 다중 행 비교 연산자와 함께 사용할 수 있다. e.g. IN, ALL, ANY, … |
    | 다중 컬럼 서브쿼리 | 서브쿼리가 여러 컬럼의 데이터를 출력한다. |

## 4.2 뷰(VIEW)
### 4.2.1 뷰(VIEW)란?

- 특정 SELECT 문 결과에 이름을 붙여 재사용이 가능한 오브젝트이다.

- 뷰는 가상 테이블로 조회만 가능하다.

### 4.2.2 뷰 생성 및 삭제

``` sql
-- scott schema
-- CREATE VIEW
CREATE OR REPLACE VIEW V_DEPT AS
SELECT A.DEPTNO
      ,DNAME
      ,COUNT(A.DEPTNO) AS COUNT
FROM EMP A
INNER JOIN DEPT B
    ON A.DEPTNO = B.DEPTNO
GROUP BY DEPTNO, DNAME;

-- USING VIEW
SELECT * FROM V_DEPT;
-- Output
-- 10|ACCOUNTING|3
-- 20|RESEARCH    |5
-- 30|SALES     |6

DROP VIEW V_DEPT;
```

## 4.3 집합 연산자
### 4.3.1 집합 연산자란?

- 집합 연산자는 두 개 이상 쿼리의 결과에 대한 집합을 가지고 연산하는 명령어이다.

### 4.3.2 UNION / UNION ALL

- UNION, UNION ALL은 합집합 개념이다. 둘은 매우 유사하나 차이점은, UNION ALL은 중복된 항목도 모두 조회한다는 점이다.

| ID (A) | NAME (A) || ID (B) | NAME (B) |
| --- | --- |-| --- | --- |
| 1 | A || 1 | B |
| 2 | C || 2 | C |
| 3 | A || 3 | C |
| 4 | B || 4 | B |
| 5 | F ||||

``` sql
-- UNION ALL
SELECT * FROM TABLE_A
UNION ALL
SELECT * FROM TABLE_B;
-- Output
-- 1|A
-- 2|C
-- 3|A
-- 4|B
-- 5|F
-- 1|B
-- 2|C
-- 3|C
-- 4|B

-- UNION
SELECT * FROM TABLE_A
UNION
SELECT * FROM TABLE_B;
-- Output
-- 1|A
-- 2|C
-- 3|A
-- 4|B
-- 5|F
-- 1|B
-- 3|C
```

### 4.3.3 INTERSECT

- INTERSECT는 교집합 개념이다. 공통된 부분만 중복을 제거하여 조회한다.

``` sql
SELECT * FROM TABLE_A
INTERSECT
SELECT * FROM TALBE_B
-- Output
-- 2|C
-- 4|B
```

### 4.3.4 MINUS / EXCEPT

- MINUS / EXCEPT는 차집합 개념이다. 먼저 위치한 SELECT문을 기준으로 다른 SELECT 문과 공통된 레코드를 제외한 항목만 조회한다.

``` sql
SELECT * FROM TABLE_A
EXCEPT
SELECT * FROM TALBE_B
-- Output
-- 1|A
-- 3|A
-- 5|F
```

## 4.4 그룹 함수
### 4.4.1 그룹 함수란?

- 하나의 행을 그룹으로 묶어 연산하여 하나의 결과로 리턴한다.

    | 집계 함수 | COUNT, SUM, AVG, MAX, MIN 등 |
    | --- | --- |
    | 소계 함수 | ROLLUP, CUBE, GROUPING SETS 등 |

### 4.4.2 ROLLUP

- 소그룹 간의 소계 및 총계를 계산하는 함수이다.

``` sql
-- scott schema
SELECT JOB
      ,DEPTNO
      ,COUNT(*) AS CNT
FROM EMP
GROUP BY ROLLUP(JOB, DEPTNO);
-- Output
-- CLERK    |10  |1
-- CLERK    |20  |2
-- CLERK    |30  |1
-- CLERK    |NULL|4
-- ANALYST  |20  |2
-- ANALYST  |NULL|2
-- MANAGER  |10  |1
-- MANAGER  |20  |1
-- MANAGER  |30  |1
-- MANAGER  |NULL|3
-- SALESMAN |30  |4
-- SALESMAN |NULL|4
-- PRESIDENT|10     |1
-- PRESIDENT|NULL|1
-- NULL     |NULL|14
```

### 4.4.3 CUBE

- 소그룹 간의 소계 및 총계를 다차원적으로 계산하는 함수이다.

``` sql
-- scott schema
SELECT JOB
      ,DEPTNO
      ,COUNT(*) AS CNT
FROM EMP
GROUP BY CUBE(JOB, DEPTNO);
-- Output
-- NULL       |NULL|14
-- NULL       |10     |3
-- NULL       |20     |5
-- NULL       |30     |6
-- CLERK      |NULL|4
-- CLERK      |10     |1
-- CLERK      |20     |2
-- CLERK      |30     |1
-- ANALYST    |NULL|2
-- ANALYST    |20     |2
-- MANAGER    |NULL|3
-- MANAGER    |10     |1
-- MANAGER    |20     |1
-- MANAGER    |30     |1
-- SALESMAN |NULL|4
-- SALESMAN |30  |4
-- PRESIDENT|NULL|1
-- PRESIDENT|10     |1
```

### 4.4.4 GROUPING SETS

- 특정 항목에 대한 소계를 구하는 함수이다. 인자값으로 ROLLUP이나 CUBE를 사용할 수 있다.

``` sql
-- scott schema
SELECT JOB
      ,DEPTNO
      ,COUNT(*) AS CNT
FROM EMP
GROUP BY GROUPING SETS(JOB, DEPTNO, ());
-- Output
-- NULL     |10  |3
-- NULL     |20  |5
-- NULL     |30  |6
-- NULL     |NULL|14
-- PRESIDENT|NULL|1
-- MANAGER  |NULL|3
-- ANALYST  |NULL|2
-- SALESMAN |NULL|4
-- CLERK    |NULL|4
```

### 4.4.5 GROUPING

- ROLLUP, CUBE, GROUPING SETS 등과 함께 쓰인다. 소계를 나타내는 행의 NULL 값을 처리할 수 있다.

``` sql
-- scott schema
SELECT CASE GROUPING(JOB) WHEN 1 THEN 'TOTAL' ELSE JOB END
      ,DEPTNO
      ,COUNT(*) AS CNT
FROM SCOTT.EMP
GROUP BY ROLLUP(JOB, DEPTNO);
-- Output
-- CLERK    |10  |1
-- ...
-- SALESMAN |NULL|4
-- PRESIDENT|10  |1
-- PRESIDENT|NULL|1
-- TOTAL    |NULL|14
```

## 4.5 윈도우 함수
### 4.5.1 윈도우 함수란?

Row 간의 관계를 쉽게 정의하기 위한 함수이다. OVER 키워드와 함께 사용한다.

| 순위 함수 | RANK, DENSE_RANK, ROW_NUMBER |
| --- | --- |
| 집계 함수 | SUM, MAX, MIN, AVG, COUNT |
| 행 순서 함수 | FRST_VALUE, LAST_VALUE, LAG, LEAD |
| 비율 함수 | CUME_DIST, PERCENT_RANK, NTILE, RATIO_TO_REPORT |

### 4.5.2 순위 함수

- RANK : 순위를 매긴다. 같은 순위가 존재할 경우 존재하는 수만큼 다음 순위를 건너뛴다.

``` sql
-- scott schema
SELECT JOB
      ,COUNT(*) AS CNT
      ,RANK() OVER(ORDER BY COUNT(*) DESC) AS RANK
FROM SCOTT.EMP
GROUP BY JOB;
-- Output
-- SALESMAN |4|1
-- CLERK    |4|1
-- MANAGER  |3|3
-- ANALYST  |2|4
-- PRESIDENT|1|5

-- 직급별 급여 순위
SELECT ENAME
      ,JOB
      ,SAL
      ,RANK() OVER(PARTITION BY JOB ORDER BY SAL DESC) AS RANK
FROM SCOTT.EMP;
-- Output
-- FORD  |ANALYST  |3000|1
-- SCOTT |ANALYST  |3000|1
-- MILLER|CLERK    |1300|1
-- ADAMS |CLERK    |1100|2
-- JAMES |CLERK    |950|3
-- SMITH |CLERK    |800|4
-- JONES |MANAGER  |2975|1
-- BLAKE |MANAGER  |2850|2
-- CLARK |MANAGER  |2450|3
-- KING  |PRESIDENT|5000|1
-- ALLEN |SALESMAN |1600|1
-- TURNER|SALESMAN |1500|2
-- WARD  |SALESMAN |1250|3
-- MARTIN|SALESMAN |1250|3
```

- DENSE_RANK: 순위를 매긴다. 같은 순위가 존재하더라도 다음 순위를 건너뛰지 않는다.

``` sql
-- scott schema
SELECT JOB
      ,COUNT(*) AS CNT
      ,DENSE_RANK() OVER(ORDER BY COUNT(*) DESC) AS RANK
FROM SCOTT.EMP
GROUP BY JOB;
-- Output
-- SALESMAN |4|1
-- CLERK    |4|1
-- MANAGER  |3|2
-- ANALYST  |2|3
-- PRESIDENT|1|4
```

- ROW_NUMBER: 순위를 매긴다. 동일한 순위라도 각기 다른 순위를 부여한다.

``` sql
-- scott schema
SELECT JOB
      ,COUNT(*) AS CNT
      ,ROW_NUMBER() OVER(ORDER BY COUNT(*) DESC) AS RANK
FROM SCOTT.EMP
GROUP BY JOB;
-- Output
-- SALESMAN |4|1
-- CLERK    |4|2
-- MANAGER  |3|3
-- ANALYST  |2|4
-- PRESIDENT|1|5
```

### 4.5.3 집계 함수

- **SUM:** 데이터의 합계를 리턴한다. 인수(Argument)로는 숫자형 데이터 타입(NUMBER, FLOAT, …)만 가능하다.

``` sql
SELECT * FROM STUDENTS;
-- Output
-- 1|A|JAVA|10
-- 2|A|NODE|45
-- 3|B|JAVA|12
-- 4|B|NODE|51
-- 5|C|JAVA|8
-- 6|C|NODE|30

SELECT SUM(NUM) FROM (
    SELECT 1 AS NUM FROM DUAL
    UNION ALL
    SELECT 2 AS NUM FROM DUAL
);

SELECT STUDENT_NAME
      ,SUBJECT
      ,SCORE
      ,SUM(SCORE) OVER(PARTITION BY STUDENT_NAME) AS TOTAL_SCORE
FROM STUDENTS;
-- Output
-- A|JAVA|10|55
-- A|NODE|45|55
-- B|JAVA|12|63
-- B|NODE|51|63
-- C|JAVA|8 |38
-- C|NODE|30|38

SELECT STUDENT_ID
      ,STUDENT_NAME
      ,SUBJECT
      ,SCORE
      ,SUM(SCORE) OVER(PARTITION BY STUDENT_NAME ORDER BY SCORE) AS SUM1
      ,SUM(SCORE) OVER(PARTITION BY STUDENT_NAME ORDER BY SCORE RANGE UNBOUNDED PRECEDING) AS SUM2
FROM STUDENTS;
-- Output
-- 1|A|JAVA|10|10|10
-- 2|A|NODE|45|55|55
-- 3|B|JAVA|12|12|12
-- 4|B|NODE|51|63|63
-- 5|C|JAVA|8 |8 |8
-- 6|C|NODE|30|38|38
```

- **MAX:** 데이터의 최대값을 리턴한다.

``` sql
-- 과목별 최대값 조회
SELECT STUDENT_NAME
      ,SUBJECT
      ,SCORE
FROM (
    SELECT STUDENT_NAME
          ,SUBJECT
          ,SCORE
          ,MAX(SCORE) OVER(PARTITION BY SUBJECT) MAX_SCORE
    FROM STUDENTS
)
WHERE SCORE = MAX_SCORE;
-- Output
-- B|JAVA|12
-- B|NODE|51
```

- MIN: 데이터의 최솟값을 리턴한다.

``` sql
-- 과목별 최솟값 조회
SELECT STUDENT_NAME
      ,SUBJECT
      ,SCORE
FROM (
    SELECT STUDENT_NAME
          ,SUBJECT
          ,SCORE
          ,MIN(SCORE) OVER(PARTITION BY SUBJECT) MIN_SCORE
    FROM STUDENTS
)
WHERE SCORE = MIN_SCORE;
-- Output
-- C|JAVA|8
-- C|NODE|30
```

- **AVG:** 데이터의 평균값을 리턴한다.

``` sql
-- 과목별 평균값 이상
SELECT STUDENT_NAME
      ,SUBJECT
      ,SCORE
FROM (
    SELECT STUDENT_NAME
          ,SUBJECT
          ,SCORE
          ,AVG(SCORE) OVER(PARTITION BY SUBJECT) AVG_SCORE
    FROM STUDENTS
)
WHERE SCORE >= AVG_SCORE;
```

- **COUNT:** 데이터의 건수를 리턴한다.

``` sql
SELECT COUNT(*)
FROM STUDENTS
WHERE SUBJECT = 'JAVA';
```

| ⁉️ 윈도우 함수 사용 옵션 |
|--------------------|
| BETWEEN UNBOUNDED PRECEDING AND n PRECEDING |
| BETWEEN UNBOUNDED AND CURRENT ROW |
| BETWEEN UNBOUNDED PRECEDING AND n FOLLWING |
| BETWEEN n PRECEDING AND n PRECEDING |
| BETWEEN n PRECEDING AND CURRENT ROW |
| BETWEEN n PRECEDING AND n FOLLOWING |
| BETWEEN n PRECEDING AND UNBOUNDED FOLLOWING |
| BETWEEN CURRENT ROW AND n FOLLOWING |
| BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING |
| BETWEEN n FOLLOWING ROW AND n FOLLOWING |
| BETWEEN n FOLLOWING ROW AND UNBOUNDED FOLLOWING |
| UNBOUNDED PRECEDING (default: RANGE UNBOUNDED PRECEDING) |
| n PRECEDING |
| CURRENT ROW |

### 4.5.4 행 순서 함수

- **FIRST_VALUE:** 파티션 별 선두에 위치한 데이터를 구한다.

``` sql
-- scott schema
SELECT ENAME
      ,JOB
      ,SAL
      ,FIRST_VALUE(SAL) OVER(PARTITION BY JOB ORDER BY SAL DESC)
FROM SCOTT.EMP
WHERE JOB IN ('MANAGER', 'SALESMAN');
-- Output
-- JONES |MANAGER |2975|2975
-- BLAKE |MANAGER |2850|2975
-- CLARK |MANAGER |2450|2975
-- ALLEN |SALESMAN|1600|1600
-- TURNER|SALESMAN|1500|1600
-- WARD  |SALESMAN|1250|1600
-- MARTIN|SALESMAN|1250|1600
```

- **LAST_VALUE:** 파티션 별 마지막에 위치한 데이터를 구한다.

``` sql
-- scott schema
SELECT ENAME
      ,JOB
      ,SAL
      ,LAST_VALUE(SAL) OVER(
          PARTITION BY JOB
          ORDER BY SAL
          ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING)
FROM SCOTT.EMP
WHERE JOB IN ('MANAGER', 'SALESMAN');
-- Output
-- CLARK |MANAGER |2450|2975
-- BLAKE |MANAGER |2850|2975
-- JONES |MANAGER |2975|2975
-- WARD  |SALESMAN|1250|1600
-- MARTIN|SALESMAN|1250|1600
-- TURNER|SALESMAN|1500|1600
-- ALLEN |SALESMAN|1600|1600
```

- **LAG:** 파티션 별 특정 수만큼 앞선 데이터를 구한다.

``` sql
-- scott schema
SELECT ENAME
      ,JOB
      ,SAL
      ,LAG(SAL, 3) OVER(ORDER BY SAL) AS LAG
FROM SCOTT.EMP
WHERE JOB IN ('MANAGER', 'SALESMAN');
-- Output
-- MARTIN|SALESMAN|1250| - 
-- WARD  |SALESMAN|1250| - 
-- TURNER|SALESMAN|1500| - 
-- ALLEN |SALESMAN|1600|1250
-- CLARK |MANAGER |2450|1250
-- BLAKE |MANAGER |2850|1500
-- JONES |MANAGER |2975|1600
```

- LEAD: 파티션 별 특정 수만큼 뒤에 있는 데이터를 구한다.

``` sql
-- scott schema
SELECT ENAME
      ,JOB
      ,SAL
      ,LEAD(SAL, 2) OVER(PARTITION BY JOB ORDER BY SAL) AS LAG
FROM SCOTT.EMP
WHERE JOB IN ('MANAGER', 'SALESMAN');
-- Output
-- CLARK |MANAGER |2450|2975
-- BLAKE |MANAGER |2850| - 
-- JONES |MANAGER |2975| - 
-- WARD  |SALESMAN|1250|1500
-- MARTIN|SALESMAN|1250|1600
-- TURNER|SALESMAN|1500| - 
-- ALLEN |SALESMAN|1600| -
```

### 4.5.5 비율 함수

- **RATIO_TO_REPORT:** 파티션 별 합계에서 비율을 구한다.

``` sql
-- scott schema
SELECT ENAME
      ,JOB
      ,SAL
      ,SUM(SAL) OVER() AS SUM
      ,ROUND(SAL / SUM(SAL) OVER(), 3) AS "SAL/SUM"
      ,ROUND(RATIO_TO_REPORT(SAL) OVER(), 3) AS RATIO_TO_REPORT
FROM SCOTT.EMP
WHERE JOB = 'SALESMAN';
-- Output
-- ALLEN |SALESMAN|1600|5600|.286|.286
-- WARD  |SALESMAN|1250|5600|.223|.223
-- MARTIN|SALESMAN|1250|5600|.223|.223
-- TURNER|SALESMAN|1500|5600|.268|.268
```

- **PERCENT_RANK:** 파티션의 첫 행을 0, 마지막 행을 1로 놓고 현재 행의 위치를 백분위 순위 값으로 구한다.

``` sql
SELECT ROUND(PERCENT_RANK() OVER(ORDER BY NUM), 3) FROM (
    SELECT 1 AS NUM FROM DUAL
    UNION ALL
    SELECT 2 FROM DUAL
    UNION ALL
    SELECT 3 FROM DUAL
    UNION ALL
    SELECT 4 FROM DUAL
);
-- Output
-- 0
-- .333
-- .667
-- 1
```

- **CUME_DIST:** 파티션에서 누적 백분율을 구한다.

``` sql
SELECT ROUND(CUME_DIST() OVER(ORDER BY NUM), 3) FROM (
    SELECT 1 AS NUM FROM DUAL
    UNION ALL
    SELECT 2 FROM DUAL
    UNION ALL
    SELECT 3 FROM DUAL
    UNION ALL
    SELECT 4 FROM DUAL
);
-- Output
-- .25
-- .5
-- .75
-- 1
```

- **NTILE:** 주어진 수만큼 행들을 n등분하여 등급을 구한다. 할당할 행이 남았을 경우 맨 앞 그룹부터 채운다.

``` sql
-- scott schema
SELECT ENAME
      ,JOB
      ,SAL
      ,NTILE(4) OVER(ORDER BY SAL) AS "NTILE_4"
      ,NTILE(5) OVER(ORDER BY SAL) AS "NTILE_5"
FROM SCOTT.EMP;
-- Output
-- SMITH |CLERK    |800 |1|1
-- JAMES |CLERK    |950 |1|1
-- ADAMS |CLERK    |1100|1|1
-- MARTIN|SALESMAN |1250|1|2
-- WARD  |SALESMAN |1250|2|2
-- MILLER|CLERK    |1300|2|2
-- TURNER|SALESMAN |1500|2|3
-- ALLEN |SALESMAN |1600|2|3
-- CLARK |MANAGER  |2450|3|3
-- BLAKE |MANAGER  |2850|3|4
-- JONES |MANAGER  |2975|3|4
-- FORD  |ANALYST  |3000|4|4
-- SCOTT |ANALYST  |3000|4|5
-- KING  |PRESIDENT|5000|4|5
```

### 4.5.6 Top-N 쿼리

상위 n개의 데이터를 추출하기 위한 쿼리를 말한다.

- **ROWNUM:** 실제로 존재하지 않는 가짜 컬럼(Pseudo)을 생성한다.

``` sql
-- scott schema
SELECT ROWNUM
      ,ENAME
      ,JOB
      ,SAL
FROM (
    SELECT ENAME
          ,JOB
          ,SAL
    FROM SCOTT.EMP
    WHERE JOB IN ('PRESIDENT', 'ANALYST', 'MANAGER')
    ORDER BY SAL DESC
);
-- Output
-- 1|KING |PRESIDENT|5000
-- 2|FORD |ANALYST  |3000
-- 3|SCOTT|ANALYST  |3000
-- 4|JONES|MANAGER  |2975
-- 5|BLAKE|MANAGER  |2850
-- 6|CLARK|MANAGER  |2450

SELECT ROWNUM
      ,ENAME
      ,JOB
      ,SAL
FROM (
    SELECT ENAME
          ,JOB
          ,SAL
    FROM SCOTT.EMP
    WHERE JOB IN ('PRESIDENT', 'ANALYST', 'MANAGER')
    ORDER BY SAL DESC
)
~~WHERE ROWNUM = 5~~; --> WHERE ROWNUM <= 5
-- Output
-- no data found
```

- 윈도우 함수와 순위 함수

``` sql
-- scott schema
-- ROW_NUMBER()
SELECT *
FROM (
    SELECT ROW_NUMBER() OVER(ORDER BY SAL DESC) AS ROW_NUM
          ,ENAME
          ,JOB
          ,SAL
    FROM SCOTT.EMP
)
WHERE ROW_NUM <= 5;

-- RANK()
SELECT *
FROM (
    SELECT RANK() OVER(ORDER BY SAL DESC) AS RANK
          ,ENAME
          ,JOB
          ,SAL
    FROM SCOTT.EMP
)
WHERE RANK <= 5;

-- DENSE_RANK()
SELECT *
FROM (
    SELECT DENSE_RANK() OVER(ORDER BY SAL DESC) AS D_RANK
          ,ENAME
          ,JOB
          ,SAL
    FROM SCOTT.EMP
)
WHERE D_RANK <= 5;
```

### 4.5.7 셀프 조인(Self Join)

셀프 조인은 자신과 조인하는 것을 말한다. 같은 테이블이 두 번 이상 등장하므로 Alias를 열 표기를 명확(`ORA-00918: column ambiguously defined`)하게 해야 한다.

``` sql
-- scott schema
SELECT A.EMPNO AS MGR_NO
      ,ENAME AS MGR_NM
      ,B.EMPNO
      ,B.ENAME
FROM SCOTT.EMP A
INNER JOIN SCOTT.EMP B
    ON A.EMPNO = B.MGR;
-- 나(A)의 사원번호가 관리자 번호(B)인 인스턴스를 연결(=)하겠다.
-- 내가 관리자인 녀셕들 내 밑으로 집합!
-- 7566|JONES|7902|FORD
-- 7566|JONES|7788|SCOTT
-- 7698|BLAKE|7499|ALLEN
-- 7698|BLAKE|7900|JAMES
-- 7698|BLAKE|7844|TURNER
-- 7698|BLAKE|7654|MARTIN
-- 7698|BLAKE|7521|WARD
-- 7782|CLARK|7934|MILLER
-- 7788|SCOTT|7876|ADAMS
-- 7839|KING |7698|BLAKE
-- 7839|KING |7782|CLARK
-- 7839|KING |7566|JONES
-- 7902|FORD |7369|SMITH
```

### 4.5.8 계층 쿼리

계층 구조를 이루는 컬럼을 출력할 때 사용한다.

``` sql
-- 계층 쿼리에서 사용할 수 있는 키워드
-- CONNECT_BY_ROOT: 루트 노드의 컬럼 값을 반환
-- CONNECT_BY_ISLEAF: 가장 하위 노드인 경우 1을 반환 이외에는 0을 반환
-- ORDER SIBLINGS BY: 같은 레벨끼리 정렬하기 위해서 사용한다. (~~ORDER BY~~)
-- PRIOR: 이전 PATH와 현재 Row의 PARENT_PATH가 일치하는 Row를 연결하라
SELECT LEVEL
      ,PATH
      ,PARENT_PATH
      ,SYS_CONNECT_BY_PATH(PATH, '/') AS CONNECT_BY_PATH
      ,CONNECT_BY_ROOT PATH
      ,CONNECT_BY_ISLEAF
FROM (
    SELECT 'Macintosh HD' AS PATH, NULL AS PARENT_PATH FROM DUAL
    UNION ALL
    SELECT 'USER' AS PATH, 'Macintosh HD' AS PARENT_PATH FROM DUAL
    UNION ALL
    SELECT 'Shared' AS PATH, 'USER' AS PARENT_PATH FROM DUAL
)
START WITH PARENT_PATH IS NULL
CONNECT BY PRIOR PATH = PARENT_PATH;
-- Output
-- 1|Macintosh HD|            |/Macintosh HD            |Macintosh HD|0
-- 2|USER        |Macintosh HD|/Macintosh HD/USER       |Macintosh HD|0
-- 3|Shared      |USER        |/Macintosh HD/USER/Shared|Macintosh HD|1
```