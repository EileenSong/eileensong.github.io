---
layout: article
title: 5. 관리 구문
permalink: /notes/kr/sql-developer/chapter-05
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

## 5.1 DML
### 5.1.1 DML의 정의

DML(Data Manipulation Language)은 데이터 조작어로 DDL로 정의된 엔터티에 맞게 데이터를 입력하거나 수정, 삭제, 조회하는 명령어이다.

### 5.1.2 INSERT

데이블에 데이터를 입력하는 명령어이다.

- 기본 문법:   
`INSERT INTO table (col1, col2, ...) VALUES (val1, val2, ...);`   
`INSERT INTO table (col1, col2, ...) SELECT col1, col2, ... FROM table;`

``` sql
-- scott schema
INSERT INTO SCOTT.EMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
VALUES (9999, 'THEODORE', 'MANAGER', '7839', SYSDATE, 3000, NULL, 30);

INSERT INTO EMP A
SELECT 9999 AS EMPNO
	  ,'THEODORE' AS ENAME
	  ,B.JOB
	  ,B.MGR
	  ,SYSDATE()
	  ,3000 AS SAL
	  ,B.COMM
	  ,B.DEPTNO
FROM EMP B
WHERE ENAME = 'BLAKE';
```

### 5.1.3 UPDATE

이미 저장된 데이터를 갱신하는 명령어이다.

- 기본 문법:   
`UPDATE table SET col1 = value1, col2 = value2, … WHERE …`

``` sql
-- scott schema
-- 전체 ENAME이 변경되므로 WHERE 절을 사용해 업데이트 Row를 지정해야 한다.
UPDATE EMP
SET ENAME = 'THEODORE';

UPDATE EMP
SET ENAME = 'THEODORE'
WHERE EMPNO = 7839;
```

### 5.1.4 DELETE

이미 저장된 데이터를 삭제하는 명령어이다.

- 기본 문법:   
`DELETE FROM table WHERE …`

``` sql
-- scott schema
DELETE FROM EMP
WHERE EMPNO = 7839;
```

### 5.1.5 MERGE

테이블에 새로운 데이터를 입력하거나 이미 저장된 데이터를 변경, 삭제하는 작업을 한 번에 할 수 있다.

- 기본 문법:

``` sql
MERGE
 INTO target_table
USING source_table
   ON 조건
WHEN MATCHED THEN
	UPDATE SET col1 = value1, col2 = value2, ... WHERE ...
	DELETE WHERE ...
WHEN NOT MATCHED THEN
	INSERT (col1, col2, ...)
	VALUE (value1, value2, ...);

```

## 5.2 TCL
### 5.2.1 TCL의 정의

TCL(Transaction Control Language)은 트랜잭션 제어 언어이다. 보안, 무결성 회복, 병행 제어 등을 정의한다는 관점에서 DCL로 구분하기도 한다.

### 5.2.2 COMMIT, ROLLBACK, SAVEPOINT

- **COMMIT:** 모든 작업을 정상적으로 처리하겠다고 확정하는 명령어이다. 모든 변경 사항을 영구 저장하고 트랜잭션이 종료된다.

- **ROLLBACK:** 작업 도중에 문제가 발생했을 때, 변경 사항을 취소하고 트랜잭션을 종료한다.

- **SAVEPOINT:** ROLLBACK 수행 시 전체 작업을 취소하지 않고 일부 작업만 취소할 수 있도록하는 명령어이다.

``` sql
INSERT INTO TABLE_A VALUES(1, 'TEST01');
SAVEPOINT SP01;
INSERT INTO TABLE_A VALUES(2, 'TEST02');
SAVEPOINT SP02;
ROLLBACK TO SP1;
INSERT INTO TABLE_A VALUES(3, 'TEST03');
COMMIT;
SELECT * FROM TABLE_A;
-- Output
-- 1|TEST01
-- 3|TEST03
```

## 5.3 DDL
### 5.3.1 DDL의 정의

DDL(Data Definition Language)은 데이터 정의어이다. DDL 명령어를 사용하여 테이블, 뷰, 사용자 정의 함수 등과 같은 리소스를 생성, 변경, 삭제할 수 있다.

### 5.3.2 CREATE

테이블을 생성하기 위한 명령어이다.

