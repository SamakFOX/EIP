# 8-2. 고급 SQL 작성  

---
### **인덱스 (INDEX)**  
---  

- 인덱스는 자료를 빠르고 효율적으로 검색하기 위해 사용한다.  
- 위치(주소)를 관리,기억하는 [**인덱스파일**]과 실제 데이터를 기억하는 [**데이터파일**]로 구성된다.  
- 데이터를 검색할때는 먼저 인덱스파일에서 주소를 찾고 주소에 있는 데이터를 읽는다.  

> 기본인덱스 (Primary Index) : 기본키 속성으로 만든 인덱스  
> 보조인덱스 (Secondary Index) : 일반 속성으로 만든 인덱스  
> 인덱스는 [ 키값, 주소 ] 쌍으로 저장된다.

① 인덱스 개념  

<참고> {} : 반복, [] : 생략가능, | : 선택  
```sql
CREATE [UNIQUE] INDEX 인덱스명 ON 테이블명 (속성명 ASC | DESC);
```
★ UNIQUE : 중복값 없이 모든 속성값이 고유값을 갖도록 지정 (해당 속성이 이미 unique거나, 중복이 상관없을때는 생략 가능)  
★ FOREIGN KEY (속성명) REFERENCES 참조테이블명(속성명) : 외래키 지정 (참조 무결성)  
★ CONSTRAINT 제약조건명 CHECK (속성명=범위값) : 특정 속성에 범위 지정 (도메인 무결성)  
★ CREATE TABLE | VIEW AS SELECT ~ : 뒤 서브쿼리 결과로 신규 테이블/뷰 생성  

| CREATE문 인덱스 생성 예시 |
|---|

```sql
CREATE UNIQUE INDEX Student_idx ON Student(st_num ASC);
```

