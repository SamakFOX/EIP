# 8-4. 절차형 SQL 작성

## 목차
- [1. 절차형 SQL](#PLSQL)
- [2. 저장 프로시저](#StoredProcedure)
- [3. 사용자 정의 함수](#UserFunc)
&nbsp;

<a id="PLSQL"></a>

---
### **♣ 절차형 SQL (Procedural SQL)**  
---  

### ① 절차형 SQL
&nbsp;- 종류 : 프로시저, 사용자 정의 함수, 트리거  
&nbsp;- 필수 구성요소 : DECLARE, BEGIN, END  
&nbsp;- 블록의 DECLARE에 의해 선언되는 절차형 SQL과 CREATE/DROP에 의해 저장 및 제거되는 절차형 SQL  

&nbsp;
### ② PL/SQL (Procedural Language Extension to SQL)
&nbsp;- SQL을 확장한 절차적 언어  
&nbsp;- 사용자가 PL/SQL 블록을 보내면 서버에서 PL과 SQL을 분리하여 처리  
&nbsp;- 블록단위 실행 (BEGIN ~ END;/)  
&nbsp;- 변수, 상수 선언 가능, 제어문 구현과 예외처리 가능  

&nbsp;
### ③ PL/SQL 기본블록 구조와 변수  
| 구문 | 의미 | 필수여부 | 사용방법 |
|---|---|---|---|
| DECLARE | 선언부 | 선택절 | 변수와 상수 선언 (블록 내 사용 → 지역변수) |
| BEGIN | 실행부 | 필수절 | 명령문 |
| EXCEPTION | 예외처리부 | 선택절 | 예외처리 명령문 |
| END; | 종료 | 필수절 | 실행문 종료 예약어 (마지막 라인에 /를 입력해 종료) |

&nbsp;
### ④ 선택문  
<참고> {} : 반복, [] : 생략가능, | : 선택  
```ts
-- IF-ELSE
IF 조건식 THEN 명령문; ···
[ELSE IF 조건식 THEN 명령문; ···]
[ELSE IF 조건식 THEN 명령문; ···]
[ELSE 명령문; ···]
END IF;
-- CASE (BREAK 없이도 동일하게 동작)
CASE 변수명
[WHEN 레이블 THEN 명령문;]
[WHEN 레이블 THEN 명령문;]
[ELSE 명령문;]
END CASE;
```

&nbsp;
### ⑤ 반복문  
&nbsp;
**5-1. LOOP**  
&nbsp; · EXIT는 무한루프 무조건 탈출  
&nbsp; · 조건식이 붙으면 조건식이 참일 때 탈출  
```ts
LOOP
  명령문;
  명령문;
  ···
  EXIT [WHEN 조건식]
END LOOP;
```
&nbsp;
**5-2. WHILE**  
&nbsp; · 조건식이 참일 경우에만 내부 블록 실행  
```ts
WHILE 조건식 LOOP
  명령문;
  명령문;
  ···
END LOOP;
```
&nbsp;
**5-3. FOR-IN**  
&nbsp; · 첨자변수가 시작값부터 1씩 증가하여 끝값이 될 때까지 내부 블록 실행  
```ts
FOR 첨자변수 IN [REVERSE] 시작값 .. 끝값 LOOP
  명령문;
  명령문;
  ···
END LOOP;
```
```sql
-- 1부터 5까지 돌며 i 출력
BEGIN
  FOR i IN 1..5 LOOP
    DBMS_OUTPUT.PUT_LINE(i);
  END LOOP;
END;
-- 사원 테이블을 돌며 이름:급여 를 출력
BEGIN
  FOR i IN Employee LOOP
    DBMS_OUTPUT.PUT_LINE(i.em_name || ' : ' || i.em_salary);
  END LOOP;
END;
```

&nbsp;
### ★ 프로시저를 통한 출력
```sql
-- 출력 가능하도록 설정
SET SERVEROUTPUT ON;
-- 내용 출력
BEGIN
  < PL/SQL문 >
  ···
  DBMS_OUTPUT.PUT_LINE( 출력 내용 );
END;
/
```

&nbsp;
<a id="StoredProcedure"></a>

---
### **♣ 저장 프로시저 (Stored Procedure)**  
---  

### ① 저장 프로시저 : DB에 저장된 사용자의 PL/SQL 명령문  
&nbsp; · CREATE PROCEDURE로 프로시저 생성  
&nbsp; · DECLARE로 생성한 프로시저와 다르게 여러번 반복 호출/사용 가능  
```ts
CREATE [OR REPLACE] PROCEDURE 프로시저명 (
  매개변수명 [모드] 자료형,
  매개변수명 [모드] 자료형, ···
)
IS
  지역변수 선언문;
BEGIN
  명령문;
  명령문;
END;
```
※ 모드 : IN / OUT / INOUT - 데이터 흐름에 따라 입출력모드 설정  

&nbsp;
### ② 저장 프로시저 실행, 제거  
```ts
EXECUTE 프로시저명('매개변수');
DROP PROCEDURE 프로시저명;
```
```sql
EXECUTE del_procedure('최설이');
DROP PROCEDURE del_procedure;
```
&nbsp;
<a id="UserFunc"></a>

---
### **♣ 사용자 정의 함수 (User Function)**  
---  

### ① 사용자 정의 함수 생성  
&nbsp; ★ 결과를 되돌려받기 위해 반환자료형과 값을 기술해야 함  
```ts
CREATE [OR REPLACE] FUNCTION 함수명 (
  매개변수명 [모드] 자료형,
  매개변수명 [모드] 자료형, ···
)
RETURN 반환형
IS [AS]
  지역변수 선언문;
BEGIN
  명령문;
  명령문;
  ···
  RETURN 반환값;
END;
```
★ OR REPLACE : 동일 이름일 경우 새 내용으로 교체  
★ 모드 : IN 매개변수만 사용 가능  
★ RETURN 반환형 : 필수절, 리턴될 결과의 자료형  
★ RERUTN 반환값 : 생략불가  

&nbsp;
### ② 호출 및 출력  

**2-1. 함수명으로 호출**  
```sql
VARIABLE var_rst NUMBER;
EXECUTE :var_rst := BONUS(1100);
PRINT var_rst;
```
&nbsp;- 자료형이 NUMBER인 var_rst 변수 생성  
&nbsp;- BONUS(1100)을 호출하여 반환값을 var_rst에 저장
&nbsp;- var_rst를 출력  

**2-2. DML에서 호출**  
```sql
SELECT BONUS(1100) FROM DUAL;
```

&nbsp;
### ③ 제거  
```sql
DROP FUNCTION BONUS;
```
&nbsp;
<a id="UserFunc"></a>

---
### **♣ 이벤트와 트리거 (Event & Trigger)**  
---  

### ① 트리거
&nbsp; - DB에 이벤트가 발생할 때마다 자동으로 수행되는 저장 프로시저  
&nbsp;&nbsp;&nbsp;&nbsp; ㄴ DML에 의해 수행되는 '데이터 조작어 기반 트리거'  
&nbsp;&nbsp;&nbsp;&nbsp; ㄴ DDL에 의해 수행되는 '데이터 정의어 기반 트리거'  

&nbsp; ★ 행 트리거 (Row-Level Trigger) : 행에 변화가 생길 때 수행, FOR EACH ROW 욥션  
&nbsp; ★ 문장 트리거 (Statement-Level Trigger) : 이벤트에 의해 단 한번만 수행  
```ts
CREATE [OR REPLACE] TRIGGER 트리거명
{BEFORE | AFTER}
이벤트 [OR 이벤트] ON 테이블명
[FOR EACH ROW]
[WHEN (조건식)]
[DECLARE 지역변수명 자료형;]
BEGIN
  명령문;
  명령문;
  ···
END;
/
```
★ BEFORE | AFTER : 실행 시점 - 이벤트 전/후  
★ 이벤트 : INSERT/UPDATE/DELETE - 여러개 사용 시 'OR'로 연결  
★ FOR EACH ROW : 해당 옵션이 존재하면 행트리거로 수행됨  
```sql
CREATE OR REPLACE TRIGGER raise_salary
BEFORE UPDATE OF em_salary ON Employee
FOR EACH ROW
BEGIN
  DBMS_OUTPUT.PUT_LINE('인상 전 급여 : ' || :OLD.SALARY);
  DBMS_OUTPUT.PUT_LINE('인상 후 급여 : ' || :NEW.SALARY);
END;
/
```
★ OLD,NEW : Oracle에서 FOR EACH ROW 수행 중 변경 전후 값을 기억하는 레코드  

&nbsp;
### ② 트리거 자동 수행
&nbsp; - INSERT, UPDATE, DELETE 수행 시 마다  
```sql
-- 출력 가능하도록 설정
> SET SERVEROUTPUT ON;
-- BEFORE 트리거 발동
> UPDATE Employee SET em_salary = em.salary * 1.1 WHERE dept_id = '교육부';
```

&nbsp;
### ③ 트리거 제거
```ts
DROP TRIGGER 트리거명;
```
```sql
DROP TRIGGER raise_salary;
```

&nbsp;
### 다음 글 계속 읽기 →
| 9-1. SW 개발 보안 설계 | [![읽어보기](https://img.shields.io/badge/읽어보기-blue?style=for-the-badge)](https://github.com/SamakFOX/EIP/blob/main/Basic/9-1_SWDev-SecureDesign.md)   |
|:---:|:---:|
