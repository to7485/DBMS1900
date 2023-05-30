## RDBMS(관계형데이터베이스)
- 관계형 데이터베이스는 현재 가장 많이 사용하고 있는 데이터베이스의 한 종류이다.
- 관계형 데이터베이스는 테이블로 이루어져 있다.
- 테이블은 이름을 가지고 있고, 행과 열 그리고 거기에 대응하는 값을 가집니다.
- 관계형 데이터베이스는 위와 같이 구성된 테이블이 다른 테이블과 관계를 맺고 모여있는 집합체로 이해할 수 있다.

### 관계형 데이터베이스의 특징
- 데이터의 분류, 정렬, 탐색 속도가 빠르다.
- 오랫동안 사용된 만큼 신뢰성이 높고, 어떤 상황에서도 데이터의 무결성을 보장해준다.
- 기존에 작성된 스키마를 수정하기가 어렵다.
- 데이터베이스의 부하를 분석하는것이 어렵다.

어제 SELECT * FROM EMPLOYEES; 를 통해서 EMPLOYEES 테이블을 조회해봤는데요. <br>

항상 시작하기전에 CMD 창 열고 우리DB가 잘 있나 확인을 해보세요 SQLPLUS hr/hrㄱ<br>
그리고 DBeaver에서 했던 명령어를 그대로 사용할 수 있습니다.<br>

