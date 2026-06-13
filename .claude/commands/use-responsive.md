---
name: use-responsive
description: Tailwind 브레이크포인트를 감지하는 반응형 커스텀 훅을 생성합니다
argument-hint: ""
---

## 작업 내용

`useResponsive` 커스텀 훅을 생성합니다. Tailwind CSS의 기본 브레이크포인트(sm, md, lg, xl)를 감지해서 현재 화면 크기에 따라 조건부 렌더링이나 로직 분기가 가능합니다.

### 생성될 파일 구조

```
src/
└── hooks/
    └── useResponsive.ts
```

### 파일 내용

**useResponsive.ts**
```typescript
'use client';

import { useEffect, useState } from 'react';

interface ResponsiveBreakpoints {
  isMobile: boolean;
  isTablet: boolean;
  isDesktop: boolean;
  isLargeDesktop: boolean;
  width: number;
}

const BREAKPOINTS = {
  sm: 640,
  md: 768,
  lg: 1024,
  xl: 1280,
};

export function useResponsive(): ResponsiveBreakpoints {
  const [width, setWidth] = useState<number>(0);
  const [isHydrated, setIsHydrated] = useState(false);

  useEffect(() => {
    const handleResize = () => {
      setWidth(window.innerWidth);
    };

    if (typeof window !== 'undefined') {
      setWidth(window.innerWidth);
      setIsHydrated(true);
      window.addEventListener('resize', handleResize);
      return () => window.removeEventListener('resize', handleResize);
    }
  }, []);

  return {
    isMobile: width < BREAKPOINTS.sm,
    isTablet: width >= BREAKPOINTS.sm && width < BREAKPOINTS.lg,
    isDesktop: width >= BREAKPOINTS.lg && width < BREAKPOINTS.xl,
    isLargeDesktop: width >= BREAKPOINTS.xl,
    width: isHydrated ? width : 0,
  };
}
```

### 사용 예시 1: 조건부 렌더링

```typescript
import { useResponsive } from '@/hooks/useResponsive';

export function ResponsiveNav() {
  const { isMobile, isTablet, isDesktop } = useResponsive();

  return (
    <nav>
      {isMobile && <MobileNav />}
      {isTablet && <TabletNav />}
      {isDesktop && <DesktopNav />}
    </nav>
  );
}
```

### 사용 예시 2: 다양한 로직 분기

```typescript
import { useResponsive } from '@/hooks/useResponsive';

export function ProductGrid() {
  const { isMobile, isTablet, isDesktop } = useResponsive();

  const columns = isMobile ? 1 : isTablet ? 2 : 3;
  const itemsPerPage = isMobile ? 5 : isTablet ? 10 : 20;

  return (
    <div className={`grid grid-cols-${columns} gap-4`}>
      {/* 상품 렌더링 */}
    </div>
  );
}
```

### 주의사항

- 훅은 클라이언트 컴포넌트에서만 사용 가능합니다 ('use client')
- 브레이크포인트 값은 Tailwind CSS의 기본 값과 동일합니다
  - sm: 640px
  - md: 768px
  - lg: 1024px
  - xl: 1280px
- SSR 환경에서 hydration을 안전하게 처리하도록 구현되어 있습니다
- 리사이즈 이벤트 리스너는 자동으로 정리됩니다
- `width` 값은 초기에 0이고, hydration 후에 실제 값으로 업데이트됩니다
- 성능 최적화를 위해 실제 필요한 경우에만 사용하세요 (리사이즈 이벤트는 빈번하게 발생)
