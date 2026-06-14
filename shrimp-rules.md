# Development Guidelines

프로젝트 AI 에이전트를 위한 개발 규칙 및 의사결정 기준 문서입니다.

---

## Project Overview

**프로젝트명**: Notion CMS Blog
**목적**: Notion을 CMS로 활용한 개인 기술 블로그
**기술 스택**: Next.js 15, TypeScript, Tailwind CSS, shadcn/ui
**배포**: Vercel
**개발 단계**: Phase 1 (프로젝트 초기 설정) 준비 중

### 핵심 문서
- **PRD**: @docs/PRD.md (기능 정의 및 요구사항)
- **ROADMAP**: @docs/ROADMAP.md (5개 Phase 개발 로드맵)
- **CLAUDE.md**: @CLAUDE.md (프로젝트 기술 스택 및 코드 스타일)

---

## Project Architecture

### 폴더 구조

```
project-name/
├── src/
│   ├── app/                 # Next.js 15 app directory
│   │   ├── page.tsx        # 홈 페이지
│   │   ├── posts/
│   │   │   └── [slug]/     # 글 상세 페이지 (동적 라우팅)
│   │   └── category/       # 카테고리 페이지
│   ├── components/          # 재사용 컴포넌트
│   │   ├── Header.tsx
│   │   ├── Footer.tsx
│   │   ├── PostCard.tsx
│   │   ├── Layout.tsx
│   │   └── NotionRenderer.tsx
│   ├── lib/                 # 유틸리티 함수 및 API 래퍼
│   │   └── notion.ts       # Notion API 래퍼
│   ├── types/               # TypeScript 타입 정의
│   │   └── notion.ts       # Notion 관련 타입
│   └── styles/              # 전역 스타일
├── docs/                    # 프로젝트 문서
├── public/                  # 정적 자산
├── .claude/                 # Claude Code 설정
└── package.json
```

### 파일 조직 원칙

- **모든 파일은 src/ 하위에 배치**
- **새 기능 추가 시**: 관련된 타입, 컴포넌트, 유틸 함수를 함께 수정
- **app directory 사용**: pages/ 디렉토리 금지 (Next.js 15 규칙)

---

## Code Standards

### TypeScript 설정

**필수 규칙**:
- `tsconfig.json`: `"strict": true`로 설정
- `"noImplicitAny": true` (any 타입 금지)
- 모든 함수는 **명시적 매개변수 타입** 및 **반환 타입** 정의 필수
- 리액트 props는 interface 또는 type으로 정의

**예시 (정확한 방식)**:
```typescript
interface PostCardProps {
  title: string;
  category: string;
  publishedDate: Date;
}

export function PostCard({ title, category, publishedDate }: PostCardProps) {
  // ...
}
```

**금지 사항**:
- `any` 타입 사용 금지
- 타입 정의 없는 props 전달 금지
- `as const`의 과도한 사용 금지

---

### ESLint 규칙 준수

**설정**:
- Next.js 공식 ESLint config 사용
- 추가 규칙: `eslint-plugin-react-hooks`, `@typescript-eslint`
- `.eslintrc.json`에서 명시된 규칙 따르기

**필수 준수**:
- 모든 변수 사용 전 선언
- console.log는 개발 중에만 사용 (프로덕션 배포 전 제거)
- React hooks 의존성 배열 완전성

---

### 코딩 스타일

| 항목 | 규칙 |
|------|------|
| 들여쓰기 | 2칸 (탭 금지) |
| 변수명 | camelCase (예: `fetchedPosts`, `isLoading`) |
| 함수명 | camelCase (예: `fetchPosts()`, `renderContent()`) |
| 컴포넌트명 | PascalCase (예: `PostCard`, `NotionRenderer`) |
| 타입명 | PascalCase (예: `Post`, `Category`) |
| 상수명 | UPPER_SNAKE_CASE (예: `API_BASE_URL`, `MAX_PAGE_SIZE`) |
| 파일명 | 컴포넌트: PascalCase (예: `PostCard.tsx`), 유틸: camelCase (예: `fetchPosts.ts`) |

