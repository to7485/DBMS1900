day03 메모장 생성
## 모델링

회사에서 클라이언트에 대한 요청이 왔을 때 그 요청에 맞게 끔 분석을 하고 거기에 맞게 설계를 하는 모델링에 대한 개념을 배워보려고 한다.<br>
클라이언트와 서버에 대한 개념도 알아보도록 합시다.<br>

우리가 어떤 언어더라도 먼저 코딩을 하는거보다 문서로 작업을 먼저 하게 된다 이런걸 기획이라고 한다.<br>
요청 한 고객에 니즈를 기반으로 기획팀에서 기획안을 만든다. 그걸 잘 분석하고 데이터베이스를 설계하는것이다.<br>
DBMS 에서도 어떤 어플리케이션을 만들 때 어플리케이션에 맞게끔 데이터베이스 테이블도 설계를 해야한다.<br>
그래서 어떤 순서로 모델링을 하는지 알아보도록 하겠습니다.<br>

### 모델링이란?
- 고객의 요구사항에 맞춰 복잡한현실세계의 데이터를 단순화시켜 데이터베이스로 표현하기 위한 기법

### 데이터 모델링의 특징
- 추상화 : 현실세계를 일정한 형식에 맞춰 간략하게 표현해야 합니다.
- 단순화 : 누구나 쉽게 이해할 수 있도록 제한된 표기법이나 언어를 사용해야 합니다.
- 명확화 : 명확하게 의미가 해석되어야 하고, 한 가지 의미만을 가져야 합니다.

1. 요구사항 분석 : 고객과의 의사소통을 통해 고객의 업무 프로세스를 완벽하게 이해하고, 그들의 요구사항을 구체적으로 뽑아내는 과정
	- 회원, 주문, 상품 3가지를 관리하고자 한다.
	

