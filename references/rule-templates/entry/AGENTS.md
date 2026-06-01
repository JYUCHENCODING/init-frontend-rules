# AGENTS.md

本项目使用 `.agents/rules/` 目录管理 AI 编码规则。所有 AI 编码助手（Claude Code、Cursor、Codex 等）均通过本文件了解项目约定。

## 规则索引

| 规则文件 | 说明 | 适用范围 |
|----------|------|----------|
| [通用规则](./.agents/rules/universal.md) | AI 沟通语言、最小改动原则、设计原则 | 所有文件 |
| [系统规则](./.agents/rules/system.md) | 项目概述、技术栈、目录结构、开发命令 | 所有文件 |
| [组件规则](./.agents/rules/components.md) | 组件拆分原则、命名规范、框架示例 | 组件文件 |
| [样式规则](./.agents/rules/styling.md) | 样式组织、变量定义、嵌套规则 | 样式文件 |
| [格式规则](./.agents/rules/format.md) | 代码检查、导入顺序、命名规范表 | 源文件 |
| [注释规则](./.agents/rules/comments.md) | JSDoc 模板、注释原则 | 源文件 |
| [TypeScript 规则](./.agents/rules/typescript.md) | 类型规范、禁止 any、类型守卫 | TypeScript 文件 |
| [测试规则](./.agents/rules/testing.md) | 测试组织、AAA 模式、覆盖率 | 测试文件 |
| [Git 规则](./.agents/rules/git.md) | 提交格式、分支命名、PR 规范 | 所有文件 |
| [安全规则](./.agents/rules/security.md) | 密钥管理、XSS 防护、输入验证 | 所有文件 |

## 阅读顺序

1. **首次使用**：先读 `universal.md` 和 `system.md`，了解项目基本约束
2. **写代码前**：根据当前任务选择对应规则文件
   - 写组件 → `components.md`
   - 写样式 → `styling.md`
   - 写 API → `format.md`（API 模块规范部分）
   - 写测试 → `testing.md`
3. **提交前**：检查 `git.md` 确认提交格式

## 核心原则（速查）

- 沟通语言：中文
- 最小改动：不修改无关代码、不删注释、不升级依赖
- 代码胜文档：代码与文档冲突时以代码为准
- 不过度设计：写当前需求的最简代码
