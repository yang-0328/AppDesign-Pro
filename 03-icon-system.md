# Icon Resolution System - 图标解析系统

> **模块定位**: 7层深度图标选型架构，从策略到代码的完整流程
> **依赖关系**: [02-style-presets.md](./02-style-presets.md)（风格匹配）
> **预估行数**: ~350 行

---

## 核心规则

当 App 设计需要图标时，Skill 必须：

1. **自动匹配**最佳图标库（基于当前 Style Preset）
2. **提供 2-3 个候选**方案（库名 + 图标名）
3. **输出多框架**直接可用代码
4. **优先单一**图标库保证一致性；仅在主库覆盖不足时混用第二库
5. **注释标注**每个图标：`{library}:{name} - {purpose} - {fallback_library}:{fallback_name}`

---

## Layer 1: Icon Library Index（图标库索引）

### A. Universal Neutral（通用中性，适合大多数场景）

| Library | License | 图标数 | 特征 | CDN/Package |
|---|---|---|---|---|
| **Lucide** | MIT | 1500+ | 线性、简洁、1.5px 描边、圆角端点 | lucide.dev |
| **Phosphor** | MIT | 9000+ | 6 种权重 (Thin→Duotone)、极其灵活 | phosphoricons.com |
| **Remix Icon** | Apache 2.0 | 3000+ | Fill/Line 双风格、中性克制 | remixicon.com |
| **Tabler Icons** | MIT | 5000+ | 1.5px 描边、覆盖全面、多格式 | tabler.io/icons |
| **Feather** | MIT | 280+ | 经典 24px、1.5px 描边、极简 | feathericons.com |
| **Heroicons** | MIT | 300+ | Outline/Solid/Mini 三套 | heroicons.com |

### B. Chinese / Life Service（中文/生活服务，美团/支付宝风格）

| Library | License | 图标数 | 特征 | CDN/Package |
|---|---|---|---|---|
| **IconPark** (字节) | Apache 2.0 | 2600+ | 多主题 (Linear/Fill/Duotone/FullColor)、中文App首选 | iconpark.oceanengine.com |
| **Arco Icons** (字节) | MIT | 700+ | 配套 Arco Design、B端+生活 | arco.design |
| **TDesign Icons** (腾讯) | MIT | 1000+ | 配套 TDesign、微信生态 | tdesign.tencent.com |
| **Ant Design Icons** | MIT | 400+ | 配套 Ant Design、企业级 | ant.design/components/icon |

### C. Material / Android Native

| Library | License | 图标数 | 特征 | CDN/Package |
|---|---|---|---|---|
| **Material Symbols** (Google) | Apache 2.0 | 3500+ | 可变权重/Fill/Grade/Optical size | fonts.google.com/icons |
| **Material Design Icons** | Apache 2.0 | 7000+ | 社区扩展版、Fill/Outline | pictogrammers.com |

### D. iOS / Apple Native

| Library | License | 图标数 | 特征 | CDN/Package |
|---|---|---|---|---|
| **SF Symbols** | Apple License | 5000+ | iOS 原生、仅 Apple 平台 | Apple Developer |
| **Phosphor (Regular/Thin)** | MIT | 9000+ | 最佳跨平台 SF Symbols 替代品 | phosphoricons.com |

### E. Playful / Youth（潮流/年轻）

| Library | License | 图标数 | 特征 | CDN/Package |
|---|---|---|---|---|
| **Phosphor (Bold/Duotone)** | MIT | 9000+ | 粗体、双色调、视觉冲击力强 | phosphoricons.com |
| **IconPark (Full Color)** | Apache 2.0 | 2600+ | 多彩色主题、节日图标 | iconpark.oceanengine.com |

### F. Luxury / Minimal（奢华/极简）

| Library | License | 图标数 | 特征 | CDN/Package |
|---|---|---|---|---|
| **Lucide** | MIT | 1500+ | 精细 1.2px、优雅克制 | lucide.dev |
| **Phosphor (Thin)** | MIT | 9000+ | 16px 光学尺寸、超细描边 | phosphoricons.com |
| **Radix Icons** (Modulz) | MIT | 300+ | 15px、极简几何、小元素 | radix-ui.com/icons |

### G. Enterprise / B-End（企业/B端）

| Library | License | 图标数 | 特征 | CDN/Package |
|---|---|---|---|---|
| **Ant Design Icons** | MIT | 400+ | 企业标准 | ant.design |
| **Carbon Icons** (IBM) | Apache 2.0 | 2300+ | 企业 SaaS、数据密集型 | carbondesignsystem.com |
| **Fluent UI Icons** (Microsoft) | MIT | 2800+ | Office/Teams 风格 | fluenticons.co |
| **Tabler Icons** | MIT | 5000+ | 通用管理后台 | tabler.io |

