## 집계함수
집계함수는 많이 사용이 된다.<br>
회원가입이나 로그인을 할 때도 쓰인다.<br>
중복검사를 할 때, 댓글의 전체 개수를 세야 할 때, 등 데이터베이스 부분에서 집계가 가능하다.<br>

- 집계 함수(NULL은 포함하지 않는다) : 여러 개의 값을 하나의 값으로 집계하여 나타낸다.
 좋을 수도 있으나 평균같은걸 낼 때 조심해야 한다. 전체개수에서 NULL의 개수가 빠져버리기 때문에 NULL인것도 전체 개수에 포함할건지 말건지 결정을 잘 해야한다.

- WHERE 절에서는 사용할 수 없다.

|함수|기능|
|---|------|
|COUNT|행들의 개수를 반환|
|MIN|행들의 최소값을 반환|
|MAX|행들의 최대값을 반환|
|SUM|행들의 합계를 반환|
|AVG|행들의 평균을 반환한다.|
|STDDEV|행들의 표준편차를 반환한다.|
|VARIANCE|행들의 분산을 반환한다.|

```SQL
SELECT AVG(HEIGHT), MAX(HEIGHT),MIN(HEIGHT),SUM(HEIGHT),COUNT(HEIGHT) FROM PLAYER.

--PLAYER 테이블에서 HEIGHT 개수 검색(NULL 포함해서 COUNT 하기) NULL이라는 값을 포함하려면 NULL 이면 안된다 즉 NVL을 사용해야 한다.

SELECT COUNT(HEIGHT) FROM PLAYER;--> 447개
SEECT * FROM PLAYER; --> 480개
SELECT COUNT(NVL(HEIGHT,0)) FROM PLAYER;

**일반적으로 그룹함수와 일반컬럼을 함께 쓸 수 없다.
select count(*), first_name from employees;
 
예) 사원테이블의 전체 사원수를 출력
select COUNT(*) FROM EMPLOYEES;

예) 사원테이블에서 보너스를 받는 사원의 수를 출력
select count(commIssion_pct) from employees;

문제) 사원테이블에서 직종이 SA_REP인 사원들의 평균급여, 급여최고액, 급여최저액, 급여의 총 합계를 출력하시오.
select AVG(salary) AVG,MAX(salary) MAX,MIN(salary) MIN,SUM(salary) SUM
from employees
WHERE JOB_ID = 'SA_REP';

예) 부서의 갯수를 출력
select count(DISTINCT department_id)from employees;

DISTINCT : 중복된 값은 세지 않는다.

문) 부서번호가 80번인 부서의 사원들의 급여의 평균을 출력

SELECT ROUND(AVG(SALARY), 1) AVG FROM EMPLOYEES WHERE DEPARTMENT_ID = 80;

예) 관리자의 수를 출력
distinct : 중복된 값을 제외
select count(distinct manager_id) from employees;


문제) 사원테이블에서 50번에 속하는 사원들의 급여의 최대값과 최소값을 출력하세요
select Max(salary),MIN(SALARY), FROM EMPLOYEES WHERE DEAPRTMENT_ID = 50;
```

### 그룹화(GROUP BY)
- 특정테이블에서 소그룹을 만들어 결과를 분산시켜 얻고자 할 때
- GROUP BY : ~별(예 : 포지션별 평균 키)

