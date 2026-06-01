---
description: 组件开发规范和命名约定
globs: "src/**/*.{vue,jsx,tsx}"
alwaysApply: false
---

# 组件规则

## 组件拆分原则

1. **禁止所有逻辑写在主页面**：页面组件只负责组装子组件，不包含复杂业务逻辑
2. **单文件不超过 500 行**：超过则必须拆分为子组件
3. **同一代码出现两次以上**：抽取为公共组件或工具函数
4. **子组件放在 `components/` 目录**：每个组件一个文件夹

```
components/
├── userCard/
│   ├── index.vue        # 组件入口
│   └── avatar.vue       # 子组件
├── dataTable/
│   └── index.vue
└── ...
```

## 命名约定

- **目录名**：小驼峰（camelCase），有业务语义
  - 禁止：`list`、`item`、`data`、`form`、`modal`
  - 推荐：`queryPanel`、`resultTable`、`detailDrawer`、`userCard`
- **组件名**：PascalCase
- **文件名**：`index.vue` 或 `index.tsx`

## ========== Vue 3 (Composition API + script setup) ==========

```vue
<template>
  <div class="query-panel">
    <div class="query-panel__header">
      <h3 class="query-panel__title">{{ title }}</h3>
    </div>
    <div class="query-panel__body">
      <slot />
    </div>
    <div class="query-panel__footer">
      <button @click="handleSearch">查询</button>
      <button @click="handleReset">重置</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const props = defineProps({
  title: { type: String, default: '查询条件' },
  showReset: { type: Boolean, default: true }
})

const emit = defineEmits(['search', 'reset'])
const loading = ref(false)

function handleSearch() {
  loading.value = true
  emit('search')
}

function handleReset() {
  emit('reset')
}
</script>

<style lang="scss" scoped>
.query-panel {
  padding: 16px;
  background: #fff;
  border-radius: 8px;
}
</style>
```

### 组合式函数（Composables）

```typescript
// composables/usePagination.ts
import { ref, computed } from 'vue'

export function usePagination(defaultPageSize = 10) {
  const currentPage = ref(1)
  const pageSize = ref(defaultPageSize)
  const total = ref(0)
  const totalPages = computed(() => Math.ceil(total.value / pageSize.value))

  return { currentPage, pageSize, total, totalPages }
}
```

## ========== Vue 2 (Options API) ==========

```vue
<template>
  <div class="query-panel">
    <div class="query-panel__header">
      <h3 class="query-panel__title">{{ title }}</h3>
    </div>
    <div class="query-panel__body">
      <slot />
    </div>
    <div class="query-panel__footer">
      <button @click="handleSearch">查询</button>
      <button v-if="showReset" @click="handleReset">重置</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'QueryPanel',
  props: {
    title: { type: String, default: '查询条件' },
    showReset: { type: Boolean, default: true }
  },
  data() {
    return { loading: false }
  },
  methods: {
    handleSearch() {
      this.loading = true
      this.$emit('search')
    },
    handleReset() {
      this.$emit('reset')
    }
  }
}
</script>

<style lang="scss" scoped>
.query-panel {
  padding: 16px;
  background: #fff;
  border-radius: 8px;
}
</style>
```

### 注意事项

- `props` -> `data` -> `computed` -> `watch` -> `methods` -> 生命周期钩子
- 若项目有 `@vue/composition-api`，可在新组件中使用 Composition API

## ========== React ==========

```tsx
import { useState, type ReactNode } from 'react'

interface QueryPanelProps {
  title?: string
  showReset?: boolean
  children: ReactNode
  onSearch?: () => void
  onReset?: () => void
}

export function QueryPanel({
  title = '查询条件',
  showReset = true,
  children,
  onSearch,
  onReset
}: QueryPanelProps) {
  const [loading, setLoading] = useState(false)

  return (
    <div className="query-panel">
      <div className="query-panel__header">
        <h3 className="query-panel__title">{title}</h3>
      </div>
      <div className="query-panel__body">{children}</div>
      <div className="query-panel__footer">
        <button onClick={onSearch} disabled={loading}>查询</button>
        {showReset && <button onClick={onReset}>重置</button>}
      </div>
    </div>
  )
}
```

### 自定义 Hook

```typescript
// hooks/usePagination.ts
import { useState, useMemo } from 'react'

export function usePagination(defaultPageSize = 10) {
  const [currentPage, setCurrentPage] = useState(1)
  const [pageSize] = useState(defaultPageSize)
  const [total, setTotal] = useState(0)
  const totalPages = useMemo(() => Math.ceil(total / pageSize), [total, pageSize])

  return { currentPage, pageSize, total, totalPages, setCurrentPage, setTotal }
}
```

### React 注意事项

- 函数组件 + Hooks，不使用 class 组件
- Props 接口命名：`{ComponentName}Props`
- 命名导出用于共享组件，default export 用于页面组件
- 使用 `children` 而非配置式 props
