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
### 1-1. 다중 행 함수  
&nbsp;&nbsp;· 전체 또는 그룹별로 연관 및 계산분석을 한 단일 결과값 반환  
&nbsp;&nbsp;· GROUP BY 절에서는 기준 컬럼명  
&nbsp;&nbsp;· SELECT / HAVING 절에서는 집계함수  
&nbsp;&nbsp;· 분석을 위한 윈도우 함수는 SELECT 절에서 사용

### 1-2. 집계함수 (Aggregate Function)  
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
SELECT SUM(math) FROM Score WHERE name <> '홍길동';
-- 과학 점수의 평균값 검색
SELECT AVG(science) FROM Score;
-- 갯수 카운트 + (별칭을 AS로 쓰는 경우, AS 없이 쓰는 경우)
SELECT COUNT(*) AS "학생 수", COUNT(math) "수학 응시생 수" FROM Score;
```

