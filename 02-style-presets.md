# Style Presets - 样式预设库

> **模块定位**: 16种成熟的设计风格 + 自动匹配引擎，即插即用
> **依赖关系**: [00-core-foundations.md](./00-core-foundations.md)（颜色、间距、字体基础）
> **预估行数**: ~400 行

---

## 使用指南

当用户指定风格时，从下方选择对应预设。每个预设包含：
- 完整的颜色 Token 系统
- 间距密度规范
- 圆角、字体、布局特征
- 核心组件样式
- 图标选型建议（详见 [03-icon-system.md](./03-icon-system.md)）

---

## Preset 1: Super-App (美团 / 支付宝 / 抖音生活)

**Use for:** 本地生活、电商、O2O、多功能聚合平台

### 设计语言
- **信息密度**: 极高，每个屏幕承载大量内容
- **色彩**: 高饱和度品牌色大面积使用 + 白/浅灰底色
- **圆角**: 小圆角 4–8dp，卡片 8–12dp
- **间距**: 紧凑型，4dp 基础单位

### 颜色系统

```css
:root {
  /* Brand */
  --brand-primary: #FFD000;
  --brand-primary-light: #FFF8E0;
  --brand-secondary: #FF7E00;
  --brand-deep: #FF5722;

  /* Background */
  --bg-page: #F5F5F5;
  --bg-card: #FFFFFF;
  --bg-search: #F0F0F0;

  /* Text */
  --text-title: #1A1A1A;
  --text-body: #333333;
  --text-secondary: #666666;
  --text-caption: #999999;
  --text-price: #FF4400;

  /* Tags */
  --tag-discount-bg: #FFF0E6;
  --tag-discount-text: #FF4400;
  --tag-brand-bg: #FFF8E0;
  --tag-brand-text: #FF7E00;
  --tag-new-bg: #E8F5E9;
  --tag-new-text: #4CAF50;
  --tag-delivery-bg: #E3F2FD;
  --tag-delivery-text: #1677FF;
}
```

### 字体规范
- **标题**: Bold 16–18sp（系统默认黑体）
- **正文**: Regular 14sp
- **说明文字**: Regular 12sp
- **价格/数字**: DIN / Medium 18–24sp

### 间距规范
| 元素 | 间距值 |
|------|--------|
| 卡片内边距 | 12dp |
| 卡片间距 | 8dp |
| 页面边距 | 12–16dp |
| 列表项内边距 | 12dp 16dp |

### 布局模式
```
┌──────────────────────────────────────┐
│ [定位栏] [搜索框(胶囊形)]           │ ← 顶部区域
├──────────────────────────────────────┤
│ [图标网格 5列×2行] [分页指示器]     │ ← 功能入口
├──────────────────────────────────────┤
│ [轮播Banner 16:9 圆角卡片]          │ ← 营销位
├──────────────────────────────────────┤
│ 🍜 附近美食              查看更多 > │ ← 内容区块
│ ┌─────┐ ┌─────┐ ┌─────┐           │
│ │商家│ │商家│ │商家│  横向滚动     │
│ └─────┘ └─────┘ └─────┘           │
├──────────────────────────────────────┤
│ [首页] [视频] [赚钱] [消息] [我的]   │ ← 底部Tab栏
└──────────────────────────────────────┘
```

### 核心组件
- **网格入口**: 圆形图标 + 文字标签，支持横向分页
- **促销卡片**: 大图 + 标题 + 标签（满减/折扣）+ 价格
- **商家列表项**: 左侧Logo + 右侧（名称+评分+月售+人均+距离+配送费+标签）
- **标签系统**: 彩色圆角小标签（满30减5, 品牌, 新店）
- **搜索框**: 胶囊形状，带轮播占位文案

---

## Preset 2: Luxury (高端奢侈品)

**Use for:** 高端电商、珠宝、精品酒店、艺术画廊

