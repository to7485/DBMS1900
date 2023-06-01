## TCL(Transaction Control Language): (DML을 위한 명령어) 트랜젝션 제어 언어
- 트랜잭션 : 데이터베이스의 작업을 처리하는 논리적 연산 단위[분리될 수 없는 최소 단위]
    - 어떠한 작업을 하기 위해서는 Select ,Insert, Update 를 사용해야 하는데 이걸 하나의 작업 단위라고 한다.


### 트랜잭션의 특성
- 원자성(Atomicity) : 원자와 같이 데이터베이스 연산들이 나눌수도, 줄일수 없는 하나의 유닛으로서 취급됨.
    - 트랜잭션에 정의된 연산들은 모두 성공적으로 실행되던지 아니면 전혀 실행되지 않은 상태로 남아야 함

```SQL
10,000원이 든 계좌 A에서 0원이 든 계좌 B로 돈 10,000원을 입금한다.
-- query1
UPDATE 계좌
SET 잔액 = 0
WHERE 계좌번호 = A;
-- query2
UPDATE 계좌
SET 잔액 = 10000
WHERE 계좌번호 = B;
```
  
  
- 일관성(Consistency) : 데이터베이스의 트랜잭션이 제약조건, cascades, triggers를 포함한 정의된 모든 조건에 맞게 데이터 값이 변경됨
    - 트랜잭션 실행 전의 데이터베이스 내용이 잘못되지 않으면 트랜잭션 실행 후에도 데이터베이스 내용이 잘못되면 안됨

- 고립성(Isolation) : 특정 DBMS에서 다수의 유저들이 같은 시간에 같은 데이터에 접근하였을 때 수행중인 트랜잭션이 완료될 때 까지<br> 다른 트랜잭션의 요청을 막음으로서 데이터가 꼬이는걸 방지한다.
    - 트랜잭션 실행 도중에 다른 트랜잭션의 영향을 받아 잘못된 결과를 만들면 안됨

- 영속성,지속성(Durability) : 트랜잭션 실행이 성공적일 때, 그 트랜잭션이 갱신한 데이터베이스 내용은 영구적으로 저장됨.


## TCL의 종류

- COMMIT : DML로 변경된 데이터를 데이터베이스에 적용할 때 사용
  - INSERT,UPDATE,DELETE문 등을 사용한 후 변경 작업을 데이터베이스에 반영하기 위해 사용
  - COMMIT문 사용시 이전 데이터는 영원히 지워짐
  - COMMIT문 사용시 모든 사용자가 변경된 데이터 확인 가능
  - SQLServer는 기본적으로 AUTO COMMIT모드이므로 자동으로 COMMIT을 수행한다.<br> (DML문이 성공하면 자동으로 COMMIT을 실행한다.)
  - COMMIT 실행시 하나의 트랜잭션 과정을 종료<br> → COMMIT이 실행되기 전에 다른 사용자가 완료되지 않은 데이터를 보거나 변경할 수 없음<br>

사용법 : COMMIT;<br>

- ROLLBACK : DML로 변경된 데이터를 변경 이전 상태로 되돌릴 때 사용
    - 데이터에 대한 변경사항 취소
    - ROLLBACK문 사용시 이전 데이터 다시 재저장 -> COMMIT되지 않은 상위 트랜잭션을 모두 ROLLBACK시킴
    - ROLLBACK문 사용시 관련 행의 잠금(LOCKING)이 풀려 다른 사용자들이 데이터 변경 가능
    - SQL Server는 기본적으로 AUTO COMMIT모드이므로 자동으로 COMMIT, 오류가 발생하면 자동으로 ROLLBACK처리

사용법 : ROLLBACK;<br>

