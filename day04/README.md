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
   
### 조건절
원하는 자료를 검색하기 위한 조건절<br>
WHERE 절에서는 결과를 제한하기 위한 조건을 기술할 수 도 있다.<br>
WHERE절은 조회하려는 데이터에 특정 조건을 부여할 목적으로 사용하기 때문에 FROM절 뒤에 오게 된다.

WHERE 절의 조건식은 아래 내용으로 구성된다.<br>
- 컬럼명(보통 조건식의 좌측에 위치)
- 비교 연산자
- 문자,숫자,표현식(보통 조건식의 우측에 위치)
- 비교 칼럼명(JOIN 사용시)

조건식 : 참 또는 거짓 둘 중 하나의 결과가 나오는 식<br>
X > 10
- WHERE 조건식
- \>, < : 초과, 미만
- \>=, <= : 이상, 이하
- = : 같다
- < >, !=, ^= : 같지않다.
- AND : 두 조건식이 모두 참이면 참
- OR : 둘 중 하나라도 참이면 참

```
SELECT * FROM FLOWER; *는 컬럼명부터 검색을 하기 때문에 실제로 컬럼을 쳐주는게 속도가 훨씬 빠르다. 지금은 데이터가 별로 없어서 차이가 안나지만 나중에 백만, 천만개 있으면 속도 차이가 분명히 난다.
SELECT FLOWERNAME,FLOWERCOLOR, FLOWERPRICE FROM FLOWER;

INSERT INTO FLOWER
(FLOWERNAME, FLOWERCOLOR, FLOWERPRICE)
VALUES('장미','RED',3000);

INSERT INTO FLOWER -> 영역 드래그 하고 CTRL+ALT + ↓ 누르면 복사됨
(FLOWERNAME, FLOWERCOLOR, FLOWERPRICE)
VALUES('장미','RED',3000); -> 똑같은 값을 넣으면 오류남

INSERT INTO FLOWER
(FLOWERNAME, FLOWERCOLOR, FLOWERPRICE)
VALUES('해바라기','YELLOW',5000);

실제로 고객이 값을 넣고 컨트롤 엔터를 치겠어요? 버튼을 클릭하거나 바코드를 대거나 등록하기 버튼을 누른다거나. 자동으로 주입이 되겠죠 이런것들을 필요로 하면은 사실 데이터베이스 하나만으로는 뭔갈 만들기가 힘들다. 데이터베이스는 하나의 도구일 뿐이고 입력받거나 하는건 어플리케이션 ,모바일,웹 이런걸 필요로 한다.

INSERT INTO FLOWER
(FLOWERNAME, FLOWERCOLOR, FLOWERPRICE)
VALUES('할미꽃','WHITE',9000);

-- 추가 : 부모와 자식 중 부모테이블의 값을 먼저 추가해야 한다.
부모에 값이 없는데 어떻게 외래키로 가져다 쓸꺼냐

INSERT INTO POT
(POTID, POTCOLOR, POTSHAPE, NAME)
VALUES('20210505001','WHITE','물레방아','장미');

INSERT INTO POT
(POTID, POTCOLOR, POTSHAPE, NAME)
VALUES('20210505002','BLACK','타원형','해바라기');

INSERT INTO POT
(POTID, POTCOLOR, POTSHAPE, NAME)
VALUES('20210505003','RED','사각형','할미꽃');

INSERT INTO POT
(POTID, POTCOLOR, POTSHAPE, NAME)
VALUES('20210505004','RED','타원형','할미꽃');


--삭제 : 부모와 자식중 자식테이블에서 참조하는 값들을 먼저 삭제해야 한다.
DELETE FROM POT WHERE NAME = '장미';

DELETE FROM FLOWER WHERE FLOWERNAME = '장미';

```
ERD 보여주며 부모와 자식 관계를 다시한번 구분시켜주자
- 부모 : ◇ ◇가 출발해서 자식에 닿은 것
- 자식 : ● ●가 출발해서 부모에 닿은 것

