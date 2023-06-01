## 함수적 종속성
- 하나의 테이블에서 한 컬럼의값(X)가 다른 컬럼의 값(Y)을 결정하는 관계를 함수적 종속이라고 할 수 있다.
- 정규화가 잘된 테이블일수록 결정자 X는 PK한개이고 종속자가 여러개인 구조를 가진다.

![image](https://github.com/to7485/DBMS1900/assets/54658614/aae45b62-c205-4b27-9754-6e8e1955f012)

위처럼 X가 다른 모든 컬럼의 값을 결정하는<br>
X -> Y1<br>
X -> Y2<br>
X -> Y3<br>
X -> Y4<br>
와 같은 관계를 갖는다.<br>
위와 같은 성격을 완전함수 종속이라고 한다.<br>

### 완전함수 종속
- 종속자가 기본키에만 종속되며, 기본키가 여러 속성으로 구성되어 있을 경우 기본키를 구성하는<br> 모든속성이 포함된 기본키의 부분집합에 종속된 경우를 완전함수종속이라고 한다.

![image](https://github.com/to7485/DBMS1900/assets/54658614/d86c2feb-4b34-4e29-98f6-bd92774e5aa1)

- 주민번호를 알면 이름,성별,주소를 모두 식별할 수 있다.
- 하지만 이름만으로는 성별, 주소, 주민번호 어떤것도 식별할 수 없다.

위의 테이블은<br>
주민번호(X) -> 이름(Y1)<br>
주민번호(X) -> 성별(Y2)<br>
주민번호(X) -> 주소(Y3)<br>
와 같은 관계가 성립하는<br>
주민번호를 제외한 다른 속성들이 다른 속성에 종속되지 않는 완전 함수 종속이 이루어진 테이블이다. <br>

### 부분함수 종속
- 테이블에서 기본키가 복합키일 경우 기본키를 구성하는 속성 중 일부에게 종속된 경우를 부분함수 종속이라고 한다.

![image](https://github.com/to7485/DBMS1900/assets/54658614/3b523043-27f6-4f98-84ef-ddd4bcc1940c)

위의 테이블에서 기본키는(이름,성별)이다.<br>

(이름,성별) -> 주소<br>
(이름,성별) -> 지역번호<br>
관계가 성립하게 된다.

하지만<br>
(이름) -> 주소<br>
의 관계도 성립이 된다.<br>

이 경우 기본키가 여러 속성으로 구성되어있을 경우 기본키를 구성하는 속성 중 일부에게 종속된 경우이다.<br>

### 이행함수 종속
- 테이블에서 X,Y,Z라는 세개의 컬럼이 존재할 때 X->Y,Y->Z이란 종속관계가 잇을 경우, X->Z가 성립되는것을 이행적 함수 종속이라고 한다.
- 위의 테이블에서 X(이름,성별) -> Y(주소), X(이름,성별) -> Z(지역번호)관계를 알았다.
- 그런데 Y(주소) -> Z(지역)의 관계도 성립이 된다.

## 정규화
- 테이블 만들때 모델링을 하면서 정확하게 만들었지만 불필요한 컬럼이라던지 불필요한 요소를 걸러내는 작업이 필요하다.
- 정규화 라는 규칙에 맞게끔 설계를 해야한다는 뜻이다.
- 논리 모델링시 정규화를 진행하게 된다.

정규화 : 삽입/수정/삭제의 이상현상을 제거, 데이터의 중복 최소화, 대부분 3차 정규화 까지만 진행<br>
1차,2차 진행하면서 5차까지 하는 경우가 있는데 정규화를 하다보면 하나의 테이브를 계속 분리를 해서 데이터를 가져오는 작업을 할 때 불편하다. 그래서 보통 3차 정규화 까지 진행을 한다.<br>

- 정규화의 이점
	- 불필요한 데이터 반복을 제거함으로써 저장공간을 최소화할 수 있으며, 데이터의 변경 시 데이터의 불일치성을 최소화하고, 연산작업을 최소화할 수 있다.

## 정규화 순서

- 정규화 전

![image](https://github.com/to7485/DBMS1900/assets/54658614/f826559e-de8b-48c8-bb93-b055c52bd212)

- 1차 정규화(1NF) :  도메인이 원자값이어야 한다.
	- 하나의 컬럼에 값이 1개만 있어야 한다, 반복적인 컬럼값이 나타나는 경우<br>
	- 보통 취미를 체크할때 체크박스를 사용해서 여러개를 선택하게 할 수 있다.<br>

![image](https://github.com/to7485/DBMS1900/assets/54658614/6dead6ed-e5e4-4835-b85b-e74e46b22182)


- 2차 정규화(2NF) : 부분적 함수 종속 제거
	- 1차 정규화의 조건을 만족하며 테이블의 모든 컬럼이 서로 관계가 있어야 한다.

위의 데이터를 2차 정규화의 조건에 맞게 변형한 것이다.

![image](https://github.com/to7485/DBMS1900/assets/54658614/34a102bc-b677-4238-adc0-a753d3e36efb)

![image](https://github.com/to7485/DBMS1900/assets/54658614/55918093-c16c-4088-ae31-57feba062c75)

하지만 현재 회원의 활동 상태를 파악하지 못하는 단점이 있으므로 두 테이블의 관계를 정의하는 테이블이 필요하다.

![image](https://github.com/to7485/DBMS1900/assets/54658614/1ce95ab2-d5f7-4624-bf9a-c5bb62e79a2f)


- 3차 정규화(3NF) :  2차 정규화의 조건을 만족하면서 이행적 함수 종속 제거를 제거해야 한다.
	- 하나의 컬럼이 다른 컬럼을 대표할 수 없다.<br>

위의 회원상태 테이블에서<br>
X(회원ID) -> Y(지역코드) 의존성을 가지고,<br>
Y(지역코드) -> Z(거주지)에 의존성을 가지므로<br>
X(회원ID) -> Z(거주지) 의존성을 가진다.<br>
따라서 기본키가 아닌 데이터 컬럼이 다른 컬럼의 결정요소가 되므로 이것을 제거하기 위해 항목들을 분리해야 한다.

![image](https://github.com/to7485/DBMS1900/assets/54658614/04fde3e6-2b76-4062-a58d-3a746e812b37)

![image](https://github.com/to7485/DBMS1900/assets/54658614/7630feb3-fa14-49bb-98ac-0165d7ededc3)

데이터베이스에서 정규화가 필요한 이유 : 데이터베이스를 잘못 설계하면 불필요한 데이터 중복으로인해 공간이 낭비된다. 이러한 현상을 (Anomaly)라고 한다.


## 이상현상(Anormally)

![image](https://user-images.githubusercontent.com/54658614/216899745-f0096be9-e560-40d9-82af-ddfaf26f1367.png)

  -삽입이상<br>
> 새 데이터를 삽입하기 위해 불필요한 데이터도 삽입해야 하는 문제
  EX)담당 프로젝트가 정해지지 않은 사원이 있다면, 프로젝트 코드에 NULL을 작성할 수 없으므로 이 사원은 테이블에 추가할 수 없다. 따라서 '미정'이라는 프로젝트 코드를 따로 만들어서 삽입해야한다.

  -갱신이상<br>
 >중복 행 중 일부만 변경하여 데이터가 불일치하게 된는 모순의 문제
  한 명의 사원은 반드시 하나의 부서에만 속할 수 있다.만약 “이현준”이 보안팀으로 부서을 옮길 시 3개 모두 갱신해 주지 않는다면개발팀인지 보안팀인지 알 수 없다. 이러한 현상을 갱신이상이라고 한다.

  -삭제이상<br>
>행을 삭제하면 꼭 필요한 데이터까지 함께 삭제되는 문제, 이순신이 담당한 프로젝트를 박살내서 드랍된다면 이순신행을 모두 삭제하게 된다. 따라서 프로젝트에서 드랍되면 정보를 모두 드랍하게 된다. 이러한 현상을 삭제 이상이라고 한다.

이러한 현상이 발생하는 이유는 테이블이 정규화가 되어있지 않기 때문이다.<br>
정규화를 진행하기 위해서는 각 컬럼간의 관련성을 파악해야 하고,<br>
이 관련성을 "함수적 종속성"(Functional Dependecy)이라고 한다.<br>
따라서 하나의 테이블에서는 하나의 함수적 종속성만 존재하도록 정규화를 한다.<br>

## NULL
NULL : 정의되지 않은 값<br>
	빈 값 대신 미정 값을 부여할 때 사용 가능(PK는 불가능, FK는 가능)<br>

NOT NULL 제약조건<br>
	ALTER TABLE 테이블명 MODIFY 컬럼명 NOT NULL;<br>

제약조건 삭제<br>
	ALTER TABLE 테이블명 DROP CONSTRAINT 제약조건 이름;<br>

조건식<br>
	컬럼명 IS NULL : 컬럼 값이 NULL이면 참<br>
	컬럼명 IS NOT NULL : 컬럼 값이 NULL이 아니면 참<br>

NULL 값을 다른 값으로 변경<br>
	NVL() : NULL값 대신 다른 값으로 변경 후 검색<br>
	NVL2() : NULL일 때의 값, NULL이 아닐 때의 값을 각각 설정<br>
  
```SQL
--PLAYER 테이블에서 POSITION이 NULL인 선수 검색
SELECT * FROM PLAYER WHERE "POSITION" IS NULL;
SELECT * FROM PLAYER WHERE "POSITION" IS NOT NULL;

--PLAYER 테이블에서 POSITION이 NULL이면 '미정'으로 결과 출력하기
SELECT NVL("POSITION",'미정') FROM PLAYER WHERE "POSITION" IS NULL;
SELECT PLAYER_NAME “선수 이름”, NVL("POSITION",'미정') 포지션 FROM PLAYER;
SELECT PLAYER_NAME “선수 이름”, NVL2("POSITION",'확정','미정') AS 포지션 FROM PLAYER;

--PLAYER 테이블에서 NATION이 NULL이 아니면 등록, NULL이면 미등록으로 변경
SELECT PLAYER_NAME "선수 이름", NVL(NATION, '등록','미등록') "국가 등록 여부" FROM PLAYER;
```

## 함수
- 기본적으로 쿼리문을 더욱 강력하게 하고 데이터값을 조작하는데 도움이 되도록 하는 메서드의 개념
1. 자원에 대한 연산을 수행할 수 있다.
2. 개별적인 데이터 항목을 수행할 수 있다.

## SQL함수
- 사용자가 필요한 기능을 만드는 함수가 아닌, 오라클 자체적으로 제공하는 함수
- 상황에 맞는 적절한 함수를 사용하기 위해서는 어떤 기능을 하는 함수들이 존재하는지 정확하게 파악하고 있어야 한다.

### 내장함수의 종류
- 단일행 함수 : 1개의 행값이 함수에 적용되어 1개의 행을 반환한다.
- 그룹 함수 : 1개 이상의 행의 값이 함수에 적용되어 1개의 값을 반환한다.

## 문자함수
|함수|기능|
|---|------|
|ASCII|지정된 문자의 ASCII값을 반환한다.|
|CHR|지정된 수치와 일치하는 ASCII코드를 반환한다.|
|RPAD|왼쪽 정렬 후 오른쪽에 생긴 빈공백에 특정 문자를 채워 반환한다.|
|LPAD|오른쪽 정렬 후 오른쪽에 생긴 빈공백에 특정 문자를 채워 반환한다.|
|TRIM|문자열 공백 문자들을 삭제한다.|
|RTRIM|문자열 오른쪽(뒤)의 공백 문자들을 삭제한다.|
|LTRIM|문자열 왼쪽(뒤)의 공백 문자들을 삭제한다.|
|LOWER|지정된 문자를 소문자로 반환한다.|
|UPPER|지정된 문자를 대문자로 반환한다.|
|INSTR|특정 문자의 위치를 반환한다.|
|INITCAP|지정된 문자열의 첫 단어를 대문자로 나머지는 소문자로 반환한다.|
|SUBSTR|시작 위치부터 선택 개수만큼 문자를 반환한다.|
|LENGTH|문자열의 길이를 반환한다.|
|REPLACE|첫 번째 파라미터로 지정한 문자를 두번째 파라미터로 지정한 문자로 바꿔준다.|

```SQL
-- 지정된 문자 ASCII값을 반환한다.
SELECT ASCII('A') FROM DUAL; --결과 : 65

-- 지정된 수치와 일치하는 ASCII코드를 반환한다.
SELECT CHR(65) FROM DUAL; --결과 A

-- 왼쪽 정렬 후 오른쪽에 생긴 빈 공백에 특정 문자를 채워 반환한다.
-- RPAD(데이터,고정길이,문자)
-- 고정길이 안에 데이터를 출력하고 남는 공간을 문자로 채운다.

SELECT RPAD(DEPT_NAME,10,'*') FROM DEPT;

-- 오른쪽 정렬 후 왼쪽에 생긴 빈 공백에 특정 문자를 채워 반환한다.
-- LPAD(데이터,고정길이,문자)
-- 고정길이 안에 데이터를 출력하고 남는 공간을 문자로 채운다.

SELECT LPAD(DEPT_NAME,10,'*') FROM DEPT;

-- 문자열 공백 문자들을 삭제한다
SELECT TRIM('  HELLOW  ') FROM DUAL;

-- 컬럼이나 대상 문자열에서 특정 문자가 첫 번째나 마지막 위치에 있으면, 해당 특정 문자를 잘라낸 후 남은 문자열만 반환한다.
SELECT TRIM('zzz' FROM 'zzzHELLOWzzz') FROM DUAL;

-- 문자열 오른쪽(뒤)의 공백 문자들을 제거한다.
SELECT RTRIM('HELLOW  ')FROM DUAL;

-- 문자열 왼쪽(앞)의 공백 문자들을 제거한다.
SELECT LTRIM('   HELLOW')FROM DUAL;

-- 특정문자의 위치를 반환한다.
SELECT INSTR('HELLOW','O') FROM DUAL;

- 문자열에서 1번째 자리부터 검색하여 두번째로 나오게 되는 글자가 위치하는 자리를 반환한다.
SELECT INSTR('HELLOW','L',1,2) FROM DUAL;

-- 찾는 문자열이 없으면 0을 반환한다.
SELECT INSTR('HELLOW','Z') FROM DUAL;

--첫 문자를 대문자로 변환하는 함수 공백이나 /를 구분자로 인식함
select initcap('good morning') from dual;
select initcap('good/morning') from dual;

-- 문자열의 길이를 반환한다.
select length('john') from dual;

-- 첫 번째 지정한 문자를 두번째 지정한 문자로 바꿔 반환한다.

문) 부서번호가 50번인 사원들의 이름을 출력하되 이름중 'el'을 모두 '**'로 대체하여 출력하시오
SELECT REPLACE(FIRST_NAME,'el','**') FROM EMPLOYEES WHERE DEPARTMENT_ID = 50;

```

## 숫자함수
|함수|기능|
|---|------|
|ABS|절대값을 반환한다.|
|ROUND|특정 자리수에서 반올림을 하여 반환한다.|
|FLOOR|주어진 숫자보다 작거나 같은 정수중 최대값을 반환한다.|
|TRUNC|특정 자리수에서 잘라내고 반환한다.|
|SIGN|주어진 값의 음수,정수,0 여부를 반환한다.|
|CEIL|주어진 숫자보다 크거나 같은 정수 중 최소값을 반환한다.|
|MOD|나누기 후 나머지를 반환한다.|
|POWER|주어진 숫자의 지정된 수 만큼의 제곱값을 반환한다.|

```SQL
-- 절대값을 반환한다.
SELECT -10,ABS(-10) FROM DUAL;

-- 특정 자리수에서 반올림하여 반환한다.
-- 지정한 숫자가 양수이면 소수점 아래, 음수이면 소수점 위를 의미한다. 생략되면 반올림해서 정수를 반환한다
SELECT ROUND(1234.567,1),ROUND(1234.567,-1),ROUND(1234.567) FROM DUAL;

-- 주어진 숫자보다 작거나 같은 정수 중 최대값을 반환한다.
SELECT FLOOR(2), FLOOR(2.1) FROM DUAL;

-- 특정 자리수를 버리고 반환한다.
SELECT TRUNC(1234.567,1),TRUNC(1234.567,-1),TRUNC(1234.567) FROM DUAL;

-- 주어진 값의 음수,정수,0 여부를 반환한다.
--음수는 -1, 0은 0, 양수는 1, NULL은 NULL을 반환한다.
SELECT SIGN(-10),SIGN(0),SIGN(10),SIGN(NULL) FROM DUAL;

-- 주어진 숫자보다 크거나 같은 정수 중 최소값을 반환한다.
SELECT CEIL(2),CEIL(2.1) FROM DUAL;

-- 나누기 후 나머지를 반환한다.
SELECT MOD(1,3),MOD(2,3),MOD(3,3),MOD(4,3),MOD(0,3) FROM DUAL;

-- 주어진 숫자의 지정된 수 만큼 제곱값을 반환한다.
SELECT POWER(2,1),POWER(2,2),POWER(2,3),POWER(2,0) FROM DUAL;

```

## 날짜함수
|함수|기능|
|---|------|
|ADD_MONTHS|특정날짜에 개월수를 더해준다. |
|MONTHS_BETWEEN|주어진 두 개의 날짜 간격 개월을 반환한다.|
|NEXT_DAY|주어진 일자가 다음에 나타나는 지정요일(1:일요일 ~ 7:토요일)의 날짜를 반환한다.|
|LAST_DAY|주어진 일자가 포함된 월의 말일을 반환한다.|

※ 날짜 + 날짜 : 날짜끼리는 더하기가 안됩니다.

- SYSDATE : 현재 날짜

```SQL
-- 특정 날짜에 개월수를 더한다.

select sysdate, add_months(sysdate, 2)from dual;

문제) 사원테이블에서 모든 사원의 입사일로부터 6개월 뒤의 날짜를 이름, 입사일, 6개월뒤 날짜 순으로 출력
select first_name, hire_date,add_months(hire_date,6) new_date from employees;

문제) 사번이 120번인 사원이 입사후 3년 6개월째 되는날 진급예정이다. 진급 예정 날짜를 구하시오.
select fist_name, add_months(hire_date, 42) promotion from employees where employee_id = 120;

select trunc( months_between(sysdate, '01/01/2015'), 2)from dual;

2) MONTHS_BETWEEN : 두 날짜 사이의 개월수를 구한다

문제)모든 사원들이 입사일로부터 오늘가지 몇개월이 경과했는지 출력

SELECT sysdate, hire_date, MONTHS_BETWEEN(sysdate,hire_date) FROM EMPLOYEES;

문제) 사원들의 이름, 입사일, 입사후 오늘까지의 개월수를 조회하되 입사기간이 200개월 이상인 사람만 출력하고 입사개월수는
소수점 이하 한자리까지만 버림하시오.

SELECT FIRST_NAME, HIRE_DATD, TRUNC(MONTHS_BETWEEN(SYSDATDE,HIRE_DATE),1) MONTHS FROM EMPLOYEES
WHERE TRUNC(MONTHS_BETWEEN(SYSDATDE,HIRE_DATE),1) >= 200;

문제)입사기간이 160개월 이상인 사원들의 이름, 입사일, 입사후 개월수를 출력

select first_name, hire_date,mon from employees where trunc(months_between(sysdate, hire_date)>=200),0) mon

문제) 사번이 120번인 사원이 입사후 3년 6개월이 되는날 퇴사했다. 이 사원의 사번,이름,입사일,퇴사일을 출력

SELECT EMPLOYEE_ID, FIRST_NAME, HIRE_DATE, add_months(HIRE_DATE, 42) fired FROM EMPLOYEES WHERE EMPLOYEE_ID = 120;

SELECT SYSDATE,
	NEXT_DAY(SYSDATE-8,'SUN') prev_sunday, --이전 일요일 
	NEXT_DAY(SYSDATE,'SUN')   next_sunday --다음 일요일 
FROM DUAL
  
SELECT NEXT_DAY(SYSDATE, 1)        FROM DUAL;
SELECT NEXT_DAY(SYSDATE, '일요일') FROM DUAL;
SELECT NEXT_DAY(SYSDATE, '일')     FROM DUAL;
SELECT NEXT_DAY(SYSDATE, 'SUNDAY') FROM DUAL;
SELECT NEXT_DAY(SYSDATE, 'SUN')    FROM DUAL;

SELECT LAST_DAY(TO_DATE('2022-01-17', 'YYYY-MM-DD')) FROM dual
```

오라클에서는 날짜형 데이터를 문자열 데이터로 변환하는 TO_CHAR()함수를 사용합니다.
- TO_CHAR('문자열','날짜포맷')

```SQL
SELECT TO_CHAR(sysdate,'yyyy-mm-dd') FROM dual;
SELECT TO_CHAR(sysdate,'yyyy-mm-dd day') FROM dual;
SELECT TO_CHAR(sysdate,'yyyy-mm-dd HH:MI:SS') FROM dual;
```

### 날짜형식 FORMATTING 모델
|모델|기능|
|---|------|
|SCC, CC|세기|
|YYYY,YY|연도|
|MM|월|
|DD|일|
|DAY|요일|
|MON|월명(JAN)|
|MONTH|월이름이 다나온다(JANUARY)|
|HH,HH24|시간|
|MI|분|
|SS|초|