### H. Cyberpunk / Futuristic（赛博朋克/未来感）

| Library | License | 图标数 | 特征 | CDN/Package |
|---|---|---|---|---|
| **Phosphor (Bold)** | MIT | 9000+ | 粗重、高对比度 | phosphoricons.com |
| **MingCute** | Apache 2.0 | 3000+ | 锐利线条、科技感 | mingcute.com |
| **Unicons** | MIT | 1200+ | 线性、工业风 | iconscout.com/unicons |

---

## Layer 2: Style → Icon Mapping（风格→图标库映射）

```yaml
super-app:
  primary: "IconPark"
  fallback: "Phosphor (Fill)"
  weight: "Fill main + Line auxiliary"
  size: "24dp grid icon, 20dp list icon, 16dp inline icon"

luxury:
  primary: "Lucide"
  fallback: "Phosphor (Thin)"
  weight: "Thin / 1.2px stroke"
  size: "20dp standard, generous breathing room"

playful:
  primary: "Phosphor (Duotone/Bold)"
  fallback: "IconPark (Full Color)"
  weight: "Bold / Duotone two-tone"
  size: "28-32dp large, paired with rounded containers"

enterprise:
  primary: "Ant Design Icons"
  fallback: "Carbon Icons"
  weight: "Outlined (1.5px)"
  size: "16dp in forms, 20dp in lists, 24dp in navigation"

organic:
  primary: "Phosphor (Regular)"
  fallback: "Lucide"
  weight: "Regular / 1.5px"
  size: "24dp standard"

editorial:
  primary: "Lucide"
  fallback: "Radix Icons"
  weight: "1.2px ultra-thin"
  size: "16-20dp, small and delicate"

clinical:
  primary: "Phosphor (Regular)"
  fallback: "Tabler Icons"
  weight: "Regular"
  size: "24dp standard, accessibility first"

cyberpunk:
  primary: "MingCute"
  fallback: "Phosphor (Bold)"
  weight: "Bold / high-contrast stroke"
  size: "24dp standard, can be enlarged as decoration"
  color: "Neon cyan (#00FFD1) / neon pink (#FF006E) + glow effect"
  note: "Icons paired with CSS glow/shadow effects"

  fintech:
    primary: "Phosphor (Regular)"
    fallback: "Material Symbols"
    weight: "Regular"
    size: "20dp in lists, 24dp in nav, 16dp inline"
    color: "Navy (#1E3A5F) or gold (#D4A843)"
    note: "Numbers first, icons as visual aid. Tabular figures preferred."
  
  education:
    primary: "Phosphor (Regular)"
    fallback: "Lucide"
    weight: "Regular"
    size: "24dp standard, 32dp for achievement badges"
    color: "Orange (#FF6B35) or purple (#7C3AED)"
    note: "Achievement badges use filled variant. Progress icons animate on completion."
  
  streaming:
    primary: "Phosphor (Bold)"
    fallback: "Remix Icon (Fill)"
    weight: "Bold / Fill"
    size: "24dp navigation, 16dp metadata, 32dp play button"
    color: "White on dark, red (#E50914) for play, orange (#FF4500) for live"
    note: "Play button is the hero icon. Live badge uses pulse animation."
  
  fitness:
    primary: "Phosphor (Bold)"
    fallback: "Material Symbols"
    weight: "Bold"
    size: "24dp standard, 48dp center action button"
    color: "Orange (#FF6B00) for energy, green (#34C759) for complete"
    note: "Ring progress replaces many traditional icons. Icons paired with large numbers."
  
  travel:
    primary: "Phosphor (Regular)"
    fallback: "Lucide"
    weight: "Regular"
    size: "24dp standard, 20dp inline"
    color: "Sky blue (#2563EB) or warm gray (#78716C)"
    note: "Location pins and map markers are key. Photo-heavy layouts reduce icon usage."
  
  food:
    primary: "Phosphor (Regular)"
    fallback: "Lucide"
    weight: "Regular"
    size: "24dp standard, 20dp in recipe steps"
    color: "Tomato red (#E74C3C) or herb green (#27AE60)"
    note: "Timer icon for cooking steps. Ingredient checkmarks with checkbox interaction."
  
  productivity:
    primary: "Lucide"
    fallback: "Tabler"
    weight: "Regular / 1.5px"
    size: "16dp in sidebar, 20dp in editor, 24dp in navigation"
    color: "Single text color or accent"
    note: "Icons used sparingly. Hover to reveal actions. Drag handle indicates reorderable items."
  
  social:
    primary: "Phosphor (Regular)"
    fallback: "Lucide"
    weight: "Regular"
    size: "20dp post actions, 24dp tab bar, 32dp story rings"
    color: "Like red (#E0245E) for heart, blue (#1DA1F2) for brand"
    note: "Like animation transitions from regular → fill on tap. Comment/repost/share standard pattern."
```

