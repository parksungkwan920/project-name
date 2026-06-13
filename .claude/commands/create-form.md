---
name: create-form
description: React Hook Form + Zod + shadcn/ui 폼을 생성합니다
argument-hint: form-name
---

## 작업 내용

`$1`이라는 이름의 폼 컴포넌트를 생성합니다. React Hook Form, Zod, shadcn/ui를 사용합니다.

### 생성될 파일 구조

```
src/
├── components/forms/
│   └── $1-form.tsx           # 폼 컴포넌트
└── schemas/
    └── $1.schema.ts          # Zod 스키마
```

### 파일별 내용

**$1-form.tsx** - 폼 컴포넌트
```typescript
'use client';

import { zodResolver } from '@hookform/resolvers/zod';
import { useForm } from 'react-hook-form';
import { Button } from '@/components/ui/button';
import {
  Form,
  FormControl,
  FormDescription,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '@/components/ui/form';
import { Input } from '@/components/ui/input';
import { $1Schema, type $1Input } from '@/schemas/$1.schema';

export function $1Form() {
  const form = useForm<$1Input>({
    resolver: zodResolver($1Schema),
    defaultValues: {
      // 기본값 설정
    },
  });

  function onSubmit(values: $1Input) {
    console.log(values);
    // 제출 로직 구현
  }

  return (
    <Form {...form}>
      <form onSubmit={form.handleSubmit(onSubmit)} className="space-y-8">
        <FormField
          control={form.control}
          name="example"
          render={({ field }) => (
            <FormItem>
              <FormLabel>필드명</FormLabel>
              <FormControl>
                <Input placeholder="입력해주세요" {...field} />
              </FormControl>
              <FormDescription>필드 설명</FormDescription>
              <FormMessage />
            </FormItem>
          )}
        />
        <Button type="submit">제출</Button>
      </form>
    </Form>
  );
}
```

**$1.schema.ts** - Zod 스키마
```typescript
import { z } from 'zod';

export const $1Schema = z.object({
  example: z.string().min(1, '필수 입력 항목입니다'),
});

export type $1Input = z.infer<typeof $1Schema>;
```

### 주의사항

- 폼 이름은 kebab-case로 입력해주세요 (예: login-form, user-profile-form)
- 컴포넌트 파일명은 자동으로 `{이름}-form.tsx` 형식입니다
- shadcn/ui 폼 컴포넌트가 설치되어 있어야 합니다
- Zod 유효성 검사 규칙은 필요에 맞게 커스터마이징하세요
- 'use client' 지시어는 Next.js 클라이언트 컴포넌트임을 나타냅니다
