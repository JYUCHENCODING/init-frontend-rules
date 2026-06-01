---
name: init-frontend-rules
description: Frontend project AI rules initialization tool. Detects tech stack from package.json, config files, and directory structure, then generates comprehensive rule files in .agents/rules/. Also generates AGENTS.md, CLAUDE.md, and .cursorrules as entry points. Use when user says "init frontend rules", "initialize frontend rules", "setup frontend AI rules", "create frontend rule files", or wants to set up AI conventions for a new frontend project.
---

# init-rules — 项目 AI 规则初始化工具

为新项目或现有项目一键生成 AI 编码规则文件。自动检测技术栈，生成适配的规则。

## 输出

```
项目根目录/
├── .agents/rules/
│   ├── universal.md          # 通用规则
│   ├── system.md             # 系统规则
│   ├── components.md         # 组件规则
│   ├── styling.md            # 样式规则
│   ├── format.md             # 格式规则
│   ├── comments.md           # 注释规则
│   ├── typescript.md         # TypeScript 规则（条件）
│   ├── testing.md            # 测试规则（条件）
│   ├── git.md                # Git 规则
│   └── security.md           # 安全规则
├── AGENTS.md                 # 主入口，索引所有规则
├── CLAUDE.md                 # 精简入口，指向 AGENTS.md
└── .cursorrules              # Cursor IDE 兼容入口
```

## 工作流程

### Step 1: 检测

读取以下文件，确定项目技术栈：

- `package.json` → 从 dependencies/devDependencies 检测框架、版本、状态管理、样式方案、测试框架、构建工具。详见 [detection-guide.md](references/detection-guide.md)
- `tsconfig.json` → 是否使用 TypeScript、严格模式、路径别名
- 配置文件 → `vite.config.*` / `next.config.*` / `nuxt.config.*`、`tailwind.config.*`、`.eslintrc.*` / `biome.json`、`.prettierrc.*`
- 目录结构 → `src/` 布局、`components/`、`pages/`/`views/`、`api/`/`services/`、`store/`/`stores/`、`router/`

将检测结果汇总为摘要，展示给用户。

### Step 2: 确认

展示检测摘要：

```
## 检测结果

框架: Vue 3
构建工具: Vite
样式方案: SCSS
状态管理: Pinia
TypeScript: 是 (strict: true)
测试框架: Vitest

命令: dev=npm run dev, build=npm run build, test=npm run test, lint=npm run lint
```

提出 2-3 个问题：
1. 检测结果是否正确？有无遗漏的技术栈？
2. 模式：全新初始化（覆盖已有文件）还是增量（只添加缺失的）？——若 `.agents/rules/` 已存在则自动询问
3. 有无额外的项目约定需要加入？

### Step 3: 生成

按以下顺序生成文件。每个文件基于 `references/rule-templates/` 下的对应模板，替换 `{{placeholder}}` 为实际检测值。

**Phase 1: 核心规则（始终生成）**

| 顺序 | 文件 | 模板 | 关键检测变量 |
|------|------|------|-------------|
| 1 | `.agents/rules/universal.md` | [universal.md](references/rule-templates/universal.md) | — |
| 2 | `.agents/rules/system.md` | [system.md](references/rule-templates/system.md) | 项目名、技术栈表、命令、目录结构 |
| 3 | `.agents/rules/components.md` | [components.md](references/rule-templates/components.md) | framework（vue/vue3/react），选取对应变体 |
| 4 | `.agents/rules/styling.md` | [styling.md](references/rule-templates/styling.md) | styling（scss/tailwind/css-modules），选取对应变体 |
| 5 | `.agents/rules/format.md` | [format.md](references/rule-templates/format.md) | linting、formatting 工具 |
| 6 | `.agents/rules/comments.md` | [comments.md](references/rule-templates/comments.md) | — |

**Phase 2: 条件规则（按检测结果决定）**

| 条件 | 文件 | 模板 |
|------|------|------|
| tsconfig.json 存在 | `.agents/rules/typescript.md` | [typescript.md](references/rule-templates/typescript.md) |
| 检测到测试依赖 | `.agents/rules/testing.md` | [testing.md](references/rule-templates/testing.md) |
| 始终 | `.agents/rules/git.md` | [git.md](references/rule-templates/git.md) |
| 始终 | `.agents/rules/security.md` | [security.md](references/rule-templates/security.md) |

**Phase 3: 入口文件（最后生成）**

| 文件 | 模板 |
|------|------|
| `AGENTS.md` | [AGENTS.md](references/rule-templates/entry/AGENTS.md) |
| `CLAUDE.md` | [CLAUDE.md](references/rule-templates/entry/CLAUDE.md) |
| `.cursorrules` | [.cursorrules](references/rule-templates/entry/.cursorrules) |

AGENTS.md 必须在所有规则文件生成之后生成，因为它包含完整规则索引。

### Step 4: 验证

生成完成后执行：

- [ ] 所有应生成的文件存在（核心 6 + 条件 ≤4 + 入口 3）
- [ ] 每个 `.agents/rules/*.md` 有有效 YAML frontmatter（description、globs、alwaysApply 三个字段）
- [ ] 搜索 `{{` ——无残留占位符
- [ ] AGENTS.md 中的规则列表与实际文件一致
- [ ] CLAUDE.md 中的项目名、技术栈、命令与检测结果一致
- [ ] 所有文件为 UTF-8 编码

最后输出摘要报告：

```
## init-rules 完成

生成规则: 8 个 → .agents/rules/
入口文件: AGENTS.md, CLAUDE.md, .cursorrules

技术栈: Vue 3 + Vite + SCSS + Pinia + TypeScript + Vitest
模式: 全新初始化

下一步:
- 查看 AGENTS.md 了解规则组织
- 按需调整具体规则文件
- 运行 npm run dev 确认项目正常
```

## 模板使用说明

每个模板文件（`references/rule-templates/` 下）包含：
- 以 `---` 包裹的 YAML frontmatter（description、globs、alwaysApply）
- 中文叙述的规则内容
- `{{变量名}}` 形式的占位符
- 框架变体以 `## ========== Framework Name ==========` 区块分隔

使用模板时：
1. 读取模板文件
2. 根据检测到的技术栈**只保留匹配的变体区块**，删除不匹配的
3. 替换所有 `{{placeholder}}` 为检测到的实际值
4. 若某项未检测到，使用合理默认值或标注"未检测到"

## 边缘情况

| 场景 | 处理 |
|------|------|
| 无 package.json | 询问用户项目类型，仅生成 universal + format + comments + git + security |
| Monorepo | 检测 workspaces 字段；根目录生成通用规则；询问为哪个包生成框架规则 |
| 已有 `.agents/rules/` | 展示已有文件摘要；逐文件询问覆盖/跳过；AGENTS.md 自动更新索引 |
| 无 .git | 生成 git.md 但顶部注明"尚未初始化 Git 仓库" |
| 空项目（无 src/） | 生成规则，在 system.md 中注明"目录结构建立后请更新本文件" |
| 非前端项目 | 跳过 components 和 styling；生成其余规则 |

## 文件路径约定

- 所有输出文件相对于**项目根目录**（`package.json` 所在目录）
- `.agents/rules/` 放在项目根目录下
- 若未找到 `package.json`，以当前工作目录为项目根目录