### 주석 규칙

- **한국어로만 작성** (코드와 일관성)
- **비즈니스 로직만 주석 작성** (명백한 코드는 주석 금지)
- **WHY를 설명**: WHAT과 HOW가 명백하면 생략
- **단일 라인 주석만 사용** (멀티라인 doc 주석 금지)

**좋은 예**:
```typescript
// Notion API 레이트 제한 회피를 위한 캐싱
const cachedPosts = await cache.get('published-posts');
```

**나쁜 예**:
```typescript
// posts 배열
const posts = [];

/**
 * 게시물을 가져옵니다
 * @param category 카테고리
 * @returns 게시물 배열
 */
function getPosts(category: string) {}
```

---

## Functionality Implementation Standards

### Notion API 연동

**필수 패턴**:
1. `lib/notion.ts`에서 모든 Notion API 호출을 래핑
2. 각 함수는 **에러 처리** 필수 (try-catch 또는 Result 패턴)
3. **타입 안전성**: Notion 응답은 type-safe하게 변환

**작성 규칙**:
```typescript
// lib/notion.ts의 함수 구조
export async function fetchPublishedPosts(): Promise<Post[]> {
  try {
    // Notion API 호출
    // 응답 타입 검증
    // Post[] 반환
  } catch (error) {
    // 에러 로깅
    // 대체값 반환 또는 에러 전파
  }
}
```

### 컴포넌트 개발

**shadcn/ui 활용**:
- 기본 UI 컴포넌트는 **shadcn/ui에서 제공하는 것 우선 사용**
- 커스터마이징 필요 시: 기본 스타일과 구조는 유지하고 **클래스명만 수정**
- 컴포넌트 구조 변경 금지

**반응형 필수**:
- Tailwind CSS breakpoint 기준: `sm` (640px), `md` (768px), `lg` (1024px)
- 모든 페이지와 컴포넌트는 **반응형 대응** 필수
- 모바일/태블릿/데스크탑 모두 테스트

**에러 처리**:
- API 호출 실패 시 사용자 친화적 에러 메시지 표시
- Notion 데이터 누락 시 적절한 폴백 UI 제공

### 상태 관리

- **Zustand 사용** (로컬 상태 저장 필요 시)
- **localStorage 영속성**: `useLocalStorage` 훅 사용
- **전역 상태**: Zustand store로 중앙화

---

## Framework/Plugin/Third-party Library Usage Standards

### Next.js 15

**규칙**:
- **App Router 필수** (Pages Router 금지)
- **Server Components 우선** (클라이언트 컴포넌트 최소화)
- **ISR (Incremental Static Regeneration)**: 각 페이지에 `revalidate` 설정
  - 글 목록: `revalidate: 3600` (1시간)
  - 글 상세: `revalidate: 86400` (24시간)

### TypeScript

**규칙**:
- **strict mode 필수**
- **as Type 캐스팅 최소화** (타입 정의로 해결)
- **generics 활용**: 재사용 가능한 유틸 함수

### Tailwind CSS

**규칙**:
- **utility first** 원칙 준수
- **커스텀 클래스 최소화** (Tailwind 기본 클래스 활용)
- **@apply 사용 최소화**

### shadcn/ui

**규칙**:
- **기본 제공 컴포넌트 우선 사용**
- **props 확장**: 필요시 컴포넌트 props 상속하여 확장

---

## Workflow Standards

### 개발 순서 (ROADMAP 준수)

모든 개발은 ROADMAP.md의 Phase 순서 준수:

1. **Phase 1**: 프로젝트 초기 설정 (1-2일)
2. **Phase 2**: 공통 모듈 개발 (2-3일)
3. **Phase 3**: 핵심 기능 개발 (3-4일)
4. **Phase 4**: 추가 기능 개발 (2-3일)
5. **Phase 5**: 최적화 및 배포 (1-2일)