### DELETE와 TRUNCATE
- DELETE는 복구가 가능하다
- TRUNCATE는 복구가 불가능하다 -> 퇴사준비 ㄱㄱ

```
주인과 펫에 데이터 넣어보기

SELECT * FROM PET;
SELECT * FROM OWNER;

INSERT INTO OWNER ( ID, NAME, AGE, ADDRESS)
VALUES('20201005001', '이현준', 20, '서울시 마포구');

INSERT INTO OWNER ( ID, NAME, AGE, ADDRESS)
VALUES('20201005002', '홍길동', 21, '경기도 연천군');

INSERT INTO PET ( PIN, NAME, AGE, OWNERID,GENDER)
VALUES('8202001', '뽀삐', 4, '20201005002','W');

INSERT INTO PET VALUES('8202002', '바둑이', 3, '20201005001','M');

INSERT INTO PET VALUES('8202003', '노루', 10, '20201005001','M');

한명이 두마리 동물을 데리고 왔던 기록이 있다.

SELECT * FROM PET WHERE GENDER='M';

DELETE FROM OWNER
WHERE NAME = '바둑이';

INSERT INTO PET VALUES('8202002', '바둑이', 3, '20201005001','M');
바둑이 다시 추가

UPDATE OWNER
SET NAME='이순신'
WHERE ID = '20201005002';

```
### 데이터 내보내기
조회된 시트를 드래그로 잡고 우클릭 > 결과 내보내기<br>
CSV > 엑셀파일<br>
HTML > 웹<br>

NEXT 눌러주고 > 디렉토리 RESOURCE 폴더로 설정 ENCODING 방식 UTF-8 > 확인<br>

CSV, HTML, SQL 방식으로 각각 저장해보기<br>

SQL파일 > Data rows per statement Insert 하나당 하나의 행을 정할껀지 물어보는 규칙<br>
편집하면 메모장으로 편집 가능 다른이름으로 저장 ANSI 이름 q1로 변경<br>
cmd > sqlplus hr/hr<br>

truncate table pet;<br>
 
### 파일 들여오기
@ 파일 드래그 앤 드랍<br>

줄이 추가됐다고 뜨는데 실제 ide에서 select 문에서 조회를 해보면 안뜬다.<br>
임시 저장소에 저장이 되어있기 때문<br>

우리가 친 명령어가 정확한지 한번더 확인을 하려고<br>

commit; 명령어를 입력하면 완벽하게 반영이 된다.<br>

배포한 파일을 cmd를 통해서 hr 계정에 추가를 해보자<br>
cmd > sqlplus hr/hr<br>

@ 드래그앤 드랍 a1~4 다 추가하고 오류가 나면 무시하기<br>

