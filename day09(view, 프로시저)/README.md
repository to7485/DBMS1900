## VIEW
- 기존의 테이블은 그대로 놔둔 채 필요한 컬럼들 및 새로운 컬럼을 만든 가상의 테이블.
- 실제 데이터가 저장되는 것은 아니지만 view를 통해서 데이터를 관리할 수 있다.

## VIEW의 사용 목적
- 복잡한 질의를 쉽게 만들어준다.
- 여러 테이블의 JOIN, GROUP BY와 같은 복잡한 쿼리를 VIEW로 저장시켜두면 다음부터는 VIEW정보만 가져오면 된다.

## VIEW의 특징
- 가상 테이블이기 떄문에 물리적으로 구현되어 있지 않다.
- 필요한 데이터만 뷰로 정의해서 처리할 수 있기 때문에 관리가 용이. 명령문이 간단해짐.
- 뷰를 통해서 데이터를 접근하기 때문에 뷰에 나타나지 않는 데이터를 안전하게 보호하는 효율적인 기법 사용 가능.
- 뷰가 정의된 기본 테이블이나 뷰를 삭제하면 그 테이블이나 뷰를 기초로 정의된 다른 뷰도 자동으로 삭제됨.

```SQL
SELECT * FROM PLAYER;

CREATE VIEW PLAYER_AGE
AS ( SELECT ROUND((SYSDATE - BIRTH_DATE) / 365) AGE, P.* FROM PLAYER P);


SELECT * FROM PLAYER_AGE;
해당선수의 나이가 붙어진 가상테이블이 만들어진다. 매번 위의 조건식을 만들기 귀찮기 때문에 VIEW를 만들어놓고 사용하자!

DROP VIEW PLAYER_AGE;지우고 싶을 때!

--30살이 넘은 선수들 검색
SELECT * FROM PLAYER_AGE WHERE AGE > 30;

실제 VIEW라고 하는건 많은 장점이 있기 때문에 회사에 가서도 많이 사용하게 될거다.
매번 써야되는 조건도 미리 만들어놓고 필요한 것만 가져다 쓰기 때문에 굉장히 편한 장점이 있다.

SELECT * FROM EMPLOYEES;
CREATE VIEW EMPLOYEES_MANAGER AS (
SELECT E1.LAST_NAME||' '||E1.FIRST_NAME AS ENAME, 
E2.LAST_NAME||' '||E2.FIRST_NAME AS MNAME
FROM EMPLOYEES E1 
JOIN EMPLOYEES E2 
ON E1.MANAGER_ID = E2.EMPLOYEE_ID);

SELECT * FROM EMPLOYEES_MANAGER;
명령어 치면 VIEW가 생성됨

--KING STEAVEN의 사원 목록 조회
SELECT * (MNAME,ENAME을 쓰는게 조회가 더 빠르다.) FROM EMPLOYEES_MANAGER
WHERE MNAME='King Steven';

--사원 수를 조회
SELECT COUNT(*) FROM EMPLOYEES_MANAGER
WHERE MNAME='King Steven';

--PLAYER 테이블에 TEAM_NAME 컬럼을 추가한 VIEW 만들기 JOIN사용
SELECT * FROM TEAM; -> TEAM_NAME 컬럼 존재
CREATE VIEW PLAYER_TEAM_NAME AS ( 
  SELECT T.TEAM_NAME, P.* 
  FROM PLAYER P JOIN TEAM T
  ON P.TEAM_ID = T.TEAM_ID);

-- TEAM_ID이 같은 곳에서 전체내용과 팀이름컬럼을 붙힌걸 가져와라

SELECT * FROM PLAYER_TEAM_NAME;


SELECT * FROM PLAYER_TEAM_NAME
WHERE TEAM_NAME='울산현대';

--HOMETEAM_ID, STADIUM_NAME, TEAM_NAME 검색
--HOMETEAM이 없는 경기장 이름도 검색하고 VIEW로 만들기
--이 VIEW에서 TEAM_NAME이 NULL인 경기장 검색하기

홈팀이 없는 경기장도 검색이 가능하도록 VIEW로 만드는게 목적

STADIUM에 있는건 다 나와야 하기 때문에 RIGHT OUTER JOIN

CREATE VIEW STADIUM_INFO AS( 
SELECT S.HOMETEAM_ID, S.STADIUM_NAME, T.TEAM_NAME 
FROM TEAM T 
RIGHT OUTER JOIN STADIUM S
ON T.TEAM_ID = S.HOMETEAM_ID
);

SELECT * FROM STADIUM_INFO
WHERE TEAM_NAME IS NULL;
 
--EMPLOYEES 테이블에서 사원들의 FIRST_NAME 모두 검색
--사원들 중에서 매니저들은 JOB_ID 검색
--VIEW로 만들고 매니저가 아니면서 FIRST_NAME이 A로 시작하는 사원 검색

CREATE VIEW MANAGER_INFO 
AS
(
	SELECT E2.JOB_ID, E1.FIRST_NAME      E2.JOBID : 해당 매니저의 JOB_ID
	FROM EMPLOYEES E1 LEFT OUTER JOIN 
	( 
		SELECT JOB_ID ,MANAGER_ID M FROM EMPLOYEES
	) E2 
	ON E1.EMPLOYEE_ID = E2.M
);

SELECT JOB_ID, MANAGER_ID M FROM EMPLOYEES;

EMPLOYEES E1와 SELECT JOB_ID, MANAGER_ID M FROM EMPLOYEES E2가 조인을 하는 형태


SELECT * FROM MANAGER_INFO;-> JOB_ID가 NULL인 애들은 사원

SELECT FIRST_NAME FROM MANAGER_INFO
WHERE JOB_ID IS NOT NULL
GROUP BY FIRST_NAME;


SELECT FIRST_NAME FROM MANAGER_INFO
WHERE JOB_ID IS NULL AND FIRST_NAME LIKE'A%'; 

```

