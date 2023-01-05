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
WHERE search_condition -- 조건연산자조건연산자(=, <, >, <=, >=, <>, !=)관계연산자(NOT, AND, OR 등), BETEWEEN ~~~ AND, IN(), LIKE, ANY, ALL, SOME, 서브쿼리, ROWNUM, SAMPLE(퍼센트)
GROUP BY group_by_expression -- 
HAVING search_condition -- 집계 함수
ORDER BY order_expression ASC : DESC 
```
- SELECT 기본 구조
```sql
SELECT 열 이름
FROM 테이블 이름
WHERE 조건
```

- WHERE절은 조회하는 결과에 특정한 조건을 줘서, 원하는 데이터만 보고 싶을 때 사용한다.

- CREATE TABLE ... SELECT 구문은 테이블을 복사해서 사용할 경우에 주로 사용한다.

- SQL문은 크게 DML, DDL, DCL로 분류한다.
    - DML
        - SELECT, INSERT, UPDATE, DELETE
    - DDL
        - CREATE, DROP, ALTER
    - DCL
        - GRANT, REVOKE, DENY

- INSERT/UPDATE/DELETE문은 데이터의 입력/수정/삭제 기능을 한다.

## PL/SQL 고급

- Oracle은 숫자, 문자, 날짜 등의 다양한 데이터 형식을 지원한다.
    - 숫자
        - BINARY_FLOAT, BINARY_DOUBLE, NUMER(p,[s])
    - 문자
        - CHAR(), NCHAR(), VARCHAR2(), NVARCHAR2(), CLOB, NCLOB
    - 이진
        - BLOB, BFILE
    - 날짜
        - DATE, TIMESTAMP, TIMESTAMP WITH TIME ZONE, TIMESTAMP WITH LOCAL TIME ZONE
    - 기타
        - RAWID, XMLType, URIType

- 대용량 데이터의 저장과 추출을 위해서는 CLOB, BLOB의 데이터 형식을 사용한다.

- Oracle도 변수를 사용할 수 있는데, DELARE문과 함께 선언된다.
    ```SQL
    DECLARE
        변수이름1 데이터형식;
        변수이름2 데이터형식;
    BEGIN
        변수이름1 = 값;
        SELECT 열 이름 INTO 변수이름2 FROM 테이블;
    END
    ```

- Oracle은 문자열 함수, 숫자 및 수학 함수, 날짜/시간 함수, 형 변환 함수, 분석 함수, 확장 함수, 기타 함수 등 다향한 내장 함수를 제공한다.

- 두 개 이상의 테이블을 묶는 조인은 내부 조인, 외부 조인 등이 있다.
    - INNER JOIN(내부 조인)
    - LEFT/RIGHT/FULL OUTER JOIN(외부 조인)
    - CROSS JOIN(상호 조인)
    - SELF JOIN(자체 조인)
        ```SQL
        SELECT A.a, B.b
        FROM empTBL A
            INNER JOIN empTBL B ON A.a = B.b
        WHERE A.a = ~~~
        ```

- Oracle은 일반 프로그래밍 언어와 비슷한 프로그래밍 문법을 지원한다.

## 테이블과 뷰

- 제약 조건이란 데이터의 무결성을 지키기 위한 제한된 조건을 의미한다.

- 제약 조건의 종류로는 기본 키, 외래 키, Unique, Check, Default, Null 제약 조건 등이 있다.
    - PRIMARY KEY 제약 조건
        - 기본 키 제약 조건
        - 기본 키에 입력되는 값은 중복될 수 없으며, NULL 값이 입력될 수 없다.
    - FOREIGN KEY 제약 조건
        - 외래 키 제약 조건
        - 두 테이블 사이의 관계를 선언함으로 데이터의 무결성을 보장해 주는 역할
        - 외래키 관계를 설정하면 하나의 테이블이 다른 테이블에 의존하게 된다.
    - UNIQUE 제약 조건
        - '중복되지 않는 유일한 값'을 입력해야 하는 조건
        - 기본 키 제약 조건과 비슷하지만 UNIQUE는 NULL값 허용
    - CHECK 제약 조건
        - 입력되는 데이터를 점검하는 기능(EX.'마이너스 값이 들어올 수 없다' 등의 조건)
    - DEFAULT 정의
        - 값을 입력하지 않았을 때, 자동으로 입력되는 기본 값을 정의하는 방법
    - NULL 값 허용
        - 공백을 허용 하는것

- Oracle은 임시 테이블 기능을 지원한다.

- 뷰란 한마디로 '가상의 테이블'이라고 생각하면 된다.

- 구체화된 뷰는 실체가 있는 뷰다.

## 인덱스

- 인덱스를 생성하면 검색의 속도가 빨라진다.

- 인덱스의 종류로는 B-TREE 인덱스, BITMAP 인덱스, 함수 기반 인덱스, 어플리케이션 도메인 인덱스 등이 있다.
    - B-TREE(Balanced Tree, 균형 트리)
        - 자료 구조에 나오는 범용적으로 사용되는 데이터의 구조
        - ![](/img/3.PNG)

- Primary Key, Unique를 설정한 열에는 자동으로 인덱스가 생성된다.

- 인덱스는 B-Tree 구조를 갖는다.

- 인덱스의 생성, 삭제를 위해서는 CREATE INDEX/ DROP INDEX문을 사용할 수 있다.

- 인덱스가 있다고 Oracle이 반드시 인덱스를 사용하는 것은 아니다.
    - 인덱스는 열 단위에 생성된다.
    - WHERE절에서 사용되는 열에 인덱스를 만들어야 한다.
    - WHERE절에 사용되더라도 자주 사용해야 가치가 있다.
    - 데이터의 중복도가 높은 열은 인덱스를 만들어도 별 효과가 없다.
    - JOIN에 자주 사용되는 열에는 인덱스를 생성해 주는 것이 좋다.
    - INSERT/UPDATE/DELETE가 얼마나 자주 일어나는 지를 고려해야 한다.
    - 사용하지 않는 인덱스는 제거하자

## 스토어드 프로시저와 함수

- 스토어드 프로시저와 함수는 Oracle에서 제공하는 프로그래밍 기능이다.
    - 쿼리문의 집합으로 어떠한 동작을 일괄 처리하기 위한 용도

- 스토어드 프로시저는 매개변수도 사용이 가능하며, 호출은 EXECUTE문을 사용한다.
    - 정의 형식
    ```sql
    CREATE [ OR REPLACE ] PROCEDURE [ schema. ]procedure
        [ (argument [ { IN : OUT : IN OUT } ]
                    [ NOCOPY ]
                    datatype [ DEFAULT expr ]
            [, argument [ { IN : OUT : IN OUT } ]
                        [ NOCOPY ]
                        datatype [ DEFAULT expr ]
            ]
        ) ]
    [ invoker_rights_clause ]
    { IS : AS } -- 동일한 단어이며, 아무거나 사용해도 됨.
    { pl/sql_subprogram_body : EXECUTE_spec } ;
    ```
    - 약식
    ```sql
    CREATE OR REPLACE PROCEDURE 스토어드_프로시저_이름( 파라미터 ) AS
        변수 선언 부분
    BEGIN
        이 부분에 PL/SQL 프로그래밍 코딩...
    END [스토어드_프로시저_이름] ;
    ```
    - 실행문
    ```SQL
    EXECUTE 스토어드_프로시저_이름();
    EXEC 스토어드_프로시저_이름();
    ```
    - 스토어드 프로시저의 수정은  CREATE OR REPLACE PROCEDURE문을 다시 사용하면 되며, 삭제는 DROP PROCEDURE문을 사용하면 된다.

- 스토어드 프로시저 특징
    - Oracle의 성능을 향상시킬 수 있다.
    - 유지관리가 간편하다.
    - 예외 처리 및 모듈식 프로그래밍이 가능하다.
    - 네트워크 전송량의 감소

- 함수는 반환하는 값이 반드시 있으며 주로 SELECT문 안에서 사용된다.

- 테이블 형식을 반환하는 함수도 사용할 수 있다.

- 커서는 일반 프로그래밍의 파일 처리와 비슷한 방법을 제공한다.
    - 커서의 처리 순서
    - ![](/img/4.PNG)

## 트리거

- 트리거는 제약 조건과 더불어 데이터 무결성을 위해서 Oracle에서 사용할 수 있는 또 다른 기능이다.

- 트리거는 테이블 또는 뷰와 관련되어 DML문 (INSERT, UPDATE, DELET 등)의 이벤트가 발생될 때 작동하는 데이터베이스 개채 중 하나

- 트리거의 종류에는 AFTER/BEFORE/INSTEAD OF 등이 있다.
    - AFTER 트리거
        - DML문 작업 후(After) 작동한다.
        - 테이블에만 작동하며 뷰에는 작동하지 않는다.
    - BEFORE 트리거
        - DML문 작업 전(Before) 작동한다.
        - 테이블에만 작동하며 뷰에는 작동하지 않는다.
    - INSTEAD OF 트리거
        - DML문 작업 전 작동한다.
        - 뷰에만 작동하며 테이블에는 작동하지 않는다

- 기타 트리거
    - 다중 트리거
        - 하나의 테이블에 동일한 트리거가 여러 개 부착되어 있는 것을 말한다.
    - 중첩 트리거
        - 트리거가 또 다른 트리거를 작동하는 것을 말한다.
        - 중첩 트리거의 예
        ![](/img/5.PNG)
            - 1. 회원이 물건을 구매하게 되면, 물건을 구매한 기록이 '구매 테이블'에 INSERT된다.
            - 2. '구매 테이블'에 부탁된 INSERT 트리거가 작동한다. 내용은 '물품 테이블'의 남은 개수를 구매한 개수만큼 빼는 UPDATE를 한다.
            - 3. '물품 테이블'에 장착된 UPDATE 트리거가 작동한다. 내용은 '배송 테이블'에 배송할 내용을 INSERT하는 것이다.
    - 재귀 트리거
        - 트리거가 작동해서 다시 자신의 트리거를 작동시키는 것을 말한다.
        - 재귀 트리거 예
        ![](/img/6.PNG)
            - 1.INSERT 작업이 일어나면 A 테이블의 트리거가 작동해서 B 테이블에
            - 2.INSERT 작업을 하게 되고, B 테이블에서도 트리거가 작동해서
            - 3.INSERT 작업을 수행하게 된다. 그리고 다시 A 테이블의 트리거가 작동해서
            - 2.INSERT가 작동하게 되는 순환 구조를 갖는다.