### 颜色系统
```css
--bg-primary: #0A0A0A;      /* 深黑背景 */
--bg-secondary: #1A1A1A;    /* 次级背景 */
--text-primary: #F0ECE4;    /* 暖白色文字 */
--accent-gold: #C5A44E;     /* 金属金强调色 */
```

### 设计特征
- **圆角**: 极小 2–4dp 或直角
- **间距**: 超大留白（页边距 32–48dp，卡片间距 24–32dp）
- **字体**: 衬线体或超细体
  - 标题: Cormorant Garamond / Noto Serif SC Light
  - 正文: Source Serif 4 / 思源宋体
- **布局**: 大图 + 极简文字，非对称构图

### 视觉氛围
- 黑金配色传递奢华感
- 大量留白营造高端感
- 图片占比 > 60%
- 动效：优雅的淡入淡出，无弹跳效果

---

## Preset 3: Playful (潮玩年轻)

**Use for:** 社交App、游戏社区、潮牌电商、Z世代产品

### 颜色系统
```css
--bg-light: #FFFFFF;
--bg-dark: #0D0D0D;
--accent-gradient: linear-gradient(135deg, #FF6B6B, #FFD93D);
--accent-alt-gradient: linear-gradient(135deg, #6C5CE7, #00D2FF);
--spot-color: #00FF88;  /* 荧光绿点缀 */
```

### 设计特征
- **圆角**: 大圆角 16–24dp，胶囊按钮
- **间距**: 中等密度，12–16dp 基础
- **字体**: 粗几何无衬线
  - 标题: Syne Bold / 站酷快乐体
  - 正文: DM Sans / 思源黑体
- **布局**: 错落卡片、斜切角、重叠元素
- **动效**: 弹簧动画、Emoji 贴纸、粒子特效

---

## Preset 4: Enterprise (B端效率)

**Use for:** 企业SaaS、CRM、项目管理、后台管理

### 颜色系统
```css
--sidebar-bg: #001529;       /* 深蓝黑侧边栏 */
--content-bg: #F0F2F5;       /* 浅灰内容区 */
--accent-blue: #1677FF;      /* Ant Design 蓝 */
--text-primary: #1A1A1A;
--text-secondary: #666666;
```

### 设计特征
- **圆角**: 中等 6–8dp
- **间距**: 宽松，16dp 基础
- **字体**: 清晰易读 — 思源黑体 / SF Pro Text
- **布局**: 侧导航 + 面包屑 + 内容区（表格/表单/图表）

### 组件特色
- 数据表格：斑马纹行、固定列表头
- 表单：标签左对齐、错误提示红色下划线
- 图表：简洁配色（蓝/绿/橙/红）
- 导航：可折叠侧边栏 + 多级菜单

---

## Preset 5: Organic (自然清新)

**Use for:** 健康App、植物养护、瑜伽冥想、环保产品

### 颜色系统
```css
--bg-warm-white: #FAFAF5;   /* 暖白背景 */
--primary-green: #4A7C59;    /* 森林绿 */
--secondary-brown: #D4A373;  /* 暖棕色 */
--light-sage: #E9EDC9;       /* 浅鼠尾草绿 */
```

### 设计特征
- **圆角**: 大圆角 12–20dp
- **间距**: 舒适型，16–20dp 基础
- **字体**: 柔和圆润 — Lora / 苹方-简 细体
- **布局**: 流动式卡片布局、有机曲线分隔符

---

## Preset 6: Editorial (杂志编辑)

**Use for:** 阅读App、新闻资讯、生活方式内容

### 颜色系统
```css
--bg-kraft: #FDF6EC;        /* 牛皮纸白 */
--text-near-black: #2C2C2C;  /* 近黑色 */
--accent-red: #C0392B;       /* 编辑红 */
```

### 设计特征
- **圆角**: 极小或无圆角
- **间距**: 宽松，大量留白
- **字体**: 强衬线
  - 标题: Playfair Display / 思源宋体
  - 正文: Source Serif 4 / 方正书宋
- **布局**: 多栏排版、Hero图 + 长段落、引用块

