# Interaction Design - 交互设计规范

> **模块定位**: 定义手势、动画、加载状态等交互行为
> **依赖关系**: [00-core-foundations.md](./00-core-foundations.md)（间距、触摸目标）
> **预估行数**: ~120 行

---

## Gestures & Haptics（手势与触觉反馈）

### 标准手势库

| 手势 | 触发区域 | 行为 | 反馈 |
|------|----------|------|------|
| **Swipe Left/Right** | 列表项 | 删除/归档/收藏 | Haptic + 动画 |
| **Pull-to-Refresh** | 滚动容器顶部 | 刷新数据 | 骨架屏 → 内容 |
| **Long Press** | 可交互元素 | 上下文菜单/拖拽排序 | Haptic + 菜单弹出 |
| **Pinch-to-Zoom** | 图片/地图 | 缩放视图 | 平滑缩放动画 |
| **Edge Swipe** | 屏幕边缘 (iOS) | 返回上一页 | 系统默认（禁止劫持） |

### Haptic Feedback 规范

必须提供触觉反馈的场景：

| 场景 | 反馈类型 | 强度 |
|------|----------|------|
| Toggle 开关切换 | `.impact(.light)` | 轻 |
| 操作成功完成 | `.notification(.success)` | 中 |
| 破坏性操作确认 | `.warning` | 重 |
| Slider 到达刻度 | `.selection` | 轻微 |

**⚠️ 禁止事项**:
- ❌ 劫持系统手势（如 iOS 边缘滑动返回）
- ❌ 过度使用触觉反馈（造成干扰）
- ❌ 在非交互元素上触发手势

---

## Transitions & Motion（过渡与动效）

### 动画时长规范

| 类型 | 时长范围 | 使用场景 |
|------|----------|----------|
| **Micro** | 100–200ms | 按钮按下、图标切换 |
| **Standard** | 200–350ms | 页面转场、弹窗出现/消失 |
| **Complex** | 350–500ms | 复杂布局变化、共享元素转场 |

### 缓动曲线选择

```css
/* 交互元素（按钮、卡片） */
--ease-spring: cubic-bezier(0.34, 1.56, 0.64, 1);

/* 出现动画 */
--ease-out: cubic-bezier(0.16, 1, 0.3, 1);

/* 消失动画 */
--ease-in: cubic-bezier(0.7, 0, 0.84, 0);
```

### 微交互动效清单

| 元素 | 触发状态 | 动画效果 | 参数 |
|------|----------|----------|------|
| 按钮 | Press | 缩放至 0.96 | 100ms spring |
| 卡片 | Press | 轻微上浮 (y: -2px) | 150ms ease-out |
| 内容 | Load | 淡入 (opacity: 0→1) | 200ms ease-out |
| 列表项 | Insert | 向下滑入 + 淡入 | 250ms ease-out |
| 弹窗 | Open | 缩放 (0.95→1) + 淡入 | 300ms spring |

### 共享元素转场示例

**列表 → 详情页**:

```
列表项点击
    ↓
[图片] 共享元素放大至详情 Hero 区域
[标题] 位置平滑过渡到详情标题位置
[背景色] 渐变过渡到详情背景
    ↓
详情页完全展示（300ms）
```

**📖 详细实现代码**: 请查看对应平台的 [05-platform-decision-matrix.md](./05-platform-decision-matrix.md)

---

## Loading & Empty States（加载与空状态）

### 加载状态优先级

```
Skeleton Screen（骨架屏）
    ↓ （推荐，适用于内容型页面）
Shimmer Placeholder（微光占位符）
    ↓ （适用于卡片列表）
Spinner / Progress Indicator
    ↓ （最后选择，仅用于简单操作）
```

### 空状态设计公式

每个空状态必须包含：

```
┌─────────────────────────────────┐
│                                 │
│     🎨 [插图/图标]              │  ← 视觉元素（情感化）
│                                 │
│   "这里还没有内容"              │  ← 清晰的消息文案
│   "发布你的第一条动态吧"        │  ← 引导性副文案
│                                 │
│   [立即发布]                    │  ← CTA 按钮（明确行动）
│                                 │
└─────────────────────────────────┘
```

**空状态类型映射表**:

| 场景 | 插图风格 | 文案模板 | CTA |
|------|----------|----------|-----|
| 无数据 | 空盒子 | "暂无{内容}" | "去添加" |
| 搜索无结果 | 放大镜+问号 | "未找到相关结果" | "清除筛选" |
| 网络错误 | 断网连接 | "网络连接失败" | "重试" |
| 权限不足 | 锁图标 | "您没有权限查看" | "申请权限" |

### Optimistic UI（乐观更新）

适用场景：点赞、收藏、简单表单提交

```
用户操作（如点赞）
    ↓
立即更新 UI（显示已点赞状态）  ← 100ms 内响应
    ↓
后台异步发送请求
    ↓
成功 → 保持状态
失败 → 回滚 + Toast 提示
```

### Offline Strategy（离线策略）

```
网络断开时：
├── 显示缓存内容（如有）
├── 顶部横幅提示："当前为离线模式"
└── 禁用需要网络的操作（置灰 + 提示）

网络恢复时：
├── 自动同步待处理操作
├── 刷新过期缓存
└── 移除离线横幅
```

---

## Accessibility in Interactions（交互无障碍）

### 动画降级

```css
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

### 手势替代方案

每个手势操作必须提供替代方式：

| 手势 | 替代方案 |
|------|----------|
| Swipe to delete | 编辑模式 + 多选删除 |
| Long press context menu | 右上角 "更多" 按钮 |
| Pinch to zoom | 双击 / 缩放控件 |

---

## ✅ 质量检查清单

在实现交互前，确认：

- [ ] 所有动画时长符合 Micro/Standard/Complex 分类
- [ ] 使用正确的缓动曲线（spring/ease-out/ease-in）
- [ ] 尊重 `prefers-reduced-motion` 系统设置
- [ ] 每个手势都有键盘/屏幕阅读器替代方案
- [ ] 空状态包含：插图 + 文案 + CTA
- [ ] 离线状态有优雅降级方案
- [ ] Haptic feedback 仅用于关键操作点
- [ ] 不劫持系统手势

---

## 🔗 相关模块

- [核心基础](./00-core-foundations.md) - 触摸目标、间距系统
- [样式预设](./02-style-presets.md) - 各风格的动效特征差异
- [平台对比](./05-platform-decision-matrix.md) - iOS/Android 原生动画 API
