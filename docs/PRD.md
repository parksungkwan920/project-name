# PRD: 개인 개발 블로그 (Notion CMS)

## 1. 프로젝트 개요

| 항목 | 내용 |
|------|------|
| 프로젝트명 | 개인 개발 블로그 |
| 목적 | Notion을 CMS로 활용한 개인 기술 블로그 |
| CMS 선택 이유 | Notion에서 글 작성 시 자동으로 블로그에 반영 |
| 배포 환경 | Vercel |

---

## 2. 기술 스택

| 분류 | 기술 |
|------|------|
| Frontend | Next.js 15, TypeScript |
| CMS | Notion API (`@notionhq/client`) |
| Styling | Tailwind CSS, shadcn/ui |
| Icons | Lucide React |
| Deployment | Vercel |

---

## 3. Notion 데이터베이스 구조

| 필드명 | 타입 | 설명 |
|--------|------|------|
| Title | title | 글 제목 |
| Category | select | 카테고리 (예: Next.js, React, DevOps) |
| Tags | multi_select | 태그 목록 |
| Published | date | 발행일 |
| Status | select | `초안` / `발행됨` |
| Content | page content | 본문 (Notion 블록) |

> Status가 `발행됨`인 항목만 블로그에 노출

---

## 4. 주요 기능

### 4.1 블로그 글 목록 (홈)
- Notion DB에서 `Status = 발행됨` 글 목록 조회
- Published 기준 최신순 정렬
- 제목, 카테고리, 태그, 발행일 표시
- 페이지네이션 (선택적)

### 4.2 글 상세 페이지
- Notion 페이지 블록을 HTML로 변환하여 렌더링
- 지원 블록 타입: paragraph, heading 1~3, bulleted list, numbered list, code, image, quote, divider
- 메타정보 표시: 카테고리, 태그, 발행일

### 4.3 카테고리 필터링
- 카테고리별 글 목록 페이지 (`/category/[category]`)
- 전체 카테고리 목록 사이드바 또는 탭으로 표시

### 4.4 검색
- 글 제목 기반 클라이언트 사이드 검색
- 검색어 하이라이팅

### 4.5 반응형 디자인
- 모바일 / 태블릿 / 데스크탑 대응
- Tailwind CSS breakpoint 기준: `sm(640px)`, `md(768px)`, `lg(1024px)`

---

## 5. 화면 구성

```
/                       → 홈 (최근 글 목록)
/posts/[slug]           → 글 상세
/category/[category]    → 카테고리별 글 목록
```

### 공통 레이아웃
- Header: 블로그 제목, 네비게이션 (홈, 카테고리, 검색)
- Footer: 소셜 링크, 저작권

---

## 6. MVP 범위

### 포함
- [x] Notion API 연동 (글 목록, 글 상세)
- [x] 홈 페이지 (글 목록)
- [x] 글 상세 페이지 (블록 렌더링)
- [x] 카테고리 필터
- [x] 기본 반응형 스타일링

### 제외 (추후 고려)
- [ ] 댓글 기능
- [ ] 다크모드
- [ ] RSS 피드
- [ ] SEO 최적화 (sitemap, OG 태그)
- [ ] 서버 사이드 검색

---

## 7. 구현 단계

### Step 1: 환경 설정
- `@notionhq/client` 패키지 설치
- `.env.local` 에 `NOTION_API_KEY`, `NOTION_DATABASE_ID` 설정
- Notion Integration 생성 및 DB 공유 설정

### Step 2: Notion API 연동 레이어
- `lib/notion.ts` — DB 조회, 페이지 블록 조회 함수
- `types/notion.ts` — 타입 정의

### Step 3: 글 목록 페이지
- `app/page.tsx` — 홈, 최근 글 목록
- `components/PostCard.tsx` — 글 카드 컴포넌트

### Step 4: 글 상세 페이지
- `app/posts/[slug]/page.tsx` — 상세 페이지
- `components/NotionRenderer.tsx` — 블록 타입별 렌더링

### Step 5: 카테고리 페이지
- `app/category/[category]/page.tsx`

### Step 6: 스타일링 및 최적화
- shadcn/ui 컴포넌트 적용
- ISR (Incremental Static Regeneration) 설정 (`revalidate`)
- Vercel 배포

---

## 8. 환경 변수

```env
NOTION_API_KEY=secret_xxxx
NOTION_DATABASE_ID=xxxx
```

---

## 9. 폴더 구조 (예상)

```
src/
├── app/
│   ├── page.tsx                  # 홈
│   ├── posts/[slug]/page.tsx     # 글 상세
│   └── category/[category]/page.tsx
├── components/
│   ├── PostCard.tsx
│   ├── NotionRenderer.tsx
│   └── CategoryFilter.tsx
├── lib/
│   └── notion.ts                 # Notion API 래퍼
└── types/
    └── notion.ts                 # 타입 정의
```
