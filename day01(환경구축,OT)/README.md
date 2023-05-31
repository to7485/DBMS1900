# day01
데이터베이스가 무엇인가? DBMS가 무엇인가에 대해 알아보려고 한다.

### 작업공간 생성하기
공백이나 한글은 사용하지 않기
드라이브 이동 -> DBMS_LHJ -> 1.memo 폴더 2.resource

바탕화면에 하게 되면 내 윈도우즈 운영체제 계정 폴더에 들어가게 되는데 계정을 한글로 만드는 경우가 있다.<br>
그런데 상위과정으로 갈수록 프로그램이 외국에서 만든거다 보니 한글이랑 충돌이 일어날 수 있기 때문에 한글은 웬만하면 사용하지 말자.<br>

## DB(DataBase) 
- 구조화된 정보, 데이터의 조직화된 모음, 일반적으로 컴퓨터에 전자적으로 저장이 되고 데이터베이스관리시스템에 의해 제어된다.

## DBMS(DataBaseManagementSystem)
- 데이터베이스와 사용자 또는 프로그램 간의 매개체 역할을 하여 사용자가 정보의 구성,검색,수정,삭제와 같은 관리를 할 수 있게 해줍니다.



## 액셀 vs 데이터베이스

|구분|액셀|데이터베이스|
|------|---|---|
|장점|직관적,익히기 쉽다|많은 데이터 쉽게 처리 가능,복잡한 처리 가능, 동시작업 가능|
|단점|많은 데이터 처리시 느려짐<br>다소 복잡한 성능 처리 어려움<br>|사용 시 쿼리 언어에 능숙해야함<br>데이터 모델링 시 지식과 숙련도 필요|

### 인기있는 DBMS 소프트웨어
- MySQL
- Microsoft Access
- Microsoft SQL Server
- FileMaker Pro
- Oracle Database
등등

## 오라클 DBMS 설치
https://www.oracle.com/index.html

- 돋보기 버튼 누르고  XE Prior Release Archive 입력하고 검색하기
- XE Prior Release Archive 클릭하기