**규칙**:
- 이전 Phase가 완료되기 전까지 다음 Phase 시작 금지
- 각 Phase 완료 기준 확인 후 진행

### 파일 생성 규칙

**새 컴포넌트 생성 시 동시 수정 대상**:
1. `src/components/ComponentName.tsx` 생성
2. 필요시 `src/types/ComponentName.ts` 타입 추가
3. 사용하는 페이지/컴포넌트에서 import 추가

**새 페이지 생성 시 동시 수정 대상**:
1. `src/app/route/page.tsx` 생성
2. 필요시 `src/types/` 타입 추가
3. CLAUDE.md의 "주요 라우트" 섹션 업데이트

**새 API 함수 생성 시 동시 수정 대상**:
1. `src/lib/notion.ts` 함수 추가
2. `src/types/notion.ts` 타입 추가
3. 해당 함수 사용처에서 import 추가

---

## Key File Interaction Standards

### 필수 동시 수정 파일

| 수정 대상 | 동시 수정할 파일 | 이유 |
|---------|---------------|------|
| `src/types/notion.ts` | 해당 타입 사용 파일들 | 타입 정의 변경 시 사용처 수정 필수 |
| `src/lib/notion.ts` | API 호출 컴포넌트들 | API 함수 변경 시 호출 방식 일치 필요 |
| `tsconfig.json` | `package.json` | 경로 alias 추가 시 확인 |
| `CLAUDE.md` | Phase 완료 후 | 개발 정보 최신화 |
| `.env.example` | `.env.local` | 환경 변수 추가 시 동시 수정 |

### 파일 수정 체크리스트

```
컴포넌트 생성 시:
[ ] 컴포넌트 파일 생성 (.tsx)
[ ] 타입 정의 확인 (있으면 추가)
[ ] 부모 컴포넌트에서 import 확인
[ ] Storybook 테스트 (있으면 추가)

API 함수 생성 시:
[ ] lib/notion.ts에 함수 추가
[ ] types/notion.ts에 타입 추가
[ ] 에러 처리 포함
[ ] 호출 컴포넌트에서 테스트

페이지 생성 시:
[ ] app/route/page.tsx 생성
[ ] 레이아웃 적용 확인
[ ] 메타 태그 설정
[ ] 경로 documentation 추가
```

---

## AI Decision-making Standards

### 기술 선택 우선순위

#### 1. UI 컴포넌트 선택 흐름
```
새로운 UI 컴포넌트 필요?
├─ shadcn/ui에 있는가? → YES → 기본 컴포넌트 사용
├─ Tailwind CSS로 충분한가? → YES → Tailwind 유틸 사용
└─ 커스텀 필요 → shadcn/ui 기반으로 확장
```

#### 2. 상태 관리 선택 흐름
```
상태 관리 필요?
├─ 로컬 컴포넌트 상태? → useState 사용
├─ 전역 상태? → Zustand 사용
├─ localStorage 영속? → useLocalStorage 훅 사용
└─ 서버 상태? → Server Components 사용
```

#### 3. API 호출 선택 흐름
```
API 호출 필요?
├─ Notion API? → lib/notion.ts 함수 사용
├─ 외부 API? → 새 유틸 함수 작성 (lib/ 하위)
└─ 에러 처리 → try-catch 또는 Result 패턴 필수
```

### 모호한 상황에서의 의사결정

| 상황 | 의사결정 기준 |
|------|-------------|
| 컴포넌트 vs 페이지 | 재사용 가능성 있으면 컴포넌트, 단일 라우트 전용이면 페이지 |
| 상태 저장 위치 | 여러 컴포넌트에서 사용하면 Zustand, 단일 컴포넌트면 useState |
| API 함수 위치 | Notion 관련이면 lib/notion.ts, 다른 API면 별도 파일 생성 |
| 타입 위치 | 단일 파일에서만 사용하면 같은 파일, 다중 사용이면 types/ 분리 |
| 스타일링 | Tailwind 유틸로 충분하면 Tailwind, 복잡하면 CSS modules |