ide와서 테이블 새로고침<br>
```
SELECT * FROM PLAYER;

--PLAYER 테이블에서 TEAM_ID가 'K01'인 선수 검색
SELECT * FROM PLAYER WHERE TEAM_ID = 'KO1';

--PLAYER 테이블에서 TEAM_ID가 'KO1'이 아닌 선수 검색
SELECT * FROM PLAYER WHERE TEAM_ID != 'KO1';

--PLAYER 테이블에서 WEIGHT가 70이상이고 80이하인 선수 검색
SELECT * FROM PLAYER WHERE WHEIGHT >= 70 AND WEIGHT <= 80;

--PLAYER 테이블에서 WEIGHT가 70이상이고 80이하인 선수 검색
SELECT * FROM PLAYER WHERE WEIGHT BETWEEN 70 AND 80;

--PLAYER 테이블에서 'K03'이고, HEIGHT가 180인 선수
SELECT * FROM PLAYER
WHERE TEAM_ID = 'K03' AND HEIGHT < 180;

--PLAYER 테이블에서 TEAM_ID가 'KO6'이고 NICKNAME이 '제리'인 선수 검색
SELECT * FROM PLAYER WHERE TEAM_ID = 'KO6' AND NICKNAME = '제리';

--PLAYER 테이블에서 HEIGHT가 170이상이고 WEIGHT가 80이상인 선수 이름 검색
SELECT * FROM PLAYER WHERE HEIGHT >= 170 AND WEIGHT >= 80;

--STADIUM 테이블에서 SEAT_COUNT가 30000초과이고 41000 이하인 경기장 검색
SELECT * FROM STADIUM 처음보는 테이블이라면 안에 무슨내용이 담겨있는지 한번 조회해보기
SELECT * FROM STADIUM WHERE SEAT_COUNT > 30000 AND SEAT_COUNT <=41000;

--PLAYER 테이블에서 TEAM_ID가 'K02'이거나 'K07'이고 포지션은 'MF'인 선수 검색
SELECT * FROM PLAYER WHERE TEAM_ID ='K02'OR TEAM_ID ='KO7' AND “POSITION = 'MF';
검색이 잘 안되는걸 볼 수 있다. OR, AND 중에서 AND가 먼저 계산된다.
SELECT * FROM PLAYER WHERE (TEAM_ID ='K02'OR TEAM_ID ='KO7') AND “POSITION = 'MF';

SELECT * FROM PLAYER WHERE TEAM_ID  IN('K02','KO7') AND “POSITION = 'MF';
 
예) 사원테이블에서 first_name(이름),job_id(직종),salary(급여)를 조회해보자!

select first_name, job_id, salary from employees;

select employee_id, first_name, job_id, salary, commission_pct,
-- *를 통한 연산을 수행
 salary*commission_pct as comm //as를 빼고 한칸 띄우고 써도 별칭으로 써진다.
 from employees;

예) 사원테이블에서 급여가 10000이상인 사원들의 정보를 사번, 이름, 급여 순으로 출력

select employee_id, first_name, salary from employees where salary >=10000;

where절은 from 뒤에 있음

예) where절에 문자를 비교하고 싶다면
where first_name='Michael';
*==이 아님 주의, 반드시 홑따옴표로 쓴다.

예) 사원테이블에서 이름이 Michael인 사원의 사번, 이름을 조회
SELECT employee_id, first_name FROM employees WHERE first_name='Michael';

문제) 사원테이블에서 직종이 IT_PROG인 사원들의 정보를 사번, 이름, 직종, 급여 순으로 출력

select employee_id,first_name,job_id,salary from employees where job_id='IT_PROG';

예) 사원테이블에서 급여가 10000이상이면서 13000이하인 사원의 정보를 이름, 급여 순으로 조회
안타깝게도 데이터베이스에서는 && 나 ||를 쓸수가 없다 and , or로 작성을 해야한다.
SELECT first_name, salary FROM employees WHERE salary >= 10000 AND salary <= 13000;


문제) 사원테이블에서 입사일이 05년9월21일인 사원의 정보를 사번, 이름, 입사일 순으로 출력

select employee_id, first_name, hire_date from employees where hire_date='09/21/2005';


예) 사원테이블에서 입사일이 05년9월21일 이후에 입사한 사원의 정보를 사번, 이름, 입사일 순으로 출력

select employee_id, first_name, hire_date from employees WHERE hire_date>=TO_DATE('2005-09-21','YYYY-MM-DD');


데이터베이스는 날짜의 크기비교가 가능하다.

예) 사원테이블에서 06년도에 입사한 사원들의 정보를 사번, 이름, 직종, 입사일 순으로 출력

select employee_id, first_name, job_id, hire_date from employees where hire_date>='01/01/2006' 
and hire_date<='12/31/2006'

문제) 사원테이블에서 급여가 4000~ 8000사이의 사원의 이름,직종,급여를 출력
select first_name, job_id, salary from employees where salary >=4000 and salary<=8000;


문제) 사원테이블에서 직종이 SA_MAN이거나 IT_PROG인 사원들의 모든정보를 출력하시
select * from employees where job_id = 'SA_MAN' or job_id='IT_PROG';


문제) 사워테이블에서 급여가 2200, 3200, 5000, 6800를 받는 사원들의 정보를 사번, 이름 직종, 급여 순으로 출력
select employee_id, fist_name, job_id, salary from employees where salary='2200' or salary = '3200' or salary='5000' or salary='6800';

```


