---
description: Git 提交规范和分支管理
globs: "**/*"
alwaysApply: true
---

# Git 规则

## 约定式提交（Conventional Commits）

提交信息格式：`<type>(<scope>): <description>`

### Type 类型

| Type | 说明 | 示例 |
|------|------|------|
| `feat` | 新功能 | `feat(login): 添加微信授权登录` |
| `fix` | Bug 修复 | `fix(order): 修复订单金额计算错误` |
| `refactor` | 重构（不改变功能） | `refactor(api): 统一错误处理逻辑` |
| `docs` | 文档更新 | `docs(readme): 更新部署说明` |
| `style` | 格式调整（不影响逻辑） | `style(components): 统一缩进格式` |
| `test` | 测试相关 | `test(order): 添加订单列表单元测试` |
| `chore` | 构建/工具/依赖 | `chore(deps): 升级 vite 到 5.x` |
| `ci` | CI/CD 配置 | `ci: 添加自动部署流程` |

### 提交粒度

- 每个提交一个逻辑变更
- 不把多个不相关的改动混在一个提交里
- Bug 修复时不混入格式化改动

## 分支命名

```
feat/<功能名>       # feat/login-page
fix/<修复内容>       # fix/auth-token-expiry
refactor/<重构范围>  # refactor/api-error-handling
docs/<文档内容>      # docs/api-guide
```

## 禁止事项

- **禁止 force push 到 main/master 分支**
- **禁止提交包含密钥/密码/Token 的文件**
- **禁止提交 `.env` 文件**（使用 `.env.example` 作为模板）
- **禁止提交 `node_modules/`、`dist/`、`.cache/` 等构建产物**

## PR 规范

- PR 标题使用约定式提交格式
- PR 描述包含：改动内容、原因、测试方式
- 单个 PR 不超过 400 行变更（超过则拆分）
- PR 合并前确保 CI 通过
