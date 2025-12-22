# 9-2. 소프트웨어 개발 보안 구현

## 목차
- [1. 암호화 알고리즘](#EncryptAlg)
- [2. ](#)
- [3. ](#)
- [4. ](#)
- [5. ](#)
- [6. ](#)
&nbsp;

<a id="EncryptAlg"></a>

---
### **♣ 암호화 알고리즘**  
---  

### ① 시큐어 코딩 가이드의 개념
&nbsp; 대표적 웹 앱 보안 취약점 발표 사례인 OWASP TOP 10을 참고해 KISA에서 발표한 **보안 약점 가이드**  
| 가이드 7항목 |
|---|
| 입력 데이터 검증 및 표현 |
| 보안 기능 |
| 시간 및 상태 |
| 에러 처리 |
| 코드 오류 |
| 캡슐화 |
| API 오용 등 |

&nbsp;
### ② 암호화 알고리즘
★ 평문을 암호문으로, 암호문을 평문으로 바꿀 때 사용되는 알고리즘  
![알고리즘_흐름도](https://github.com/SamakFOX/EIP/blob/main/ExampleImages/EncDnc.png)

★ 암호 방식의 분류  
![암복호화_방식](https://github.com/SamakFOX/EIP/blob/main/ExampleImages/EncList.png)

&nbsp;
### ③ 비밀키 (Private Key, 대칭키) 암호화 기법 <sup>★</sup>
**비밀키 = 대칭키**  
암호키(Cryptographic Key)가 동일하기 때문에 키를 비밀로 설정  
★ 암/복호화 속도가 빠르고 알고리즘이 단순  
★ 키 분배가 공개키보다 어려움  
| 암호 방식 | 상세 설명 |
|:---:|---|
| 스트림 방식 | · 평문의 길이와 동일한 스트림(Stream)을 생성하여 비트 단위로 암호화<br>· 암호화할때 XOR 연산을 수행<br>· 종류 : RC4, A5/1, LSFR, SEAL, WEP, OFB |
| 블록 방식 | · 평문을 블록 단위로 암호화하는 대칭키 암호 시스템<br>· 종류 : DES, AES, ARIA, SEED, IDEA |

★ 블록 암호화 시스템 종류
| 종류 | 상세 설명 |
|:---:|---|
| DES | · Data Encryption Standard<br> · 1970년대 초 IBM이 개발<br> · 16라운드 Feistal 구조<br> · 평문을 64bit로 블록화하고, 실제 키의 길이는 56bit 이용<br> · 전사공격에 취약(Brute-Force Attack) |
| AES | · Advanced Encryption Standard<br> · DES를 대신하는 새로운 표준<br> · 블록 크기는 128bit, 키 길이는 128/192/256bit<br> · SPN 구조이다 |
| ARIA | · 국내 기술<br> · 경량 환경 및 하드웨어 구현 시 효율성 향상이 목적<br> · 우리나라 국가 표준<br> · 블록크기와 키 길이는 AES와 같음 |
| SEED | · 국내 기술<br> · 128bit 블록 암호 알고리즘<br> · Feistel 구조<br> · 2005년 국제 표준 |
| IDEA | · DES 대체 기술<br> · 스위스 개발<br> · 상이한 대수 그룹으로부터 세가지 연산을 혼합 |

&nbsp;
### ④ 공개키 (Public Key, 비대칭키) 암호화 기법 <sup>★</sup>
**공개키 = 비대칭키**  
암호화키와 복호화키가 달라 복호화키를 공개해도 됨  
★ 암복호화 속도가 느리고 알고리즘이 복잡  
★ 키 분배가 비밀키보다 용이  
★ RSA(Rivest Shamir Adlem), EIGama, Robin, ECC, DSS 기법 등이 있음  
| 종류 | 상세 설명 |
|:---:|---|
| RSA | · 소인수분해의 어려움에 기초<br> · 전자문서 인증 및 부인방지에 이용 |
| EIGama | · 이산대수 문제의 어려움에 기초<br> · 같은 메시지도 암호화 마다 암호문 길이가 2배 |

&nbsp;
### ⑤ 해시 (HASH)
&nbsp;· 임의 길이의 메시지를 입력받아 <mark>고정된 길이의 출력값으로 반환</mark>  
&nbsp;· 주어진 원문에서 고정길이의 의사난수 생성, 생성된 값이 해시값  
&nbsp;· 디지털 서명에서 데이터 무결성 제공  
&nbsp;· 블록체인에서 체인 형태로 사용하여 신뢰성 보장  
&nbsp;* SHA, SHA1, SHA256/224, SHA512/384, MD5, RMD160, HAS-160, RIPEMD 등  
| 종류 | 상세 설명 |
|:---:|---|
| SHA | · Secure Hash Algorithm<br> · 1993 미국 NIST 개발<br> · 가장 많이 사용하는 방식<br> ★ SHA1 : DSA에서 사용<br> ★ SHA256, SHA384, SHA512<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ㄴ 128&nbsp;&nbsp;&nbsp;&nbsp;ㄴ 192&nbsp;&nbsp;&nbsp;&nbsp;ㄴ 256<br> : AES의 키 길이인 128, 192, 256 비트에 대응하도록 출력 길이를 늘린 해시 알고리즘 |
| MD5 | · Message-Digestal Algorithm 5<br> · 1992 Ron Rivest 개발<br> ★ 널리 사용되었으나 '충돌 회피성'에서 문제가 있어 사용 X<br> ㄴ 기존 응용과의 호환으로만 사용  |
| HAS-160 | · 국내 개발된 대표적 해시함수 (SHA와 유사)<br> · 기존 MD 계열 해시함수와는 차이 |
