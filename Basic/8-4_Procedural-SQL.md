# 8-3. 응용 SQL 작성

## 목차
- [1. 절차형 SQL](#PLSQL)
- [2. ](#)
- [3. ](#)
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
