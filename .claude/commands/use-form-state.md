---
name: use-form-state
description: React Hook Form + Zod 폼의 상태를 관리하는 커스텀 훅을 생성합니다
argument-hint: ""
---

## 작업 내용

`useFormState` 커스텀 훅을 생성합니다. 폼의 제출 상태(isSubmitting), 에러 메시지, 성공 상태를 통합으로 관리합니다.

### 생성될 파일 구조

```
src/
└── hooks/
    └── useFormState.ts
```

### 파일 내용

**useFormState.ts**
```typescript
'use client';

import { useState } from 'react';

interface UseFormStateResult<T> {
  isSubmitting: boolean;
  error: string | null;
  success: boolean;
  handleSubmit: (values: T) => Promise<void>;
}

export function useFormState<T>(
  onSubmit: (values: T) => Promise<void>
): UseFormStateResult<T> {
  const [error, setError] = useState<string | null>(null);
  const [success, setSuccess] = useState(false);
  const [isSubmitting, setIsSubmitting] = useState(false);

  const handleSubmit = async (values: T) => {
    setIsSubmitting(true);
    setError(null);
    setSuccess(false);

    try {
      await onSubmit(values);
      setSuccess(true);
      setTimeout(() => setSuccess(false), 3000);
    } catch (e) {
      const errorMessage =
        e instanceof Error ? e.message : '오류가 발생했습니다';
      setError(errorMessage);
    } finally {
      setIsSubmitting(false);
    }
  };

  return { isSubmitting, error, success, handleSubmit };
}
```

### 사용 예시

```typescript
import { useFormState } from '@/hooks/useFormState';

export function LoginForm() {
  const { isSubmitting, error, success, handleSubmit } = useFormState(
    async (values) => {
      const response = await fetch('/api/login', {
        method: 'POST',
        body: JSON.stringify(values),
      });
      if (!response.ok) throw new Error('로그인 실패');
    }
  );

  return (
    <form onSubmit={(e) => {
      e.preventDefault();
      handleSubmit(formData);
    }}>
      {error && <p className="text-red-500">{error}</p>}
      {success && <p className="text-green-500">성공했습니다!</p>}
      <button disabled={isSubmitting}>
        {isSubmitting ? '제출 중...' : '제출'}
      </button>
    </form>
  );
}
```

### 주의사항

- 훅은 클라이언트 컴포넌트에서만 사용 가능합니다 ('use client')
- `onSubmit` 함수는 비동기 함수여야 합니다
- 성공 상태는 자동으로 3초 후 초기화됩니다
- 에러 발생 시 에러 메시지가 표시되고 제출 버튼은 자동으로 비활성화됩니다
