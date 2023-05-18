## 정규화
정규화를 배울거다. 테이블 만들때 모델링을 하면서 정확하게 만들었지만 불필요한 컬럼이라던지
불필요한 요소를 걸러내는 작업이 필요해요. 그걸 정규화 라는 규칙을 만들어 놨고 그 여러가지 규칙에 맞게끔 설계를 해야한다는 뜻이다. 그래서 정규화에 종류에는 어떤 것들이 있고 정규화를 왜 써야 하는지 정규화를 하지 않으면 어떤 현상이 일어나는지 알아보겠다.

정규화 : 삽입/수정/삭제의 이상현상을 제거, 데이터의 중복 최소화, 대부분 3차 정규화 까지만 진행<br>
1차,2차 진행하면서 5차까지 하는 경우가 있는데 정규화를 하다보면 하나의 테이브를 계속 분리를 해서 데이터를 가져오는 작업을 할 때 불편하다. 그래서 보통 3차 정규화 까지 진행을 한다.<br>

1차 정규화(1NF) : 하나의 컬럼에 값이 1개만 있어야 한다, 반복적인 컬럼값이 나타나는 경우<br>
보통 취미를 체크할때 체크박스를 사용해서 여러개를 선택하게 할 수 있다.<br>

![image](https://user-images.githubusercontent.com/54658614/216899505-f937ef65-f2a7-4008-91e9-81670d85791f.png)

2차 정규화(2NF) : 테이블의 모든 컬럼이 서로 관계가 있어야 한다.<br>
			모든 컬럼이 서로 관계가 없는 경우<br>
      
![image](https://user-images.githubusercontent.com/54658614/216899605-cdbcdb2b-b636-4295-96e6-04b2ed17f11e.png)

3차 정규화 하나의 컬럼이 다른 컬럼을 대표할 수 없다.<br>
![image](https://user-images.githubusercontent.com/54658614/216899685-024fee1a-b6d5-4498-84ed-dc6506336216.png)

데이터베이스에서 정규화가 필요한 이유 : 데이터베이스를 잘못 설계하면 불필요한 데이터 중복으로인해 공간이 낭비된다. 이러한 현상을 (Anomaly)라고 한다.

![image](https://user-images.githubusercontent.com/54658614/216899745-f0096be9-e560-40d9-82af-ddfaf26f1367.png)

### 이상현상의 종류
  -삽입이상<br>
> 새 데이터를 삽입하기 위해 불필요한 데이터도 삽입해야 하는 문제
  EX)담당 프로젝트가 정해지지 않은 사원이 있다면, 프로젝트 코드에 NULL을 작성할 수 없으므로 이 사원은 테이블     에 추가할 수 없다. 따라서 '미정'이라는 프로젝트 코드를 따로 만들어서 삽입해야한다.

  -갱신이상<br>
 >중복 행 중 일부만 변경하여 데이터가 불일치하게 된는 모순의 문제
  한 명의 사원은 반드시 하나의 부서에만 속할 수 있다.만약 “이현준”이 보안팀으로 부서을 옮길 시 3개 모두 갱신     해 주지 않는다면개발팀인지 보안팀인지 알 수 없다. 이러한 현상을 갱신이상이라고 한다.

  -삭제이상<br>
>행을 삭제하면 꼭 필요한 데이터까지 함께 삭제되는 문제, 이순신이 담당한 프로젝트를 박살내서 드랍된다면 이순신행을 모두 삭제하게 된다. 따라서 프로젝트에서 드랍되면 정보를 모두 드랍하게 된다. 이러한 현상을 삭제 이상이라고 한다.

이러한 현상이 발생하는 이유는 테이블이 정규화가 되어있지 않기 때문이다.<br>
정규화를 진행하기 위해서는 각 컬럼간의 관련성을 파악해야 하고,<br>
이 관련성을“함수적 종속성”(Functional Dependecy)이라고 한다.<br>
따라서 하나의 테이블에서는 하나의 함수적 종속성만 존재하도록 정규화를 한다.<br>

### 함수
X -> Y<br>

X : 결정자 -> X가 Y를 결정<br>
Y : 종속자 -> Y가 X에 종속<br>

요리를 할 때 칼이 필요하죠? 그래서 칼이 결정자가 되고 요리가 종속자가 되는거에요<br>

### NULL
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
  
```
--PLAYER 테이블에서 POSITION이 NULL인 선수 검색
SELECT * FROM PLAYER WHERE “POSITION” IS NULL;
SELECT * FROM PLAYER WHERE “POSITION” IS NOT NULL;

--PLAYER 테이블에서 POSITION이 NULL이면 '미정'으로 결과 출력하기
SELECT NVL(”POSITION”,'미정') FROM PLAYER WHERE “POSITION” IS NULL;
SELECT PLAYER_NAME “선수 이름”, NVL(”POSITION”,'미정') 포지션 FROM PLAYER;
SELECT PLAYER_NAME “선수 이름”, NVL2(”POSITION”,'확정','미정') AS 포지션 FROM PLAYER;

--PLAYER 테이블에서 NATION이 NULL이 아니면 등록, NULL이면 미등록으로 변경
SELECT PLAYER_NAME “선수 이름”, NVL(NATION, '등록','미등록')”국가 등록 여부” FROM PLAYER;
```
함수 : 기본적으로 쿼리문을 더욱 강력하게 하고 데이터값을 조작하는데 도움이 되도록 하는 메서드의 개념<br>
1)	자원에 대한 연산을 수행할 수 있다.
2)	개별적인 데이터 항목을 수행할 수 있다.

### 함수의 종류

1.	숫자형 함수
2.	문자형 함수
3.	날짜형 함수

```
함수는 우리가 직접 만든 테이블이 아닌 dual이라고 하는 가상의 테이블에서 확인을 할 수가 있다.

1. 숫자형 함수
--몫 구하기
SELECT SALARY /2 FROM EMPLOYEES;

--나머지(% 모듈러스)
SELECT MOD(10,3) FROM DUAL

문) 사번, 이름을 출력하되, 짝수 사번을 가진 사원들의 정보만 출력
SELECT EMPLOYEE_ID, FIRST_NAME FROM EMPLOYEES WHERE mod(EMPLOY_ID,2) = 0;

--값보다 큰 최근접 정수
SELECT CEIL(3.14), CEIL(-3.14) FROM DUAL
--값보다 작은 최근접 정수
SELECT CEIL(3.14), CEIL(-3.14) FROM DUAL
--반올림 함수
SELECT round(3.141592),round(3.141592,4) FROM DUAL;
--버림
SELECT TRUNC(3.9) FROM DUAL;
-글자의 길이 구하기
SELECT LENGTH(‘JHON’) FROM DUAL;

문제) 사원테이블에서 이름의 길이가 6자 이상인 사원의 사번, 이름을 출력
select employee_id ,first_name from employees where length(first_name)>=6;

--양수면 1, 0이면 0, 음수면 -1을 반환하는 함수
SELECT SIGN(-234), SIGN(0), SIGN(123) FROM DUAL;



2. 문자형 함수
1) LOWER : 알파벳 값을 소문자로 변환
2) UPPER : 알파벳 값을 대문자로 변환
  3) INITCAP : 알파벳의 첫 글자만 대문자로 변환


select upper('abc') from dual;
select upper('good morning') from dual;

예) 사원테이블의 모든 사원의 이름을 대문자로 표기하시오.
select upper('first_name') upper, first_name from employees;

이름이 첫글자만 대문자인지 전부다 소문자인지 알수가 없을 때 UPPER를 사용해서 통일을 해주면 WHERE을 쉽게 사용할 수 있다.

예) 사원테이블에서 Michael이라는 이름의 사원에 대한 사번, 이름, 직종, 입사일을 검색
 select employee_id, first_name, job_id ,hire_date 
from emplyoees 
where UPPER(first_name) = UPPER('michael') 'MICHAEL';

둘다 대문자로 만들어서 비교를 한다.

SELECT LOWER('ABC') FROM DAUL;
모조리 소문자로 만들어주는 메서드

select lower('GOOD MORNING') from dual;

UPPER,LOWER 말고도 살펴볼수 있는것들이 있다. 조금 더 보여주겠다.

예) 8번째 값 뒤로 모든 데이터를 가져와라

SELECT substr('good morning',8) from dual; 자바의 substr과 동일하다.

예) 8번째 값부터 뒤로 2개의 문장(8,9번째)만 잘라내시오

SELECT substr('good morning'8,2) FROM dual; 많이 사용하지는 않기 때문에 이런게 있다 정도 알고 있기

문) 이름, 입사년도만 출력하시오

SELECT FIRST_NAME, substr(HIRE_DATE,7) year FROM EMPLOYEES;
컬럼명이 길어지기 때문에 별칭을 정해서 깔끔하게 정리해주는것도 좋다.

예) replace() 필요한 문장을 교체하는 함수
SELECT replace('good morning','good','hi') FROM dual;

문) 부서번호가 50번인 사원들의 이름을 출력하되 이름중 'el'을 모두 '**'로 대체하여 출력하시오

SELECT REPLACE(FIRST_NAME,'el','**') FROM EMPLOYEES WHERE DEPARTMENT_ID = 50;


--initcap : 첫 문자를 대문자로 변환하는 함수
	   공백이나 /를 구분자로 인식함
select initcap('good morning') from dual;
select initcap('good/morning') from dual;











3. 날짜형 함수

* 현재 날짜를 기억하는 키워드는 sysdate 다. db에도 자료형이 있는데 그중에서 날짜만 전용으로 저장하는 자료형에 값을 넣을수 있는게 sysdate다.

SELECT sysdate FROM dual; -> 되게 중요함 게시판을 만들어도 게시한 시간을 알 수 있기 때문에


1) ADD_MONTHS : 특정 날짜에 개월수를 더한다.

select sysdate, add_months(sysdate, 2)from dual;

문제) 사원테이블에서 모든 사원의 입사일로부터 6개월 뒤의 날짜를 출력
이름, 입사일, 6개월뒤 날짜 순으로 출력
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

-날짜형식의 formatting 모델
    1) SCC또는 CC: 세기
    2) YYYY 또는  YY: 연도
    3) MM : 월
    4) DD : 일
    5) DAY : 요일
    6) MON : 월명(JAN), DB버전에 따라서 MONTH : 월이름이 다나온다(JANUARY)로 써야하는 경우도 있다.
    7) HH,HH24: 시간
    8) MI : 분
    9) SS : 초
    10) SCC : 세기
```


