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
