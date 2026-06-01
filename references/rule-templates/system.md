---
description: 项目系统概览、技术栈和架构信息
globs: "**/*"
alwaysApply: true
---

# 项目系统概览

## 项目概述

**{{projectName}}** — 基于 {{framework}} 的前端项目。

## 技术栈

| 类别 | 技术 |
|------|------|
| 框架 | {{framework}} |
| 构建工具 | {{buildTool}} |
| 语言 | {{language}} |
| 样式方案 | {{styling}} |
| 状态管理 | {{stateManagement}} |
| 测试框架 | {{testing}} |
| 代码检查 | {{linting}} |
| 代码格式化 | {{formatting}} |
| HTTP 客户端 | {{httpClient}} |
| 包管理器 | {{packageManager}} |

## 开发命令

| 命令 | 说明 |
|------|------|
| `{{devCommand}}` | 启动开发服务器 |
| `{{buildCommand}}` | 构建生产版本 |
| `{{testCommand}}` | 运行测试 |
| `{{lintCommand}}` | 代码检查 |

## 目录结构

> 以下目录结构根据项目实际检测结果生成。不存在的目录不会列出。

```
{{srcDir}}/
├── components/          # 公共组件
├── pages/               # 页面组件
├── router/              # 路由配置
├── store/               # 状态管理
├── api/                 # API 接口层
├── hooks/               # 组合式函数/Hooks
├── utils/               # 工具函数
├── styles/              # 全局样式
├── types/               # 类型定义
└── locales/             # 国际化
```

> 根据实际项目目录结构调整以上内容。

## 架构决策

### 路由方案
根据检测结果描述路由实现方式。若项目有 `router/` 目录，说明路由配置位置。

### 状态管理
若检测到状态管理库（{{stateManagement}}），描述 Store 组织方式。否则说明组件内使用本地状态。

### 网络请求
若检测到 HTTP 客户端（{{httpClient}}），说明 API 模块封装方式。否则使用原生 fetch。

### 构建配置
构建工具为 **{{buildTool}}**。

## 开发约束

- 所有改动遵循**最小 diff 原则**（详见 `universal.md`）
- 文档与代码冲突时以代码为准
- 不引入与现有技术栈冲突的新依赖
