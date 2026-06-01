---
description: 安全编码规范
globs: "src/**/*"
alwaysApply: true
---

# 安全规则

## 密钥管理

```javascript
// 禁止：硬编码密钥
const API_KEY = 'sk-abc123xyz'            // 禁止
const DB_PASSWORD = 'admin123'            // 禁止

// 正确：使用环境变量
const API_KEY = process.env.API_KEY       // Vite: import.meta.env.VITE_API_KEY
const DB_PASSWORD = process.env.DB_PASSWORD
```

所有密钥、Token、密码必须放在环境变量中，通过 `.env.example` 列出模板。

## 输入验证

```typescript
// 推荐使用 Zod 进行运行时验证
import { z } from 'zod'

const UserSchema = z.object({
  name: z.string().min(1).max(50),
  email: z.string().email(),
  age: z.number().int().min(0).max(150).optional()
})

const user = UserSchema.parse(inputData) // 验证失败会抛出错误
```

## XSS 防护

```html
<!-- 禁止：直接渲染用户输入的 HTML -->
<div v-html="userInput"></div>           <!-- 禁止 -->

<!-- 正确：使用文本插值 -->
<div>{{ userInput }}</div>               <!-- 自动转义 -->
```

```tsx
// 禁止：dangerouslySetInnerHTML 未消毒
<div dangerouslySetInnerHTML={{ __html: userInput }} />  // 禁止

// 正确：使用 DOMPurify 消毒
import DOMPurify from 'dompurify'
<div dangerouslySetInnerHTML={{ __html: DOMPurify.sanitize(userInput) }} />
```

## 敏感信息保护

```javascript
// 禁止：在日志/错误信息中输出敏感信息
console.log('密码:', password)            // 禁止
console.log('Token:', token)              // 禁止
throw new Error(`登录失败，密码: ${password}`) // 禁止

// 正确：脱敏输出
console.log('密码:', '****')
throw new Error('登录失败，请检查用户名和密码')
```

## 依赖安全

- 定期检查依赖漏洞：`npm audit` 或 `pnpm audit`
- 不引入废弃/不再维护的依赖包
- 固定依赖版本（`package-lock.json`/`yarn.lock`/`pnpm-lock.yaml` 提交到 Git）
