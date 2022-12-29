# 이것이 오라클 이다.

## 데이터베이스 모델링

- 데이터베이스 모델링 개념

    ![데이터베이스 모델링 개념](/img/1.PNG)

    - 현실에서 쓰이는 것을 테이블로 변경하기 위한 작업

- 데이터베이스 모델링 3단계

    - 개념적 모델링
    - 논리적 모델링
    - 물리적 모델링

    ![모델링 예시1](/img/2.PNG)
```sql
CREATE TABLE buytbl (
    username           NCHAR(3),
    prodname           NCHAR(3),
    price              INTEGER,
    amount             INTEGER,
    usertbl_username   NCHAR(3) NOT NULL
);

CREATE TABLE usertbl (
    username   NCHAR(3) NOT NULL,
    birthyer   INTEGER,
    addr       NCHAR(2),
    mobile     VARCHAR2(13)
);

ALTER TABLE usertbl ADD CONSTRAINT usertbl_pk PRIMARY KEY ( username );

ALTER TABLE buytbl
    ADD CONSTRAINT buytbl_usertbl_fk FOREIGN KEY ( usertbl_username )
        REFERENCES usertbl ( username );
```

## Oracle 유틸리티 사용법

- SQLPlus
```sql
HELP INDEX
-- SQLPlus 명령어 목록 확인
DESCRIBE TABLE
-- 테이블 구조를 보여준다. = DESC TABLE
LIST
-- 마지막 수행된 SQL문을 보여준다. = L
RUN
-- 마지막 SQL문을 다시 실행한다. = /
DEL
-- SQLPlus는 마지막 명령어를 버퍼(메모리)에 저장한다 DEL은 버터를 비운다.
APPEND
-- 버퍼 뒤에 명령어 추가
REPLACE
-- 새로운 명령어 덮어 쓰기
SHOW USER
-- 현재 사용자를 출력해 줌
CONNECT HR/1234@XE
-- HR 사용자로 접속
COLUMN 열 이름 HEADING "출력이름" FORMAT 출력폭
-- 출력되는 헤더(열 이름) 변경, 폭 조절
SPOOL 파일경로
SPOOL OFF
-- 명령어 파일에 저장 시작, 종료
```

## PL/SQL 기본

- SELECT 전체 구문 형식
```sql
WITH <Sub Query>
SELECT select_list -- DISTINCT
FROM table_source
WHERE search_condition -- 조건연산자조건연산자(=, <, >, <=, >=, <>, !=)관계연산자(NOT, AND, OR 등), BETEWEEN ~~~ AND, IN(), LIKE, ANY, ALL, SOME, 서브쿼리, ROWNUM,
GROUP BY group_by_expression -- 
HAVING search_condition -- 집계 함수
ORDER BY order_expression ASC : DESC 
```
- SELECT 약식
```sql
SELECT 열 이름
FROM 테이블 이름
WHERE 조건
```


-- ORDER BY 
