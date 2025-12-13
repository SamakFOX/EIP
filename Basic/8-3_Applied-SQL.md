# 8-3. 응용 SQL 작성

## 목차
- [1. 집계함수](#AggregateFunc)
- [2. GROUP BY](#GroupBy)
- [3. 윈도우함수](#WindowFunc)

<a id="AggregateFunc"></a>

---
### **♣ 집계함수 (Agregate Function)**  
---  
&nbsp;
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

---
### **♣ GROUP BY 절을 사용하는 그룹처리 함수**  
---  
&nbsp;
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
&nbsp;· 주어진 컬럼 수보다 하나 더 큰 레벨로 반환 → 컬럼수가 N일때 N+1 레벨로 반환  
&nbsp;! ROLLUP 함수의 컬럼들은 모두 SELECT절에 추가되어야 유의미한 결과 분석 가능  
```sql
-- 학과, 학년, 수학평균을 성적테이블에서 검색 + 학과&학년으로 ROLLUP
SELECT dept_id 학과, grade 학년, ROUND(AVG(math)) "수학 평균"
FROM Score
GROUP BY ROLLUP(dept_id, grade);
```
※ ROUND : 반올림하여 정수로 표현  
&nbsp;
예시 데이터가 다음과 같을 때  
| dept_id | grade | math |
|---|---|---|
| 10 | 1 | 80 |
| 10 | 1 | 90 |
| 10 | 2 | 70 |
| 20 | 1 | 85 |
