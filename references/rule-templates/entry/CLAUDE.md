# CLAUDE.md

## 项目身份

**{{projectName}}** — 基于 {{framework}} 的前端项目。

## 技术栈

{{framework}} + {{buildTool}} + {{styling}} + {{stateManagement}}{{#if typescript}} + TypeScript{{/if}}

## 常用命令

| 命令 | 说明 |
|------|------|
| `{{devCommand}}` | 启动开发服务器 |
| `{{buildCommand}}` | 构建生产版本 |
| `{{testCommand}}` | 运行测试 |
| `{{lintCommand}}` | 代码检查 |

## AI 编码规则

本项目使用 `.agents/rules/` 管理编码规则。入口文件：[AGENTS.md](./AGENTS.md)

### 速查

- 沟通用中文，最小改动原则
- 组件命名 PascalCase，文件命名 kebab-case
- 嵌套不超过 3 层，禁止 `&` 拼接类名
- 禁止 `any`、硬编码密钥、`v-html`/`dangerouslySetInnerHTML`
- 提交格式：`feat(scope): description`