![image](https://user-images.githubusercontent.com/54658614/215700901-1ad531c8-4abf-4b0a-ba79-896566ac1ab9.png)

번호에 대한 이름과 나이를 알고 싶을 때는 Table B에서 번호를 가져와서 그 번호로 Table A에 있는걸 조회하는 거에요. 근데 항상 조회를 할 때는 조건을 건다.<br>
전체 정보가 필요할 때도 있지만 한 사람의 정보만 필요할 때도 있기 때문.<br>

이러한 구조를 가지는 것을 Table, Relation(오라클), Class라고 부른다.<br>

![image](https://user-images.githubusercontent.com/54658614/215701179-8c9ad11a-d221-43fc-ad73-62fb01a4ae8e.png)

USER 테이블에 번호가 없고 이름과 나이만 있다고 칩시다. 그런데 동명이인이 있을 수 있잖아요?<br>
그래서 같은 이름을 조회하면 동며이인도 조회가 되기 때문에 식별하기가 어렵다. 그래서 어떤 값을 찾기 위해서는 중복이 안되는 데이터가 필요하다. 그래야 그 데이터를 조건을 걸어서 가져왔을때 정확하게 우리가 찾는 하나의 정보를 가져올 수 있다. 하나의 테이블에서 중복이 없고 또는 값을 안적어도 되지만(NULL) NULL이 아니고 중복이 안되는 조건을 PK 라고 한다.<br>

![image](https://user-images.githubusercontent.com/54658614/215701253-34d4d9d6-67af-4519-b963-9995ddbebaf9.png)

## DDL(Data Definition Language) : 데이터 정의어
![image](https://user-images.githubusercontent.com/54658614/215701311-552dc21b-4015-4706-a001-70e59c8b043f.png)

![image](https://user-images.githubusercontent.com/54658614/215701465-b9de4d27-42e2-424b-b1e7-1ef201d29ffb.png)

안적은 타입도 많지만 지금부터 배울 필요는 없고 필요하다 생각이 되면 공부를 하면 되고 NUMBER, VARCHAR2, DATE 세가지만 가지고도 충분히 데이터를 관리할 수 있다.<br>
엑셀처럼 DataBase도 배워놓으면 만약 나중에 빅데이터를 배울때 도움이 많이 된다.<br>

```
day02 스크립트 생성
CREATE TABLE TBL_MEMBER(
	NAME VARCHAR2(500),
	AGE NUMBER
);
```
 
### TBL_MEMBER 삭제
```
DROP TABLE TBL_MEMBER;
```

해당 영역 전체화면 : 클릭 후 CTRL + M

```
--주석 : 해석하지 못하게 하는 문법
--1. 쿼리문에 설명글을 달 때
--2. 지금 당장 사용하지 안흔 소스코드를 실행하고 싶지 않을 때
```
### 자동차 테이블 생성
제약 조건 : 테이블을 생성할 때 특정 컬럼에 조건을 부여하여 들어오는 데이터를 검사
```
CREATE TABLE TBL_CAR(
	ID NUMBER,
	BRAND VARCHAR2(100),
	COLOR VARCHAR2(100),
	PRICE NUMBER,
	CONSTRAINT CAR_PK PRIMARY KEY(ID)
);
```
### 테이블 삭제
```
DROP TABLE TBL_CAR;
```
### 제약 조건 삭제
```
ALTER TABLE TBL_CAR DROP CONSTRAINT CAR_PK;
```
### 제약 조건 추가
```
ALTER TABLE TBL_CAR ADD CONSTRAINT CAR_PK PRIMARY KEY(ID);

SELECT * FROM TBL_CAR;	
```
### 동물테이블 생성

```
CREATE TABLE TBL_ANIMAL(
	ID NUMBER PRIMARY KEY, 
--제약조건을 만들어서 PK를 설정하는 법이 있고 이와 같이 간단하게 만드는법도 있다.
"TYPE" VARCHAR2(100), 
--오라클에서는 TYPE은 명령어 이기 때문에 명령어를 컬럼으로 사용하고 싶다면 쌍따옴표 안에 넣어야 한다.
	AGE NUMBER(3),
	FEED VARCHAR2(100)
);
--기존 제약조건 삭제(PK)
properties > constrains 탭으로 들어가면 제약조건에 이름이 정해져 있다.
ALTER TABLE TBL_ANIMAL DROP CONSTRAINT Name

--제약조건 추가(PK)
ALTER TABLE TBL_ANIMAL ADD CONSTRAINT ANIMAL_PK PRIMARY KEY(ID);
DROP TABLE TBL_ANIMAL;

만들었으면 확인을 하고 싶은데요 DML이라는걸 쓰는데 아직 안배웠지만 한번 써보도록 할게요.

SELECT * FROM TBL_ANIMAL;
```
### 제약조건 DEFAULT와 체크

#### 학생 테이블 생성

```
CREATE TABLE TBL_STUDENT(
	ID NUMBER,
	NAME VARCHAR2(100),
	MAJOR VARCHAR2(100),
GENDER CHAR(1) DEFAULT 'W' NOT NULL CONSTRAINT BAN_CHAR CHECK(GENDER = 'M' OR GENDER ='W'),
	BIRTH DATE CONSTRAINT BAN_DATE CHECK(BIRTH >= TO_DATE('1980-01-01','YYYY-MM-DD')),
	CONSTRAINT STD_PK PRIMARY KEY(ID)
);
```
DEFAULT : 값을 넣지 않는다면 들어갈 값

데이터 추가해보기 dml을 배우지는 않았지만 먼저 한번 써보자 외울필요는 없음<br>
스튜던트 테이블로 가서 > er 다이어그램 > 테이블 우클릭 > Generate SQL > INSERT<br>
테이블을 사용할 때 계정명을 붙히지 않을 것이기 때문에 Use Fully 체크 해제 > copy<br>
```
INSERT INTO TBL_STUDENT
(ID, NAME, MAJOR, GENDER, BIRTH)
VALUES(0,'한동석','컴퓨터공학과','A',to_date('1980-01-02','YYYY-MM-DD'));
ban_char 위반 오류 뜸
INSERT INTO TBL_STUDENT
(ID, NAME, MAJOR, GENDER, BIRTH)
VALUES(0,'한동석','컴퓨터공학과','M',to_date('1979-01-02','YYYY-MM-DD'));
ban_date 위반 오류 뜸
INSERT INTO TBL_STUDENT
(ID, NAME, MAJOR, GENDER, BIRTH)
VALUES(0, '한동석', '컴퓨터공학과', 'M' , to_date('2000-05-05','YYYY-MM-DD'));

SELECT * FROM TBL_STUDENT;
INSERT INTO TBL_STUDENT
(ID,NAME,MAJOR,BIRTH) --GENDER 생략해보기
VALUES(0,'홍길동','컴퓨터공학과',TO_DATE('2000-09-05','YYYY-MM-DD'));
STD_PK 위반 오류 학번이 같을수는 없다!
INSERT INTO TBL_STUDENT
(ID,NAME,MAJOR,BIRTH) --GENDER 생략해보기
VALUES(1,'홍길동','컴퓨터공학과',TO_DATE('2000-09-05','YYYY-MM-DD'));
GENDER를 넣지 않았지만 오류가 나지 않는다 DEFAULT 값으로 여자로 넣었기 때문에

테이블 안의 내용 삭제 해보기

TRUNCATE TABLE TBL_STUDENT;
```
### 테이블을 만들 때 조심해야 하는 부분

무결성 : 데이터 무결성은 데이터베이스에 저장된 데이터의 정확성, 일관성, 유효성을 지키는 것이다. 보통 데이터 무결성은 제약조건으로 데이터베이스 시스템이 강제한다.<br>
	- 정확성 : 데이터는 애매하지 않아야 한다. EX)노르스름하다 하면 안된다.<br>
	- 일관성 : 각 사용자가 일관된 데이터를 볼 수 있도록 해야한다.<br>
	- 유효성 : 데이터가 실제 존재하는 데이터여야 한다.<br>
			모르겠으면 NULL 이라도 써라 ' ' 따옴표 안을 비워두지 말것<br>

1. 개체 무결성 : 모든 테이블이 PK로 선택된 컬럼을 가져야 한다.<br>
PK로 선택된 컬럼은 고유한 값을 가져야 하며, 빈 값, NULL값은 허용하지 않는다.<br>

2. 참조 무결성 : 참조 관계에 있는 두 테이블의 데이터가 항상 일관된 값을 갖도록 유지되어야 한다.<br>
다시 말해, 외래키 값은 Null이거나 참조하는 테이블의 기본키값과 동일해야 한다.<br>

3. 도메인 무결성 : 컬럼의 타입, NULL값의 허용 등에 대한 사항을 정의하고 올바른 데이터가 입력되었는 지를 확인하는 것 (제약조건을 잘 설정했는가~)<br>
 
※도메인(domain) : 속성들이 각각 가질 수 있는 값들의 집합<br>
예를 들어 '성별'이라는 속성에서 '남', '여'를 제외한 데이터를 제한되어야 한다.<br>

4. NULL 무결성 : 테이블의 특정 속성 값을 Null이 될 수 없도록 제한했다면 해당 속성에 Null이 있으면 안된다.<br>

5. 고유 무결성 : 테이블의 특정 속성에 대해 고유한 값을 가지도록 조건이 주어진 경우, 각 레코드가 가지는 값들이 달라야 한다. 예를 들어, '이름, '나이'는 서로 같은 값이 있을 수 있지만, '학번'의 경우, 서로 다른 값을 가져야 한다.<br>