## PL/SQL
- Oracle's Procedural Language extension to SQL
- 오라클에서 SQL을 확장하여 사용하는 프로그래밍 언어. 이름과 같이 절차적 프로그래밍 언어이다. 

## PL/SQL의 기본구조
```SQL
DECLARE - 선언부

IS 

BEGIN - 실행부
  쿼리문을 작성할 수 있다.
END;
```

## PL/SQL을 사용하는 이유
- 대용량 데이터를 연산해야 할 때, WAS등의 서버로 전송해서 처리하려면 네트워크에 부하가 많이 걸릴 수 있다.<br> 이때 프로시져나 함수를 사용하여 데이터를 연산하고 가공한 후에, 최종 결과만 서버에 전송하면 부담을 많이 줄일 수 있다.
- 로직을 수정하기 위해 서버를 셧다운 시키지 않아도 된다. 서버에서는 단순히 DB에 프로시저를 호출하여 사용하면 된다.
- 쿼리문을 직접 노출하지 않는 만큼, SQL injection의 위험성이 줄어든다.
- 블록 단위로 유연하게 사용할 수 있다.

## 단점

## 프로시저(PROCEDURE)
- PL/SQL의 대표적인 부 프로그램에 프로시저가 있다.
- 데이터베이스에 대한 일련의 작업을 정리한 절차를 RDBMS에 저장한 것으로 영구 저장 모듈이라고도 합니다.
- 일련의 쿼리를 마치 하나의 함수처럼 실행하기 위한 쿼리의 집합입니다.
- 거의 함수와 비슷하다.

### 장점
- 하나의 요청으로 여러 SQL문을 실행시킬 수 있습니다.
- 네트워크 소요 시간을 줄여 성능을 개선할 수 있습니다.
- 기능변경이 편하다.

### 단점
- 문자나 숫자 연산에 사용하면 오히려 C,JAVA보다 느린 성능을 볼 수 있다.
- 유지보수가 쉽지 않다.

## 프로시저의 생성
```SQL
CREATE OR REPLACE PROCEDURE 프로시져이름 (
        매개변수1 IN 데이터타입:= 값,
        매개변수2 IN 데이터타입%TYPE
);

IS
  함수 내에서 사용할 변수, 상수 등 선언 , 밑에서 반복되서 사용될 문장을 하나로 선언해서 사용하겠다.

BEGIN

    실행할 문장

END 프로시져 이름;
```

프로시저 사용시 매개변수를 정의한 순서에 맞게 값을 넣어줘야 전달이 된다.

```SQL
CALL 프로시저의 이름(값1,값2)
```

## day09 스크립트 생성하기
- 오라클에서 화면 출력을 위해서는 PUT_LINE이란 프로시저를 이용합니다.
- PUT_LINE은 오라클이 제공하는 프로지서로서, DBMS_OUTPUT 패키지에 묶여 있습니다!
- 그래서 PUT_LINE을 사용할때에는 DBMS_OUTPUT.PUT_LINE과 같이 사용을 해야 합니다.

```sql
BEGIN
	DMBS_OUTPUT.PUT_LINE('HELLO');
END;
```

![image](https://github.com/to7485/DBMS1900/assets/54658614/3d5c8cee-8dbb-4dab-845a-f68b94d1a07c)

실행을 하면 결과를 내기 위한 권한이 없다고 나온다 CMD 창으로 이동을해서 권한을 줘보자.

```
CMD SQLPLUS SYS AS SYSDBA
1111로 접속

set serveroutput on ->서버에서 출력하는 거를 on으로 설정해라
EXIT
```

전체 구역 잡고 실행하기 

![image](https://github.com/to7485/DBMS1900/assets/54658614/e82b78ba-07f1-491e-a3ac-a819f6e12b62)








