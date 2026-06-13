---
name: setup-mcp
description: GitHub, Supabase, Vercel MCP 서버 설정 가이드
argument-hint: ""
---

## 개요

이 프로젝트는 3개의 MCP 서버를 지원합니다. 각 서버를 사용하려면 토큰을 발급받고 환경변수로 등록해야 합니다.

## 설정 단계

### 1. GitHub MCP 설정

**토큰 발급:**
1. https://github.com/settings/tokens 접속
2. "Generate new token (classic)" 클릭
3. 권한 설정: `repo`, `read:user`, `gist` 선택
4. 토큰 복사

**환경변수 설정:**
```bash
# PowerShell (영구 설정)
[Environment]::SetEnvironmentVariable("GITHUB_PERSONAL_ACCESS_TOKEN", "your-token-here", "User")

# 또는 .env.local
GITHUB_PERSONAL_ACCESS_TOKEN=your-token-here
```

**사용 예시:**
```
"현재 레포의 열린 PR 목록 보여줘"
"issue #42 상세 내용 조회해줘"
"recent commits 검색해줘"
```

---

### 2. Supabase MCP 설정

**토큰 발급:**
1. https://app.supabase.com/account/tokens 접속
2. "Create new token" 클릭
3. 토큰명 입력 (예: "claude-mcp")
4. 토큰 복사

**환경변수 설정:**
```bash
# PowerShell (영구 설정)
[Environment]::SetEnvironmentVariable("SUPABASE_ACCESS_TOKEN", "your-token-here", "User")

# 또는 .env.local
SUPABASE_ACCESS_TOKEN=your-token-here
```

**사용 예시:**
```
"users 테이블 스키마 확인해줘"
"최근 생성된 레코드 10개 조회"
"storage의 파일 목록 보여줘"
```

---

### 3. Vercel MCP 설정

**토큰 발급:**
1. https://vercel.com/account/tokens 접속
2. "Create Token" 클릭
3. 토큰명 입력 (예: "claude-mcp")
4. Scope: "Full Account" 선택
5. 토큰 복사

**환경변수 설정:**
```bash
# PowerShell (영구 설정)
[Environment]::SetEnvironmentVariable("VERCEL_ACCESS_TOKEN", "your-token-here", "User")

# 또는 .env.local
VERCEL_ACCESS_TOKEN=your-token-here
```

**사용 예시:**
```
"최근 배포 3개 상태 보여줘"
"preview URL 생성해줘"
"환경변수 목록 확인해줘"
```

---

## 모든 서버 확인

Claude Code 재시작 후:
```
/mcp
```

위 명령으로 3개 서버가 `Available` 상태인지 확인합니다.

## 문제 해결

### MCP 서버가 로드되지 않음
1. `.mcp.json` 파일 위치 확인: 프로젝트 루트 (`C:\Users\Metanet\workspace\my-starter-kit\.mcp.json`)
2. 환경변수 확인: PowerShell에서 `$env:GITHUB_PERSONAL_ACCESS_TOKEN` 등으로 확인
3. Claude Code 완전 재시작 (`Ctrl+Shift+Q` 후 재실행)

### 토큰 오류
- 토큰이 만료되지 않았는지 확인
- 권한 스코프가 충분한지 확인
- 토큰이 정확히 복사되었는지 확인 (앞뒤 공백 제거)

## 참고 문서

- GitHub API: https://docs.github.com/en/rest
- Supabase: https://supabase.com/docs
- Vercel: https://vercel.com/docs/rest-api
