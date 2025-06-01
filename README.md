# Dr.Wait⁺ (닥터웨잇)

![Dr.Wait Logo](./public/logo.png)

> **의료 대기 없이** 병원 예약·진료 관리와 멤버십 결제를 지원하는 웹 애플리케이션  
> Next.js 기반의 프론트엔드(React)와 Spring Boot 기반의 백엔드(REST API)를 결합해,  
> **사용자 위치 기반 병원 검색**, **회원 관리**, **구독형 멤버십 결제** 기능을 제공합니다.

---

## 목차

- [프로젝트 소개](#프로젝트-소개)  
- [주요 기능](#주요-기능)  
- [기술 스택](#기술-스택)  
- [폴더 구조](#폴더-구조)  
  - [ dr-wait-frontend (프론트엔드) ](#dr-wait-frontend-프론트엔드)  
  - [ dr-wait-backend (백엔드) ](#dr-wait-backend-백엔드)  
- [환경 변수 및 설정](#환경-변수-및-설정)  
  - [프론트엔드](#프론트엔드)  
  - [백엔드](#백엔드)  
- [로컬 개발 환경 구성 및 실행 방법](#로컬-개발-환경-구성-및-실행-방법)  
  - [프론트엔드 실행](#프론트엔드-실행)  
  - [백엔드 실행](#백엔드-실행)  
- [데모 스크린샷](#데모-스크린샷)  
- [API 명세](#api-명세)  
- [협업 툴 및 Git 워크플로우](#협업-툴-및-git-워크플로우)  
- [팀원 및 기여](#팀원-및-기여)  
- [라이선스](#라이선스)  
- [문의/연락처](#문의연락처)  

---

## 프로젝트 소개

**Dr.Wait⁺(닥터웨잇)** 은 사용자가 병원 방문 전 — 특히 진료 대기 — 과정을 최소화할 수 있도록 돕는 **병원 검색 및 예약/진료 관리 서비스**입니다.  
- **현재 위치**를 기반으로 주변 병원을 **카테고리별**(종합병원·내과·치과·약국 등)로 검색  
- 지도로 실시간 **대기 현황**과 **거리 정보**를 확인  
- **회원 가입/로그인**을 통한 개인화 기능  
- **멤버십 구독 결제**(월간/연간) 및 **쿠폰 등록** 기능  
- 마이페이지에서 **진료 기록** 조회 및 관리  

### 주요 목표
1. **모바일/웹 환경**에서 사용자가 손쉽게 병원을 찾고,  
2. **실시간 지도**로 대기 현황과 위치를 한눈에 파악하게 하여,  
3. **멤버십 결제**를 통해 추가 혜택(할인·쿠폰 등)을 제공하고,  
4. **보험·진료 기록 관리**를 통합하여 편리한 의료 경험을 제공

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

### 3. 쿠폰 & 멤버십 결제
- **쿠폰 등록**  
  - 가입 환영 쿠폰 입력  
  - 유효한 쿠폰 코드 입력 시 성공 메시지  
- **멤버십 플랜**  
  - 월간 구독(1,000원), 연간 구독(10,000원) 등 플랜 카드 UI  
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

### 5. 인증 & 보안 (백엔드)
- **Spring Security**를 활용한 JWT 기반 인증/인가  
  - `PasswordEncoder`로 비밀번호 해시 처리 + UUID 기반 사용자 ID  
  - 인증이 필요한 API 엔드포인트 필터링(`permitAll()`, `authenticated()`)  
- **Custom 에러 처리**  
  - 구체적인 에러 코드 및 메시지 처리 (예: `USER_NOT_FOUND`, `ALREADY_JOINED_FAMILY` 등)  
  - 공통 예외 처리 핸들러로 Bad Request(400), Unauthorized(401), Conflict(409) 등 HTTP 상태 코드 매핑  

---

## 기술 스택

### Frontend (dr-wait-frontend)
- **Framework & Library**  
  - Next.js (App Router, Server Components/Client Components)  
  - React 18 + TypeScript  
- **스타일링**  
  - Tailwind CSS (유틸리티 퍼스트 CSS)  
  - Sass (SCSS 모듈)  
- **지도 & 위치 API**  
  - Kakao Maps JavaScript SDK (`libraries=services`)  
  - 커스텀 훅 `useGeolocation` (브라우저 Geolocation API 래핑)  
- **인증 & 결제 UI**  
  - React Hooks (`useState`, `useEffect`, `useRef`)  
  - `react-icons` (폰트 아이콘)  
- **상태 관리**  
  - Local state (기본 `useState`) 및 React Query (선택적 사용 가능)  

### Backend (dr-wait-backend)
- **Framework & Language**  
  - Spring Boot (2.x) + Java 11+  
  - Gradle 빌드 시스템  
- **보안**  
  - Spring Security (JWT 기반 인증/인가)  
  - `io.jsonwebtoken:jjwt-api` (JWT 라이브러리)  
- **데이터베이스**  
  - MySQL / MariaDB  
  - JPA (Hibernate)  
- **환경 변수 관리**  
  - `dotenv-java` 라이브러리 (`io.github.cdimascio:dotenv-java`)  
  - `.env` 파일에서 DB URL, 사용자명, 비밀번호, JWT 비밀키 읽어옴  
- **커스텀 예외 처리**  
  - `ErrorCode` enum 클래스 + `CustomException`  
  - 전역 `@ControllerAdvice`로 예외 매핑

---

## 폴더 구조

```bash
Dr-Wait/
├─ dr-wait-frontend/            # → ① 프론트엔드 (Next.js) 프로젝트
│   ├─ public/
│   │   ├─ favicon.ico
│   │   ├─ current-location.svg
│   │   └─ (기타 이미지/아이콘)
│   ├─ src/
│   │   ├─ app/
│   │   │   ├─ layout.tsx           # 웹앱 전체 레이아웃 (폰트, Kakao Script)
│   │   │   ├─ page.tsx             # 홈 화면 (예: 병원 검색 가이드)
│   │   │   ├─ login/
│   │   │   │   ├─ page.tsx         # 로그인 페이지 (아이디/비밀번호)
│   │   │   │   └─ page.module.scss  # SCSS 모듈 (로그인 전용)
│   │   │   ├─ signup/
│   │   │   │   ├─ page.tsx         # 회원가입 페이지  
│   │   │   │   └─ page.module.scss  # SCSS 모듈 (회원가입 전용)
│   │   │   ├─ hospital/
│   │   │   │   ├─ page.tsx         # 병원 검색 페이지 (Kakao Maps 연동)  
│   │   │   │   └─ page.module.scss  # SCSS 모듈 (병원 검색 전용)
│   │   │   ├─ membership/
│   │   │   │   ├─ page.tsx         # 멤버십 결제 페이지  
│   │   │   │   └─ page.module.scss  # SCSS 모듈 (멤버십 전용)
│   │   │   └─ (추가 라우트)  
│   │   ├─ components/              # 재사용 컴포넌트
│   │   │   ├─ TopBar.tsx           # 상단 네비게이션 바  
│   │   │   ├─ TabBar.tsx           # 하단 네비게이션 바  
│   │   │   ├─ ProfileCard.tsx      # 프로필 카드 컴포넌트  
│   │   │   ├─ RowScroll.tsx        # 가로 스크롤 래퍼  
│   │   │   └─ (기타 컴포넌트)  
│   │   ├─ hooks/                   # 커스텀 훅  
│   │   │   └─ useGeolocation.ts    # 브라우저 Geolocation 래퍼  
│   │   ├─ data/                    # 정적 데이터  
│   │   │   ├─ searchKeywords.ts    # 기본 키워드(카페, 편의점 등)  
│   │   │   └─ hospitalKeywords.ts  # 병원 키워드(종합병원, 치과 등)  
│   │   ├─ types/                   # TS Type 정의  
│   │   │   └─ kakao/maps/search.ts # Kakao Maps API 응답 타입  
│   │   ├─ assets/                  # 이미지, SVG, 폰트 리소스  
│   │   ├─ styles/                  # 전역/공용 스타일 및 Tailwind 설정  
│   │   │   ├─ globals.scss  
│   │   │   ├─ tailwind-all.css  
│   │   │   └─ tailwind.config.js  
│   │   ├─ .env.local               # (로컬) 환경 변수: NEXT_PUBLIC_KAKAO_JAVASCRIPT_KEY  
│   │   ├─ next.config.js  
│   │   ├─ package.json  
│   │   └─ tsconfig.json  
│   └─ README.md                    # 프론트엔드 전용 README (현재 파일 포함되지 않음)
│
├─ dr-wait-backend/               # → ② 백엔드(Spring Boot) 프로젝트
│   ├─ src/
│   │   ├─ main/
│   │   │   ├─ java/com/drwait/
│   │   │   │   ├─ auth/
│   │   │   │   │   ├─ controller/     # 인증 관련 Controller  
│   │   │   │   │   ├─ service/        # 인증 로직 Service  
│   │   │   │   │   └─ dto/            # 요청/응답 DTO  
│   │   │   │   ├─ hospital/
│   │   │   │   │   ├─ controller/     # 병원 정보 조회 API  
│   │   │   │   │   ├─ service/        # 병원 검색 로직  
│   │   │   │   │   └─ repository/     # JPA Repository  
│   │   │   │   ├─ membership/
│   │   │   │   │   ├─ controller/     # 구독 결제 API  
│   │   │   │   │   ├─ service/        # 결제 처리 로직  
│   │   │   │   │   └─ dto/            # 결제 요청/응답 DTO  
│   │   │   │   ├─ user/
│   │   │   │   │   ├─ controller/     # 회원 정보 조회/수정  
│   │   │   │   │   ├─ service/        # 회원 서비스 로직  
│   │   │   │   │   ├─ dto/            # 사용자 관련 DTO  
│   │   │   │   │   └─ repository/     # JPA Repository  
│   │   │   │   ├─ config/             # SecurityConfig, JwtConfig 등  
│   │   │   │   ├─ exception/          # CustomException, ErrorCode 정의  
│   │   │   │   └─ DrWaitApplication.java  # Spring Boot 메인 클래스  
│   │   │   └─ resources/
│   │   │       ├─ application-dev.yml   # 개발 환경 설정  
│   │   │       ├─ application-prod.yml  # 운영 환경 설정  
│   │   │       └─ (기타 리소스)  
│   ├─ .env                            # 환경 변수 (DB URL, JWT_SECRET 등)  
│   ├─ build.gradle                  
│   └─ README.md                       # 백엔드 전용 README (아래 참조)  
│
└─ README.md                         # 프로젝트 루트 최종 README