- SAVEPOINT : 오류 복구 처리에 효과적인 방법 -> 전체 트랜잭션을 ROLLBACK하지 않고도 오류 복귀 가능
    - 효과적으로 오류 복구 처리를 위해 저장점을 저장할 때 사용<br> -> 전체 트랜잭션을 ROLLBACK하지 않고 현시점에서 SAVEPOINT까지 일부 트랜잭션만 오류 복귀 가능
    - 복잡한 대규모 트랜잭션에서 에러가 발생했을 때 주로 사용
    - 복수의 저장점 정의 가능
    - 저장점까지 롤백할 때는 "ROLLBACK TO 저장점;" 사용

사용법 : SAVEPOINT 세이브포인트이름;<br>
ROLLBACK TO 세이브포인트이름;<br>

## day05 스크립트 생성

```SQL
현재는 자동으로 COMMIT을 해주는 상태이다. 위의 작업표시줄에 T모양을 눌러 손바닥 모양으로 바꾼다. 내가 직접 트랜젝션을 관리하겠다는 의미

SELECT * FROM PLAYER;
COMMIT;

SELECT는 데이터가 바뀌는게 없기 때문에 롤백을 쓸일이 거의 없지만
데이터가 바뀌는 INSERT, UPDATE, DELETE는 롤백을 쓰는 경우가 있다.

--PLAYER 테이블에서 TEAM_ID가 'K01'인 선수 이름을 내 이름으로 바꾸기
UPDATE PLAYER
SET PLAYER_NAME ='이현준'
WHERE TEAM_ID='K01';

SELECT * FROM PLAYER WHERE TEAM_ID = 'K01';
이름이 전부 이름으로 바뀐걸 확인하고 잘못 바꿨다고 생각하면 위에 롤백 버튼을 누르면 원래대로 돌아간다.

--PLAYER 테이블에서 POSITION이 'MF'인 선수 삭제하기
DELETE FROM PLAYER
WHERE POSITION = 'MF';

SELECT * FROM PLAYER
WHERE POSITION = 'MF';

롤백

--PLAYER 테이블에서 HEIGHT가 180이상인 선수 삭제하기
DELETE FROM PLAYER
WHERE HEIGHT >= 180;

SELECT * FROM
WHERE HEIGHT >= 180;
```
## ALIAS 별칭
- AS(ALIAS) : 별칭	컬럼이 너무 길다면 별명을 쥐서 대신 사용할 수 있음
    - SELECT절 : AS 뒤에 별칭 작성, 한칸 띄우고 작성
    - FROM절 : 한 칸 띄우고 작성

```SQL
SELECT PLAYER_ID AS "선수 번호" FROM PLAYER
SELECT PLAYER_ID "선수 번호" FROM PLAYER;
SELECT PLAYER_ID "선수 번호", PLAYER_NAME "선수 이름" FROM PLAYER;

--PLAYER 테이블에서 BACK_NO "등 번호"로, NICKNAME을 "선수 별명" 으로 바꿔서 검색
SELECT BACK_NO "등 번호", NICKNAME "선수 별명" FROM PLAYER;


두개 이상의 테이블에서 각각의 컬럼을 조회하려고 한다면 어떤테이블에서 온 컬럼인지 확실하게 적어줘야 한다.
SELECT PLAYER.TEAM_ID, TEAM.TEAM_ID FROM PLAYER, TEAM;

근데 테이블명이 길기 때문에 FROM절에서 별칭을 준다.
SELECT P.TEAM_ID, T.TEAM_ID FROM PLAYER P , TEAM T;

SELECT * FROM STADIUM;
SELCT * FROM TEAM;

SELECT T.TEAM_ID "팀 아이디", S.ADDRESS "주소", T.TEL FROM STARDIUM S, TEAM T;

커밋하기 귀찮으니까 다시 오토커밋으로 바꿔주세요.
```

## CONCATENATION(연결)
- CONCATENATION(연결) : ||