---

## Preset 7: Minimal Clinical (极简医疗)

**Use for:** 健康医疗、体检报告、预约挂号

### 颜色系统
```css
--bg-pure-white: #FFFFFF;
--medical-blue: #2196F3;     /* 医疗蓝 */
--health-green: #4CAF50;     /* 健康绿 */
--warning-orange: #FF9800;    /* 警告橙 */
--error-red: #F44336;        /* 错误红 */
```

### 设计特征
- **圆角**: 8–12dp
- **间距**: 宽松，安全区域充足
- **字体**: 清晰无衬线，大字号
- **布局**: 表单驱动、步骤流程清晰划分

---

## Preset 8: Neon Cyberpunk (赛博朋克)

**Use for:** 夜店App、音乐平台、潮流社区

### 颜色系统
```css
--bg-dark-purple: #0D0D1A;  /* 深紫黑 */
--neon-cyan: #00FFD1;        /* 霓虹青 */
--neon-pink: #FF006E;        /* 霓虹粉 */
--neon-gold: #FFD700;        /* 霓虹金 */
```

### 设计特征
- **圆角**: 0–4dp，锐利边缘
- **字体**: 等宽/未来感
  - 标题: Orbitron / DM Mono
  - 正文: JetBrains Mono / DM Mono
- **布局**: 密集网格、故障效果、扫描线纹理
- **特效**: CSS glow/shadow、霓虹发光效果

---

## Preset 9: Fintech (金融科技)

**Use for:** 银行App、理财、支付、股票交易、保险

### 颜色系统
```css
--bg-white: #FFFFFF;
--bg-light: #F8FAFC;
--primary-navy: #1E3A5F;
--accent-gold: #D4A843;
--positive-green: #16A34A;
--negative-red: #DC2626;
--text-dark: #0F172A;
--text-mid: #475569;
```

### 设计特征
- **圆角**: 8–12dp，保守稳重
- **间距**: 宽松 16–20dp
- **字体**: 现代无衬线 + 等宽数字
  - 标题: Inter / SF Pro Display
  - 数字: Inter Display / SF Mono (Tabular Figures)
- **布局**: 仪表盘式、数据可视化优先
- **安全**: FaceID/指纹图标突出
- **图标**: Phosphor Regular / Material Symbols

---

## Preset 10: Education (在线教育)

**Use for:** 学习平台、K12、知识付费、技能培训

### 颜色系统
```css
--bg-warm: #FFFDF7;
--primary-orange: #FF6B35;
--secondary-blue: #4A90D9;
--success-green: #52C41A;
--accent-purple: #7C3AED;
```

### 设计特征
- **圆角**: 大圆角 12–20dp，友好温和
- **间距**: 舒适 16–24dp
- **字体**: 圆体/无衬线，易读
  - 标题: Nunito / 鸿蒙圆体
  - 正文: Inter / 思源黑体
- **布局**: 课程卡片流、进度追踪、打卡日历
- **动效**: 完成动画、徽章收集
- **图标**: Phosphor Regular / Lucide

---

## Preset 11: Media Streaming (视频流媒体)

**Use for:** 短视频、长视频、直播、播客

### 颜色系统
```css
--bg-dark: #0A0A0A;
--bg-elevated: #1A1A1A;
--accent-play: #E50914;
--accent-live: #FF4500;
--text-bright: #FFFFFF;
--text-dim: #A0A0A0;
```

### 设计特征
- **圆角**: 中等 8–12dp
- **间距**: 紧凑 8–12dp，最大化内容
- **字体**: 清爽无衬线
  - 标题: Netflix Sans / SF Pro
  - 正文: Roboto / 思源黑体
- **布局**: 沉浸式全屏、横向滚动行
- **图标**: Phosphor Bold / Remix Icon Fill

---

## Preset 12: Fitness (运动健身)

**Use for:** 跑步、健身、运动追踪、健康管理

