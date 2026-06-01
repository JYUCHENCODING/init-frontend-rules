---
description: 样式编写规范和设计系统
globs: "src/**/*.{scss,css,vue,jsx,tsx,less}"
alwaysApply: false
---

# 样式规则

## 样式文件组织

- 全局变量文件：`src/styles/variables.scss`（颜色、字号、间距等）
- 工具类文件：`src/styles/utilities.scss`（布局、文本、间距等通用类）
- 组件样式：在 `.vue` 文件 `<style scoped>` 中或同级 `.scss` 文件中

## ========== SCSS / Sass ==========

### 变量定义

```scss
// ===== 颜色 =====
$color-primary: #1890ff;
$color-success: #52c41a;
$color-warning: #faad14;
$color-danger: #f5222d;
$color-text: #333333;
$color-text-secondary: #666666;
$color-border: #d9d9d9;
$color-bg: #f5f5f5;

// ===== 字号 =====
$font-size-sm: 12px;
$font-size-base: 14px;
$font-size-lg: 16px;
$font-size-xl: 20px;

// ===== 间距 =====
$spacing-xs: 4px;
$spacing-sm: 8px;
$spacing-md: 16px;
$spacing-lg: 24px;
$spacing-xl: 32px;
```

### 嵌套规则

- **禁止用 `&` 拼接类名**：必须写完整的嵌套类名，方便搜索和重构
- **嵌套不超过 3 层**

```scss
// 正确：完整类名嵌套
.query-panel {
  padding: 16px;

  .query-panel__header { margin-bottom: 16px; }
  .query-panel__title { font-size: 16px; }
}

// 错误：& 拼接类名（禁止）
.query-panel {
  &__header { margin-bottom: 16px; }   // 禁止
  &__title { font-size: 16px; }         // 禁止
}
```

### Mixin 定义

```scss
// 文本省略
@mixin text-ellipsis {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

// Flex 居中
@mixin flex-center {
  display: flex;
  align-items: center;
  justify-content: center;
}
```

### 工具类样式

```scss
.fc-primary { color: $color-primary; }
.fc-success { color: $color-success; }
.fc-danger { color: $color-danger; }
.text-ellipsis { @include text-ellipsis; }
.flex { display: flex; }
.flex-center { @include flex-center; }
.flex-between { display: flex; align-items: center; justify-content: space-between; }
```

## ========== Tailwind CSS ==========

### 核心原则

- **优先使用 Tailwind 工具类**，不写自定义 CSS
- **在 `tailwind.config.ts` 中扩展主题**，不硬编码颜色值

### 主题扩展

```typescript
// tailwind.config.ts
export default {
  theme: {
    extend: {
      colors: {
        primary: '#1890ff',
        success: '#52c41a',
        warning: '#faad14',
        danger: '#f5222d'
      }
    }
  }
}
```

### 组件编写

```html
<div class="p-4 bg-white rounded-lg shadow-sm">
  <h3 class="mb-4 text-lg font-semibold text-gray-800">{{ title }}</h3>
  <div class="mb-4"><slot /></div>
  <div class="flex gap-2">
    <button class="px-4 py-2 bg-primary text-white rounded">查询</button>
    <button class="px-4 py-2 border border-gray-300 rounded">重置</button>
  </div>
</div>
```

### 避免事项

- 不使用 `!important`
- 不使用内联样式：`style="color: red"` → 用 Tailwind 类
- 不硬编码颜色值
- 类名过多时用 `@apply` 提取：

```scss
.btn-primary {
  @apply px-4 py-2 bg-primary text-white rounded hover:bg-primary/90;
}
```

## ========== CSS Modules ==========

```css
/* UserCard.module.css */
.card {
  padding: 16px;
  background: #fff;
  border-radius: 8px;
}
.cardTitle {
  font-size: 16px;
  font-weight: 600;
}
```

```tsx
import styles from './UserCard.module.css'

export function UserCard() {
  return (
    <div className={styles.card}>
      <h3 className={styles.cardTitle}>用户卡片</h3>
    </div>
  )
}
```

## 通用约束

- **移动优先**：先写移动端样式，再用媒体查询适配大屏
- **嵌套不超过 3 层**
- **颜色使用变量/Token**：不在组件中硬编码色值
- **避免使用 ID 选择器**：全部使用 class 选择器
- **scoped 优先**：Vue 组件样式加 `scoped`
