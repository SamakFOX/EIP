# 8-1. SQL 응용  

---
### **DDL (Data Definition Language) : 데이터 정의어**  
---  

### ① CREATE : 테이블/스키마/도메인/인덱스 정의  

### · 테이블 정의
<참고> {} : 반복, [] : 생략가능, | : 선택  
```ts
CREATE TABLE 테이블명 (
{속성명 데이터타입 [NOT NULL], }
  [PRIMARY KEY (속성명),]
  [UNIQUE (속성명),]
  [FOREIGN KEY (속성명) REFERENCES 참조테이블명(속성명)]
    [ON DELETE CASCADE | SET NULL | SET DEFAULT | NO ACTION]
    [ON DELETE CASCADE | SET NULL | SET DEFAULT | NO ACTION]
  [CONSTRAINT 제약조건명 CHECK (속성명=범위값)]
);
```
★ PRIMARY KEY (속성명) : 기본키 지정 (개체 무결성)  
★ UNIQUE (속성명) : 중복값 없이 모든 속성값이 고유값을 갖도록 지정  
★ FOREIGN KEY (속성명) REFERENCES 참조테이블명(속성명) : 외래키 지정 (참조 무결성)  
★ CONSTRAINT 제약조건명 CHECK (속성명=범위값) : 특정 속성에 범위 지정 (도메인 무결성)  
★ CREATE TABLE | VIEW AS SELECT ~ : 뒤 서브쿼리 결과로 신규 테이블/뷰 생성  

| 데이터 타입 |
|---|
| INT : 정수<br>FLOAT : 실수<br>CHAR : 고정길이 문자<br>VARCHAR : 가변길이 문자<br>TIME : 시간<br>DATE : 날짜 |

| 명명규칙 | 특이사항 |
|---|---|
| 영문자, 숫자, 밑줄 사용 가능 | 첫 글자는 영문자 |
| 공백, 특수문자 사용 불가 | |
| 대소문자 구분 없음 | 소문자 권장 |
| SQL 예약어 사용 불가 | ex) select, table 등 |

&nbsp;
| CREATE문 테이블 생성 예시 |
|---|
> 컬럼 속성의 경우 컬럼 선언시에 설정해도 되고 별도로 설정해도 된다.  
> 여러 컬럼의 조합에 속성을 줄 때에는 별도로 설정해야 함.  
> ex) UNIQUE (name, phone) -> (이름과 핸드폰번호)의 조합이 중복이 없어야 할 경우.  
```sql
CREATE TABLE Student (
    st_id INT NOT NULL,
    st_name VARCHAR(50) NOT NULL,
    st_phone CHAR(13),
    dept_id INT,
    age INT,
    
    PRIMARY KEY (st_id),
    UNIQUE (st_phone),
    
    FOREIGN KEY (dept_id) REFERENCES Department(dept_id)
        ON DELETE SET NULL
        ON UPDATE CASCADE,
    
    CONSTRAINT chk_age CHECK (age >= 0)
);
```
또는
```sql
CREATE TABLE Student (
    st_id INT NOT NULL PRIMARY KEY,
    st_name VARCHAR(50) NOT NULL,
    st_phone CHAR(13) UNIQUE,
    dept_id INT,
    age INT CHECK (age >= 18),
    
    FOREIGN KEY (dept_id) REFERENCES Department(dept_id)
        ON DELETE SET NULL
        ON UPDATE CASCADE
);
```

★ AS : 뒤 서브쿼리 결과로 신규 테이블 생성  
```ts
CREATE TABLE AS SELECT ~
```
| AS 활용 예시 |
|---|

```sql
CREATE TABLE Adult_Student AS
SELECT st_id, st_name, st_phone FROM Student WHERE age >= 20;
```
&nbsp;
### · 스키마 정의
스키마 : [ 데이터베이스 ]-[ 스키마 ]-[ 테이블/뷰/인덱스 ] 구조에서 용도별 영역을 구분하기 위해 사용됨  

스키마를 분리하고 권한을 주기 위해 생성  
(ex> CREATE SCHEMA sales; CREATE TABLE sales.orders(...); GRANT SELECT ON SCHEMA sales TO s_team;)

CREATE SCHEMA : 사용자에게 DB 구축에 필요한 권한을 주기 위한 스키마를 만들 때 사용  
(ex> GRANT ~ ON SCHEMA ~ TO ~ ;)  
```ts
CREATE SCHEMA 스키마명 AUTHORIZATION 사용자명;
```
★ AUTHORIZATION : 스키마에 대한 권한 지정 (스키마 주인)  
&nbsp;&nbsp;&nbsp;&nbsp;→ 생성/수정/삭제에 대한 권한을 가짐  
&nbsp;&nbsp;&nbsp;&nbsp;→ 다른 사용자에게 권한을 줄 수 있음  

| 스키마 생성 예시 |
|---|

```sql
CREATE SCHEMA university AUTHORIZATION univmaster;
```
&nbsp;
### · 도메인 정의
도메인 : 특정 속성이 가질 수 있는 값의 범위를 지정하기 위해 사용됨  
※ 표준SQL에서만 사용하고 대부분 DBMS에서 미지원하므로 이해만.  

```ts
CREATE DOMAIN 도메인명 데이터타입
  [DEFAULT 기본값]
  [CONSTRAINT 제약조건명 CHECK(VALUE IN (범위값))];
```

| 도메인 생성 예시 |
|---|

```sql
CREATE DOMAIN GenderType CHAR(1)
  DEFAULT 'N'
  CONSTRAINT gender_chk CHECK (VALUE IN ('M', 'F', 'N'));
```
성별을 남/여/미선택 세가지만 들어오도록 지정  
&nbsp;
### · 인덱스 정의
인덱스 : 테이블의 검색 속도를 빠르게 하기 위해 데이터의 위치 정보를 저장한 자료 구조  
※ [ 정렬된 키(key) + 실제 데이터 주소] 로 구성되며, 시스템에서 자동적으로 관리됨  

```ts
CREATE [UNIQUE] INDEX 인덱스명
  ON 테이블명 (속성명 [ASC | DESC])
  [CLUSTER];
```
★ UNIQUE : 중복 없이 고유값을 갖도록 설정  
★ ON 테이블명 : 선언한 테이블의 속성으로 인덱싱  
★ ASC/DESC : 오름차순/내림차순  
★ CLUSTER : 클러스터드 인덱스로 설정 (실제 테이블을 인덱스를 기준으로 재정렬)  

| 인덱스 생성 예시 |
|---|

```sql
CREATE UNIQUE INDEX idx_phone
  ON Student (st_phohe ASC);
```

---
### **DML (Data Manipulation Language) : 데이터 제어어**  
---  
