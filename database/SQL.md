```
## SQL
SQL(Structured Query Language)은 데이터베이스를 관리하고 데이터를 처리하기 위한 표준 프로그래밍 언어
데이터의 삽입, 조회, 수정, 삭제 등의 작업을 수행

### 기본 규칙
SQL 문은 세미콜론(;)으로 끝나야함
    여러 줄로 작성 가능하며, 가독성을 위해 적절한 들여쓰기 사용을 권장
SQL 키워드는 대소문자를 구분하지 않음
    SELECT와 select는 동일하게 작동함
    하지만 가독성을 위해 키워드는 대문자로 작성하는 것이 관례
주석 작성
    -- 또는 /* */을 활용
SQL은 여러 개의 공백을 하나의 공백으로 처리
    가독성을 위해 적절한 공백과 줄바꿈을 사용
테이블이나 컬럼명에 공백이 필요한 경우 보통 SnakeCase(user_profile)를 사용
    DB 예약어와 겹칠 경우 따옴표(MySQL은 ', Oracle/PostgreSQL은 "")로 감싸야함

### SQL 명령어 분류
DDL(Data Definition Language, 데이터 정의어)
    데이터베이스 구조를 정의하고 변경하는 명령어
DML(Data Manipulation Language, 데이터 조작어)
    데이터를 조작(입력, 수정, 삭제, 조회)하는 명령어
DCL(Data Control Language, 데이터 제어어)
    데이터베이스의 접근 권한을 제어하는 명령어
TCL(Transaction Control Language, 트랜잭션 제어어)
    트랜잭션을 제어하는 명령어

트랜잭션
    데이터베이스의 상태를 변화시키는 하나의 논리적 작업 단위