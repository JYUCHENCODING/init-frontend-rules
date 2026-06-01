---
description: 注释规范和文档编写要求
globs: "src/**/*.{js,ts,vue,jsx,tsx}"
alwaysApply: true
---

# 注释规则

## 注释原则

- **说 WHY 不说 WHAT**：解释为什么这样做，而不是描述代码做了什么
- **改动代码时同步更新注释**：不留下过时注释
- **旧代码不追溯**：不强行给已有代码添加注释（除非当前任务涉及该代码）
- **不为显而易见的代码写注释**

```javascript
// ❌ 废话注释
// 从 API 获取用户信息
const user = await getUserInfo()

// ✅ 有用的注释
// 此处必须先获取用户信息再查订单，因为订单接口依赖用户的 region 参数
const user = await getUserInfo()
```

## JSDoc 模板

### 函数 / 方法

```javascript
/**
 * 根据用户 ID 和日期范围获取订单列表
 * @param {number} userId - 用户 ID
 * @param {string} startDate - 开始日期 YYYY-MM-DD
 * @param {string} endDate - 结束日期 YYYY-MM-DD
 * @returns {Promise<Order[]>} 订单列表
 */
export async function getOrders(userId, startDate, endDate) {}
```

### API 模块

```javascript
/**
 * 获取用户信息
 * @description 通过用户 ID 获取用户基本信息
 * @param {number} userId - 用户 ID
 * @returns {Promise<UserInfo>} 用户信息对象
 */
export function getUserInfo(userId) {}
```

### 组件 Props

```javascript
// Vue Options API
props: {
  /** 用户 ID */
  userId: { type: Number, required: true },
  /** 是否显示操作按钮，默认 true */
  showActions: { type: Boolean, default: true }
}
```

```typescript
// Vue 3 Composition API / React
interface Props {
  /** 用户 ID */
  userId: number
  /** 是否显示操作按钮 */
  showActions?: boolean
}
```

### 常量

```javascript
/**
 * 订单状态枚举
 * @readonly
 */
export const ORDER_STATUS = {
  PENDING: 0,    // 待支付
  PAID: 1,       // 已支付
  SHIPPED: 2,    // 已发货
  COMPLETED: 3   // 已完成
}
```

## 样式注释

SCSS 中使用分隔注释来组织样式区块：

```scss
// ==================== 布局 ====================
.page-container { display: flex; }

// ==================== 表单区域 ====================
.form-section { margin-bottom: 16px; }
```

## 禁止事项

- 不要写"修改人"、"修改时间"等版本跟踪信息（Git 已经记录）
- 不要保留注释掉的大段代码 — 直接删除
- 不要用注释解释明显的代码行为
- 注释语言保持项目现有风格，新建项目默认使用中文
