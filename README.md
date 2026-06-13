# 개인 개발 블로그 (Notion CMS)

Notion을 CMS로 활용한 개인 기술 블로그입니다. Notion에서 글을 작성하면 자동으로 블로그에 반영됩니다.

## 🚀 주요 특징

- **Notion CMS**: Notion 데이터베이스를 직접 관리 인터페이스로 사용
- **자동 배포**: Notion에 글을 발행하면 블로그에 자동 반영
- **카테고리 필터링**: 카테고리별로 글 목록 조회
- **반응형 디자인**: 모바일부터 데스크탑까지 완벽 대응
- **TypeScript**: 완전한 타입 안정성

## 📚 기술 스택

| 분류 | 기술 |
|------|------|
| Frontend | Next.js 15, TypeScript |
| CMS | Notion API |
| Styling | Tailwind CSS, shadcn/ui |
| Icons | Lucide React |
| Deployment | Vercel |

## 📋 Notion 데이터베이스 구조

Notion에서 다음 필드를 가진 데이터베이스 생성:

| 필드명 | 타입 | 설명 |
|--------|------|------|
| Title | title | 글 제목 |
| Category | select | 카테고리 |
| Tags | multi_select | 태그 목록 |
| Published | date | 발행일 |
| Status | select | `초안` / `발행됨` |
| Content | page content | 본문 |

> Status가 `발행됨`인 항목만 블로그에 노출

## 🛠️ 시작하기

### 1. 환경 변수 설정

`.env.local` 파일을 생성하고 다음을 추가합니다:

```env
NOTION_API_KEY=your_notion_api_key
NOTION_DATABASE_ID=your_database_id
```

### 2. Notion Integration 생성

1. [Notion Developers](https://www.notion.com/my-integrations)에서 새 Integration 생성
2. API 키 복사
3. Notion 데이터베이스에서 Integration 공유

### 3. 의존성 설치 및 실행

```bash
npm install
npm run dev
```

브라우저에서 [http://localhost:3000](http://localhost:3000) 접속

## 📄 구현 범위 (MVP)

- [x] Notion API 연동 (글 목록, 글 상세)
- [x] 홈 페이지 (글 목록)
- [x] 글 상세 페이지 (블록 렌더링)
- [x] 카테고리 필터
- [x] 기본 반응형 스타일링

## 🔄 라우트

| 경로 | 설명 |
|------|------|
| `/` | 홈 - 최근 글 목록 |
| `/posts/[slug]` | 글 상세 페이지 |
| `/category/[category]` | 카테고리별 글 목록 |

## 📦 프로젝트 구조

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

## 🚀 배포

Vercel에 배포하는 것을 권장합니다:

```bash
npm install -g vercel
vercel
```

## 📖 상세 문서

프로젝트의 상세한 사양은 [docs/PRD.md](./docs/PRD.md)를 참조하세요.

## 📝 라이선스

MIT
