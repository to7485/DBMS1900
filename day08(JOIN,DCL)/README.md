# JOIN
- 데이터를 불러올 때 하나의 테이블만 사용하는 것이 아니라<br> 두 개 이상의 테이블을 합쳐서 필요한 데이터를 추출할 때 사용하는 것이 조인기능이다.
- JOIN을 사용하려면 두개의 테이블이 적어도 하나의 컬럼을 공유해야 한다.
- 보통 기본키(PK)와 외래키(FK)값을 이용하여 JOIN을 한다.

## JOIN의 종류
1. Inner Join : 각 테이블에서 조인 조건에 일치되는 데이터만 가져온다.
  - A와 B테이블의 공통된 부분을 의미한다 보통 교집합 이라고 부른다.

  - 테이블명A INNER JOIN 테이블명B ON 조건식

  - 테이블명A JOIN 테이블명B ON 조건식


![image](https://github.com/to7485/DBMS1900/assets/54658614/ec1e1ec4-6182-4476-ac97-cd9917c57002)

```SQL
--EMP 테이블의 부서번호로 DEPT테이블의 지역 검색하기.
SELECT * FROM EMP;
SELECT * FROM DEPT;

SELECT * FROM EMP JOIN DEPT ON EMP.DEPTNO = DEPT.DEPTNO;

컬럼을 쓸 때 어떤테이블에 속하는지 정확히 명시해주고, 테이블명이 길면 별칭을 달아준다.
--DEPT 테이블에서 부서번호에 해당하는 위치를 가져올 수 있다.
SELECT EMP.DEPTNO, EMP.ENAME, EMP.JOB, DEPT.LOC
FROM EMP JOIN DEPT
ON EMP.DEPTNO = DEPT.DEPTNO;

--PLAYER테이블에서 송종국선수가 속한 팀의 전화번호 검색하기
SELECT * FROM PLAYER;
SELECT * FROM TEAM;
SELECT P.TEAM_ID, P.PLAYER_NAME, T.TEL
FROM PLAYER P JOIN TEAM T
ON P.TEAM_ID = T.TEAM_ID AND P.PLAYER_NAME = '송종국';

--JOBS 테이블에서 JOB_ID로 EMPLOYEES의 JOB_TITLE,EMAIL,성,이름 검색

SELECT * FROM JOBS;
SELECT * FROM EMPLOYEES;

SELECT J.JOB_ID, J.JOB_TITLE, E.EMAIL,
E.LAST_NAME||' '||E.FIRST_NAME 이름
FROM JOBS J JOIN EMPLOYEES E ON J.JOB_ID = E.JOB_ID;

--급여로 등급 나누기
SELECT * FROM SALGRADE;
SELECT * FROM EMP;

등가 조인 : =을 통한 JOIN
비등가 조인 : 부등호를 통한 JOIN(테이블간의 컬럼 값들이 정확히 일치하지 않을 경우 사용)

SELECT *E.EMPNO, E.ENAME, S.GRADE, E.SAL
FROM SALGRADE S JOIN EMP E
ON E.SAL BETWEEN S.LOSAL AND S.HISAL; -> 등호가 없기 때문에 비등가 조인이다.

SELECT *E.EMPNO,D.DEPTNO, E.ENAME, S.GRADE, E.SAL
FROM SALGRADE S JOIN EMP E
ON E.SAL BETWEEN S.LOSAL AND S.HISAL; -> 등호가 없기 때문에 비등가 조인이다.
JOIN DEPT D
ON E.DEPTNO = D.DEPTNO;

--JOIN이나 ON이 헷갈린다면 WHERE을 사용할수도 있다.

SELECT E.EMPNO,D.DEPTNO, E.ENAME, S.GRADE, E.SAL
FROM SALGRADE S, EMP E, DEPT D
WHERE E.SAL BETWEEN S.LOSAL AND S.HISAL AND E.DEPTNO = D.DEPTNO;

--USING(): 중복되는 컬럼이 생길시 맨 앞으로 출력하며 중복 컬럼을 한 개만 출력한다.

SELECT * FROM EMP INNER JOIN DEPT
ON EMP.DEPTNO = DEPT.DEPTNO;

SELECT * FROM EMP INNER JOIN DEPT USING(DEPTNO);

--DEPT테이블의 LOC별 평균 SAL를 반올림하고, SAL 총합 을 구하세요.
SELECT * FROM DEPT;
SELECT * FROM EMP;

SELECT D.LOC, ROUND(AVG(E.SAL)),SUM(E.SAL)
FROM DEPT D JOIN EMP E ON D.DEPTNO = E.DEPTNO
GROUP BY D.LOC;

```

2. Natural Join :  두 테이블 간의 동일한 이름을 갖는 모든 컬럼들에 대해 등가조인(EQUI JOIN)을 수행한다.
  - 컬럼 이름 뿐만 아니라 타입이 모두 같아야 한다.

```SQL
SELECT * FROM EMP INNER JOIN DEPT
ON EMP.DEPTNO = DEPT.DEPTNO;

SELECT * FROM EMP E NATURAL JOIN DEPT D; -- USING을 쓴 효과

SELECT * FROM EMP INNER JOIN DEPT
USING(DEPTNO);

```

3. Outer Join : 두 개의 테이블 중 조건이 거짓이라도 지정한 테이블의 모든 정보가 검색되어야 할 때 사용
  - Left Outer Join

![image](https://github.com/to7485/DBMS1900/assets/54658614/5f54020a-c31d-4fb8-b82c-47cd10b81c24)

  - Right Outer Join

![image](https://github.com/to7485/DBMS1900/assets/54658614/93f6ebb6-c570-4cd7-9293-9d59941c3ec0)

  - Full Outer Join

![image](https://github.com/to7485/DBMS1900/assets/54658614/7ddb1ec8-98af-4db9-8411-89ce92c08032)

  
```SQL
--LEFT OUTER JOIN
SELECT * FROM STADIUM;
SELECT * FROM TEAM;

SELECT * FROM STADIUM JOIN TEAM ON HOMETEAM_ID = TEAM_ID; -- NULL값은 제외하고 검색이 된다.

SELECT * FROM STADIUM LEFT OUTER JOIN TEAM ON HOMETEAM_ID = TEAM_ID;


--RIGHT OUTER JOIN
SELECT * FROM STADIUM RIGHT OUTER JOIN TEAM ON HOMETEAM_ID = TEAM_ID;

--LEFT와 RIGHT의 차이점 : 결과가 왼쪽 테이블 전체 대상이면 LEFT를, 오른족 테이블의 전체 데이터가 대상이라면 RIGHT를 사용한다.


--FULL OUTER JOIN
--죄측 이블과 우측테이블의 데이터를 모두 읽어서 중복되는 데이터는 삭제한 JOIN 결과를 보여준다.

SELECT * FROM STADIUM FULL OUTER JOIN TEAM
ON HOMETEAM_ID = TEAM_ID;
```

4. Self Join : 동일 테이블 사이의 Join
  - 동일 테이블 사이의 조인을 수행하면 테이블과 컬럼 이름이 모두 동일하기 때문에 Alias를 반드시 지정해야 한다.

```SQL
--MGR이라고 하는 컬럼이 있는데 매니저의 이름을 알고싶을 때
SELECT * FROM EMP;

SELECT * FROM EMP E1 JOIN EMP E2
ON E1.MGR = E2.EMPNO;

```











