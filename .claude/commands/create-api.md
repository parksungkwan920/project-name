---
name: create-api
description: Next.js API 라우트와 DTO를 생성합니다
argument-hint: api-name
---

## 작업 내용

`$1`이라는 이름의 새 Next.js API 라우트를 생성합니다. 레이어드 아키텍처 패턴을 따릅니다.

### 생성될 파일 구조

```
src/
├── app/api/$1/
│   └── route.ts              # API 라우트 (Controller)
├── dtos/
│   └── $1.dto.ts             # Request/Response DTO
└── services/
    └── $1.service.ts         # 비즈니스 로직 (Service)
```

### 파일별 내용

**route.ts** - API 라우트
```typescript
import { $1Service } from '@/services/$1.service';
import { NextRequest, NextResponse } from 'next/server';

export async function POST(req: NextRequest) {
  try {
    const body = await req.json();
    const result = await $1Service.create(body);
    return NextResponse.json(result, { status: 201 });
  } catch (error) {
    return NextResponse.json(
      { error: '요청 처리 중 오류가 발생했습니다' },
      { status: 500 }
    );
  }
}

export async function GET() {
  try {
    const result = await $1Service.findAll();
    return NextResponse.json(result);
  } catch (error) {
    return NextResponse.json(
      { error: '요청 처리 중 오류가 발생했습니다' },
      { status: 500 }
    );
  }
}
```

**$1.dto.ts** - DTO
```typescript
export interface Create$1Dto {
  // 요청 필드 정의
}

export interface $1Response {
  id: string;
  createdAt: Date;
  // 응답 필드 정의
}
```

**$1.service.ts** - Service
```typescript
import { Create$1Dto, $1Response } from '@/dtos/$1.dto';

class $1Service {
  async create(dto: Create$1Dto): Promise<$1Response> {
    // 비즈니스 로직 구현
    throw new Error('Not implemented');
  }

  async findAll(): Promise<$1Response[]> {
    // 비즈니스 로직 구현
    throw new Error('Not implemented');
  }
}

export const $1Service = new $1Service();
```

### 주의사항

- API 이름은 kebab-case로 입력해주세요 (예: user-profile, product-list)
- DTO 파일명은 자동으로 PascalCase로 변환됩니다
- Service 클래스는 싱글톤 패턴으로 생성됩니다
- 에러 핸들링과 유효성 검사는 필요에 맞게 추가하세요
