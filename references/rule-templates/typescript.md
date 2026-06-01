---
description: TypeScript 类型规范和约束
globs: "src/**/*.ts"
alwaysApply: false
---

# TypeScript 规则

## 严格模式

项目启用 TypeScript 严格模式（`tsconfig.json` 中 `strict: true`），必须遵守：
- `noImplicitAny` — 不允许隐式 `any`
- `strictNullChecks` — 严格空值检查
- `strictFunctionTypes` — 严格函数类型检查

## 禁止 `any`

```typescript
// 禁止
const data: any = getUserInfo()

// 推荐
const data: UserInfo = getUserInfo()

// 不确定类型时
const data: unknown = getExternalData()
if (isUserInfo(data)) {
  // data 在此作用域内类型为 UserInfo
}
```

## Type vs Interface

- **`interface`** — 用于对象结构定义、可扩展的类型
- **`type`** — 用于联合类型、交叉类型、类型别名

```typescript
// interface：对象类型
interface UserInfo {
  id: number
  name: string
  email: string
}

// type：联合类型
type OrderStatus = 'pending' | 'paid' | 'shipped' | 'completed'

// type：工具类型
type Nullable<T> = T | null
type DeepPartial<T> = { [K in keyof T]?: DeepPartial<T[K]> }
```

## 可辨识联合类型 + Exhaustive Check

```typescript
type Result<T> =
  | { status: 'success'; data: T }
  | { status: 'error'; error: string }
  | { status: 'loading' }

function handleResult<T>(result: Result<T>) {
  switch (result.status) {
    case 'success':
      return result.data
    case 'error':
      throw new Error(result.error)
    case 'loading':
      return null
    default:
      // exhaustive check：若遗漏任何 variant，编译时报错
      const _exhaustive: never = result
      return _exhaustive
  }
}
```

## 类型断言

```typescript
// 禁止：as 强制断言（除非绝对确定）
const el = document.querySelector('.box') as HTMLDivElement

// 推荐：类型守卫
const el = document.querySelector('.box')
if (el instanceof HTMLDivElement) {
  el.style.display = 'none'
}

// 使用 satisfies（TypeScript 4.9+）进行类型检查而不改变类型
const config = {
  host: 'localhost',
  port: 3000
} satisfies ServerConfig
```

## 泛型约束

```typescript
// 推荐：使用 extends 约束泛型
function getProperty<T extends Record<string, unknown>>(obj: T, key: keyof T) {
  return obj[key]
}

// 避免裸泛型
function getLength<T>(list: T) { return list.length } // 禁止
```

## 导入类型

```typescript
// 仅导入类型时使用 type 关键字（有利于 tree-shaking）
import type { UserInfo, OrderItem } from '@/types'
import { getUserInfo } from '@/api/user'
```