---

## Layer 3: Scene → Candidate Table（场景→候选表）

### 常用功能图标

| 场景 | IconPark | Phosphor | Lucide | Remix Icon | Material | Tabler |
|------|----------|----------|--------|------------|----------|--------|
| 首页 | Home | House | Home | ri-home-4-line | home | home |
| 搜索 | Search | MagnifyingGlass | Search | ri-search-line | search | search |
| 消息 | Message | Chats | MessageSquare | ri-message-3-line | chat | message |
| 我的 | User | User | User | ri-user-3-line | person | user |
| 设置 | Setting | GearSix | Settings | ri-settings-3-line | settings | settings |
| 返回 | Back | ArrowLeft | ArrowLeft | ri-arrow-left-line | arrow_back | arrow-left |
| 分享 | Share | ShareNetwork | Share | ri-share-line | share | share |
| 收藏 | Like | Heart | Heart | ri-heart-3-line | favorite | heart |
| 购物车 | ShoppingCart | ShoppingCart | ShoppingCart | ri-shopping-cart-2-line | shopping_cart | shopping-cart |
| 定位 | Location | MapPin | MapPin | ri-map-pin-line | location_on | map-pin |

### 本地生活/电商专用

| 场景 | IconPark | Phosphor | Material |
|------|----------|----------|----------|
| 外卖 | Takeaway | ForkKnife | restaurant |
| 美食 | CookingPot | CookingPot | restaurant_menu |
| 酒店 | Bed | Bed | hotel |
| 电影 | Movie | FilmSlate | movie |
| 打车 | Taxi | Car | local_taxi |
| 超市 | ShoppingBag | ShoppingBag | store |
| 优惠券 | Coupon | Ticket | confirmation_number |
| 钱包 | Wallet | Wallet | account_balance_wallet |

### 底部 Tab 栏标准配置 (Super-App)

```yaml
Home:
  active: "IconPark: Home (Fill) / Phosphor: House (Fill)"
  inactive: "IconPark: Home (Line) / Phosphor: House (Line)"

Discover:
  active: "IconPark: Discovery (Fill) / Phosphor: Compass (Fill)"
  inactive: "IconPark: Discovery (Line) / Phosphor: Compass (Line)"

Orders:
  active: "IconPark: Order (Fill) / Phosphor: Note (Fill)"
  inactive: "IconPark: Order (Line) / Phosphor: Note (Line)"

Profile:
  active: "IconPark: User (Fill) / Phosphor: User (Fill)"
  inactive: "IconPark: User (Line) / Phosphor: User (Line)"
```

---

## Layer 4: Multi-Framework Code Output（多框架代码输出）

### React / React Native (Iconify)

```bash
npm install @iconify/react
```

```jsx
import { Icon } from '@iconify/react';

{/* icon: icon-park-solid:home - 首页入口 - phosphor:house-fill */}
<Icon icon="icon-park-solid:home" width={24} color="#FFD000" />

{/* icon: ph:heart-fill - 收藏激活状态 - lucide:heart */}
<Icon icon="ph:heart-fill" width={24} color="#FF4400" />

{/* icon: material-symbols:search - 搜索框 - tabler:search */}
<Icon icon="material-symbols:search" width={24} />
```

### Vue (Iconify)

```bash
npm install @iconify/vue
```

```vue
<script setup>
import { Icon } from '@iconify/vue';
</script>

<template>
  {/* icon: ph:house-fill - Tab栏首页 - lucide:home */}
  <Icon icon="ph:house-fill" :width="24" color="#FFD000" />
</template>
```

### SwiftUI

```swift
// Option 1: SF Symbols (Apple 原生，最佳体验)
Image(systemName: "house.fill")
    .font(.system(size: 24, weight: .medium))
    .foregroundColor(Color(hex: "FFD000"))

// Option 2: Iconify SVG → 转换为 PDF 放入 Asset Catalog
Image("icon_home")

// Option 3: 第三方库 (swift-iconify 或手动 SVG)
```

### Jetpack Compose

```kotlin
// Option 1: Material Icons
Icon(
    imageVector = Icons.Filled.Home,
    contentDescription = "首页",
    tint = Color(0xFFFFD000)
)

// Option 2: 从 Iconify 下载 SVG → vectorDrawable
Icon(
    painter = painterResource(R.drawable.ic_home),
    contentDescription = "首页",
    tint = Color(0xFFFFD000)
)
```

### Flutter

```yaml
# pubspec.yaml
dependencies:
  iconify_flutter: ^latest
```