```SQL
--누구누구의 별명은 뭐뭐 이다.
SELECT PLAYER_NAME||'의 별명은'|| NICKNAME||'이다' FROM PLAYER

SELECT * FROM PLAYER
--00의 포지션은 00이다.
SELECT E_PLAYER_NAME||'의 포지션은'||"POSITION"||'이다' 작전회의 FROM PLAYER;
```
## LIKE(유사검색)
- LIKE : 포함된 문자열의 값을 찾음, 문자의 개수도 제한을 줄 수 있음.
- % : 모든값
- _ : 하나의 값
ex)'M%' : M으로 시작하는 모든 값<br>
ex)'%a' : a로 끝나는 모든 값<br>
ex)'%a%' : 값의 어디든 a를 포함하고 있는 모든 값(앞,뒤,중간 어딘가에 존재하면 된다.)<br>
ex)'M_ _ _ _' : M으로 시작하는 값들 중 전체 길이가 5글자인 값<br>

만약 데이터에 _가 들어있는 데이터를 찾고싶다면 %\\_% 이렇게 써야 한다.<br>

```SQL
예)사원테이블에서 사원들의 이름 중 M으로 시작하는 사원의 정보를 사번, 이름, 직종 순으로 출력
select employee_id, first_name, job_id from employees where first_name LIKE 'M%';
%는 뒤로는 뭐가 몇글자가 오든 상관 안하겠다는 의미

문)사원테이블에서 이름이 d로 끝나는 사원의 사번, 이름 직종을 출력
select employee_id ,first_name, job_id from employees where first_name LIKE'%d';

예)이름의 어디라도 a가 포함되어 있는 사원의 정보를 이름, 직종 순으로 출력
select first_name, job_id from employees where first_name LIKE'%a%';

예)이름의 첫글자가 M이면서 총 7글자의 이름을 가진 사원의 정보를 사번, 이름순으로 검색
select employee_id, first_name from employees where first_name like 'M______'

문제) 사원테이블에서 이름의 세번째 글자에 a가들어가는 사원들의 정보를 사번, 이름 순으로 출력
select employee_id, first_name from employees where first_name like '_ _ a%';

문제) 이름에 소문자o가 들어가면서 이름이 a로 끝나는 사원들의 정보를 이름, 급여 순으로 조회
-- o와 a가 동시에 존재하는 이름을 검색, 단 o다음에 a가존재해야 한다.
select first_name, salary from employees where first_name like '%o%a';

문제) 이름이 H로 시작하면서 6글자 이상인 사원들의 정보를 사번, 이름 순으로 조회
select employee_id ,first_name from employees where first_name like'H_ _ _ _ _%';

문제) 사원테이블에서 이름에 s자가 포함되어 있지 않은 사원들만 사번, 이름으로 검색하시오
select employee_id, first_name from employees where first_name not like'%s%';

SELECT *FROM TEAM WHERE TEAM_NAME LIKE'%천마';
SELECT * FROM PLAYER WHERE PLAYER_NAME LIKE '김%';
SELECT * FROM PLAYER WHERE PLAYER_NAME LIKE '김_';

--PLAYER 테이블에서 이씨 찾기
SELECT * FROM PLAYER WHERE PLAYER_NAME LIKE '이%';
SELECT * FROM PLAYER WHERE PLAYER_NAME LIKE '이_';


--PLAYER 테이블에서 김씨와 이씨 찾기
SELECT * FROM PLAYER WHERE PLAYER_NAME LIKE '이%'OR PLAYER_NAME LIKE '김%';
SELECT * FROM PLAYER WHERE PLAYER_NAME LIKE '이_' OR PLAYER_NAME LIKE '김_';

--PLAYER 테이블에서 이씨가 아닌 사람 찾기
--NOT
SELECT * FROM PLAYER WHERE NOT PLAYER_NAME LIKE '이%';

--PLAYER 테이블에서 세자리 이름을 가진 김씨가 아닌 사람
SELECT * FROM PLAYER WHERE PLAYER_NAME LIKE '김__'; 김반,김석은 조회됨
```
