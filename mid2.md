
# SQLite를 활용한 고전 게임 정보 관리 데이터베이스

## 1. 프로젝트 소개

SQLite를 활용하여 고전 게임 정보를 저장하고 검색할 수 있는 데이터베이스를 구현하였다.

게임 이름, 출시 연도, 장르, 평점을 저장하며, 게임 이름이나 장르를 기준으로 원하는 정보를 검색할 수 있다.

## 2. 개발 환경

- 데이터베이스: SQLite
- 사용 언어: SQL
- 실행 프로그램: DB Browser for SQLite

## 3. 데이터베이스 설계

테이블명: `games`

| 필드명 | 자료형 | 설명 |
|---|---|---|
| id | INTEGER | 게임 번호 (기본키) |
| name | TEXT | 게임 이름 |
| year | INTEGER | 출시 연도 |
| genre | TEXT | 게임 장르 |
| rating | INTEGER | 게임 평점 (1~5) |

## 4. 테이블 생성

```sql
CREATE TABLE games (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    year INTEGER,
    genre TEXT,
    rating INTEGER
);
```

## 5. 데이터 입력

4개의 고전 게임 정보를 데이터베이스에 입력하였다.

```sql
INSERT INTO games (name, year, genre, rating)
VALUES
('Super Mario Bros.', 1985, 'Platform', 5),
('Tetris', 1984, 'Puzzle', 5),
('Pac-Man', 1980, 'Arcade', 4),
('Sonic the Hedgehog', 1991, 'Platform', 4);
```

## 6. 데이터 검색

### 6-1. 전체 게임 검색

```sql
SELECT * FROM games;
```

등록된 모든 게임 정보를 조회한다.

### 6-2. 게임 이름으로 검색

```sql
SELECT * FROM games
WHERE name = 'Tetris';
```

게임 이름이 Tetris인 데이터를 검색한다.

### 6-3. 장르로 검색

```sql
SELECT * FROM games
WHERE genre = 'Platform';
```

장르가 Platform인 게임을 검색한다.

### 6-4. 평점으로 검색

```sql
SELECT name, year, rating
FROM games
WHERE rating = 5;
```

평점이 5점인 게임의 이름, 출시 연도, 평점을 조회한다.

## 7. 예상 실행 결과

### 전체 게임 목록

| ID | 게임 이름 | 출시 연도 | 장르 | 평점 |
|---:|---|---:|---|---:|
| 1 | Super Mario Bros. | 1985 | Platform | 5 |
| 2 | Tetris | 1984 | Puzzle | 5 |
| 3 | Pac-Man | 1980 | Arcade | 4 |
| 4 | Sonic the Hedgehog | 1991 | Platform | 4 |

### 장르 검색 결과

`WHERE genre = 'Platform'`을 실행하면 다음 두 게임이 조회된다.

- Super Mario Bros.
- Sonic the Hedgehog

## 8. 구현 결과 및 느낀 점

SQLite를 이용하여 데이터베이스 테이블을 생성하고 데이터를 입력하는 방법을 익혔다. 또한 SELECT와 WHERE 명령어를 활용하여 조건에 맞는 데이터를 검색할 수 있었다.

이번 실습을 통해 데이터베이스의 기본 구조와 SQL을 이용한 데이터 관리 및 검색 과정을 이해할 수 있었다.

## 9. 결론

고전 게임 정보를 관리하는 데이터베이스를 구현하여 테이블 생성, 데이터 입력, 전체 조회 및 조건 검색 기능을 확인하였다.
