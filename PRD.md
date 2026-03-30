# PRD — 뱅크법률집사 (조합원 전용 앱)

## 1. Product Overview

| 항목 | 내용 |
|------|------|
| **서비스명** | 뱅크법률집사 — 조합원 (BankCasa Member) |
| **한 줄 설명** | 신협·단위농협·새마을금고 조합원이 법률 상담, 현황 조회, 서류 안내 등을 이용하는 모바일 최적화 앱. bankcasa-demo와 동일 코드베이스에서 `role=member`로 진입. |
| **대상 사용자** | 신협·단위농협·새마을금고 조합원 |

## 2. Tech Stack

| 구분 | 기술 |
|------|------|
| 프레임워크 | React 19 + TypeScript |
| 빌드 도구 | Vite 8 |
| 스타일링 | Tailwind CSS |
| 라우팅 | HashRouter (react-router-dom) |
| 상태 관리 | Context API (AppProvider) |
| 저장소 | localStorage |
| 배포 | GitHub Pages (gh-pages 브랜치) |

> **참고:** bankcasa-demo와 동일 코드베이스. URL 파라미터 `role=member`로 진입.

## 3. Architecture

```
SPA (bankcasa-demo 동일 코드베이스)
├── URL 진입: #/?role=member&inst=xxx&name=xxx
├── AppProvider → role='member' 설정
├── 모바일형 헤더 (사이드바 없음)
├── MemberDashboard
│   └── 서비스 카드 리스트
└── Pages/ (bankcasa-demo와 공유)
```

**핵심 차이점:** 사이드바 없음. 모바일형 헤더 + 서비스 카드 UI로 구성.

## 4. Pages & Routes

### 대시보드 서비스 카드

| 순서 | 카드 | 경로 | 설명 |
|------|------|------|------|
| 1 | 집변 법률상담 | 외부 링크 | **최상단, 검정 강조**, "즉시 상담 가능" 표시 |
| 2 | 내 법률 현황 조회 | `/member-portal` | 조합원 개인 법률 현황 대시보드 |
| 3 | AI 서류 안내 | `/consult` | AI 상담 → 서류 안내 플로우 |
| 4 | 대출 기한 연장 신청 | `/consult` | 대출 기한 연장 관련 상담 |
| 5 | 담보물 변경 신청 | `/consult` | 담보물 변경 관련 상담 |
| 6 | 커뮤니티·공지사항 | `/community` | 조합원 커뮤니티, 공지 확인 |

### 전체 라우트

bankcasa-demo의 모든 라우트를 공유하되, 조합원 역할에 해당하는 페이지만 대시보드 카드로 노출.

## 5. Data Models

bankcasa-demo와 동일:

```typescript
interface User {
  institution: string;
  name: string;
  role?: 'teller' | 'member' | 'chairman';
}

interface ConsultHistory {
  id: string;
  date: string;
  caseType: string;
  caseName: string;
  status: string;
  formData?: Record<string, any>;
  claimCalc?: ClaimCalculation;
}
```

## 6. Key Features

### 6.1 집변 법률상담 (최상단 강조)
- 외부 링크 (`homelawyer.kr`)
- "즉시 상담 가능" 배지 표시
- 검정 강조 카드 스타일

### 6.2 내 법률 현황 조회
- 조합원 개인의 대출/담보/법적조치 현황 조회
- 진행 중인 법률 절차 상태 확인

### 6.3 AI 서류 안내
- 의사결정 트리 기반 상담
- 필요 서류 자동 안내

### 6.4 대출 기한 연장 / 담보물 변경
- 상담 플로우를 통한 신청 안내

### 6.5 커뮤니티·공지사항
- 조합원 전용 게시판
- 기관 공지사항 확인

## 7. Design System

| 속성 | 값 |
|------|-----|
| 레이아웃 | **모바일 최적화** — 사이드바 없음, 헤더 + 카드 리스트 |
| 배경색 | `#ffffff` |
| 텍스트색 | `#000000` |
| 구분선 | `1px solid #e5e5e5` |
| 집변 카드 | 최상단 배치, 검정 강조, "즉시 상담 가능" 배지 |
| 서비스 카드 | 세로 리스트, 터치 최적화 |
| 폰트 | Pretendard |

## 8. External Integrations

| 서비스 | URL | 연동 방식 |
|--------|-----|-----------|
| 집변 (법률 상담) | `homelawyer.kr` | `window.open()` — 최상단 강조 카드 |
| 법령정보 | `law.go.kr` | 외부 링크 |

## 9. Deployment

| 항목 | 값 |
|------|-----|
| 호스팅 | GitHub Pages |
| 브랜치 | `gh-pages` |
| URL | `https://{owner}.github.io/bankcasa-member/` |
| 진입 | URL 파라미터 `#/?role=member&inst=xxx&name=xxx` |

## 10. Known Limitations

- **bankcasa-demo와 동일 코드베이스** — URL 파라미터로 역할 분기
- **모든 데이터는 목업** — 실 API 연동 없음
- **AI 상담은 정적 의사결정 트리** — LLM 미연동
- **대출 기한 연장/담보물 변경은 상담 안내만** — 실제 신청 처리 미구현
- **커뮤니티는 읽기 전용 목업** — 실제 게시글 작성 미지원
- **인증/인가 없음**
