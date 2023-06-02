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
)

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

## 간단한 프로시저 만들어보기

![image](https://github.com/to7485/DBMS1900/assets/54658614/e3a34547-86e6-4150-a3a6-85637f282534)

X에 3을 넣는다고 가정하면 3이라는 값을 X에 전달하게 되고 X가 우측에 2X+1의 식에 값을 전달하게 되서 결과값이 나오게 된다.<br>
X의 역할은 외부에서 전달된 값과 F라는 함수를 연결해주는 매개체의 역할을 한다.

```SQL
CREATE OR REPLACE PROCEDURE F 프로시저 이름
(
	X  IN NUMBER 외부에서 전달받을 값이 있으면 매개변수에 정리를 해놓는다.
	   (외부에서 전달을 받을거라면 IN을 써야 한다.IN은 생략이 가능하다)
)
IS IS영역에다가 연산을 할껄 써야되는데 2X+1를 쓸껀데 변수를 선언할 필요는 없다.
BEGIN 
	DBMS_OUTPUT.ENABLE;
	DBMS_OUTPUT.PUT_LINE(2*X+1);
END;

호출 CALL F(2);

```

## 프로시저와 SQL
- 우리가 그동안 배운 DML과 프로시저를 접목시켜보자.

### JOBS테이블에 데이터를 INSERT해주는 프로시저 만들기
- 매번 INSERT INTO JOBS VALUES()를 해서 써주기가 너무 귀찮다.
- 프로시저를 만들어서 CALL 프로시저명(값1,값2...)만 해서 INSERT를 대신할 수 있다.

```SQL
SELECT * FROM JOBS; -> 4개의 컬럼값을 갖고 있는걸 확인할 수 있다.

CREATE OR REPLACE PROCEDURE MY_NEW_JOB_PROC -> 프로시저 이름이 조금 길긴한데 프로시저의 기능을 잘 설명하기 위해서는 조금 길어져도 상관이 없다.
(
	우리가 해당 테이블에 INSERT를 하기 위해서는 컬럼이 4개니까 4개의 값을 외부에서 받아야한다.
	그런데 사용자가 무슨 값을 넣을지 예측할 수 없다. 그렇기 때문에 CALL할때 전달해주는 값을 받아줄 매개변수가 있어야 한다.

	P_JOB_ID IN JOBS.JOB_ID%TYPE, ->JOB_ID의 타입을 그대로 가져가겠다.
	P_JOB_TITLE IN JOBS.JOB_TITLE%TYPE,
	P_MIN_SALARY IN JOBS.MIN_SALARY%TYPE,
	P_MAX_SALARY IN JOBS.MAX_SALARY%TYPE
)
IS
BEGIN
	INSERT INTO JOBS(JOB_ID,JOB_TITLE,MIN_SALARY, MAX_SALARY)
	VALUES(P_JOB_ID,P_JOB_TITLE,P_MIN_SALARY,P_MAX_SALARY);
	DBMS_OUTPUT.ENABLE;
	DBMS_OUTPUT.PUT_LINE('ALL DONE ABOUT '||' '||P_JOB_ID);
END;

```
- CREATE부터 END;까지 드래그하고 실행하기

```SQL
CALL MY_NEW_JOB_PROC('IT', 'Developer', 14000, 20000);

SELECT * FROM JOBS;

```
- 다시 프로시저를 실행하게 되면 PK 위반이라서 오류가 나게 된다.
- 오류가 나게 하는것이 아니라 PK가 겹치게 되면 수정이 되거나 삭제가되게 만들어보자.

## IF문

1. IF 조건 THEN 실행문;<br>END IF;
2. IF 조건 THEN 실행문;<br>ELSE 실행문;<br>END IF;
3. IF 조건 THEN 실행문;<br>ELSIF 조건문 THEN 실행문;<br>ELS 실행문;<br>END IF;

### 점수에 맞는 학점 출력하기
```SQL
DECLARE declare [선언부] - 변수, 상수를 선언할 수 있습니다
	SCORE NUMBER := 80; =은 같다 라는 뜻이기 때문에 :=로 써줘야 대입이 된다.
	GRADE VARCHAR2(5); 어떤 학점인지 정확히 할 수 없기 때문에 초기화를 할 수 없다.
BEGIN
	IF SCORE >= 90 THEN GRADE := 'A';
	ELSIF SCORE >= 80 THEN GRADE := 'B';
	ELSIF SCORE >= 70 THEN GRADE := 'C';
	ELSIF SCORE >= 60THEN GRADE := 'D';
	ELSE GRADE := 'F';
END IF;
DBMS_OUTPUT.ENABLE;
DBMS_OUTPUT.PUT_LINE('당신의 점수: '||SCORE||'점'||CHR(10)||'학점: '||GRADE);
END;

```

## 프로시저 수정하기
- INSERT기능을 하는 프로시저를 만들었는데 PK제약조건으로 인해 겹칠 때가 있다.
- 데이터가 겹치가 되면 오류를 내는게 아니라 UPDATE를 해서 내용을 덮어버리자.

```SQL
CREATE OR REPLACE PROCEDURE MY_NEW_JOB_PROC
(--안에 개변수를 넣어야 하는데 현재 JOBS 테이블에서는 4개를 넣어야 한다.
 --사용자가 어떤걸 넣을지는 알수가 없다.
	P_JOB_ID IN JOBS.JOB_ID%TYPE, --JOB_ID의 타입을 그대로 가져가겠다
	P_JOB_TITLE IN JOBS.JOB_TITLE%TYPE,
	P_MIN_SALARY IN JOBS.MIN_SALARY%TYPE,
	P_MAX_SALARY IN JOBS.MAX_SALARY%TYPE
)
IS
	CNT NUMBER := 0;
BEGIN
	SELECT COUNT(JOB_ID) INTO CNT CNT JOBS로부터 CNT를 셋다면 변수에 저장을 해줘
	FROM JOBS WHERE JOB_ID = P_JOB_ID;
	IF CNT = 0 THEN 
		INSERT INTO JOBS(JOB_ID,JOB_TITLE,MIN_SALARY, MAX_SALARY) 
		VALUES(P_JOB_ID,P_JOB_TITLE,P_MIN_SALARY,P_MAX_SALARY);
		DBMS_OUTPUT.ENABLE;
		DBMS_OUTPUT.PUT_LINE('INSERT ALL DONE ABOUT '||' '||P_JOB_ID);
--P_JOB_ID와 JOB_ID 가 일치 하는 곳에서 CNT가 값이 올라갈 것이고 0개 일때는 일치하는게 없기 때문에 추가를 해주면 되죠?
	ELSE
		UPDATE JOBS --사용자가 전달한 PK값이 중복이 있기 때문에 오게 되는 것
			그렇기 때문에 이미 있는 값을 추가하는게 아니고 수정해주겠다
		SET
		JOB_TITLE = P_JOB_TITLE,
		MIN_SALARY = P_MIN_SALARY,
		MAX_SALARY = P_MAX_SALARY
		WHERE JOB_ID = P_JOB_ID;
		DBMS_OUTPUT.ENABLE;
		DBMS_OUTPUT.PUT_LINE('UPDATE ALL DONE ABOUT '||' '||P_JOB_ID);
	END IF;
END;
SELECT * FROM JOBS;

CALL MY_NEW_JOB_PROC('IT','Developer',5000,20000);

```
### 제거를 하는 프로시저 만들기
- JOB_ID 데이터를 하나 전달해서 DELETE문이 작동하는 프로시저 만들어보기

```SQL
CREATE OR REPLACE PROCEDURE DEL_JOB_PROC
(
	P_JOB_ID IN JOBS.JOB_ID%TYPE 보통 DELETE를 하면 하나의 행을 삭제하는데 중복되는 값이 있으면 안되기 때문에 PK 값을 매개변수로 받아서 삭제해보자.
)
IS
	CNT NUMBER := 0;
BEGIN
	SELECT COUNT(JOB_ID) INTO CNT
	FROM JOBS WHERE JOB_ID = P_JOB_ID; 값이 있는지 없는지 검사를 해야하기 때문에 CNT에 넣어주자.
	IF CNT !=0 THEN 0이 아니라는 것은 삭제할 JOB_ID가 존재하고 있다. 삭제해주자.
		DELETE FROM JOBS
		WHERE JOB_ID = P_JOB_ID;
		DBMS_OUTPUT.ENABLE;
		DBMS_OUTPUT.PUT_LINE('DELETE ALL DONE ABOUT '||' '||P_JOB_ID);
	ELSE
		DBMS_OUTPUT.ENABLE;
		DBMS_OUTPUT.PUT_LINE('NO EXIST '||' '||P_JOB_ID);
	END IF;
END;

```

## 시퀀스
- 오라클 데이터베이스에서 특정 규칙에 맞는 연속 숫자를 생성하는 객체입니다.
- 회원가입을 할 때 회원가입한 사람들에게 순차적으로 번호를 부여할 수 있다.
- 게시판에 작성된 게시글 순서대로 순차적인 게시글 번호를 부여할 수 있다.
### 시퀀스 생성하기
```SQL
CREATE SEQUENCE 시퀀스명;
```

### 시퀀스의 다양한 옵션
- 시퀀스를 만들면서 다양한 옵션을 지정할 수 있다.

```SQL
INCREMENT BY 1 --1씩 증가
Start with 1 --1부터 카운팅
MINVALUE 1 -- 최소값
MAXVALUE 100; -- 최대값

캐시를 쓰거나 노캐시를 쓰거나 둘중에 하나만 써야 한다.
CACHE 20; -- 미리 20개의 INDEX공간을 확보 20명이 동시에 접속해서 글을 써도 버벅거리지 않게 해준다.
NOCACHE; -- 1개의 INDEX공간만 확보
```

- 시퀀스를 이미 만들었다면 SQL문을 통해서 옵션을 설정할 수 있다.
```SQL
ALTER SEQUENCE MEMO_SEQ INCREMENT BY 1 MINVALUE 1 MAXVALUE 100 NOCYCLE NOCACHE ORDER ;
```

### 시퀀스의 주의할점
- cache옵션을 사용하면 속도를 증가시기키 위해 sequence번호를 한번에 여러개씩 메모리에 올려놓고 작업을 한다.
- 이러한 경우 DB를 중지시키거나 전원이 OFF되는 경우에 메모리에 있던 번호가 삭제되기 때문에 1,2,3 잘 나오다가 21부터 나올 수 있다.

### Person테이블 만들기
```SQL
CREATE TABLE PERSON(
	IDX NUMBER,
	NAME VARCHAR2(200),
	AGE NUMBER
);
```

### 시퀀스 생성하기
```SQL
CREATE SEQUENCE PERSON_SEQ;
```

### 데이터 추가하기
- 시퀀스명.NEXTVAL : 해당 시퀀스에서 다음 순번 값을 자동으로 가져온다.
```SQL
INSERT INTO PERSON VALUES(PERSON_SEQ.NEXTVAL, '홍길동',30);
INSERT INTO PERSON VALUES(PERSON_SEQ.NEXTVAL, '김길동',27);
INSERT INTO PERSON VALUES(PERSON_SEQ.NEXTVAL, '이디비',16);
```

- SELECT절에서도 NEXTVAL을 할 수 있는데 값이 증가가 되버리니 주의하자.

```SQL
SELECT PERSON_SEQ.NEXTVAL FROM DUAL;
```

### 결과 조회해보기
```SQL
SELECT * FROM PERSON;
```

![image](https://github.com/to7485/DBMS1900/assets/54658614/67f5296d-8e3a-4788-b745-9ff2e7bf6ab2)

### 현재 시퀀스의 값 보기
```SQL
SELECT PERSON_SEQ.CURRVAL FROM DUAL;
```

![image](https://github.com/to7485/DBMS1900/assets/54658614/fcbf80ae-b3f1-49ce-95dd-8171e213e6b9)


### 시퀀스값 초기화 하는법
- 제일 좋은 방법은 삭제를 했다가 다시 만드는것입니다.
- 하지만 대부분의 경우 권한이 없기 때문에 삭제를 할수가 없습니다.

```SQL
1. 현재 시퀀스의 값을 확인한다.
SELECT PERSON_SEQ.CURRVAL FROM DUAL;

보류
-- SELECT LAST_NUMBER FROM USER_SEQUENCES WHERE SEQUENCE_NAME='시퀀스명';

2. 현재 시퀀스 값 만큼 INCREMENT를 뺀다.
ALTER SEQUENCE PERSON_SEQ INCREMENT -3;

3. 다시 NEXTVAL을 하면 1로 바뀌어있다.
SELECT PERSON_SEQ.NEXTVAL FROM DUAL;

4. 다시 시퀀스의 증가량을 INCREMENT를 1로 설정한다.
ALTER SEQUENCE PERSON_SEQ INCREMENT BY 1;
```

### 시퀀스 삭제
```SQL 
DROP SEQUENCE 시퀀스명;
```










