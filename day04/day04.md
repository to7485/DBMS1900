## DML(Data Manipulation Language) 데이터 조작어
-SELECT:조회(검색)
   -SELECT 컬럼명1,컬럼명2... FROM 테이블명
   -WHERE 조건식; 처음보는거지만 밑에서 설명하겠다.

-INSERT:추가
   -INSERT INTO 테이블명(컬럼명1, 컬럼명2...) VALUES(값1,값2...) DEFAULT 쓸 때
   -INSERT INTO 테이블명 VALUES(값1,값2,....) 무조건 만든 컬럼 개수만큼 값을 넣어야함

-UPDATE:수정
   UPDATE 테이블명<br>
   SET 기존컬럼명 = 새로운 값<br>
   WHERE 조건식 조건을 달지 않으면 테이블 전체가 바뀌어 버린다.<br>

-DELETE:삭제	TRUNCATE는 다 날려버리는건데 DELETE는 1개씩 지운다.
   DELETE FROM 테이블명	행 1개가 통째로 날아감<br>
   WHERE 조건식<br>

