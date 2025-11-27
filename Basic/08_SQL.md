# 08. SQL  

---
### **DDL (Data Definition Language) : 데이터 정의어**  
---  

① CREATE : 테이블/스키마/도메인/인덱스 정의  

<참고> {} : 반복, [] : 생략가능, | : 선택  
```sql
CREATE TABLE 테이블명
({속성명 데이터타입 [NOT NULL], }
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
★ 명명규칙
| 규칙 | 특이사항 |
|---|---|
| 영문자, 숫자, 밑줄 사용 가능 | 첫 글자는 영문자 |
| 공백, 특수문자 사용 불가 | |
| 대소문자 구분 없음 | 소문자 권장 |
| SQL 예약어 사용 불가 | ex) select, table 등 |

★ 데이터 타입
| 예약어 | 타입 |
|---|---|
| INT | 정수 |
| FLOAT | 실수 |
| CHAR | 고정길이 문자 |
| VARCHAR | 가변길이 문자 |
| TIME | 시간 |
| DATE | 날짜 |