- **테이블명/컬럼명:**

    - 이름은 문자로 시작해야 한다.

    - 허용된 사이즈 이하로 문자를 입력해야 한다.

    - 같은 사용자 소유의 테이블명은 중복이 불가능하며 한 테이블 안에서 컬럼명은 중복이 불가능하다.

    - 키워드는 사용이 불가능하다.

- **타입:**

    - **CHAR(n):** 고정길이 문자를 저장한다.

    - **VARCHAR2(n):**  가변길이 문자를 저장한다.

    - **CLOB:** 대용량 텍스트 타입 데이터를 저장한다.

    - **NUMBER(p, s):** 가변 숫자를 저장한다.

    - **DATE:** 연, 월, 일, 시, 분, 초를 저장한다.

    - **TIMESTAMP:** 연, 월, 일, 시, 분, 초, 밀리초를 저장한다.

* **제약조건:**

    - `NOT NULL`: 해당 컬럼은 NULL 값을 저장할 수 없다.

    - `UNIQUE` / `CONSTRAINT 제약조건이름 UNIQUE (*컬럼명*)`: 해당 컬럼은 서로 다른 값을 가져야 한다.

    - `PRIMARY KEY` / `CONSTRAINT 제약조건이름 PRIMARY KEY (컬럼명)`: 해당 컬럼은 NULL 값을 저장할 수 없고 중복된 값도 가질 수 없다.

    - `FOREIGN KEY REFERENCES table(column)` / `CONSTRAINT 제약조건이름 FOREIGN KEY REFERENCES (컬럼명)`: 테이블 간 관계를 설정한다.

        + 참조 무결성 관련 옵션: `CASCADE`, `SET NULL`, `SET DEFAULT`, `RESTRICT`, `NO ACTION`

    - `DEFAULT 기본값`: 해당 컬럼의 기본값을 설정한다. <sub><i>e.g. XXX_YN(여부) 컬럼인 경우 DEFAULT ‘N’로 설정할 수 있다</i></sub>.

    - `CHECK 컬럼명 IN (값1, 값2, …)`: 해당 컬럼은 정의된 값만 저장할 수 있다.

``` sql
CREATE TABLE 테이블명 (
	컬럼명 타입 제약조건
);

-- 기존 테이블을 복사할 때 사용
CREATE TABLE AS SELECT * FROM 테이블명;
```

### 5.2.3 ALTER

생성된 테이블의 구조를 변경하기 위한 명령어이다.

- **컬럼 추가:** `ALTER TABLE 테이블명 ADD 컬럼명 타입 제약조건;`

- **컬럼 삭제:** `ALTER TABLE 테이블명 DROP COLUMN 컬럼명;`

- **컬럼 변경:** `ALTER TABLE 테이블명 MODIFY (컬럼명1 타입 제약조건, 컬럼명2 타입 제약조건, …);`

- **컬럼명 변경:** `ALTER TABLE 테이블명 RENAME COLUMS 변경할컬럼명 TO 기존컬럼명;`

- **제약조건 추가:** `ALTER TABLE 테이블명 ADD CONSTRAINT 제약조건명 제약조건 (컬럼명);`

- **테이블 삭제:** `DROP TABLE 테이블명 참조무결성옵션;`

- **데이터 모두 삭제:** `TRUNCATE TABLE 테이블명;`   
(로그가 남지 않으며 ROLLBACK이 불가능해 DDL로 분류한다.)

## 5.4 DCL
### 5.4.1 DCL의 정의

DCL(Data Control Language)는 데이터 제어어로 사용자를 생성하고 권한을 부여할 수 있다.

### 5.4.2 사용자 관련 명령어

- **사용자 생성:** `CREATE USER 사용자명 IDENTIFIED BY 비밀번호;`

- **사용자 변경:** `ALTER USER 사용자명 IDENTIFIED BY 비밀번호;`

- 사용자 삭제: `DROP USER 사용자명;`

### 5.4.3 권한 관련 명령어

- **권한 부여:** `GRANT 권한 TO 사용자명;`

- **권한 회수:** `REVOKE 권한 FROM 사용자명;`

### 5.4.4. ROLE 관련 명령어

- **ROLE 생성:** `CREATE ROLE 롤명;`

- **ROLE에 권한 부여:** `GRANT 권한 TO 롤명;`

- **ROLE을 사용자에게 부여:** `GRANT 롤명 TO 사용자명;`