```dart
import 'package:iconify_flutter/iconify_flutter.dart';
import 'package:iconify_flutter/icons/ph.dart';

{/* icon: phosphor:house - 首页图标 - lucide:home */}
Iconify(Ph.house, size: 24, color: Color(0xFFFFD000))
```

### HTML / Web

```html
<!-- Option 1: Iconify Web Component -->
<script src="https://code.iconify.design/iconify-icon/1.0.7/iconify-icon.min.js"></script>
<iconify-icon icon="icon-park-solid:home" width="24" style="color: #FFD000"></iconify-icon>

<!-- Option 2: UnoCSS (推荐用于 Vite 项目) -->
<i class="i-icon-park-solid:home text-2xl text-[#FFD000]" />
<i class="i-ph:house-fill text-2xl text-[#FFD000]" />

<!-- Option 3: Icon Font CDN -->
<link href="https://cdn.jsdelivr.net/npm/remixicon@4.0.0/fonts/remixicon.css" rel="stylesheet">
<i class="ri-home-4-fill" style="font-size:24px;color:#FFD000"></i>
```

---

## Layer 5: Container Styles（容器样式）

### 圆形背景（网格入口）

```css
.icon-circle {
  width: 48px;
  height: 48px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}

/* Super-App: 品牌色浅底 */
.icon-circle--superapp { background: #FFF8E0; }
.icon-circle--superapp iconify-icon { color: #FF7E00; font-size: 24px; }

/* Playful: 渐变底 */
.icon-circle--playful { background: linear-gradient(135deg, #FF6B6B, #FFD93D); }
.icon-circle--playful iconify-icon { color: #FFFFFF; font-size: 24px; }

/* Luxury: 透明或极淡底 */
.icon-circle--luxury { background: transparent; }
.icon-circle--luxury iconify-icon { color: #C5A44E; font-size: 20px; }
```

### 圆角方形（功能卡片）

```css
.icon-square {
  width: 40px;
  height: 40px;
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
}

/* Enterprise 风格 */
.icon-square--enterprise { background: #E6F4FF; }
.icon-square--enterprise iconify-icon { color: #1677FF; font-size: 20px; }
```

### 行内标签图标

```css
.tag-with-icon {
  display: inline-flex;
  align-items: center;
  gap: 4px;
  padding: 2px 8px;
  border-radius: 4px;
  font-size: 12px;
  height: 20px;
}
.tag-with-icon--discount { background: #FFF0E6; color: #FF4400; }
.tag-with-icon--brand { background: #FFF8E0; color: #FF7E00; }
.tag-with-icon--delivery { background: #E3F2FD; color: #1677FF; }
```

---

## Layer 6: Annotation Standard（注释标注标准）

每个图标在代码中必须包含注释：

```
格式: {library}:{name} - {用途} - {fallback_library}:{fallback_name}
```

### 正确示例 ✅

```jsx
{/* icon: icon-park:search - 搜索入口 - phosphor:magnifying-glass */}
<Icon icon="icon-park:search" />

{/* icon: phosphor:heart-fill - 收藏激活状态 - lucide:heart */}
<Icon icon="ph:heart-fill" color="#FF4400" />

{/* icon: remix-icon:restaurant-2-line - 美食分类入口 - icon-park:cooking-pot */}
<Icon icon="ri:restaurant-2-line" />
```

### 错误示例 ❌

```jsx
// ❌ 缺少 fallback 标注
<Icon icon="ph:home" />

// ❌ 缺少用途说明
<Icon icon="icon-park:search" /* lucide:search */>

// ❌ 格式不正确
{/* home icon */}
<Icon icon="ph:house-fill" />
```

---

## Layer 7: Quality Checklist（质量检查清单）

最终确认前，验证：

- [ ] 整个设计使用**单一图标库**（最多混用 2 个）
- [ ] 图标风格与 Style Preset 匹配（描边粗细/圆润度/复杂度）
- [ ] 图标大小遵循统一规范（最多 3 种不同尺寸）
- [ ] 所有图标都有无障碍标签 (`aria-label` / `contentDescription`)
- [ ] Tab 栏图标有 Fill 和 Line 两种状态
- [ ] 每个图标都标注了 fallback 选项
- [ ] 图标容器的背景/圆角/间距符合设计系统
- [ ] 无反模式：
  - ❌ 混用 3+ 个不同风格的图标库
  - ❌ 同一页面描边粗细不一致
  - ❌ 图标大小随机（忽大忽小）
  - ❌ 硬编码颜色而非使用 Token

---

## 🔗 相关模块

- [样式预设](./02-style-presets.md) - 各风格的图标选型偏好
- [核心基础](./00-core-foundations.md) - 颜色 Token 系统
- [工具集成](./06-tool-integration.md) - Figma 图标库同步工作流