### 颜色系统
```css
--bg-dark: #0F0F0F;
--bg-card: #1C1C1E;
--energy-orange: #FF6B00;
--calories-red: #FF3B30;
--cardio-green: #34C759;
--text-bright: #FFFFFF;
```

### 设计特征
- **圆角**: 大圆角 16–24dp，动感
- **间距**: 宽松 16–24dp
- **字体**: 粗体无衬线，数字突出
  - 标题: Helvetica Now / SF Pro
  - 数据: DIN Alternate / SF Mono
- **布局**: 全屏深色 + 大数字 + 环形进度
- **动效**: 环形进度动画、数据刷新
- **图标**: Phosphor Bold / Material Symbols

---

## Preset 13: Travel (旅行探索)

**Use for:** 酒店预订、机票、攻略、游记

### 颜色系统
```css
--bg-warm: #FFFBF5;
--sky-blue: #2563EB;
--sunset-orange: #FF7E47;
--ocean-teal: #0D9488;
--warm-gray: #78716C;
```

### 设计特征
- **圆角**: 大圆角 12–20dp
- **间距**: 舒适 16–24dp，留白多
- **字体**: 优雅无衬线
  - 标题: Montserrat / 思源黑体
  - 正文: Inter
- **布局**: 大图卡片 + 情绪化文案
- **图标**: Phosphor Regular / Lucide

---

## Preset 14: Food & Recipe (美食菜谱)

**Use for:** 菜谱App、烘焙、美食社区、餐厅点评

### 颜色系统
```css
--bg-cream: #FFFBF0;
--tomato-red: #E74C3C;
--herb-green: #27AE60;
--butter-yellow: #F39C12;
--chocolate-brown: #6B4226;
```

### 设计特征
- **圆角**: 大圆角 16–24dp，圆润暖感
- **间距**: 舒适 16–20dp
- **字体**: 衬线/手写风格
  - 标题: Playfair Display / 思源宋体
  - 正文: Nunito / 苹方
- **布局**: 大图卡片、步骤式布局
- **图标**: Phosphor Regular / Lucide

---

## Preset 15: Productivity (效率工具)

**Use for:** 笔记、待办、日历、项目管理、文档协作

### 颜色系统
```css
--bg-page: #F7F6F3;
--bg-card: #FFFFFF;
--text-primary: #37352F;
--accent-blue: #2383E2;
--accent-pink: #E16259;
--accent-yellow: #DFAB01;
--accent-green: #0F7B6C;
```

### 设计特征
- **圆角**: 小圆角 4–8dp，高效简洁
- **间距**: 紧凑 8–16dp
- **字体**: 清晰无衬线
  - 标题: Inter / SF Pro
  - 正文: Inter / 思源黑体
- **布局**: 侧边栏+内容区、拖拽排序
- **图标**: Lucide / Tabler

---

## Preset 16: Social Media (社交媒体)

**Use for:** 朋友圈、动态、社区、论坛

### 颜色系统
```css
--bg-light: #FAFAFA;
--bg-card: #FFFFFF;
--primary-blue: #1DA1F2;
--like-red: #E0245E;
--comment-green: #17BF63;
--text-dark: #14171A;
```

### 设计特征
- **圆角**: 中等 8–16dp
- **间距**: 中等 12–16dp
- **字体**: 友好无衬线
  - 标题: Inter / SF Pro
  - 正文: Inter / 思源黑体
- **布局**: 时间线流、底部Tab
- **图标**: Phosphor Regular / Lucide

---

## 🎯 自动匹配系统 (Auto-Matching Engine)

**使用方式**: 描述你的 App 类型，系统自动匹配最佳风格和图标库。

### 完整匹配表

