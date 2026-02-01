## SELECT
SELECT문은 아래와 같은 구문들을 포함
```
SELECT [DISTINCT] [column1], [column2] [AS alias]
FROM table [AS alias]
[WHERE condition1 AND/OR condition2]
[GROUP BY column1]
[HAVING group_condition]
[ORDER BY column1 ASC/DESC]
[LIMIT N] [OFFSET M];
```

### 기본 SELECT
모든 필드 조회
```
SELECT * FROM 테이블명;
```

특정 컬럼 조회
칼럼이 띄어쓰기가 있는 경우, ""로 감싸서 하나의 칼럼으로 인식하게 만듦
```
SELECT 테이블명.컬럼명1, 테이블명.컬럼명2 FROM 테이블명;
```
컬럼을 선택할 때 테이블명을 생략해도 되지만, 생략하지 않는 것이 성능상, 그리고 이후 여러 테이블을 합쳐서 조회할 때 이점이 있음

alias 설정
테이블명을 축약하여 사용 가능
```
SELECT t.컬럼명1, t.컬럼명2 FROM 테이블명 AS t;
SELECT t.컬럼명1, t.컬럼명2 FROM 테이블명 t;
```

조회 시 컬럼의 이름을 변경 가능
```
SELECT t.컬럼명1 AS first, t.컬럼명2 AS second FROM 테이블명 t;
SELECT t.컬럼명1 first, t.컬럼명2 second FROM 테이블명 t;
```
as는 생략 가능

중복 제거
```
SELECT DISTINCT 컬럼명 FROM 테이블명;
```

### WHERE
WHERE 절은 데이터를 필터링할 때 사용하는 조건절
문자의 경우 ''(홑따옴표)를 사용

비교 연산자
```
WHERE population > 1000000
WHERE population >= 1000000
WHERE population < 1000000
WHERE population <= 1000000
WHERE population = 1000000
WHERE population != 1000000  -- 또는 <>
```

논리 연산자
```
WHERE population > 1000000 AND continent = 'asia'
WHERE population > 1000000 OR continent = 'asia'
WHERE NOT population < 1000000
```

범위
```
WHERE population BETWEEN 1000000 AND 2000000
WHERE population NOT BETWEEN 1000000 AND 2000000
WHERE created_at BETWEEN '2024-01-01' AND '2024-12-31'
```

포함
```
WHERE code IN ('KOR', 'JPN', 'CHN')
WHERE code NOT IN ('KOR', 'JPN', 'CHN')
```

NULL 여부
```
WHERE LifeExpectancy IS NULL
WHERE LifeExpectancy IS NOT NULL
```

패턴매칭
    %: 0개 이상의 문자
    _: 1개의 문자
    LIKE 대신 ILIKE를 활용하면 대소문자를 구분하지 않음
```
WHERE Name LIKE 'S%'        -- S로 시작
WHERE Name LIKE '%on'       -- on으로 끝남
WHERE Name LIKE '%on%'      -- on이 포함
WHERE Name NOT LIKE 'S%'    -- S로 시작하지 않음
```

### ORDER BY
ORDER BY는 결과를 정렬하는 구문
```
ORDER BY 컬럼명 [ASC/DESC]
```
ASC : 오름차순 (기본값, 생략가능)
DESC: 내림차순

### LIMIT, OFFSET
조회 할 레코드들의 개수를 제한
    ORDER BY와 함께 사용

LIMIT : 갯수
    한 페이지에 보여줄 개수

OFFSET : OFFSET에 해당하는 다음 값 부터
    (페이지 번호 - 1) * 페이지당 개수