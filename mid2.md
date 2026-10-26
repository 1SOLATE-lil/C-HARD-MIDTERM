# 1. 과제 주제: 게임 플레이 기록 관리 DB  
시나리오: 사용자가 플레이한 게임의 이름, 장르, 평점, 한 줄 감상평을 저장하고, 게임 이름이나 장르를 기준으로 원하는 기록을 검색하는 데이터베이스를 만든다.  
# 2. 데이터베이스 설계  
  
테이블명: games  
필드명	자료형	설명  
id	INTEGER	게임 번호, 기본키  
name	TEXT	게임 이름  
genre	TEXT	게임 장르  
rating	INTEGER	평점 (1~5)  
review	TEXT	한 줄 감상평  
  
# 3. SQLite 구현 코드  
```

CREATE TABLE games (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    year INTEGER,
    genre TEXT,
    rating INTEGER
);

INSERT INTO games (name, year, genre, rating)
VALUES
('Super Mario Bros.', 1985, 'Platform', 5),
('Tetris', 1984, 'Puzzle', 5),
('Pac-Man', 1980, 'Arcade', 4),
('Sonic the Hedgehog', 1991, 'Platform', 4);

-- 전체 게임 검색
SELECT * FROM games;

-- 게임 이름으로 검색
SELECT * FROM games
WHERE name = 'Tetris';

-- 장르로 검색
SELECT * FROM games
WHERE genre = 'Platform';

-- 평점이 5점인 게임 검색
SELECT name, year, rating
FROM games
WHERE rating = 5;
```
# 4. 예상 실행 결과  
ID	게임 이름	출시 연도	장르	평점  
1	Super Mario Bros.	1985	Platform	5  
2	Tetris	1984	Puzzle	5  
3	Pac-Man	1980	Arcade	4  
4	Sonic the Hedgehog	1991	Platform	4  