2. 개념적 설계(개념 모델링) : 데이터베이스에 테이블로 어떻게 짤지 모델링을 생각해보는것
![image](https://user-images.githubusercontent.com/54658614/215704352-53e35c38-4d08-461f-9ce0-23d5dae6eea1.png)


3. 논리적 설계(논리모델링) 
	- 개념 모델링을 바탕으로 스키마를 설계하는 단계
	- 어떤걸 PK로 할 것이고 어떤걸 FK로 사용할 건지 제약조건을 설정하는 단계
![image](https://user-images.githubusercontent.com/54658614/215704561-7a220819-60e8-44ad-9f1b-cd4e41ecb862.png)

우리가 주문을 하기 위해선 주문한 방식이 무었이냐.

### 조합키 지정하는법
```
CONSTRAINT TEST_PK(기본키이름) PRIMARY KEY(CODE, SEQ)
```
4. 물리적 설계(물리 모델링) 
	- DBMS및 하드웨어의 특성을 고려하여 물리적으로 데이터베이스 스키마를 
	- 컬럼명들도 같이 생각을 해보고 타입은 뭘로할지 생각을 해보는 것.

```
USER		
USER_ID:VARCHAR2(100)		
--------------------------		
USER_PW:VHARCHAR2(100)		
USER_NAME:VHARCHAR2(200)		
USER_ADDRESS:VHARCHAR2(300)		
USER_EMAIL:VHARCHAR2(300)		
USER_BIRTH:DATE

PRODUCT	
PRODUCT_NUM:NUMBER		
--------------------------		
PRODUCT_NAME:VHARCHAR2(300)		
PRODUCT_PRICE:NUMBER	
PRODUCT_COUNT:	NUMBER
	
ORDER
ORDER_NUM:NUMBER		
--------------------------
ORDER_DATE:DATE
USER_ID:VARCHAR2(100)
PRODUCT_NUM:NUMBER
```
테이블을 설계할 때 항상 모델링을 먼저 하고 CREATE로 테이블을 만들것 실수할 확률이 현저하게 줄어든다.

5. 구현
```
CREATE TABLE "USER"(
	USER_ID VARCHAR2(100) PRIMARY KEY,
	USER_PW VARCHAR2(100),
	USER_ADDRESS VARCHAR2(300),
	USER_EMAIL VARCHAR2(300),
	USER_BIRTH DATE
);

CREATE TABLE PRODUCT(
	PRODUCT_NUM NUMBER PRIMARY KEY,
	PRODUCT_NAME VARCHAR2(300),
	PRODUCT_PRICE NUMBER,
	PRODUCT_COUNT NUMBER
);

CREATE TABLE "ORDER"(
	ORDER_NUM NUMBER PRIMARY KEY,
	ORDER_DATE DATE,
	USER_ID VARCHAR2(100), --외래키로 참조할 컬럼의 데이터타입과 길이를 반드시 맞춰줘야 합니다.
	PRODUCT_NUM NUMBER,
	CONSTRAINT USER_FK FOREIGN KEY(USER_ID) REFERENCES "USER"(USER_ID),
	CONSTRAINT PRODUCT_FK FOREIGN KEY(PRODUCT_NUM) REFERENCES PRODUCT(PRODUCT_NUM)
);
```
## 데이터 모델링 실습

1. 요구사항
	- 꽃 테이블과 화분 테이블 2개가 필요하고, 꽃을 구매할 때 화분도 같이 구매한다.
	- 꽃은 이름과 색깔, 가격이 있다.
	- 화분은 제품 번호, 색깔, 모양, 꽃 이름이 있다.

2. 개념적 설계(개념 모델링)
![image](https://user-images.githubusercontent.com/54658614/215705598-770d8323-f9d6-4ed4-bc2a-1c6d1b585bd1.png)

3. 논리적 설계(논리 모델링)
![image](https://user-images.githubusercontent.com/54658614/215705734-398d083b-c9b0-409e-b3f8-21b6274067e2.png)

4. 물리적 설계(물리 모델링)
```
FOLOWER
------------------------
FOWER_NAME:VARCHAR2(200),
COLOR:VARCHAR2(100),
PRICE:NUMBER
---------------------------------------
CONSTRAINT PRIMARY KEY(FLOWER_NAME)
---------------------------------------

POT
------------------------
POT_ID:VARCHAR2(100),
POT_COLOR:VARCHAR2(100),
SHAPE:VARCHAR2(200),
FLOWER_NAME:VARCHAR2(200),
---------------------------------------
CONSTRAINT FOREIGN KEY(FLOWER_NAME) REFERENCES FLOWER(FLOWER_NAME)
---------------------------------------
```
5. 구현

day03 스크립트 생성
```
/*
FOLOWER
------------------------
FOWER_NAME:VARCHAR2(200),
COLOR:VARCHAR2(100),
PRICE:NUMBER
---------------------------------------
CONSTRAINT PRIMARY KEY(FLOWER_NAME)
---------------------------------------
*/

DROP TABLE FLOWER;

CREATE TABLE FLOWER(
	FLOWERNAME VARCHAR2(200),
	FLOWERCOLOR VARCHAR2(100),
	FLOWERPRICE NUMBER,
	CONSTRAINT FLOWER_PK PRIMARY KEY(FLOWERNAME)
);

SELECT * FROM FLOWER;
 
/*
POT
------------------------
POT_ID:VARCHAR2(100),
POT_COLOR:VARCHAR2(100),
SHAPE:VARCHAR2(200),
FLOWER_NAME:VARCHAR2(200),
---------------------------------------
CONSTRAINT FOREIGN KEY(FLOWER_NAME) REFERENCES FLOWER(FLOWER_NAME)
---------------------------------------
*/
CREATE TABLE POT(
	POTID VARCHAR2(100),
	POTCOLOR VARCHAR2(100),
	POTSHAPE VARCHAR2(200),
	NAME VARCHAR2(200),
	CONSTRAINT POT_PK PRIMARY KEY(POTID),
	CONSTRAINT POT_FK FOREIGN KEY(NAME) REFERENCES FLOWER(FLOWERNAME)
);

SELECT * POT;
```
새로고침 후 두개의 테이블이 연결되어 있는지 확인하기 위해 플라워 테이블로 들어가서
다이어 그램을 보면 서로 연결이 되어있는걸 확인할 수 있다.

```
--펫 주인과 펫
	OWNER
	ID VC2(200)
	------------
	NAME VC2(200)
	AGE N(5)
	ADDRESS VC2(300)

	PET
	PIN VC2(300)
	-------------
	NAME VC2(200)
	AGE N(5)
	OWNER_NAME VC2(200)

한명의 주인이 여러마리의 애완동물을 키울 수 있다.

CREATE TABLE OWNER(
ID VARCHAR2(200) PRIMARY KEY,
NAME VARCHAR2(200) NOT NULL,
AGE NUMBER(5) DEFAULT 20,
ADDRESS VARCHAR2(300)	
);

SELECT * FROM OWNER;
 
CREATE TABLE PET(
	PIN VARCHAR2(300) PRIMARY KEY,
	NAME VARCHAR2(200) NOT NULL,
	AGE NUMBER(5) DEFAULT 1,
	OWNERNAME VARCHAR2(200), OWNERID로 가져와야함
	CONSTRAINT PET_FK FOREIGN KEY(OWNERID) REFERENSES OWNER(ID)
);

SELECT * FROM PET;

동물병원에서는 성별에 따라 수술하는게 다를테니 테이블드랍을 하고 gender를 추가해보자.

DROP TABLE PET;

CREATE TABLE PET(
	PIN VARCHAR2(300) PRIMARY KEY,
	NAME VARCHAR2(200) NOT NULL,
	AGE NUMBER(5) DEFAULT 1,
	OWNERID VARCHAR2(200), OWNERID로 가져와야함
	GENDER CHAR(1) DEFAULT 'M' NOT NULL CONSTRAINT CHECK_CHAR CHECK(GENDER IN('M','W')),
	CONSTRAINT PET_FK FOREIGN KEY(OWNERID) REFERENSES OWNER(ID)
);
```


