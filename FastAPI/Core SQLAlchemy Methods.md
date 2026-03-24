## ORM (Object-Relational Mapping)

ORM은 객체 지향 프로그래밍 언어와 관계형 데이터베이스(RDBMS) 사이의 서로 다른 데이터 구조를 해결하기 위한 기술이다.
개발자가 복잡한 SQL 쿼리를 직접 작성하는 대신, 파이썬 클래스와 메서드를 이용해 데이터를 다룰 수 있게 돕는 기술이다.

- **생산성 향상**
    
    SQL 문법 대신 익숙한 프로그래밍 언어로 데이터베이스를 조작할 수 있어 개발 속도가 빨라진다.
    
- **유지보수 용이**
    
    데이터베이스 종류가 바뀌더라도 ORM 설정만 변경하면 되므로 코드의 재사용성이 높다.
    
- **직관적인 코드**
    
    비즈니스 로직과 데이터 로직이 분리되어 코드가 훨씬 깔끔해진다.
    

# SQLAlchemy 주요 메서드

## 데이터 생성

- 단일 객체 추가 (add)

```python
new_post = Post(title="첫 번째 글", content="내용입니다")
db.add(new_post)
db.commit()
```

```sql
INSERT INTO posts (title, content) VALUES ('첫 번째 글', '내용입니다') RETURNING posts.id;
```

- 여러 객체 일괄 추가 (add_all)

```python
posts = [
    Post(title="두 번째", content="내용2"),
    Post(title="세 번째", content="내용3")
]
db.add_all(posts)
db.commit()
```

```sql
INSERT INTO posts (title, content) VALUES ('두 번째', '내용2'), ('세 번째', '내용3');
```

## 데이터 조회

- 기본키 기반 조회 (get)
    
    ID가 1인 객체가 이미 세션 메모리(Identity Map)에 있다면 SQL을 실행하지 않고 즉시 반환한다. 메모리에 없을 경우에만 SQL이 실행된다.
    

```python
post = db.get(Post, 1)
```

```sql
SELECT posts.id, posts.title, posts.content FROM posts WHERE posts.id = 1;
```

### stmt와 scalars

SQLAlchemy는 데이터를 바로 가져오는 대신,'무엇을 가져올지 정의하는 단계'와 '실제로 가져와서 변환하는 단계'를 분리한다.

- stmt (Statement) 
SQL 설계도이다. select(Post)와 같이 작성하면 "Post 테이블에서 모든 컬럼을 선택하라"는 문장이 준비되며, 실제 DB에 요청을 보내기 전의 '대기 상태'인 파이썬 객체이다.
- scalars()
    
    DB가 보낸 행(Row) 데이터에서 객체를 골라내는 추출기이다. 
    DB는 보통 `(id, title, content)` 같은 튜플 형태로 데이터를 주는데, `scalars()`를 거치면 우리가 원하는 `Post` 클래스의 인스턴스로 변환된다.
    
- 전체 조회

```python
stmt = select(Post)
posts = db.scalars(stmt).all()
```

```sql
SELECT posts.id, posts.title, posts.content
FROM posts;
```

- 단일 조건 조회

```python
stmt = select(Post).where(Post.id == 1)
# post = db.scalars(stmt).first()
post = db.scalar(stmt)
```

```sql
SELECT posts.id, posts.title, posts.content
FROM posts
WHERE posts.id = 1
LIMIT 1;
```

- 키워드 검색

```python
stmt = select(Post).where(Post.title.like("%FastAPI%"))
posts = db.scalars(stmt).all()
```

```sql
SELECT posts.id, posts.title, posts.content
FROM posts
WHERE posts.title LIKE '%FastAPI%';
```

- 다중 조건
    - and
    
    ```python
    stmt = select(Post).where(Post.id > 10, Post.title.like("%공부%"))
    posts = db.scalars(stmt).all()
    ```
    
    ```python
    SELECT posts.id, posts.title, posts.content 
    FROM posts 
    WHERE posts.id > 10 AND posts.title LIKE '%공부%';
    ```
    
    - or
    
    ```python
    stmt = select(Post).where(or_(Post.id == 1, Post.title == "공지"))
    posts = db.scalars(stmt).all()
    ```
    
    ```python
    SELECT posts.id, posts.title, posts.content 
    FROM posts 
    WHERE posts.id = 1 OR posts.title = '공지';
    ```
    
- 다중 복잡 조건

```python
stmt = select(Post).where(
    and_(
        Post.id > 10,
        or_(Post.title == "공지", Post.title == "이벤트")
    )
)
posts = db.scalars(stmt).all()
```

```sql
SELECT posts.id, posts.title, posts.content
FROM posts
WHERE posts.id > 10 AND (posts.title = '공지' OR posts.title = '이벤트');
```

- 정렬 조회

```python
stmt = select(Post).order_by(Post.id.desc())
posts = db.scalars(stmt).all()
```

```sql
SELECT posts.id, posts.title, posts.content
FROM posts
ORDER BY posts.id DESC;
```

- 결과 제한

```python
stmt = select(Post).order_by(Post.id.desc()).limit(10)
posts = db.scalars(stmt).all()
```

```sql
SELECT posts.id, posts.title, posts.content
FROM posts
ORDER BY posts.id DESC
LIMIT 10;
```

### 결과 추출 메서드의 종류와 특징

`all()`과 `first()` 외에도 상황에 따라 데이터의 유일성을 검증할 수 있는 다양한 메서드가 존재한다.

| 메서드 | 결과가 없을 때 | 결과가 2개 이상일 때 | 특징 |
| --- | --- | --- | --- |
| **`.all()`** | `[]` (빈 리스트) | 리스트 전체 반환 | 일반적인 목록 조회에 적합 |
| **`.first()`** | `None` 반환 | 첫 번째 것만 반환 | 데이터 존재 여부만 확인할 때 유용 |
| **`.one()`** | **에러 발생** | **에러 발생** | 반드시 정확히 1개여야 하는 고유값 조회 시 사용 |
| **`.one_or_none()`** | `None` 반환 | **에러 발생** | 없거나 하나만 있어야 하는 안전한 단건 조회에 권장 |

## 데이터 수정과 더티 체킹

SQLAlchemy는 객체의 속성 변경을 감지하여 자동으로 `UPDATE` 문을 생성한다.

### 더티 체킹을 통한 수정

```python
# 1. 먼저 데이터를 조회 (Persistent 상태가 됨)
post = db.get(Post, 1)

# 2. 객체의 속성을 변경
post.title = "수정된 제목"

# 3. 커밋 시점에 변경 사항이 감지되어 SQL 실행
db.commit()

```

## 데이터 삭제

- 객체 기반 삭제

```python
post = db.get(Post, 1)
db.delete(post)
db.commit()
```

```sql
DELETE FROM posts WHERE posts.id = 1;
```