---
description: 测试规范和覆盖率要求
globs: "src/**/*.{test,spec}.{ts,js}"
alwaysApply: false
---

# 测试规则

## 测试框架

项目使用 **{{testing}}** 进行单元测试{{#if e2e}}，使用 **{{e2e}}** 进行端到端测试{{/if}}。

## 文件组织

- 测试文件与源文件同级，命名：`{filename}.test.ts` 或 `{filename}.spec.ts`
- E2E 测试集中在 `tests/` 或 `e2e/` 目录

## 测试原则

- **测试行为而非实现**：测试组件做了什么，不测试内部实现细节
- **测试所有状态**：Loading、Empty、Error、Success
- **每个测试独立**：无共享可变状态，每个测试自己准备数据

## AAA 模式

```typescript
// Arrange（准备）-> Act（执行）-> Assert（断言）
describe('getUserInfo', () => {
  it('should return user info when user exists', async () => {
    // Arrange
    const userId = 1
    const mockUser = { id: 1, name: 'Test' }

    // Act
    const result = await getUserInfo(userId)

    // Assert
    expect(result).toEqual(mockUser)
  })
})
```

## 必须测试的状态

```typescript
describe('UserList', () => {
  // Happy path
  it('should render user list when data loads successfully')

  // Loading state
  it('should show loading spinner while fetching data')

  // Empty state
  it('should show empty message when no users found')

  // Error state
  it('should show error message when request fails')

  // Edge case
  it('should handle null user name gracefully')
})
```

## Mock 策略

```typescript
// 外部 API：使用测试工具的 mock 机制
// Vitest
import { vi } from 'vitest'
vi.mock('@/api/user', () => ({
  getUserInfo: vi.fn().mockResolvedValue({ id: 1, name: 'Test' })
}))

// Jest
jest.mock('@/api/user', () => ({
  getUserInfo: jest.fn().mockResolvedValue({ id: 1, name: 'Test' })
}))
```

## 覆盖率阈值

- 语句覆盖率（Statements）：80%
- 分支覆盖率（Branches）：75%
- 函数覆盖率（Functions）：80%
- 行覆盖率（Lines）：80%

```json
// vitest.config.ts 或 jest.config.ts
{
  "coverage": {
    "thresholds": {
      "statements": 80,
      "branches": 75,
      "functions": 80,
      "lines": 80
    }
  }
}
```
