# Dr.Wait⁺ (닥터웨잇)

> **의료 대기 없이** 병원 예약·진료 관리와 멤버십 결제를 지원하는 웹 애플리케이션  
> Next.js 기반의 프론트엔드(React)와 Spring Boot 기반의 백엔드(REST API)를 결합해,  
> **사용자 위치 기반 병원 검색**, **회원 관리**, **구독형 멤버십 결제** 기능을 제공합니다.

---

## 목차

- [프로젝트 소개](#프로젝트-소개)
- [팀원 소개](#팀원-소개)  
- [주요 기능](#주요-기능)  
- [기술 스택](#기술-스택)  
- [환경 변수 및 설정](#환경-변수-및-설정)  
  - [프론트엔드](#프론트엔드)  
  - [백엔드](#백엔드)  
- [로컬 개발 환경 구성 및 실행 방법](#로컬-개발-환경-구성-및-실행-방법)  
  - [프론트엔드 실행](#프론트엔드-실행)  
  - [백엔드 실행](#백엔드-실행)  
- [협업 툴 및 Git 워크플로우](#협업-툴-및-git-워크플로우)  

---

## 프로젝트 소개

**Dr.Wait⁺(닥터웨잇)** 은 사용자가 병원 방문 전 — 특히 진료 대기 — 과정을 최소화할 수 있도록 돕는 **병원 검색 및 예약/진료 관리 서비스**입니다.  
- **현재 위치**를 기반으로 주변 병원을 **카테고리별**(종합병원·내과·치과·약국 등)로 검색  
- 지도로 실시간 **대기 현황**과 **거리 정보**를 확인  
- **회원 가입/로그인**을 통한 개인화 기능  
- **멤버십 구독 결제** 기능  
- 마이페이지에서 **진료 기록** 조회 및 관리  

### 주요 목표
1. **모바일/웹 환경**에서 사용자가 손쉽게 병원을 찾고,  
2. **실시간 지도**로 대기 현황과 위치를 한눈에 파악하게 하여,  
3. **멤버십 결제**를 통해 추가 혜택(할인·쿠폰 등)을 제공하고,  
4. **보험·진료 기록 관리**를 통합하여 편리한 의료 경험을 제공

---

## 팀원 소개
- 최성하 <strong style="color : yellow;">PM</strong>
- 김승조 <strong style="color : yellow;">BE</strong>
- 이서연 <strong style="color : yellow;">BE</strong>
- 이가림 <strong style="color : yellow;">FE</strong>
- 정연재 <strong style="color : yellow;">FE</strong>
- 정영석 <strong style="color : yellow;">FE</strong>

---

## 주요 기능

### 1. 회원 관리
- **회원가입 (Sign Up)**  
  - 기본 정보(이름, 아이디, 비밀번호, 전화번호, 주민등록번호) 입력  
  - 입력값 유효성 검사(영문+숫자+특수문자 8자 이상, 전화번호 형식, 주민등록번호 형식)  
- **로그인 (Login)**  
  - 가입된 아이디·비밀번호로 로그인  
  - 로그인 상태 유지(토큰 기반 인증)

### 2. 병원 검색 & 지도 연동
- **Kakao Maps SDK**를 활용한 **현재 위치 기반** 병원 검색  
  - `hospitalKeywords.ts`에 정의된 키워드(종합병원·내과·치과·한의원·약국 등)로 카테고리별 장소 검색  
  - **키워드 별 결과 개수**를 기준으로 자동 정렬  
- **병원 지도 뷰**  
  - 지도 위에 **마커 표시**  
  - 리스트 뷰 클릭 → 해당 위치로 지도 중앙 이동  
  - 패널을 펼치고 접는 기능(↑↓ 아이콘)

### 3. 멤버십 서비스 
- **멤버십 가격**  
  - 월간 구독(1,000원) 플랜 카드 UI  
  - **자동 갱신 결제** 안내  
- **결제 수단 선택**  
  - 카카오페이, 토스페이, 카드 결제 수단 제공  
  - **결제 수단 + 약관 동의** 시 “멤버십 결제하기” 버튼 활성화  
- **결제 완료 모달**  
  - 결제 성공 시 `결제 완료` 메시지 모달 오버레이

### 4. 사용자 프로필/마이페이지
- **프로필 정보 조회/수정**  
  - 이름, 전화번호, 진료 이력 조회  
- **진료 기록 관리**  
  - 병원, 진료 일시, 처방 정보 등을 기록·조회

---

## 기술 스택

### Frontend (dr-wait-frontend)
- **Framework & Library**  
  - Next.js (App Router, Server Components/Client Components)  
  - React 19 + TypeScript  
- **스타일링**  
  - Tailwind CSS (유틸리티 퍼스트 CSS)  
  - Sass (SCSS 모듈)  
- **지도 & 위치 API**  
  - Kakao Maps JavaScript SDK (`libraries=services`)  
  - 커스텀 훅 `useGeolocation` (브라우저 Geolocation API 래핑)  
- **인증 & 결제 UI**  
  - React Hooks (`useState`, `useEffect`, `useRef`)  
  - `fontawesome` (폰트 아이콘)  
- **상태 관리**  
  - Local state (기본 `useState`) 및 Nextjs server-actions (선택적 사용 가능)  

### Backend (dr-wait-backend)
- **Framework & Language**  
  - Spring Boot + Java 21 
  - Gradle 빌드 시스템  
- **보안**  
  - Spring Security (JWT 기반 인증/인가)  
  - `io.jsonwebtoken:jjwt-api` (JWT 라이브러리)  
- **데이터베이스**  
  - MySQL
- **환경 변수 관리**  
  - `dotenv-java` 라이브러리 (`io.github.cdimascio:dotenv-java`)  
  - `.env` 파일에서 DB URL, 사용자명, 비밀번호, JWT 비밀키 읽어옴  

## 환경 변수 및 설정

### 프론트엔드 (`dr-wait-frontend`)
- **BACKEND_URL="http://localhost:8080"**  
  - 설명: 백엔드 API 호출용 기본 URL

- **KAKAO_JAVASCRIPT_KEY="8d0d82e91682c60f4aca648e478d27a6"**  
  - 설명: Kakao Maps JavaScript SDK 로드용 앱 키  

- **AUTH_SECRET="GTpvqQhwZeEyl+60PBeQcQ4Z9yuAFbgPxfg/MGQfzjU="**  
  - 설명: JWT(JSON Web Token)를 생성 및 검증용 키

### 백엔드 (`dr-wait-backend`)
- **`dev.env` (최상위)** 
- gitinoire로 설정되어 있어 따로 추가 필요

```
# MySQL/MariaDB 연결 정보
DB_HOST=db.lunabi.co.kr
DB_NAME=yonsei-drwait
DB_USER=drwait
DB_PASSWORD=drwait2501!

# JWT 비밀 키
JWT_SECRET_KEY=MySuperSecretKey1234567890123456!!
JWT_ACCESS_TOKEN_VALIDITY_IN_SECONDS=3600
JWT_REFRESH_TOKEN_VALIDITY_IN_SECONDS=4000
```
---

## 로컬 개발 환경 구성 및 실행 방법

### 프론트엔드 (`dr-wait-frontend`)
0. 환경 설정
```
bash
pnpm i
```

1. 개발 서버 실행
application 파일 실행

2. 프리뷰 접근
Open [http://localhost:3000](http://localhost:3000)
