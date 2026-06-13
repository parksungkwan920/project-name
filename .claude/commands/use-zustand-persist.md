---
name: use-zustand-persist
description: Zustand 상태를 localStorage에 영속시키는 커스텀 훅을 생성합니다
argument-hint: ""
---

## 작업 내용

`useZustandPersist` 커스텀 훅을 생성합니다. Zustand 스토어를 localStorage에 저장하면서 SSR(서버 사이드 렌더링) 환경에서도 안전하게 작동합니다.

### 생성될 파일 구조

```
src/
└── hooks/
    └── useZustandPersist.ts
```

### 파일 내용

**useZustandPersist.ts**
```typescript
'use client';

import { useEffect, useState } from 'react';

interface UseZustandPersistResult<T> {
  state: T;
  setState: (value: T) => void;
  isHydrated: boolean;
}

export function useZustandPersist<T>(
  key: string,
  initialValue: T
): UseZustandPersistResult<T> {
  const [isHydrated, setIsHydrated] = useState(false);
  const [state, internalSetState] = useState<T>(initialValue);

  useEffect(() => {
    if (typeof window === 'undefined') return;

    try {
      const stored = localStorage.getItem(key);
      if (stored) {
        internalSetState(JSON.parse(stored) as T);
      }
    } catch (error) {
      console.error(`Failed to load persisted state for key "${key}":`, error);
    }

    setIsHydrated(true);
  }, [key]);

  const setState = (value: T) => {
    internalSetState(value);
    if (typeof window !== 'undefined') {
      try {
        localStorage.setItem(key, JSON.stringify(value));
      } catch (error) {
        console.error(`Failed to persist state for key "${key}":`, error);
      }
    }
  };

  return { state: isHydrated ? state : initialValue, setState, isHydrated };
}
```

### 사용 예시

```typescript
import { useZustandPersist } from '@/hooks/useZustandPersist';

interface User {
  id: string;
  name: string;
  email: string;
}

const initialUser: User = {
  id: '',
  name: '',
  email: '',
};

export function UserProfile() {
  const { state: user, setState: setUser, isHydrated } = useZustandPersist(
    'user',
    initialUser
  );

  if (!isHydrated) {
    return <p>로딩 중...</p>;
  }

  return (
    <div>
      <p>이름: {user.name}</p>
      <p>이메일: {user.email}</p>
      <button
        onClick={() =>
          setUser({ ...user, name: '새로운 이름' })
        }
      >
        업데이트
      </button>
    </div>
  );
}
```

### 주의사항

- 훅은 클라이언트 컴포넌트에서만 사용 가능합니다 ('use client')
- SSR 환경에서의 hydration mismatch를 방지하기 위해 `isHydrated` 상태를 확인해야 합니다
- localStorage에 저장할 수 있는 데이터는 JSON 직렬화 가능한 객체만 가능합니다
- 매개변수 `key`는 중복되지 않는 고유한 값이어야 합니다
- 대용량 데이터는 성능상 localStorage에 저장하지 않는 것이 좋습니다