![20230406_134017](https://user-images.githubusercontent.com/54658614/230273656-8c50ea22-68c2-49e7-8f39-fdc8de10be0a.png)

- 운영체제에 맞는 버전 다운로드 하기

![image](https://user-images.githubusercontent.com/54658614/230274134-c09aeefb-0b7d-4241-90c9-7a709fa389b7.png)

- 압축 풀고 폴더 안에 있는 setup.exe 실행하기

![image](https://user-images.githubusercontent.com/54658614/230274775-60330aba-0bed-4f43-a7f2-aaa557ec13c6.png)

- 동의 눌러주고 next 진행한다.

![image](https://user-images.githubusercontent.com/54658614/230274935-47d28b33-9579-414b-ad1d-e12f8aec893a.png)

- 설치할 경로를 정해줄 수 있는데 따로 바꿔줄 필요는 없다.(항상 필요한 용량은 확보해두자)

![image](https://user-images.githubusercontent.com/54658614/230275196-3d0c701d-3d0c-4084-a4c0-bc6ebc85310e.png)

- 혹시 DBMS를 지웠다가 다시 까는 경우 이러한 문구가 뜰 수 있다. 

![image](https://user-images.githubusercontent.com/54658614/230275283-67444bb5-02db-4007-b56c-d202bbd3026b.png)

- 설치했던 곳에 있는 폴더를 제거 하고 진행해주자

![image](https://user-images.githubusercontent.com/54658614/230275348-6f5ea512-ad8b-47dc-8e15-074a42e51cca.png)

- DB에 접속할 계정의 비밀번호를 설정해야 한다. 까먹지 않을 비밀번호로 설정하자.

![image](https://user-images.githubusercontent.com/54658614/230275461-9b9e5730-b1e6-44b9-8f82-1060c12a32d0.png)

- 그리고 쭉 진행해준다.

- 설치를 모두 마치면 다음과 아이콘을 더블클릭하여 실행할 수 있다.

![image](https://user-images.githubusercontent.com/54658614/230275813-e86781f6-4251-4534-b196-2e2750e2221e.png)

- 실행을 하면 다음과 같은 화면이 나오는데 실행이 안될 수 도 있다.

![image](https://user-images.githubusercontent.com/54658614/230275882-c3a1fbe5-f768-4eae-bc7e-86ae8cd550f0.png)

- 이러한 오류가 발생할 수 있다.

![image](https://user-images.githubusercontent.com/54658614/230276206-104cb7ae-9bf5-40b9-97d1-9281b08cbac7.png)


![image](https://user-images.githubusercontent.com/54658614/230276002-0da8350c-ec68-429f-8cf1-8afd45647ba2.png)

![image](https://user-images.githubusercontent.com/54658614/230276054-7835d3f8-dc93-435f-a0a1-5231886b99a0.png)

- 이렇게 바꿔주자

![image](https://user-images.githubusercontent.com/54658614/230276265-885421ae-af8b-4092-ab5d-d793754b2657.png)

내가 어디에 설치됐는지 확인할 수 있는지???
win+r > cmd > sqlplus sys as sysdba > 비밀번호 입력 > 엔터
비밀번호 입력해도 제대로 안나오는걸 확인할 수 있음 불편하죠?? 어쩔수 없어 왜? 보안 때문에
sql> 나오면 성공
 
## 오라클DBMS버전
i:internet -> 인터넷이 막 보급되면서 내pc에만 저장이 되는게 아니라 인터넷을 따라가서 다른컴퓨터에서 데이터 교환을 할 수 있다.<br>
g: grid -> 바둑판 데이터를 표로 만들어서 관리할거다<br>
c: cloud -> 서버를 두더라도 가상서버를 만들어서 조금씩관리할 수 있는것. 점점 클라우드를 많이 사용하는 이유는 데이터의 용량이 커지기 때문<br>

각각에 버전에서 어필하고 싶은 부분을 뒤에다 써놓은것.<br>

홀수 : 안정화된 버전<br>
짝수 : 테스트버전<br>

## 계정
대표적으로 세가지로 분류할 수 있다.

각각의 계정별로 목적이 다르다. 개발자들은 실무에서 일반계정을 사용할 거고 db를 좀더 전문적으로 배워서 dba라는 직종으로 가게 되면 관리자 계정으로 업무를 하게 될 것이다.<br>
- sys : 모든 명령어에 대한 권한을 갖고 있는 계정, DB생성 및 삭제 가능<br>
- system : 권한은 sys와 동일하지만 DB를 생성할 권한이 없다.<br>
- 일반 계정 : 해당 스키마 관리(hr, op, he, scott,...)<br>

## DBeaver IDE 설치하기
- IDE : 통합 개발 환경(IDE)이란 프로그래머가 소프트웨어 코드를 효율적으로 개발하도록 돕는 소프트웨어 애플리케이션입니다.<br> 이는 소프트웨어 편집, 빌드, 테스트, 패키징과 같은 기능을 사용하기 쉬운 하나의 애플리케이션에 통합하여 개발자 생산성을 높입니다.

※ DBeaver는 JDK 가 설치되어있지 않으면 작동을 하지 않는다.

## jdk 다운로드
jdk 1.8버전 다운로드<br>
환경변수 설정<br>
지금 로그인한 계정<br>
전체 계정에 대한 설정 > 우리가 하려는 것<br>
제어판 -> 시스템 및 보안 -> 시스템 -> 고급 시스템 설정<br>
환경변수 -> 시스템 변수 -> 새로만들기<br>
변수이름 : JAVA_HOME<br>
변수 값 :  자바가 설치된 경로<br>
path 더블클릭 밑에 빈공간 더블클릭 -> %JAVA_HOME%\bin<br>

cmd javac -version ,   java –version<br>

DBeaver 홈페이지 : https://dbeaver.io/

![image](https://user-images.githubusercontent.com/54658614/230538837-c45cd02a-6a1c-40f6-aea6-8a4963a98544.png)

최신버전은 어떤 기능이 바뀌었는지 파악하기 힘들기 때문에 수업을 할 때는 구버전인 5.2.5 버전을 사용할 것이다.<br>

스크롤을 아래로 내려서 이전버전을 받을 수 있는 아카이브로 들어가기

![image](https://user-images.githubusercontent.com/54658614/230538904-603b27c8-c8ba-45f7-b5c6-67375a0f96de.png)

다운로드 받고싶은 버전을 찾아서 들어간다.

![image](https://user-images.githubusercontent.com/54658614/230538950-2bc68b34-8342-4dc5-b197-14cef9b19079.png)

본인의 운영체제에 맞게 다운로드를 해준다.

![image](https://user-images.githubusercontent.com/54658614/230539001-37599182-a5e1-483a-8f0e-b366df1c94d0.png)

util 폴더에 압축을 풀고 사용하면 된다.

![image](https://user-images.githubusercontent.com/54658614/230539081-19ddf391-5326-49f7-9cb9-79ef62a6b9c3.png)

처음 DBeaver를 켜면 다음과 같은 창이 뜬다.

![image](https://user-images.githubusercontent.com/54658614/230539138-9f00132d-1b55-4ece-a541-5a06cbd79309.png)

- 뜨지 않는다면 왼쪽 위에 플러그 모양 버튼을 눌러주자

![image](https://user-images.githubusercontent.com/54658614/230539187-6d7a4044-4694-4153-a7d6-a0e90affa78c.png)

빈칸을 다음과 같이 채워넣어주면 된다.

![image](https://user-images.githubusercontent.com/54658614/230539317-eb33e20e-c975-4591-b1c8-9e2c092d87e3.png)

아마 처음 Oracle을 설치했다면 hr 계정이 잠겨있을 것이다.

## hr 계정 잠금 풀기

win + r 키를 눌러 실행을 켜고 cmd를 입력하여 프롬포트를 연다. 

![image](https://user-images.githubusercontent.com/54658614/230539594-b6527235-5bf4-4639-b569-c856d5eb2ece.png)

- sqlplus : db접속하기
	- system 계정으로 로그인을 하자
	
![image](https://user-images.githubusercontent.com/54658614/230539678-2fd407ff-d9cc-4062-8d11-86f2a838d1d9.png)

- 계정 잠금 풀기 : Alter user 계정명 account unlock;
- 계정 비밀번호 설정하기 : alter user 계정명 identified by 비밀번호;

![image](https://user-images.githubusercontent.com/54658614/230539865-a33af57e-0cd6-4419-8e1b-e195be2d5d54.png)

디비버로 돌아와서 TestConnetion을 누르면 드라이버를 요구한다.<br>

### driver
Driver files -> Oracle 11g drivers 선택 driver는 무엇인가?? 내부와 외부를 연결해주는 통로.<br>
DBeaver와 서버를 연결을 해야 하는데 연결을 도와주는게 드라이버이다.<br>

Ojdbc6.jar 가 필요하다고 창이 뜬다.<br>
데이터베이스 깔았던 곳에 저게 있기 때문에 Add JARs oraclexe > app > product > 11.2.0 > server > jdbc> lib> ojdbc6.jar  Test Connetion > finish<br>

![image](https://user-images.githubusercontent.com/54658614/230539995-5daa712a-9c47-415a-8c71-69f1af30fa0b.png)

![image](https://user-images.githubusercontent.com/54658614/230540093-c5bf48ed-4662-4a82-8164-4f261564c4bc.png)

ok를 누르면 오라클과 DBeaver의 연결이 완료된다.

연결된 오라클을 펼쳐보면 여러가지 샘플 테이블들을 볼 수 있다.

![image](https://user-images.githubusercontent.com/54658614/230540232-2a4e0e59-3486-4d1f-a91b-aa27ac508ea2.png)

스크립트를 생성하여 여러가지 쿼리문들을 작성할 수 있다.

![image](https://user-images.githubusercontent.com/54658614/230540303-417e24ee-0ef6-4d32-bdca-57061b2bf603.png)

Select Data Source 창이 뜸 오라클 선택 F2 눌러서 이름 day00로 수업 날짜에 맞춰 변경<br>
글자가 잘안보인다면 windows > Preferences > font 검색 > Colors and Fonts > Basic > Text Font 더블클릭 > 글꼴과 크기 변경 가능

## SQL문
- 원하는 결과를 얻어오기 위해 DB에 요청하는 요청문장(Query문)
- SQL문장은 대소문자를 구별하지 않는다.
- 한줄 또는 여러줄에 걸쳐 입력하는 것이 가능
- SQL문장의 끝은 세미콜론(;)으로 맺어야 한다.

## SQL문장의 종류
1. DDL(Data Definition Language) : 데이터 정의어
	- create, alter, drop등의 키워드를 가지는 문장
2. DML(Data Manipulation Language) : 데이터 조작어 사실상 가장 많이 사용하는 쿼리문이 될것! 
	- select, insert, update, delete를 가지는 문장
3. DCL(Data Controll Language) : 데이터 제어어
	- grant, revoke등의 키워드를 가지는 문장
4. TCL(Transaction Controll Language) : 트랜잭션 제어어(트랜잭션을 제어하는 구문)
	- commit, rollback, savepoint

html,파이썬이 있다.<br>
한줄 씩 번역되고 빈번한 수정이 있을 때 효율적이다.<br>
전체를 실행하지 않고 부분만 실행하고 싶을 때에도 편리한 언어이다.<br>
일괄처리를 할 때에는 컴파일 언어(c,java,c++)보다 효율이 떨어진다.<br>

### dbms 소통방식

```
사용자
-------------------------------------
	↕	↕	↕
고객관리	     ↕	    주문관리
응용 프로그램	  ↕	  응용프로그램
    ↕		↕   	   ↕
-------------------------------------
dbms
-------------------------------------
```

## 스키마
- 스키마란?
1. 스키마는 데이터베이스의 구조와 제약 조건에 관한 전반적인 명세를 기술한 메타데이터의 집합이다.
2. 스키마는 데이터베이스를 구성하는 데이터 개체(Entity), 속성(Attribute), 관계(Relationship) 및 데이터 조작 시 데이터 값들이 갖는 제약 조건 등에 관해 전반적으로 정의한다.
3. 스키마는 사용자의 관점에 따라 외부 스키마, 개념 스키마, 내부 스키마로 나눠진다.

![image](https://github.com/to7485/DBMS1900/assets/54658614/ace7ecdc-7205-4dd6-af09-b51108f5d130)

- 스키마의 특징
1. 스키마는 데이터 사전(Data Dictionary)에 저장되며, 다른 이름으로 메타데이터라고도 한다.
2. 스키마는 현실 세계의 특정한 한 부분의 표현으로서 특정 데이터 모델을 이용해서 만들어진다.
3. 스키마는 시간에 따라 불변인 특성을 갖는다.
4. 스키마는 데이터의 구조적 특성을 의미하며, 인스턴스에 의해 규정된다.

### 스키마의 종류
- 스키마는 사용자의 관점에 따라서 외부, 개념, 내부 스키마로 구분하게 됩니다.

1. 개념스키마 : 전체적인 뷰
- 조직체 전체를 관장하는 입장에서 DB를 정의한 것
- 관계, 제약조건, 접근권한, 보안정책, 무결성 구칙에 관한 사항을 포함하고 있다.
- 따라서 개념스키마를 '스키마'라고 칭하기도 하며, DB전체를 기술한것이기 때문에 한 개밖에 존재할 수 없다.

2. 내부스키마 : 물리적인 저장장치 입장에서 DB가 저장되는 방법을 기술한 것
- 구체적인 개념으로 스키마를 디스크 기억장치에 물리적으로 구현하기 위한 방법을 기술한 것
- 데이터베이스의 물리적 저장구조를 정의
- 디스크에 어떤 구조로 저장할 것인가
- 데이터의 실제 저장방법을 기술
- 물리적인 저장장치와 밀접한 계층
- 시스템 프로그래머나 시스템 설계자가 보는 관점의 스키마

3. 외부스키마 : 사용자 뷰
- 사용자나 응용 프로그래머가 개인의 입장에서 필요한 데이터베이스의 논리적 구조를 정의
- 실세계에 존재하는 데이터들을 어떤 형식, 구조, 배치 화면을 통해 사용자에게 보여줄 것인가
- 전체 데이터베이스의 한 논리적 부분 -> 서브 스키마
- 하나의 데이터베이스에는 여러 개의 외부 스키마가 존재할 수 있다.
- 같은 데이터베이스에 대해서도 서로 다른 관점을 정의할 수 있도록 허용
- 일반 사용자는 질의어를 이용 DB를 쉽게 사용


스키마를 누가 관리한다? 여러분이 로그인한 hr 계정<br>

## 테이블
- 행과 열로 이루어진 데이터의 집합을 테이블이라고 한다. 엑셀을 켜고 보는 모양과 매우 흡사하다.
- 일반적인 데이터베이스에서는 행과 열만 있으면 테이블이라고 하지만, 관계형데이터베이스에서는 특별한 제약을 추가해서 릴레이션(relation)이라고 부른다.

### 행(row)
- 테이블을 구성하는 데이터들 중 가로로 묶은 데이터셋을 의미한다. 일반적으로 한 명에 대한 정보를 가지고 있다.
- 이 또한 관계형 데이터베이스에서는 튜플(tuple), 또는 레코드(record)라는 이름으로 불린다.
- 행의 수를 카디널리티 라고 한다.

### 열(column)
- 테이블을 구성하는 데이터들 중 세로로 묶은 데이터넷을 의미한다. 일반적으로 열은 그 테이블의 속성을 의미한다.
- 열을 구성하는 값들은 같은 도메인(domain)으로 되어있다. 이 또한 관계형 데이터베이스에서는 속성(attribute)라는 이름으로 불린다.
- 열의 수를 디그리(degree)라고 한다.

※ 도메인(domain) : 데이터베이스에서 필드에 채워질수 있는 값들의 집합이다.
- 예를들어 성별을 저장해야 하는데 다른것이 오면 안된다.

## 스크립트 생성하기

Scripts -> control + ]<br>

Select Data Source 창이 뜸 오라클 선택 F2 눌러서 이름 day00로 수업 날짜에 맞춰 변경<br>
글자가 잘안보인다면 windows > Preferences > font 검색 > Colors and Fonts > Basic > Text Font 더블클릭 > 글꼴과 크기 변경 가능<br>

폰트 크기는 cntl + ,- 로 변경 가능<br>

### 조회하기
SELECT * FROM EMPLOYESS;

사람의 언어이고 인터프리터가 컴퓨터의 언어로 바꿔준다. cntl + enter

마치 액셀같다.<br>
만들어져있는 테이블을 데이터베이스를 집중적으로 공부하는 사람들은  이런걸 처음부터 설계하고 튜닝이라는 작업을 할것이다.<br>
개발자들은 누가 로그인을 하고 마이페이지를 눌렀네? 그러면 그 사람에 대한 이름이나 전화번호 같은 모든 정보를 다 출력해주는 것 정도 할 줄 알면 된다.<br>
