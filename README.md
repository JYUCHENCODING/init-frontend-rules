# init-frontend-rules

[中文文档](#chinese) | [English Docs](#english)

---

<a id="english"></a>

## English

**init-frontend-rules** is an AI coding rules initialization tool for frontend projects. It detects your tech stack from `package.json`, config files, and directory structure, then generates a comprehensive set of AI rule files under `.agents/rules/` along with entry points (`AGENTS.md`, `CLAUDE.md`, `.cursorrules`) compatible with Claude Code, Cursor, Codex, and other AI coding assistants.

### Quick Install

```bash
# Install via skills.sh
skills.sh install init-frontend-rules --from JYUCHENCODING/init-frontend-rules
```

### What It Does

1. **Detects** — scans `package.json`, `tsconfig.json`, config files, and `src/` layout to identify:
   - Framework (Vue 2/3, React, Angular, Svelte)
   - Meta-framework (Next.js, Nuxt, Remix, Astro)
   - Build tool (Vite, Webpack, Vue CLI, CRA)
   - State management (Pinia, Vuex, Zustand, Redux, Jotai, etc.)
   - Styling solution (Tailwind CSS, SCSS, Less, CSS-in-JS)
   - TypeScript usage and strictness
   - Testing framework (Vitest, Jest, Playwright, Cypress)
   - Linting & formatting tools
   - HTTP client & query library
   - i18n solution
   - Package manager (npm, yarn, pnpm)
   - Dev / build / test / lint commands

2. **Confirms** — shows the detection summary and asks 2-3 questions to verify accuracy before generating anything.

3. **Generates** — writes rule files from templates, replacing placeholders with detected values and keeping only the framework-specific variants that match your stack.

4. **Validates** — checks all generated files have valid YAML frontmatter, no leftover placeholders, and consistent cross-references.

### Output Structure

```
project-root/
├── .agents/rules/
│   ├── universal.md          # Universal rules (language, minimal changes, design principles)
│   ├── system.md             # Project overview, tech stack, directory structure, commands
│   ├── components.md         # Component naming, structure, framework-specific patterns
│   ├── styling.md            # Style organization, nesting, variable conventions
│   ├── format.md             # Code quality, import order, naming conventions
│   ├── comments.md           # JSDoc templates, comment principles
│   ├── typescript.md         # TypeScript rules (conditional: tsconfig.json exists)
│   ├── testing.md            # Test organization, AAA pattern, coverage (conditional)
│   ├── git.md                # Commit format, branch naming, PR conventions
│   └── security.md           # Secret management, XSS prevention, input validation
├── AGENTS.md                 # Main entry — indexes all rules with reading order
├── CLAUDE.md                 # Compact entry — redirects to AGENTS.md
└── .cursorrules              # Cursor IDE compatible entry point
```

### Supported Tech Stack Detection

| Category | Supported |
|----------|-----------|
| Framework | Vue 2/3, React, Angular, Svelte |
| Meta-framework | Next.js, Nuxt, Remix, Astro |
| Build tool | Vite, Webpack, Vue CLI, CRA |
| State management | Pinia, Vuex, Zustand, Redux Toolkit, Jotai, Recoil, MobX |
| Styling | Tailwind CSS, SCSS/Sass, Less, styled-components, Emotion, CSS Modules |
| TypeScript | Yes (detects strict mode, path aliases) |
| Testing | Vitest, Jest, Playwright, Cypress |
| Linting | ESLint, Biome, Oxlint |
| Formatting | Prettier |
| HTTP client | Axios, Ky, Got, TanStack Query, SWR |
| i18n | Vue I18n, react-i18next, Nuxt I18n, Next.js Intl |
| Package manager | npm, yarn, pnpm |
| Monorepo | Yes (detects workspaces, asks which package to configure) |

### Usage Flow

```
User: /init-frontend-rules

AI:   [Scans project...]
      ## Detection Results
      Framework: Vue 3
      Build Tool: Vite
      Styling: SCSS
      State Management: Pinia
      TypeScript: Yes (strict: true)
      Testing: Vitest

      1. Is this correct? Any missing tech?
      2. Mode: fresh (overwrite) or incremental (add missing only)?
      3. Any additional project conventions?

User: [Confirms or adjusts]

AI:   [Generates all rule files...]
      ## init-rules Complete
      Generated rules: 8 → .agents/rules/
      Entry files: AGENTS.md, CLAUDE.md, .cursorrules
      Tech stack: Vue 3 + Vite + SCSS + Pinia + TypeScript + Vitest
```

### Edge Cases Handled

| Scenario | Behavior |
|----------|----------|
| No `package.json` | Asks user for project type, generates core rules only |
| Monorepo | Detects workspaces, asks which package to configure |
| Existing `.agents/rules/` | Shows existing files, asks file-by-file overwrite/skip |
| No `.git` | Generates `git.md` with note to initialize repo |
| Empty project (no `src/`) | Generates rules with note to update after structure is built |
| Non-frontend project | Skips `components.md` and `styling.md` |

### File Structure of This Repository

```
init-frontend-rules/
├── SKILL.md                          # Skill definition (entry point for skills.sh)
├── README.md                         # This file
└── references/
    ├── detection-guide.md            # Complete tech stack detection rules
    └── rule-templates/
        ├── universal.md              # Template: universal rules
        ├── system.md                 # Template: system/project rules
        ├── components.md             # Template: component rules (Vue & React variants)
        ├── styling.md                # Template: styling rules (SCSS & Tailwind variants)
        ├── format.md                 # Template: formatting rules
        ├── comments.md               # Template: comment rules
        ├── typescript.md             # Template: TypeScript rules
        ├── testing.md                # Template: testing rules
        ├── git.md                    # Template: git rules
        ├── security.md               # Template: security rules
        └── entry/
            ├── AGENTS.md             # Template: main entry point
            ├── CLAUDE.md             # Template: compact entry point
            └── .cursorrules          # Template: Cursor IDE compatibility
```

---

<a id="chinese"></a>

## 中文

**init-frontend-rules** 是一个前端项目 AI 编码规则初始化工具。它从 `package.json`、配置文件和目录结构中自动检测技术栈，然后在 `.agents/rules/` 下生成一套完整的 AI 规则文件，同时生成入口文件（`AGENTS.md`、`CLAUDE.md`、`.cursorrules`），兼容 Claude Code、Cursor、Codex 等 AI 编码助手。

### 快速安装

```bash
# 通过 skills.sh 安装
skills.sh install init-frontend-rules --from JYUCHENCODING/init-frontend-rules
```

### 功能概述

1. **检测** — 扫描 `package.json`、`tsconfig.json`、配置文件及 `src/` 目录结构，识别：
   - 框架（Vue 2/3、React、Angular、Svelte）
   - 元框架（Next.js、Nuxt、Remix、Astro）
   - 构建工具（Vite、Webpack、Vue CLI、CRA）
   - 状态管理（Pinia、Vuex、Zustand、Redux、Jotai 等）
   - 样式方案（Tailwind CSS、SCSS、Less、CSS-in-JS）
   - TypeScript 使用及严格模式
   - 测试框架（Vitest、Jest、Playwright、Cypress）
   - 代码检查与格式化工具
   - HTTP 客户端与请求库
   - 国际化方案
   - 包管理器（npm、yarn、pnpm）
   - 开发/构建/测试/检查命令

2. **确认** — 展示检测摘要，生成前通过 2-3 个问题确认准确性。

3. **生成** — 基于模板写入规则文件，自动替换占位符，只保留与技术栈匹配的框架变体。

4. **验证** — 检查所有生成文件是否具备有效 YAML frontmatter、无残留占位符、交叉引用一致。

### 输出结构

```
项目根目录/
├── .agents/rules/
│   ├── universal.md          # 通用规则（沟通语言、最小改动、设计原则）
│   ├── system.md             # 项目概述、技术栈、目录结构、开发命令
│   ├── components.md         # 组件命名、结构、框架特定模式
│   ├── styling.md            # 样式组织、嵌套、变量规范
│   ├── format.md             # 代码质量、导入顺序、命名规范
│   ├── comments.md           # JSDoc 模板、注释原则
│   ├── typescript.md         # TypeScript 规则（条件生成：存在 tsconfig.json）
│   ├── testing.md            # 测试组织、AAA 模式、覆盖率（条件生成）
│   ├── git.md                # 提交格式、分支命名、PR 规范
│   └── security.md           # 密钥管理、XSS 防护、输入验证
├── AGENTS.md                 # 主入口，索引所有规则及阅读顺序
├── CLAUDE.md                 # 精简入口，指向 AGENTS.md
└── .cursorrules              # Cursor IDE 兼容入口
```

### 支持的技术栈检测

| 类别 | 支持项 |
|------|--------|
| 框架 | Vue 2/3、React、Angular、Svelte |
| 元框架 | Next.js、Nuxt、Remix、Astro |
| 构建工具 | Vite、Webpack、Vue CLI、CRA |
| 状态管理 | Pinia、Vuex、Zustand、Redux Toolkit、Jotai、Recoil、MobX |
| 样式方案 | Tailwind CSS、SCSS/Sass、Less、styled-components、Emotion、CSS Modules |
| TypeScript | 支持（检测 strict 模式、路径别名） |
| 测试 | Vitest、Jest、Playwright、Cypress |
| 代码检查 | ESLint、Biome、Oxlint |
| 格式化 | Prettier |
| HTTP 客户端 | Axios、Ky、Got、TanStack Query、SWR |
| 国际化 | Vue I18n、react-i18next、Nuxt I18n、Next.js Intl |
| 包管理器 | npm、yarn、pnpm |
| Monorepo | 支持（检测 workspaces，询问为哪个包配置） |

### 使用流程

```
用户: /init-frontend-rules

AI:   [扫描项目中...]
      ## 检测结果
      框架: Vue 3
      构建工具: Vite
      样式方案: SCSS
      状态管理: Pinia
      TypeScript: 是 (strict: true)
      测试框架: Vitest

      1. 检测结果是否正确？有无遗漏的技术栈？
      2. 模式：全新初始化（覆盖已有文件）还是增量（只添加缺失的）？
      3. 有无额外的项目约定需要加入？

用户: [确认或调整]

AI:   [生成所有规则文件...]
      ## init-rules 完成
      生成规则: 8 个 → .agents/rules/
      入口文件: AGENTS.md, CLAUDE.md, .cursorrules
      技术栈: Vue 3 + Vite + SCSS + Pinia + TypeScript + Vitest
```

### 边缘情况处理

| 场景 | 处理方式 |
|------|----------|
| 无 `package.json` | 询问用户项目类型，仅生成核心规则 |
| Monorepo | 检测 workspaces 字段，询问为哪个包配置 |
| 已有 `.agents/rules/` | 展示已有文件摘要，逐文件询问覆盖/跳过 |
| 无 `.git` | 正常生成 `git.md`，顶部注明需初始化仓库 |
| 空项目（无 `src/`） | 生成规则，注明目录结构建立后需更新 |
| 非前端项目 | 跳过 `components.md` 和 `styling.md` |

### 本仓库文件结构

```
init-frontend-rules/
├── SKILL.md                          # 技能定义（skills.sh 入口）
├── README.md                         # 本文件
└── references/
    ├── detection-guide.md            # 完整技术栈检测规则
    └── rule-templates/
        ├── universal.md              # 模板：通用规则
        ├── system.md                 # 模板：系统/项目规则
        ├── components.md             # 模板：组件规则（Vue & React 变体）
        ├── styling.md                # 模板：样式规则（SCSS & Tailwind 变体）
        ├── format.md                 # 模板：格式规则
        ├── comments.md               # 模板：注释规则
        ├── typescript.md             # 模板：TypeScript 规则
        ├── testing.md                # 模板：测试规则
        ├── git.md                    # 模板：Git 规则
        ├── security.md               # 模板：安全规则
        └── entry/
            ├── AGENTS.md             # 模板：主入口
            ├── CLAUDE.md             # 模板：精简入口
            └── .cursorrules          # 模板：Cursor IDE 兼容
```

### License

MIT
