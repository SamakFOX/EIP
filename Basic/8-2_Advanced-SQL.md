# 8-2. 고급 SQL 작성  

## 목차
- [1. INDEX](#INDEX)
- [2. VIEW](#VIEW)
- [3. SubQuery](#SubQuery)

<a id="INDEX"></a>

---
### **♣ 인덱스 (INDEX)**  
---  

· 인덱스는 자료를 빠르고 효율적으로 검색하기 위해 사용한다.  
· 위치(주소)를 관리,기억하는 [**인덱스파일**]과 실제 데이터를 기억하는 [**데이터파일**]로 구성된다.  
· 데이터를 검색할때는 먼저 인덱스파일에서 주소를 찾고 주소에 있는 데이터를 읽는다.  

· 기본인덱스 (Primary Index) : 기본키 속성으로 만든 인덱스  
· 보조인덱스 (Secondary Index) : 일반 속성으로 만든 인덱스  
> 인덱스는 [ 키값, 주소 ] 쌍으로 저장된다.  

[ 장점 ] 검색 속도 향상, 시스템 부하 감소  
[ 단점 ] 추가 DB 공간 필요, 잦은 변경작업으로 인한 성능저하  
> 변경이 잦고 양이 많은 데이터에는 인덱스를 만들지 않는 것이 성능상 유리하다.  

### ① 인덱스 개념  

<참고> {} : 반복, [] : 생략가능, | : 선택  
```sql
CREATE [UNIQUE] INDEX 인덱스명 ON 테이블명 (속성명 ASC | DESC);
```
★ UNIQUE : 중복값 없이 모든 속성값이 고유값을 갖도록 지정 (해당 속성이 이미 unique거나, 중복이 상관없을때는 생략 가능)  

| CREATE문 인덱스 생성 예시 |
|---|

```sql
CREATE UNIQUE INDEX Student_idx ON Student(st_num ASC);
```

### ② 인덱스 구조  
2-1. B-트리 : 효율 증대를 위해 균형있는 트리구조 차용 → Balanced Tree Index  
![B-Tree](https://github.com/SamakFOX/EIP/blob/main/ExampleImages/B-Tree.png)
★ 검색은 루트노드에서부터 시작한다. → 업다운방식  
★ 모든 리프노드(단말노드)는 같은 레벨에 있다.  
★ 차수가 m일때 루트노드와 리프노드를 제외한 모든 노드는 최소 m/2개 최대 m개의 서브트리를 갖는다.  

2-2. B<sup>+</sup>-트리 : B 트리의 변형으로, 인덱스세트와 순차세트가 있음  
![B+-Tree](https://github.com/SamakFOX/EIP/blob/main/ExampleImages/B%2B-Tree.png)
★ 순차세트의 단말노드에는 모든 키 값이 다시 나타나므로, 단말노드 만으로도 순차 검색이 가능  

2-3. 클러스터드 인덱스 (Clustered Index) : 하나의 속성으로 정렬 후 테이블을 재구성하여 인덱스 생성  
![ClusteredIndex](https://github.com/SamakFOX/EIP/blob/main/ExampleImages/ClusteredIndex.png)
※ 진행방식 : 정렬 → 테이블 재구성 → Page 분리 → 인덱싱  
※ 테이블을 재구성하므로, 한 테이블에 한개의 인덱스만 생성 가능  

2-4. 넌 클러스터드 인덱스 (Non Clustered Index) : 테이블을 재구성하지 않고, 데이터 주소로 인덱스 생성  
![NonClusteredIndex](https://github.com/SamakFOX/EIP/blob/main/ExampleImages/NonClusteredIndex.png)
※ 진행방식 : Page 분리 → ROW ID로 구성된 인덱스 생성 → Root 인덱스 생성  

★ 클러스터와 넌 클러스터  
검색 방식은 동일 (최상위 인덱스에서 검색하고 하위 페이지에서 데이터 탐색)  
테이블 재구성 여부만 다름 (N-CI는 인덱스와 루트인덱스 2개 생성)  

### ③ 인덱스 정의어 : DDL문 사용
　**1. 인덱스 생성**
```ts
CREATE [UNIQUE] INDEX 인덱스명 ON 테이블명 (속성명 [ASC | DESC]) [CLUSTER];
```
★ UNIQUE : 중복값 없이 모든 인덱스값이 고유값을 갖도록 지정  
★ ON__(__) : 인덱스를 만들 속성을 지정 → 복합인덱스 가능 (ON 학생(이름,점수))  
★ ASC/DESC : 오름차순/내림차순  
★ CLUSTER : 선언 시 클러스터드 인덱스로 설정  
※ 검색의 효율화를 위해 클러스터드가 지향되나, 상황에따라 판단  

　**2. 인덱스 제거**  
```ts
DROP INDEX 인덱스명;
ALTER TABLE 테이블명 DROP 인덱스명;
```
★ DROP INDEX : 독립 인덱스 제거  
★ ALTER ~ DROP : 종속 인덱스 제거  
```sql
-- '이름' 인덱스를 제거
DROP INDEX idx_st_name;
-- PK 인덱스, PK 제약조건 모두 제거
ALTER TABLE Student DROP PRIMARY KEY;
-- UNIQUE 인덱스 제거
ALTER TABLE Student DROP CONSTRAINT uk_phone;
```

　**3. 인덱스 수정** : 기존 인덱스를 DROP 후 재생성함  
```ts
ALTER [UNIQUE] INDEX 인덱스명 ON 테이블명 (속성명 [ASC | DESC]);
```
※ 인덱스를 수정하는 경우는 매우 드물며, 일부 DBMS는 수정쿼리를 제공하지 않음 → 출제가능성 낮음 (이해만)  

&nbsp;

<a id="VIEW"></a>

---
### **♣ 뷰 (VIEW)**  
---  
· 뷰는 하나 이상의 테이블로부터 유도되어 만들어진 가상테이블  
· 물리적 기억공간을 차지하지 않으며, 논리적 독립성을 제공  
· SELECT문은 사용자가 검색한 일부를 보여주기 위한 임시테이블 → 이후에도 사용하려면 뷰를 직접 생성  

### ① 뷰 생성  
```ts
CREATE VIEW 뷰명 [(뷰_속성명)]
AS SELECT 기본테이블_속성명
[WHERE 조건]
[WITH CHECK OPTION];
```
★ 기본테이블의 속성과 뷰 속성명은 다르게 부여할 수 있음  
★ WITH CHECK OPTION : 뷰의 생성, 수정, 삽입 시 WHERE 조건에 맞지 않으면 실행되지 않도록 함  
```sql
-- (id, name, contact) 속성으로 'ThirdGradeContact' 뷰 생성, Student 테이블에서 3학년만 필터링
CREAT VIEW ThirdGradeContact (id, name, contact)
AS SELECT st_id, st_name, st_phone FROM Student WHERE SUBSTR(st_id, 1, 2) <= '22';
-- Student 테이블 기본속성대로 'ThirdGradeContact' 뷰 생성, 결과는 동일하나 속성명이 달라짐
CREAT VIEW ThirdGradeContact 
AS SELECT st_id, st_name, st_phone FROM Student WHERE SUBSTR(st_id, 1, 2) <= '22';
```

### ② 뷰 삭제
```ts
DROP VIEW 뷰명 [RISTRICT | CASCADE];
```
★ RISTRICT : 참조(이용)중이면 삭제하지 않음  
★ CASCADE : 참조(이용)중이라도 삭제  
```sql
-- example을 참조하는 뷰 A 생성
CREATE VIEW A AS SELECT * FROM example;
-- A를 참조하는 뷰 B 생성
CREATE VIEW B AS SELECT ex1 FROM A;
-- 뷰 A를 연쇄삭제 (A B 모두 삭제됨)
DROP VIEW A CASCADE;
```

### ③ 뷰 특징
　- 기본테이블이 제거되면 뷰도 제거됨  
　- 뷰 속성에 기본테이블의 기본키가 포함되어있지 않으면 삽입, 삭제, 갱신이 되지 않음  
　- ALTER로 변경 불가  
　- 한번 정의된 뷰는 변경이 불가, 삭제 후 재생성해야함  

### 시스템 카탈로그 (System Catalog)  

> DB 내 개체들에 대한 정보와, 정보들 간의 관계를 저장한 것  

★ 데이터 사전(Data Dictionary) 라고도 한다  
★ 사용자가 SQL문을 실행하면 시스템(DBMS)에 의해 자동 갱신  

&nbsp;

<a id="SubQuery"></a>

---
### **♣ 다중 테이블 검색**  
---  

### ① 부속(하위) 질의 : 서브쿼리  
· 서브쿼리 결과값은 메인쿼리로 반환되어 사용  
· 서브쿼리는 메인쿼리가 실행되기 이전에 한 번만 실행  
· 서브쿼리가 반환하는 행 수에 따라 <mark>단일 행 서브쿼리</mark>와 <mark>다중 행 서브쿼리</mark>로 나뉨  
· 서브쿼리는 비교연산자의 오른쪽에 기술하며, 반드시 소괄호 () 안에서 사용  
· 서브쿼리는 ORER BY 절을 사용 X → 서브쿼리는 정렬 X  

### 1-1. 단일 행 서브쿼리 (Single Row) : 결과로 오직 하나의 행(row)만 반환  
> 메인쿼리의 WHERE절에서는 단일 행 비교연산자 사용  
> (=, <>, >, >=, <, <=)  

| 연산자 | 해석 |
|---|---|
| = | 같다 |
| <> | 다르다 |
| > | 초과 |
| >= | 이상 |
| < | 미만 |
| <= | 이하 |

```sql
-- 최설이 학생의 학과와 동일한 학과생들을 성적 테이블에서 검색
SELECT * FROM Grade
WHERE dept_id = (
  SELECT dept_id FROM Grade WHERE name = '최설이'
);
-- 최설이 학생의 점수보다 높은 학과생들을 성적 테이블에서 검색
SELECT * FROM Grade
WHERE score > (
  SELECT score FROM Grade WHERE name = '최설이'
);
```

### 1-2. 다중 행 서브쿼리 (Multiple Row) : 결과로 여러 행(row) 반환
> 메인쿼리의 WHERE절에서는 단일 행 비교연산자 사용  
> (IN, ANY/SOME, ALL, EXISTS)  

| 연산자 | 해석 |
|---|---|
| IN | 하나라도 조건을 만족하면 참 |
| ANY | 하나 이상 조건을 만족하면 참 |
| SOME | 하나 이상 조건을 만족하면 참 |
| ALL | 모든 값이 조건을 만족하면 참 |
| EXISTS | 서브쿼리 결과가 존재하면 참 |

※ 참일 경우 해당 행 리턴

```sql
-- 점수 80점 이상인 학과(dept_id)에 속한 모든 학생 검색
SELECT * FROM Grade
WHERE dept_id IN (
  SELECT dept_id from Grade WHERE score >= 80
);
-- IN, =ANY 동일결과
SELECT * FROM Grade
WHERE dept_id = ANY (
  SELECT dept_id from Grade WHERE score >= 80
);
-- '최설이' 학생의 점수 중 하나 이상보다 크거나 같은 학생 검색
SELECT * FROM Grade
WHERE score >= ANY (
  SELECT score FROM Grade WHERE name = '최설이'
);
-- 특정 학과의 1등을 검색, 서브쿼리 (10013학과 인원 모두보다 큰 점수 검색)
SELECT * FROM Grade
WHERE dept_id = 10013
  AND score >= ALL (
    SELECT score FROM Grade WHERE dept_id = 10013
);
```

### ② 조인 (JOIN)
> 둘 이상의 테이블로브ㅜ터 특정 공통값을 갖는 행을 연결하거나 조합  
> **DBMS에서 매우 중요한 연산**  
> **정규화된 테이블을 JOIN해서 사용**

### 1-1. 조인의 종류
　· 논리적 조인 : SQL 작성 시 사용하는 알고리즘 (Inner, Outer, Self, Cross)   
　· 물리적 조인 : DBMS가 실제 쿼리를 실행할 때 선택하는 처리 알고리즘 (Nested Loop, Sort-Merge, Hash)  

| 알고리즘 | 방식 |
|---|---|
| 내부 조인<br>(Inner Join) | 동등조인 (Equi Join) : 동일 컬럼을 기준으로 조합<br>비동등조인(Non-Equi Join) : 동일 컬럼 없이 다른 조건 이용 |
 
