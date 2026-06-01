---
description: 代码格式、导入规范和命名约定
globs: "src/**/*.{js,ts,vue,jsx,tsx}"
alwaysApply: true
---

# 格式规则

## 代码检查

项目使用 **{{linting}}** 进行代码检查{{#if formatting}}，使用 **{{formatting}}** 进行代码格式化{{/if}}。

## 导入顺序

按以下顺序组织 import 语句，各组之间用空行分隔：

1. **外部库**（node_modules）
2. **内部模块**（路径别名或相对路径）
3. **样式文件**

```javascript
// 1. 外部库
import { ref, computed } from 'vue'
import { useRouter } from 'vue-router'

// 2. 内部模块
import { getUserInfo } from '@/api/user'
import { formatDate } from '@/utils/date'

// 3. 样式
import './index.scss'
```

## 命名规范

| 类型 | 规范 | 示例 |
|------|------|------|
| 文件名 | kebab-case | `user-profile.vue`、`use-auth.ts`、`date-utils.ts` |
| 文件夹名 | camelCase | `userProfile/`、`orderList/` |
| 组件名 | PascalCase | `UserProfile`、`OrderList` |
| 变量名 | camelCase | `userName`、`orderList` |
| 函数名 | camelCase | `getUserInfo()`、`handleSubmit()` |
| 常量 | UPPER_SNAKE_CASE | `MAX_PAGE_SIZE`、`API_BASE_URL` |
| 类型/接口 | PascalCase | `UserInfo`、`OrderItem` |
| CSS 类名 | kebab-case 或 BEM | `.user-profile`、`.order-list__item` |

## 文件结构规范

### 单文件组件（.vue）

`.vue` 文件按以下顺序组织：
1. `<template>` — 模板
2. `<script>` — 脚本（保持项目当前 API 风格：Options API 或 Composition API）
3. `<style>` — 样式

```vue
<template>
  <div class="component-name">
    <!-- 模板内容 -->
  </div>
</template>

<script>
export default {
  name: 'ComponentName',
  props: {},
  data() {},
  computed: {},
  methods: {}
}
</script>

<style scoped>
.component-name {
  /* 样式 */
}
</style>
```

### React 组件（.tsx / .jsx）

```tsx
import { useState } from 'react'

interface ComponentNameProps {
  title: string
}

export function ComponentName({ title }: ComponentNameProps) {
  return <div className="component-name">{title}</div>
}
```

## API 模块规范

- API 请求统一封装在 `{{srcDir}}/api/` 目录
- 每个业务领域一个文件：`api/user.ts`、`api/order.ts`
- 所有请求函数返回 Promise
- 错误在调用方处理，API 层只负责请求

```typescript
// api/user.ts
export interface UserInfo {
  id: number
  name: string
}

export function getUserInfo(): Promise<UserInfo> {
  return request.get('/user/info')
}
```

## 工具函数规范

- 工具函数放在 `{{srcDir}}/utils/` 目录
- 按功能分类文件：`date.ts`、`string.ts`、`validate.ts`
- 所有函数为纯函数（无副作用）
- 做好输入容错（处理 `null`、`undefined`、空字符串等边界值）
