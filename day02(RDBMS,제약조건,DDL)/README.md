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

## 제약조건(Constraints)
- 제약조건이란, 테이블에 문제가 되는/ 결함이 있는 데이터가 입력되지 않도록 미리 지정해둔 조건입니다.
- 제약조건은 테이블을 생성할 때 함께 설정할 수 있고, 추후에 생성하거나 변경할 수 도 있습니다.

※ 이미 데이터가 포함되어 있는 상황에서 제약조건을 함부로 바꾸기 어려우므로 사전에 테이블 설계를 잘 해두는 편이 좋다.
<table>
<tr>
	 <th>제약조건</th>	
	 <th>NULL 허용여부</th>	
	 <th>데이터 중복 허용여부</th>	
	 <th>특징</th>
</tr>
<tr>
	<td>PRIMARY KEY(고유키)</td>
	<td>NULL 불가</td>
	<td>중복 불가</td>
	<td>지정한 열은 유일한 값을 반드시 가져야 함 테이블 당 1개만 지정가능</td>
</tr>
	<tr>
	<td>FOREIGN KEY(외래키)</td>
	<td colspan="4">다른 테이블 열을 참조하여 해당 테이블에 존재하는 값만 입력 가능<br>다른 테이블의 고유키(PRIMARY KEY)를 참조</td>
</tr>
<tr>
	<td>UNIQUE(유일키)</td>
	<td>NULL 가능</td>
	<td>중복 불가 (* NULL끼리는 중복으로 간주하지 않음)</td>
	<td>지정한 열은 유일한 값을 반드시 가져야 함 테이블 당 1개만 지정가능</td>
</tr>
<tr>
	<td>NOT NULL</td>
	<td>NULL 불가</td>
	<td>중복 가능</td>
	<td></td>
</tr>
<tr>
	<td>CHECK</td>
	<td colspan="4">설정한 조건식을 만족하는 데이터만 입력가능<br>조건식을 만족하지 않는 데이터는 입력이 거부됨</td>
</tr>
	
</table>

## 자료형(데이터타입)
- 데이터타입이란 컬럼이 저장되는 데이터의 유형을 말합니다.

### 문자 데이터 타입
<table>
<tr>
	 <th>데이터타입</th>	
	 <th>특징</th>
</tr>
<tr>
	<td>CHAR(n)</td>
	<td>고정길이 문자 / 최대 2000byte / 디폴트 값은 1byte </td>
</tr>
<tr>
	<td> VARCHAR2(n)</td>
	<td>가변길이 문자 / 최대 4000BYTE / 디폴트 값은 1byte </td>
</tr>
<tr>
	<td>NCHAR(n)</td>
	<td>고정길이 유니코드 문자(다국어 입력가능) / 최대 2000byte / 디폴트 값은 1byte </td>
</tr>
<tr>
	<td> NVARCHAR(n)</td>
	<td>가변길이 유니코드 문자(다국어 입력가능) / 최대 2000byte / 디폴트 값은 1byte</td>
</tr>
<tr>
	<td>LONG</td>
	<td>최대 2GB 크기의 가변길이 문자형 </td>
</tr>
<tr>
	<td>CLOB</td>
	<td>대용량 텍스트 데이터 타입(최대 4Gbyte)</td>
</tr>
<tr>
	<td> NCLOB</td>
	<td>대용량 텍스트 유니코드 데이터 타입(최대 4Gbyte)</td>
</tr>
	
</table>

### 숫자 데이터 타입
- 대부분 NUMBER형을 사용한다.
<table>
<tr>
	 <th>데이터타입</th>	
	 <th>특징</th>
</tr>
<tr>
	<td>NUMBER(P,S)</td>
	<td>가변숫자 / P (1 ~ 38, 디폴트 : 38) / S (-84 ~ 127, 디폴트 값 : 0)  / 최대 22byte </td>
</tr>
<tr>
	<td>FLOAT(P)</td>
	<td>NUMBER의 하위타입 / P (1~128 .디폴트 : 128) / 이진수 기준 / 최대 22byte </td>
</tr>
<tr>
	<td>BINARY_FLOAT</td>
	<td>32비트 부동소수점 수 / 최대 4byte </td>
</tr>
<tr>
	<td> BINARY_DOUBLE</td>
	<td> 64비트 부동소수점 수 / 최대 8byte </td>
</tr>	
</table>

### 날짜 데이터 타입
<table>
<tr>
	 <th>데이터타입</th>	
	 <th>특징</th>
</tr>
<tr>
	<td> DATE</td>
	<td>BC 4712년 1월 1일부터 9999년 12월 31일, 연, 월, 일, 시, 분, 초 까지 입력 가능</td>
</tr>
<tr>
	<td>TIMESTAMP</td>
	<td>연도, 월, 일, 시, 분, 초 + 밀리초까지 입력가능</td>