---

## Prohibited Actions

### 절대 금지 사항

❌ **any 타입 사용**
```typescript
// 금지
const data: any = fetchData();

// 권장
interface DataResponse {
  id: string;
  title: string;
}
const data: DataResponse = fetchData();
```

❌ **글로벌 변수**
```typescript
// 금지
let globalCount = 0;

// 권장
const [count, setCount] = useState(0);
// 또는 Zustand store 사용
```

❌ **직접 DOM 조작**
```typescript
// 금지
document.getElementById('element').innerHTML = 'text';

// 권장
const [text, setText] = useState('text');
return <div>{text}</div>;
```

❌ **비동기 작업 없이 API 호출**
```typescript
// 금지
const data = fetchData(); // await 누락

// 권장
const data = await fetchData();
// 또는 useEffect 내에서 호출
```

❌ **타입 정의 없는 props**
```typescript
// 금지
function Card(props) {}

// 권장
interface CardProps {
  title: string;
  description: string;
}
function Card({ title, description }: CardProps) {}
```

❌ **Pages Router 사용 (Next.js 15)**
```typescript
// 금지
pages/index.tsx

// 권장
src/app/page.tsx
```

❌ **console.log를 프로덕션에 포함**
```typescript
// 금지 (프로덕션 빌드에 포함)
console.log('debug info');

// 권장 (개발 중에만)
if (process.env.NODE_ENV === 'development') {
  console.log('debug info');
}
```

### 피해야 할 패턴

- ⚠️ **복잡한 조건문**: 3단계 이상 nesting 금지 (함수로 분리)
- ⚠️ **큰 컴포넌트**: 한 컴포넌트 300줄 이상 금지 (분리 필요)
- ⚠️ **하드코딩된 값**: 모든 상수는 파일 상단 또는 .env에 정의
- ⚠️ **과도한 prop drilling**: 3단계 이상이면 Context 또는 Zustand 사용
- ⚠️ **재귀 함수**: 스택 오버플로우 위험 (루프로 변경)

---

## Environment & Build

### 환경 변수 (.env.local)

```env
NOTION_API_KEY=your_api_key
NOTION_DATABASE_ID=your_database_id
```

### 빌드 및 배포

**개발**:
```bash
npm run dev
```

**프로덕션 빌드**:
```bash
npm run build
npm run start
```

**린트 체크**:
```bash
npm run lint
```

### Vercel 배포

- **환경 변수**: Vercel 프로젝트 설정에 `NOTION_API_KEY`, `NOTION_DATABASE_ID` 추가
- **ISR 설정**: `revalidate` 값 확인 후 배포
- **빌드 시간**: 제한 없음 (평균 < 5분)

---

## Summary for AI Agents

이 문서는 AI 에이전트가 **Notion CMS Blog** 프로젝트에서 코드를 작성할 때 따라야 할 규칙입니다.

**핵심 원칙**:
1. ✅ TypeScript strict mode + ESLint 준수
2. ✅ Next.js 15 app directory 사용
3. ✅ shadcn/ui + Tailwind CSS로 UI 구성
4. ✅ 모든 파일은 타입 정의와 에러 처리 포함
5. ✅ ROADMAP의 Phase 순서 절대 준수
6. ✅ 파일 수정 시 동시 수정 대상 확인

**개발 시작 전 확인**:
- [ ] 이전 Phase가 완료되었는가?
- [ ] 필요한 타입 정의가 있는가?
- [ ] 에러 처리가 포함되어 있는가?
- [ ] 반응형 디자인이 구현되었는가?
- [ ] ESLint 경고가 없는가?
