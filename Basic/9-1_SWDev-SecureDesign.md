# 9-1. 소프트웨어 개발 보안 설계

## 목차
- [1. SW개발 보안 설계](#SWDevDesign)
- [2. ](#)
- [3. ](#)
- [4. ](#)
&nbsp;

<a id="SWDevDesign"></a>

---
### **♣ 소프트웨어 개발 보안 설계**  
---  

### ① 소프트웨어 개발 보안
&nbsp; ★ 요구공학 프로세스  
![CMMi](https://github.com/SamakFOX/EIP/blob/main/ExampleImages/CMMi.png)  
&nbsp; ※ 요구사항 도출 : 이해관계자의 상호 의견을 조율·협의를 통해 요구사항을 정제하고 분류  
&nbsp; ※ 요구사항 분석 : 제약조건을 판별 후 의견제시로 보안 요구사항 분석서에 기술  
&nbsp; ※ 요구사항 명세 : 개발 시스템의 목표 기술과 비기능적 요구사항을 명세  
&nbsp; ※ 요구사항 확인과 검증

&nbsp;
### ② 요구사항 관리
&nbsp; - 서버 보안 요구사항은 변경될 수 있으므로 지속 갱신해야함  
&nbsp; - 요구사항 추적 매트릭스를 통해 요구사항 관리  

&nbsp;
### ③ 소프트웨어 개발 보안 방법론
&nbsp; : 안전한 소프트웨어 개발에 요구되는 보안 활동들을 적용하는 개발 방법  
&nbsp; ＊ SDLC (Software Development Life Cycle)에 걸친 보안 활동  
&nbsp; ★ SDLC  
| 활동 | 상세 |
|:---:|---|
| 요구사항 분석 | 보안 항목 식별 |
| 설계 | 위협원 도출을 위한 위협 모델링 |
| 구현 | 개발 보안 가이드를 준수하여 개발 |
| 테스트 | 모의 침투 테스트, 동적 분석을 통한 취약점 진단 및 개선 |
| 유지보수 | 지속적인 개선 및 보안 패치 |

&nbsp;
### ④ SDLC 보안 적용 사례
**4-1. MS-SDL (MicroSoft-Secure Development Lifecycle) <sup>★</sup>** → 자체 수립  

**4-2. Seven Touch Points <sup>★</sup>**  
&nbsp; - 소프트웨어 보안의 모범사례를 SDLC에 통합  
&nbsp; - 각 단계에 7개의 보안 강화 활동을 집중 관리  
&nbsp;&nbsp;&nbsp;&nbsp; ㄴ 악용사례 / 보안 요구사항 / 위험 분석 / 위험 기반 보안 테스트  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; / 코드 검토 / 침투 테스트 / 보안 운영  

**4-3. CLASP (Comprehensive Lightweight Application Security Process)**  
&nbsp; - SDLC 초기단계에 보안 강화가 목적  
&nbsp; - 활동중심 / 역할기반 프로세스  
&nbsp; - 보안 관점 5가지  
| 분야 | 활동 |
|---|---|
| 개념 | 종속성 제공 |
| 역할 기반 | 역할을 창출하여 여러 관점에서 사용 |
| 활동 평가 | 적합성과 관련된 타당성 평가 |
| 활동 구현 | 24개의 보안 관련 활동 수행 |
| 취약성 | 솔루션 통합 |

&nbsp;
### ⑤ 정보 보안의 3대 요소
**5-1. 기밀성 (Confidentiality) <sup>★</sup>**  
&nbsp; : <mark>인가된 사용자만</mark>이 정보자산에 접근 가능  
&nbsp;&nbsp;&nbsp;&nbsp; ex) 방화벽, 암호, 패스워드 등  
&nbsp;&nbsp;&nbsp;&nbsp; → 신분 위장 공격 가능  

**5-2. 무결성 (Integrity, 완전성) <sup>★</sup>**  
&nbsp; : <mark>인가된 사용자가 인가된 방법으로만</mark> 수정 가능  
&nbsp;&nbsp;&nbsp;&nbsp; ex) 방화벽, 암호, 패스워드 등  
&nbsp;&nbsp;&nbsp;&nbsp; → 변경, 가장, 재전송 등 공격 가능  

**5-3. 가용성 (Availability) <sup>★</sup>**  
&nbsp; : 사용자가 <mark>필요할 때 데이터에 접근</mark>할 수 있는 능력  
&nbsp;&nbsp;&nbsp;&nbsp; ex) 방화벽, 암호, 패스워드 등  
&nbsp;&nbsp;&nbsp;&nbsp; → 서비스 거부 공격 가능  

&nbsp;
### ⑥ 위협, 자산, 취약점 개념
<img src="https://github.com/SamakFOX/EIP/blob/main/ExampleImages/Danger.png" width="500"/>

&nbsp; ※ 자산 : 기업 내부 데이터 또는 시스템  
&nbsp; ※ 위협원 : 자연재해를 포함한 임직원 및 내외부인사 등  
&nbsp; ※ 위협 : 위협원의 실제적 공격  
&nbsp; ※ 위험 : 위협원이 취약점을 분석해 시스템에 위험을 초래할 영향도 (확률)  

&nbsp;
### ⑦ 법률적 검토
&nbsp; ● 정보보호 관련 법규와 규제 등을 확인하여 올바른 보안을 적용  
&nbsp; ● 관련기관
&nbsp;&nbsp;&nbsp;&nbsp; - 행정안전부 : 보안정책 총괄  
&nbsp;&nbsp;&nbsp;&nbsp; - 한국인터넷진흥원(KISA) : 개발 보안 정책 및 가이드 개발  

&nbsp;
### ⑧ 관련법령
&nbsp; - 개인정보보호법  
&nbsp;&nbsp;&nbsp;&nbsp; : 정보주체와 개인정보처리자의 권리, 의무 등을 검토하여 요구사항에 반영  
&nbsp; - 정보통신망 이용 촉진 및 정보보호 등에 관한 법률  
&nbsp;&nbsp;&nbsp;&nbsp; : 개인정보 자료의 수집, 처리, 보관, 이용에 관한 규정을 검토하여 요구사항에 반영  
&nbsp;&nbsp;&nbsp;&nbsp; : `개인정보 보호 조치 시행령 15조` / `법률 제 28조` 분석  
&nbsp; - 신용정보의 이용 및 보호에 관한 법률  
&nbsp;&nbsp;&nbsp;&nbsp; : 개인 신용정보의 취급 단계별 보호조치 및 의무를 요구사항에 반영  
&nbsp; - 위치정보의 보호 및 이용 등에 관한 법률  
&nbsp;&nbsp;&nbsp;&nbsp; : 개인 위치정보 수집, 이용, 제공, 파기 및 정보 주체의 권리 등을 요구사항에 반영  
&nbsp; - 표준 개인정보 보호 지침  
&nbsp;&nbsp;&nbsp;&nbsp; : 개인정보 취급자와 처리자가 기준을 준수하고 예방조치에 대한 세부규정을 요구사항에 반영  
&nbsp; - 개인정보의 안정성 확보 조치 기준  
&nbsp;&nbsp;&nbsp;&nbsp; : 보호 수준 진단과 규정 검토로 요구사항 반영  

&nbsp;
### ⑨ 소프트웨어 개발 요구사항 관련 특정 정보통신기술 관련 규정/법률
&nbsp; ● RFID 프라이버시 보호 가이드라인  
&nbsp; ● 위치정보의 보호 및 이용 등에 관한 법률  
&nbsp; ● 위치정보의 관리적, 기술적 보호조치 권고 해설서  
&nbsp; ● 바이오 정보 보호 가이드라인 (생체정보)  
&nbsp; ● 뉴미디어 서비스 개인정보보호 가이드라인  

&nbsp;
### ⑩ 취약점 점검 계획서, 명세서 검토
취약점 점검 계획서 / 취약점 점검 설계 명세서 / 취약점 점검 케이스 명세서 / 취약점 점검 절차 명세서 / 취약점 점검 목록 분석 / 가정 분석 / 도식화 기법  

