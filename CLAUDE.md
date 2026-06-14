# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 프로젝트 개요

**프로젝트명**: Notion CMS Blog  
**목적**: Notion을 CMS로 활용한 개인 기술 블로그  
**배포**: Vercel  
**문서**: @docs/PRD.md (기능 정의), @docs/ROADMAP.md (개발 단계)

## 기술 스택 및 코딩 규칙

| 항목 | 설명 |
|------|------|
| Frontend | Next.js 15, TypeScript (strict mode) |
| CMS | Notion API (`@notionhq/client`) |
| Styling | Tailwind CSS, shadcn/ui |
| Icons | Lucide React |
| 린트 | ESLint 규칙 준수 필수 |
| 코드 스타일 | 2칸 들여쓰기, camelCase (변수), PascalCase (컴포넌트), any 타입 금지 |
| 반응형 | 필수 (Tailwind: sm/md/lg breakpoint) |
| 에러 처리 | API 응답과 Notion 연동 시 필수 |

## 환경 설정

`.env.local` 파일:
```env
NOTION_API_KEY=your_api_key
NOTION_DATABASE_ID=your_database_id
```

## 주요 라우트

| 경로 | 설명 |
|------|------|
| `/` | 홈 - 최근 글 목록 |
| `/posts/[slug]` | 글 상세 페이지 |
| `/category/[category]` | 카테고리별 글 목록 |

## Phase 1 완료 시 추가할 정보

다음 항목들은 Phase 1 (프로젝트 골격) 완료 후 업데이트합니다:
- 빌드/개발 명령어 (npm run dev, npm run build, npm test)
- Notion 블록 렌더러 구현 가이드
- ISR 설정 값 (revalidate)
- 테스트 실행 방법