```SQL
예) 각 부서별 급여의 평균과 총 합을 출력

SELECT DEPARTMENT_ID, COUNT(*), AVG(SALARY), SUM(SALARY) FROM EMPLOYEES GROUP BY DEPARTMENT_ID;

GROUP BY로 소그룹을 지정하고 싶은 컬럼이 있는데 이걸 SELECT에서도 사용하고 싶으면 GROUP BY에 작성해놓은 컬럼명만 SELECT에 추가할 수 있다.


부서가 같아도 직종이 다른 경우가 있기 때문에 좀더 세부적인 결과물을 가져올수 있는게 GROUP BY다.

문) 각 직종별 인원수를 출력

SELECT JOB_ID, COUNT(*) FROM EMPLOYEES GROUP BY JOB_ID;

문) 각 직종별 급여의 합을 출력

SELECT JOB_ID, SUM(SALARY) FROM EMPLOYEES GROUP BY JOB_ID;
괄호안에 어떤걸 넣어야 하는지 알면 난이도가 많이 내려간다.

문) 부서별로 가장 높은 급여를 조회

SELECT DEPARTMENT_ID, MAX(SALARY) FROM EMPLOYEES GROUP BY DEPARTMENT_ID;

문) 부서별 급여의 합계를 조회
SELECT DEPARTMENT_ID, SUM(SALARY)
FROM EMPLOYEES 
GROUP BY DEPARTMENT_ID;
```
## 그룹함수

```SQL
CREATE TABLE 월별매출 (
    상품ID VARCHAR2(5),
    월 VARCHAR2(10),
    회사 VARCHAR2(10),
    매출액 INTEGER );
    
INSERT INTO  월별매출 VALUES ('P001', '2019.10', '삼성', 15000);
INSERT INTO  월별매출 VALUES ('P001', '2019.11', '삼성', 25000);
INSERT INTO  월별매출 VALUES ('P002', '2019.10', 'LG', 10000);
INSERT INTO  월별매출 VALUES ('P002', '2019.11', 'LG', 20000);
INSERT INTO  월별매출 VALUES ('P003', '2019.10', '애플', 15000);
INSERT INTO  월별매출 VALUES ('P003', '2019.11', '애플', 10000);

SELECT * FROM 월별매출;
```

- ROLLUP
    - ROLLUP(A) : A 그룹핑 -> 합계
    - ROLLUP(A,B) : A,B그룹핑 -> A소계/합계
    - ROLLUP(A,B,C) : A,B,C그룹핑 -> (A소계,B소계)/합계
 
```SQL
SELECT 상품ID, 월, SUM(매출액) AS 매출액
FROM 월별매출
GROUP BY ROLLUP(상품ID, 월);
```