| App 类型 | 关键词 | 风格 | 主图标库 | 备选 | 权重 |
|----------|--------|------|----------|------|------|
| 外卖配送 | 外卖、美团、饿了么 | Super-App | IconPark | Phosphor Fill | Fill |
| 电商购物 | 淘宝、京东、拼多多 | Super-App | IconPark | Remix Icon | Fill |
| O2O生活 | 本地生活、58同城 | Super-App | IconPark | Material Symbols | Fill |
| 奢侈品 | 奢品、珠宝、腕表 | Luxury | Lucide | Phosphor Thin | Thin |
| 高端酒店 | 五星、度假村 | Luxury | Lucide | Radix Icons | Thin |
| 艺术画廊 | 画廊、拍卖 | Luxury | Lucide | Phosphor Thin | Thin |
| 短视频 | 短视频、抖音、vlog | Media Streaming | Phosphor Bold | Remix Icon Fill | Bold |
| 长视频 | 追剧、Netflix | Media Streaming | Phosphor | Lucide | Regular |
| 直播 | 直播、带货 | Cyberpunk | MingCute | Phosphor Bold | Bold |
| 社交/Z世代 | 年轻人、潮牌社交 | Playful | Phosphor Duotone | IconPark FullColor | Duotone |
| 游戏社区 | 玩家、电竞 | Playful | Phosphor Bold | MingCute | Bold |
| 企业后台 | SaaS、CRM、ERP | Enterprise | Ant Design Icons | Carbon Icons | Outlined |
| 数据分析 | BI、报表、大屏 | Enterprise | Carbon Icons | Tabler | Outlined |
| 文档协作 | 协作、wiki、知识库 | Productivity | Lucide | Tabler | Regular |
| 项目管理 | 项目、看板 | Productivity | Tabler | Lucide | Regular |
| 笔记待办 | 笔记、备忘、GTD | Productivity | Lucide | Feather | Regular |
| 银行金融 | 银行、理财、存钱 | Fintech | Phosphor | Material Symbols | Regular |
| 股票交易 | 股票、基金、投资 | Fintech | Phosphor | Tabler | Regular |
| 支付钱包 | 支付、转账、收款 | Fintech | Material Symbols | Phosphor | Regular |
| 医疗挂号 | 医院、挂号、问诊 | Minimal Clinical | Phosphor Regular | Tabler | Regular |
| 健康报告 | 体检、化验、病历 | Minimal Clinical | Tabler | Phosphor Regular | Regular |
| 运动跑步 | 跑步、骑行、健身 | Fitness | Phosphor Bold | Material Symbols | Bold |
| 瑜伽冥想 | 瑜伽、冥想、正念 | Organic | Phosphor Regular | Lucide | Regular |
| 自然植物 | 植物、养护、园艺 | Organic | Lucide | Phosphor Regular | Regular |
| 在线教育 | 学习、课程、K12 | Education | Phosphor Regular | Lucide | Regular |
| 知识付费 | 得到、专栏 | Education | Lucide | Tabler | Regular |
| 阅读资讯 | 新闻、杂志、阅读 | Editorial | Lucide | Radix Icons | Thin |
| 旅游酒店 | 酒店、机票、游记 | Travel | Phosphor Regular | Lucide | Regular |
| 攻略游记 | 穷游、攻略 | Travel | Lucide | Phosphor Regular | Regular |
| 菜谱烘焙 | 菜谱、下厨房 | Food & Recipe | Phosphor Regular | Lucide | Regular |
| 美食社区 | 美食、探店、点评 | Food & Recipe | Lucide | Phosphor | Regular |
| 音乐音频 | 音乐、播客、电台 | Cyberpunk | MingCute | Phosphor Bold | Bold |
| 夜店潮流 | 夜店、音乐节 | Cyberpunk | MingCute | Phosphor Bold | Bold |
| 朋友圈社区 | 微博、论坛 | Social Media | Phosphor Regular | Lucide | Regular |
| 即时通讯 | 微信、聊天、消息 | Social Media | Phosphor | Lucide | Regular |

---

## 🔗 相关模块

- [核心基础](./00-core-foundations.md) - Token 系统定义
- [图标系统](./03-icon-system.md) - 各风格的图标选型映射
- [实战案例](./09-case-studies.md) - 完整实现示例
