# 8-3. 응용 SQL 작성

## 목차
- [1. 집계함수](#AggregateFunc)
- [2. GROUP BY](#GroupBy)
- [3. 윈도우함수](#WindowFunc)
&nbsp;

<a id="AggregateFunc"></a>

---
### **♣ 집계함수 (Agregate Function)**  
---  

### ① 다중 행 함수
&nbsp;· 전체 또는 그룹별로 연관 및 계산분석을 한 단일 결과값 반환  
&nbsp;· GROUP BY 절에서는 기준 컬럼명  
&nbsp;· SELECT / HAVING 절에서는 집계함수  
&nbsp;· 분석을 위한 윈도우 함수는 SELECT 절에서 사용
&nbsp;
### ② 집계함수 (Aggregate Function)  
&nbsp;&nbsp;· 다중 행 함수로 여러 튜플(행)을 처리한 후 한 행의 결과값 반환  
&nbsp;&nbsp;· NULL값은 제외하고 처리  
★ 그룹함수 (Group Function)  
| 함수 | 해석 |
|---|---|
| SUM(컬럼명) | 합계 |
| AVG(컬럼명) | 평균 |
| COUNT(컬럼명) | 행 갯수 |
| MAX(컬럼명) | 최대값 |
| MIN(컬럼명) | 최소값 |
| STDDEV(컬럼명) | 표준편차 |
| VARIANCE(컬럼명) | 분산 |

```sql
-- 최설이 학생을 제외한 나머지 학생의 수학 총점을 검색
SELECT SUM(math) FROM Score WHERE name <> '최설이';
-- 과학 점수의 평균값 검색
SELECT AVG(science) FROM Score;
-- 갯수 카운트 + (별칭을 AS로 쓰는 경우, AS 없이 쓰는 경우)
SELECT COUNT(*) AS "학생 수", COUNT(math) "수학 응시생 수" FROM Score;
```
&nbsp;

<a id="GroupBy"></a>

---
### **♣ GROUP BY 절을 사용하는 그룹처리 함수**  
---  

### ① SELECT문 문법
```ts
SELECT [ALL | DISTICT]
  [테이블명.]컬럼명 [AS 별칭] [, [테이블명.]컬럼명 [AS 별칭], ···]
    [, 그룹함수(컬럼명) [AS 별칭]]
    [, 윈도우함수명 OVER (
        PARTITION BY 컬럼명, 컬럼명2, ···
        ORDER BY 컬럼명3, 컬럼명4, ···
        ) [AS 별칭]
    ]
FROM 테이블명 [, 테이블명 ···]
[WHERE 조건식]
[GROUP BY [ROLLUP | CUBE] (컬럼명, 컬럼명, ···)]
[HAVING 조건식]
[ORDER BY 컬럼명 [ASC | DESC]];
```
★ FROM은 필수절  
&nbsp;&nbsp;- 모든 절이 작성되었을 때 실행 순서 : FROM → WHERE → GROUP BY → HAVING → ORDER BY → SELECT  
★ ROLLUP / CUBE  
&nbsp;&nbsp;- ROLLUP : 주어진 컬럼 기준 그룹별 집계  
&nbsp;&nbsp;- CUBE : 주어진 컬럼 기준 모든 컬럼 조합의 그룹별 집계  
&nbsp;&nbsp;- GROUPING SETS : 주어진 컬럼에 대한 다양한 집계 집합  
&nbsp;
### ② ROLLUP 함수
&nbsp;· 주어진 컬럼 수보다 하나 더 큰 레벨로 반환  
&nbsp;&nbsp; → 매개변수로 주어진 컬럼수가 N일때 N+1 레벨로 반환  
&nbsp;! ROLLUP 함수의 컬럼들은 모두 SELECT절에 추가되어야 유의미한 결과 분석 가능  
```sql
-- 학과, 학년, 수학평균을 성적테이블에서 검색 + 학과&학년으로 ROLLUP
SELECT dept_id 학과, grade 학년, ROUND(AVG(math)) "수학 평균"
FROM Score
GROUP BY ROLLUP(dept_id, grade);
```
※ ROUND : 반올림하여 정수로 표현  
&nbsp;
| ROLLUP 예시 |
|---|

예시 데이터가 다음과 같을 때  
| dept_id | grade | math |
|---|---|---|
| 10 | 1 | 80 |
| 10 | 1 | 90 |
| 10 | 2 | 70 |
| 20 | 1 | 85 |

ROLLUP(dept_id, grade); 결과  
| 학과 | 학년 | 수학 평균 |
|---|---|---|
| 10 | 1 | 85 |
| 10 | 2 | 70 |
| 10 | NULL | 78 |
| 20 | 1 | 85 |
| 20 | NULL | 85 |
| NULL | NULL | 80 |

※ 학과번호와 학년이 모두 있는 행 : 학과의 학년 평균 : 1레벨  
※ 학과번호는 있으나 학년이 NULL인 행 : 학과 평균 : 2레벨  
※ 학과번호 학년 모두 NULL인 행 : 전체 평균 : 3레벨  
&nbsp;
### ③ CUBE 함수
&nbsp;· 결합 가능한 다차원적인 모든 조합에 따라 레벨이 결정됨  
&nbsp;&nbsp; → 매개변수로 주어진 컬럼 수가 N일때 2<sup>N</sup> 레벨로 반환  
&nbsp;★ UNION 연산을 반복수행하지 않고도 컬럼 조합에 대한 집계가 가능해 속도가 빠름  
&nbsp;! CUBE 함수의 컬럼들도 모두 SELECT 절에 추가되어야 유의미한 결과 분석 가능  
```sql
-- 학과, 학년, 수학평균을 성적테이블에서 검색 + 학과&학년으로 ROLLUP
SELECT dept_id 학과, grade 학년, ROUND(AVG(math)) "수학 평균"
FROM Score
GROUP BY CUBE(dept_id, grade);
```
| CUBE 예시 |
|---|

예시 데이터가 위 예시와 같을 때 CUBE(dept_id, grade); 결과  
| 학과 | 학년 | 수학 평균 |
|---|---|---|
| 10 | 1 | 85 |
| 10 | 2 | 70 |
| 20 | 1 | 85 |
| 10 | NULL | 80 |
| 20 | NULL | 85 |
| NULL | 1 | 85 |
| NULL | 2 | 70 |
| NULL | NULL | 81 |

※ 학과번호와 학년이 모두 있는 행 : 학과의 학년 평균 : 1레벨  
※ 학과번호는 있으나 학년이 NULL인 행 : 학과 평균 : 2레벨  
※ 학과번호가 NULL이고 학년이 있는 행 : 학년 평균 : 3레벨  
※ 학과번호 학년 모두 NULL인 행 : 전체 평균 : 4레벨  
&nbsp;
### ④ GROUPING SETS 함수
&nbsp;· 컬럼별 집계를 계산한 후 집계의 결과 튜플(행)만 출력  
&nbsp;· 인수로 주어지는 컬럼 순서와 상관없이 결과가 같다  
&nbsp;

<a id="WindowFunc"></a>

---
### **♣ GROUP BY 절을 사용하지 않는 윈도우함수**  
---  

### ① 윈도우함수 (Window Function) = 분석함수 (Analytic Function)  
&nbsp;· 윈도우라는 분석함수용 그룹을 정의하여 계산  
&nbsp;· SELECT 절에서만 사용 가능  
&nbsp;· OVER()절은 필수이며, OVER()절에 분석데이터를 기술하지 않으면 전체 행 대상 분석  
&nbsp;· 서브쿼리에서 사용 가능하나 중첩은 불가능  
```ts
SELECT 윈도우함수명 (컬럼명, 컬럼명 ···)
OVER(
  [PARTITION BY 컬럼명]
  [ORDER BY 컬럼명 ASC | DESC]
  [WINDOWING]
FROM 테이블명 ···;
```
### WINDOWING 구성 요소
| 요소 | 의미 |
|---|---|
| ROWS | 행 단위 기준 |
| RANGE | 값 범위 기준 |
| UNBOUNDED PRECEDING | 파티션의 첫 행 |
| CURRENT ROW | 현재 행 |
| n PRECEDING | 앞의 n행 |
| n FOLLOWING | 뒤의 n행 |

### 윈도우 함수 구성 요소
| 함수분야 | 함수명 |
|:---:|---|
| 집계 분석 | SUM(), AVG(), MAX(), MIN(), COUNT() |
| 순위 분석 | RANK(), DENSE_RANK(), ROW_NUMBER() |
| 순서 분석 | FIRST_VALUE(), LAST_VALUE(), LAG(), READ() |
| 그룹 비율 | CUME_DIST(), PERCENT_RANK(), NTILE(), RATIO_TO_REPORT() |
| 통계 분석 | STD_DEV(), VARIANCE() |

※ 순위 분석 함수는 대부분 DBMS에서 지원  
```sql
SELECT UNIQUE dept_id, gr_grade,
  COUNT(*) OVER(PARTITION BY dept_id, grade) 인원수
FROM Graduate
ORDER BY dept_id, gr_grade;
```
&nbsp;
### ② 순위 계산용 윈도우 함수  
&nbsp;**· RANK : 전체 또는 윈도우별 행의 순위를 구해줌**  
&nbsp;&nbsp; ㄴ 동일 값은 동일 순위, 다음 순위는 공동 순위에 따라 증가된 순위값 반환  
&nbsp;&nbsp; ㄴ ex) 1,2,3,3,3,6,7, ···  
```sql
-- 전체 직원에 대한 급여 순위 출력 (급여 기준, 내림차순)
-- (전체 대상이므로 partition by가 없음)
SELECT em_dept, em_name, em_salary,
  RANK() OVER(ORDER BY em_salary DESC) 전체순위
FROM Employee;
-- 사원 테이블의 부서별 사원에 대한 급여의 순위 출력 (급여 기준, 내림차순)
-- 부서별로 순위를 개별출력 ex) 관리부 1,2,2,4,5 교육부 1,2,2,2,5 영업부 1,2,3,4,5
SELECT em_dept, em_name, em_salary,
  RANK() OVER(PARTITION BY em_dept ORDER BY em_salary DESC) "부서별 순위"
FROM Employee;
```
&nbsp;**· DENSE_RANK : 전체 또는 윈도우별 행의 순위를 구해줌**  
&nbsp;&nbsp; ㄴ 동일 값의 순위와 상관없이 1 증가한 순위인 다음 순위값 반환  
&nbsp;&nbsp; ㄴ ex) 1,2,3,3,3,4,5, ···  
```sql
-- 사원 테이블의 부서별 사원에 대한 급여의 순위 출력 (급여 기준, 내림차순)
-- 부서별로 순위를 개별출력 ex) 관리부 1,2,2,3,4 교육부 1,2,2,2,3 영업부 1,2,3,4,5
SELECT em_dept, em_name, em_salary,
  DENSE_RANK() OVER(PARTITION BY em_dept ORDER BY em_salary DESC) "부서별 순위"
FROM Employee;
```
&nbsp;**· ROW_NUMBER : 각 행에 1부터 유일한 순위값 반환**  
&nbsp;&nbsp; ㄴ 같은 값도 순위가 다름 - 순서  
&nbsp;&nbsp; ㄴ ex) 1,2,3,4,5,6, ···  
```sql
SELECT em_dept, em_name, em_salary,
  ROW_NUMBER() OVER(ORDER BY em_salary DESC) 순서
FROM Employee;
```
| 윈도우 함수 결과테이블 예시 |
|---|

![WindowFunc](https://github.com/SamakFOX/EIP/blob/main/ExampleImages/WindowFunc.png)
&nbsp;
### 다음 글 계속 읽기 →
| 8-4. 절차형 SQL 작성 | [![읽어보기](https://img.shields.io/badge/읽어보기-blue?style=for-the-badge)](https://github.com/SamakFOX/EIP/blob/main/Basic/8-4_Procedural-SQL.md)   |
|:---:|:---:|