![image](https://github.com/to7485/DBMS1900/assets/54658614/0cce5aea-3337-477c-94c6-f6e60754f04b)

- CUBE
    - CUBE(A) : A그룹핑 -> 합계
    - CUBE(A,B) : A,B그룹핑/A그룹핑/B그룹핑 -> A소계,B소계/합계
    - CUBE(A,B,C) : A,B,C그룹핑/A,B그룹핑/A,C그룹핑/B,C그룹핑/A그룹핑/B그룹핑/C그룹핑 -> (A소계,B소계),(A소계),(B소계)/합계

```SQL
SELECT 상품ID, 월, SUM(매출액) AS 매출액
FROM 월별매출
GROUP BY CUBE(상품ID, 월);
```

![image](https://github.com/to7485/DBMS1900/assets/54658614/bf9caa9e-af05-48f6-9c92-356542b60073)

- GROUPING SETS
    - GROUPING SETS(A,()) : A그룹핑 -> 합계
    - GROUPING SETS(A,B,()) : A그룹핑/B그룹핑 -> 합계
 
 ```SQL
SELECT 상품ID, 월, SUM(매출액) AS 매출액
FROM 월별매출
GROUP BY GROUPING SETS(상품ID, 월);
 ```
 
 ![image](https://github.com/to7485/DBMS1900/assets/54658614/0d56776f-7458-4702-894d-50627e57ee79)


## HAVING절
그룹함수에 대한 조건처리가 필요할 때 사용하는 Query<br>
조건식을 사용할 때 그룹함수가 필요하다면 반드시 having 키워드를 사용해야 한다!!!!!<br>

```SQL
예) 각 부서의 급여의 최대값, 최소값, 인원수를 출력하자 단, 급여의 최대값이 8000이상인 결과만 보여줄 것.

select department_id, MAX(salary),MIN(salary),COUNT(*)
from employees 
group by department_id
having MAX(salary)>=8000; -> 그룹함수가 사용된 조건이라면 WHERE 이 아닌 HAVING을 써야한다.
    --WHERE MAX(SALARY) >= 8000;
문제) 각 부서별 인원수가 20명 이상인 부서의 정보를 부서번호, 급여의 합, 급여의 평균, 인원수 순으로 출력 단, 급여의 평균은 소수점 2자리 반올림

SELECT DEPARTMENT_ID, SUM(SALARY), ROUND(AVG(SALARY),2), COUNT(*)
FROM EMPLOYEES
GROUP BY DEPARTMENT_ID
HAVING COUNT(*) >= 20;

문제) 부서별, 직종별로 그룹화 하여 결과를 부서번호, 직종, 인원수 순으로 출력, 단, 직종이 'MAN'으로 끝나는 경우만 출력
SELECT DEPARTMENT_ID,JOB_ID,COUNT(*)
FROM EMPLOYEES
WHERE JOB_ID LIKE '%MAN'-> HAVING으로 해도 값이 나오긴 하지만 일반컬럼이기 때문에 WHERE쓰는게 좋다.
GROUP BY DEPARTMENT_ID,JOB_ID;

문제)각 부서별 평균 급여를 소수점 한자리까지 버림으로 출력 단, 평균 급여가 10000미만인 그룹만 조회해야 하며
 부서번호가 50이하인 부서만 조회 
SELECT DEPARTMENT_ID, TRUNC(AVG(SALARY),1)
FROM EMPLOYEES
WHERE DEPARTMENT_ID <= 50
GROUP BY DEPARTMENT_ID
HAVING AVG(SALARY) < 10000;

문제) 각 부서별 부서번호, 급여의 합, 평균, 인원수 순으로 출력 단, 급여의 합이 30000이상인 경우만 출력해야 하며, 급여의 평균은 소수점 2자리에서 반올림 하시오.
select department_id, SUM(salary), ROUND(AVG(salary),2),COUNT(*) 
from employees 
group by department_id 
having SUM(salary)>=30000;
```

--SELECT ??? FROM ???<br>
--WHERE 조건식<br>
--GROUP BY 컬럼명<br>
--HAVING 조건식<br>

### 어떨때 HAVING을 쓰고 어떨 때 WHERE 쓰는지 알아보도록 하자

```SQL
--PLAYER 테이블에서 각 포지션별로 검색
SELECT "POSITION" FROM PLAYER GROUP BY "POSITION" HAVING "POSITION" IS NOT NULL;

--WHERE절에서 조건을 처리할 수 있다면 반드시 WHERE절에서 먼저 처리해준다.
SELECT "POSITION" FROM PLAYER
WHERE "POSITION" IS NOT NULL
GROUP BY "POSITION";

--PLAYER 테이블에서 각 포지션별로 몸무게가 80이상인 선수들의 평균 키가 180이상인 포지션,평균키 검색
SELECT * "POSITION",AVG(HEIGHT) FROM PLAYER
WHERE WEIGHT >= 80 AND AVG(HEIGHT) >= 180 WHERE절에서는 집계함수를 사용할 수 없다.
GROUP BY “POSITION”
HAVING AVG(HEIGHT)>=180;
```
### HAVING과 WHERE의 차이점
- WHERE은 기본적인 조건절로서 우선적으로 모든 필드를 조건에 둘 수 있다.
- HAVING은 GROUP BY된 이후 소그룹화된 새로운 테이블에 조건을 준다.

## 정렬
- 검색된 결과의 행을 정렬할 때는 ORDER BY절을 사용한다.
- 정렬 방법에는 오름차순과 내림차순 두가지가 있다.
	- 오름차순(ASCENDING) : 작은값부터 큰값으로 정렬
	- 내림차순(DESCENDING) : 큰값부터 작은값으로 정렬

```SQL
SELECT * FROM PLAYER ORDER BY HEIGHT;
SELECT * FROM PLAYER ORDER BY HEIGHT DESC;
SELECT * FROM PLAYER ORDER BY 12 DESC; -> 12번째 컬럼으로 정렬하겠다.

-- 컬럼의 개수를 제한하여 검색했다면 조회된 컬럼의 순서대로 정렬을 해야 한다.
SELECT HEIGHT, weight
FROM player 
ORDER BY 2 desc; -> 여기서 2는 몸무게 컬럼이 된다.

--PLAYER 테이블에서 키순, 몸무게순(오름차순)으로 검색
--첫번째 컬럼에 값이 같으면 두번째 정렬을 한다.
SELECT HEIGHT, weight FROM player ORDER BY height ASC, weight ASC;

--NULL이 아닌 값만 검색
SELECT HEIGHT, weight
FROM player 
WHERE height IS NOT NULL AND weight IS NOT null 
ORDER BY height ASC, weight asc;

예) 부서별, 직종별로 그룹을 나눠서 인원수를 출력 단, 부서번호가 낮은순으로 정렬하시오.

SELECT DEPARTMENT_ID, JOB_ID, COUNT(*) 
FROM EMPLOYEES 
GROUP BY DEPARTMENT_ID, JOB_ID 
ORDER BY DEPARTMENT_ID;

두번째에 중복이 있다면 3번째 컬럼으로 정렬

사실 정렬은 좀 느려서 데이터가 적을 때 주로 사용을 한다.
```
 
## CASE문
- CASE WHEN THEN ELSE END 어떠한 조건에 맞춰 값을 출력해주는 
- CASE WHEN 조건식 THEN '참 값' ELSE '거짓 값' END

데이터의 값을 WHEN의 조건과 차례대로 비교한 후 일치하는 값을 찾아 THEN 뒤에 있는 결과값을 리턴합니다.<br>
 
ELSE는 일치하는 값이 없을 때 선택할 디폴트값인데 필요 없으면 생략할 수 있습니다.<br>
WHEN의 값 중 일치하는 것이 없고 ELSE도 없다면 CASE 문은 NULL을 리턴합니다.<br>


```SQL
--EMP 테이블에서 SAL 3000이상이면 HIGH 1000이상이면 MID, 다 아니면 LOW
SELECT * FROM EMP;
SELECT ENAME 사원명, SAL 급여,
CASE
		WHEN SAL >= 3000 THEN 'HIGH'
		WHEN SAL >= 1000 THEN 'MID'
		ELSE 'LOW'
END
FROM EMP;

만약 WHEN 뒤의 조건 중 참인 것이 두 개 이상이라면 먼저 만나는 조건을 선택합니다.
CASE 문은 전체적으로 하나의 값으로 평가되며 값이 올 수 있는 모든 곳에 올 수 있습니다.
따라서 SELECT 문의 필드 목록에 사용할 수도 있고 WHERE 절의 조건문에도 사용할 수 있습니다.


SELECT ROUND(AVG(CASE JOB_ID WHEN 'IT_PROG' THEN SALARY END),2) 평균급여
FROM EMPLOYEES;
CASE 와 WHEN 사이에 비교하고자 하는 Column 을 넣고
WHEN 과 THEN 사이에 비교하고자 하는 값을 넣어서 비교하는 방법입니다.
 
--STADIUM 테이블에서 SEAT_COUNT 0DLTKD 30000이하면 'S'
--30001이상 50000이하면 'M' 다 아니면 'L'
--중첩 케이스문
SELECT STADIUM_NAME 경기장, SEAT_COUNT 좌석수,
	CASE
		WHEN SEAT_COUNT BETWEEN 0 AND 30000 THEN 'S'
		ELSE
		(
		CASE
			WHEN SEAT_COUNT BETWEEN 30001 AND 50000 THEN 'M'
			ELSE 'L'
		END
		)
	END
FROM STADIUM;

--PLAYER 테이블에서 WEIGHT가 50이상 70이하이면 'L'
--71이상 80이하면 'M' NULLL이면 '미등록' 다 아니면 'H'
SELECT PLAYER_NAME 선수이름 ,WEIGHT||'KG' 몸무게,
	CASE
		WHEN WEIGHT BETWEEN 50 AND 70 THEN 'L'
		WHEN WEIGHT BETWEEN 71 AND 80 THEN 'M'
		ELSE
		(
		CASE 
			WHEN WEIGHT IS NULL THEN '미등록'
			ELSE 'H'
		END
		)
	END 체급
FROM PLAYER; 
```

## SUB QUERY
쿼리문안에 또 쿼리문이 있는것 SELECT문 안에 SELECT가 하나 더 있다.

- FROM절(IN LINE VIEW) : 쿼리문으로 출력되는 결과를 테이블처럼 사용하겠다.
- SELECT절(SCALAR) : 하나의 컬럼처럼 사용되는 서브쿼리
- WHERE절(SUB QUERY) : 하나의 변수처럼 사용한다.
	- 단일행 서브쿼리 : 쿼리 결과가 단일행만을 리턴하는 서브쿼리입니다.
	- 다중행 서브쿼리 : 쿼리 결과가 다중행을 리턴하는 서브쿼리입니다.
	- 다중컬럼 서브쿼리 : 쿼리 결과가 다중컬럼을 리턴하는 서브쿼리입니다.

```SQL
--PLAYER 테이블에서 TEAM_ID가 'K01'인 선수 중 POSITION이'GK'인 선수
-- 실제 테이블 대신에 서브쿼리의 결과를 테이블처럼 사용한다.
SELECT * FROM ( SELECT * FROM player WHERE TEAM_ID='K01') WHERE "POSITION"='GK';

--PLAYER 테이블에서 평균키보다 작은 선수 검색
SELECT AVG(HEIGHT) FROM PLAYER;

SELECT * FROM PLAYER;
SELECT * FROM PLAYER WHERE  HEIGHT < ( SELECT AVG(HEIGHT) FROM PLAYER);

--PLAYER 테이블에서 전체평균키와 포지션별 평균키 구하기
SELECT "POSITION",AVG(HEIGHT),(SELECT AVG(HEIGHT) FROM PLAYER) 전체평균 FROM PLAYER
WHERE "POSITION"IS NOT NULL
GROUP BY "POSITION";

--CASE문으로 변경
SELECT
	ROUND(AVG(CASE "POSITION" WHEN'GK' THEN HEIGHT END), 2) AS GK,
	ROUND(AVG(CASE "POSITION" WHEN'DF' THEN HEIGHT END), 2) AS DF,
	ROUND(AVG(CASE "POSITION" WHEN'FW' THEN HEIGHT END), 2) AS FW,
	ROUND(AVG(CASE "POSITION" WHEN'MF' THEN HEIGHT END), 2) AS MF,
	ROUND(AVG(HEIGHT),2) AS "전체평균"
FROM PLAYER;

 
--PLAYER테이블에서 NICKNAME이 NULL인 선수들은 정태민 선수의 닉네임으로 바꾸기
SELECT NICKNAME FROM PLAYER WHERE PLAYER_NAME = '정태민';

UPDATE PLAYER
SET NICKNAME = (SELECT NICKNAME FROM PLAYER WHERE PLAYER_NAME='정태민')
WHERE NICKNAME IS NULL;

SELECT * FROM PLAYER WHERE NICKNAME='킹카';

--EMPLOYEES 테이블에서 평균 급여보다 낮은 사원들의 급여를 10% 인상
SELECT * FROM EMPLOYEES;


SELECT AVG(SALARY) FROM EMPLOYEES; -- 6461.83...

UPDATE EMPLOYEES 
SET SALARY = SALARY * 1.1
WHERE SALARY < (SELECT AVG(SALARY) FROM EMPLOYEES);

--PLAYER 테이블에서 평균 키보다 큰 선수들을 삭제

SELECT AVG(HEIGHT) FROM PLAYER;

DELETE FROM PLAYER WHERE HEIGHT > (SELECT AVG(HEIGHT) FROM PLAYER);
```
