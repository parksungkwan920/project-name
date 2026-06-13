---
name: add-component
description: React 컴포넌트를 생성합니다 (TypeScript + Tailwind CSS)
argument-hint: ComponentName
---

## 작업 내용

`$1`이라는 이름의 새 React 함수형 컴포넌트를 `src/components/` 폴더에 생성합니다.

### 실행 단계

1. `src/components/` 폴더가 존재하는지 확인
2. `$1.tsx` 파일 생성 (PascalCase 이름)
3. 다음 기본 템플릿 추가:
   - TypeScript 타입 정의
   - React 함수형 컴포넌트
   - Tailwind CSS 클래스
   - 간단한 JSX 구조

### 생성될 파일 구조

```
src/components/$1.tsx
```

### 템플릿 코드

파일에 다음과 같은 내용을 추가합니다:

```typescript
interface ${1}Props {
  // 여기에 props 타입을 정의하세요
}

export function $1({ }: ${1}Props) {
  return (
    <div className="flex items-center justify-center">
      <p className="text-lg font-semibold">$1 컴포넌트</p>
    </div>
  );
}
```

### 주의사항

- 컴포넌트 이름은 PascalCase로 입력해야 합니다 (예: MyButton, UserCard)
- 이미 존재하는 파일명이면 덮어쓰지 않습니다
- props 인터페이스는 필요에 맞게 수정하세요
