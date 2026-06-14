# 개인 개발 블로그 (Notion CMS) - 프로젝트 가이드

## Project Context

프로젝트의 핵심 문서들입니다. Claude Code를 사용할 때 이 문서들을 참고하세요.

- **PRD 문서**: @docs/PRD.md
- **개발 로드맵**: @docs/ROADMAP.md

## 프로젝트 개요

**프로젝트명**: 개인 개발 블로그  
**목적**: Notion을 CMS로 활용한 개인 기술 블로그  
**배포 환경**: Vercel

## 기술 스택

| 분류 | 기술 |
|------|------|
| Frontend | Next.js 15, TypeScript |
| CMS | Notion API (`@notionhq/client`) |
| Styling | Tailwind CSS, shadcn/ui |
| Icons | Lucide React |

## 개발 단계 (현재 상태)

현재 프로젝트는 **기획 단계**에 있습니다.

- [x] PRD 작성 완료
- [x] 개발 로드맵 작성 완료
- [x] GitHub 저장소 생성 완료
- [ ] Phase 1: 프로젝트 골격 (예정)
- [ ] Phase 2: 공통 모듈 (예정)
- [ ] Phase 3: 핵심 기능 (예정)
- [ ] Phase 4: 추가 기능 (예정)
- [ ] Phase 5: 최적화 및 배포 (예정)

## 주요 라우트

| 경로 | 설명 |
|------|------|
| `/` | 홈 - 최근 글 목록 |
| `/posts/[slug]` | 글 상세 페이지 |
| `/category/[category]` | 카테고리별 글 목록 |

## 환경 변수

`.env.local` 파일에 다음을 설정해야 합니다:

```env
NOTION_API_KEY=your_api_key
NOTION_DATABASE_ID=your_database_id
```

## 주의사항

1. **GitHub 저장소**: https://github.com/parksungkwan920/project-name
2. **개발 언어**: 한국어 (커밋 메시지, 주석)
3. **코드 스타일**: 
   - 들여쓰기: 2칸
   - 네이밍: camelCase (변수), PascalCase (컴포넌트)
   - any 타입 금지

## 다음 단계

Phase 1 (프로젝트 골격) 시작 준비:
1. Next.js 프로젝트 초기 설정
2. Notion API 환경 구축
3. 기본 레이아웃 구조 생성