</tr>

</table>

## DDL(Data Definition Language) : 데이터 정의어
- 테이블 조작, 제어 관련 쿼리문
  1. CREATE : 테이블 생성
  2. DROP : 테이블 삭제 복구도 안됨. 회사에서 쓰지 말것!
  3. ALTER : 테이블 수정
		- 테이블명 수정 :ALTER TABLE [원래이름] RENAME TO [새로운 테이블 명]
		- 컬럼 추가  :ALTER TABLE [테이블명] ADD([새로운 컬럼명] [컬럼 타입])
		- 컬럼명 변경 :ALTER TABLE [테이블명] RENAME COLUMN [생성된 컬럼명] TO [새로운 컬럼명]
		- 컬럼 삭제 :ALTER TABLE [테이블명] DROP COLUMN [생성된 컬럼명]
  4. TRUNCATE : 테이블 내용 전체 삭제 아~어쩔수 없이 버리다 라는 느낌 이것도 복구 안됨

## day02 스크립트 생성하기
- 테이블 생성해보기
```SQL
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

```SQL
--주석 : 해석하지 못하게 하는 문법
--1. 쿼리문에 설명글을 달 때
--2. 지금 당장 사용하지 안흔 소스코드를 실행하고 싶지 않을 때
```
### 자동차 테이블 생성
- CONSTRAINT [제약조건이름] [제약조건 종류] (컬럼명)

```SQL
CREATE TABLE TBL_CAR(
	ID NUMBER,
	BRAND VARCHAR2(100),
	COLOR VARCHAR2(100),
	PRICE NUMBER,
	CONSTRAINT CAR_PK PRIMARY KEY(ID)
);
```
### 테이블 삭제
- DROP TABLE 테이블명;

### 제약 조건 삭제
- ALTER TABLE 테이블명 DROP CONSTRAINT 제약조건명;

### 제약 조건 추가
- 테이블을 이미 생성하고 난후 제약조건을 추가해야 할 때
- ALTER TABLE 테이블명 ADD CONSTRAINT [제약조건명] [제약조건 종류](컬럼명);

### 동물테이블 생성

```SQL
CREATE TABLE TBL_ANIMAL(
	ID NUMBER PRIMARY KEY, --제약조건을 만들어서 PK를 설정하는 법이 있고 이와 같이 간단하게 만드는법도 있다.
	"TYPE" VARCHAR2(100), --오라클에서는 TYPE은 명령어 이기 때문에 명령어를 컬럼으로 사용하고 싶다면 쌍따옴표 안에 넣어야 한다.
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

``` SQL
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

```SQL
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

- 무결성 : 데이터 무결성은 데이터베이스에 저장된 데이터의 정확성, 일관성, 유효성을 지키는 것이다. 보통 데이터 무결성은 제약조건으로 데이터베이스 시스템이 강제한다.
	- 정확성 : 데이터는 애매하지 않아야 한다. EX)노르스름하다 하면 안된다.
	- 일관성 : 각 사용자가 일관된 데이터를 볼 수 있도록 해야한다.
	- 유효성 : 데이터가 실제 존재하는 데이터여야 한다.(모르겠으면 NULL 이라도 써라 ' ' 따옴표 안을 비워두지 말것)

1. 개체 무결성 : 모든 테이블이 PK로 선택된 컬럼을 가져야 한다.<br>
PK로 선택된 컬럼은 고유한 값을 가져야 하며, 빈 값, NULL값은 허용하지 않는다.<br>

2. 참조 무결성 : 참조 관계에 있는 두 테이블의 데이터가 항상 일관된 값을 갖도록 유지되어야 한다.<br>
다시 말해, 외래키 값은 Null이거나 참조하는 테이블의 기본키값과 동일해야 한다.<br>

3. 도메인 무결성 : 컬럼의 타입, NULL값의 허용 등에 대한 사항을 정의하고 올바른 데이터가 입력되었는 지를 확인하는 것 (제약조건을 잘 설정했는가~)<br>
 
※도메인(domain) : 속성들이 각각 가질 수 있는 값들의 집합<br>
예를 들어 '성별'이라는 속성에서 '남', '여'를 제외한 데이터를 제한되어야 한다.<br>

4. NULL 무결성 : 테이블의 특정 속성 값을 Null이 될 수 없도록 제한했다면 해당 속성에 Null이 있으면 안된다.<br>

5. 고유 무결성 : 테이블의 특정 속성에 대해 고유한 값을 가지도록 조건이 주어진 경우, 각 레코드가 가지는 값들이 달라야 한다. 예를 들어, '이름, '나이'는 서로 같은 값이 있을 수 있지만, '학번'의 경우, 서로 다른 값을 가져야 한다.<br>


