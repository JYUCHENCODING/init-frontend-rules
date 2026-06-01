# 技术栈检测指南

本文档定义了从项目文件中检测技术栈的完整规则。

## Phase 1: package.json 解析

读取项目根目录下的 `package.json`，解析 `dependencies` 和 `devDependencies`。

### 框架检测

| 包名 | 检测结果 | 版本判断 |
|------|---------|---------|
| `vue` | Vue | 查看版本号首位：2.x → Vue 2，3.x → Vue 3 |
| `react` | React | 配合 `react-dom` 确认 |
| `@angular/core` | Angular | — |
| `svelte` | Svelte | — |
| `@vue/composition-api` | Vue 2 + Composition API 插件 | — |

Vue 2 vs Vue 3 精确判断：
- `vue` 版本 `^2.x` → Vue 2
- `vue` 版本 `^3.x` → Vue 3
- 若同时存在 `@vue/composition-api` 则为 Vue 2 + Composition API

### 元框架检测

| 包名 | 检测结果 |
|------|---------|
| `next` | Next.js (React) |
| `nuxt` / `@nuxt/core` | Nuxt (Vue) |
| `@remix-run/react` | Remix (React) |
| `astro` | Astro |

### 构建工具检测

| 包名 | 检测结果 | 补充检查 |
|------|---------|---------|
| `vite` | Vite | 查找 `vite.config.*` |
| `@vue/cli-service` | Vue CLI (webpack) | — |
| `webpack` | Webpack | 查找 `webpack.config.*` |
| `react-scripts` | Create React App (webpack) | — |

### 状态管理检测

| 包名 | 检测结果 | 适用框架 |
|------|---------|---------|
| `pinia` | Pinia | Vue |
| `vuex` | Vuex | Vue |
| `zustand` | Zustand | React |
| `@reduxjs/toolkit` / `redux` | Redux Toolkit | React |
| `jotai` | Jotai | React |
| `recoil` | Recoil | React |
| `mobx` | MobX | React |

### 样式方案检测

| 包名 / 文件 | 检测结果 |
|-------------|---------|
| `tailwindcss` + `tailwind.config.*` | Tailwind CSS |
| `sass` / `node-sass` | SCSS/Sass |
| `less` | Less |
| `styled-components` | styled-components (CSS-in-JS) |
| `@emotion/react` | Emotion (CSS-in-JS) |

默认（无以上任何信号）→ 纯 CSS / CSS Modules。

### 测试框架检测

| 包名 | 检测结果 |
|------|---------|
| `vitest` | Vitest |
| `jest` / `@jest/globals` | Jest |
| `@playwright/test` | Playwright（E2E） |
| `cypress` | Cypress（E2E） |

### 代码质量工具检测

| 包名 / 文件 | 检测结果 |
|-------------|---------|
| `eslint` + `.eslintrc.*` / `eslint.config.*` | ESLint |
| `@biomejs/biome` + `biome.json` | Biome |
| `prettier` + `.prettierrc.*` | Prettier |
| `oxlint` | Oxlint |

### HTTP 客户端检测

| 包名 | 检测结果 |
|------|---------|
| `axios` | Axios |
| `ky` | Ky |
| `got` | Got |
| `@tanstack/react-query` / `@tanstack/vue-query` | TanStack Query |
| `swr` | SWR（React） |

### i18n 检测

| 包名 | 检测结果 |
|------|---------|
| `vue-i18n` | Vue I18n |
| `react-i18next` / `i18next` | react-i18next |
| `@nuxtjs/i18n` | Nuxt I18n |
| `next-intl` | Next.js Intl |

### 包管理器检测

检查锁文件：
- `pnpm-lock.yaml` → pnpm
- `yarn.lock` → yarn
- `package-lock.json` → npm

## Phase 2: 配置文件检查

使用 Glob 查找以下文件：

```
vite.config.{ts,js,mjs}
next.config.{ts,js,mjs}
nuxt.config.{ts,js}
webpack.config.{ts,js}
tailwind.config.{ts,js}
.eslintrc.{js,json,yaml,yml}
eslint.config.{ts,js,mjs}
biome.json
.prettierrc.{js,json,yaml,yml}
tsconfig.json
jsconfig.json
```

### tsconfig.json 解析

若存在，读取：
- `compilerOptions.strict` → 是否启用严格模式
- `compilerOptions.paths` → 路径别名（如 `@/*`）
- `include` / `exclude` → 源码目录范围

## Phase 3: 目录结构分析

读取项目根目录和 `src/`（或检测到的源码目录）的结构：

| 目录 | 说明 | 对应规则 |
|------|------|---------|
| `src/components/` | 组件目录 | components.md globs |
| `src/pages/` 或 `src/views/` | 页面目录 | 路由架构 |
| `src/api/` 或 `src/services/` | API 层 | api 相关规则 |
| `src/store/` 或 `src/stores/` | 状态管理 | state-management 规则 |
| `src/router/` 或 `src/routes/` | 路由配置 | 路由架构 |
| `src/hooks/` 或 `src/composables/` | 组合式函数/Hooks | 逻辑复用模式 |
| `src/utils/` 或 `src/helpers/` | 工具函数 | 工具函数规范 |
| `src/styles/` 或 `src/assets/styles/` | 全局样式 | 样式规则 |
| `src/types/` 或 `src/@types/` | 类型定义 | TypeScript 规则 |
| `src/locales/` 或 `src/i18n/` | 国际化 | i18n 规则 |
| `packages/` | Monorepo 子包 | monorepo 处理 |

## Phase 4: 检测结果汇总

将以上结果汇总为结构化摘要：

```
projectName: <package.json name>
framework: vue|react|angular|svelte|none
frameworkVersion: 2|3|null
metaFramework: next|nuxt|remix|astro|null
buildTool: vite|webpack|vue-cli|cra|null
typescript: true|false
typescriptStrict: true|false
styling: tailwind|scss|less|css-modules|none
stateManagement: pinia|vuex|zustand|redux|jotai|recoil|mobx|none
testing: vitest|jest|none
e2e: playwright|cypress|none
linting: eslint|biome|none
formatting: prettier|none
httpClient: axios|ky|got|fetch|none
queryLib: tanstack-query|swr|none
i18n: vue-i18n|react-i18next|nuxt-i18n|next-intl|none
packageManager: pnpm|yarn|npm
isMonorepo: true|false
srcDir: src|app|null
devCommand: <from scripts.dev>
buildCommand: <from scripts.build>
testCommand: <from scripts.test>
lintCommand: <from scripts.lint>
```

未检测到的字段值设为 `none` 或 `null`。
