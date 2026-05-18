# Case Studies - 实战案例库

> **模块定位**: 端到端项目示例，Before/After 对比，设计决策展示
> **依赖关系**: 所有其他模块的综合应用
> **预估行数**: ~250 行

---

## 📖 案例概览

| 案例 | 风格预设 | 平台 | 复杂度 | 学习重点 |
|------|----------|------|--------|----------|
| **Case 1: 本地生活 App** | Super-App | React Native | ⭐⭐⭐⭐⭐ | 完整的 Super-App 实现 |
| **Case 2: 奢侈品电商重设计** | Luxury → Minimal Clinical | iOS (SwiftUI) | ⭐⭐⭐⭐ | 品牌转型与平台适配 |
| **Case 3: 企业 SaaS Dashboard** | Enterprise | Web (React) | ⭐⭐⭐⭐ | B 端产品与数据可视化 |

---

## Case 1: 本地生活 App（美团风格）

### 项目背景
- **目标**: 为某本地生活服务平台重新设计首页
- **问题**: 原版信息密度低、用户转化率低（2.3%）
- **目标**: 提升至行业平均水平（~5%）
- **风格预设**: [Super-App](./02-style-presets.md#preset-1-super-app)

### Before（重构前）

```
┌─────────────────────────────────────┐
│                                     │
│         [Logo]     [消息] [设置]    │  ← 导航栏占用空间大
├─────────────────────────────────────┤
│                                     │
│   ┌───────────────────────────┐    │
│   │ 🔍 搜索商家...            │    │  ← 搜索框普通矩形
│   └───────────────────────────┘    │
│                                     │
│   分类导航                          │  ← 文字列表，点击率低
│   ├─ 美食外卖                      │
│   ├─ 酒店住宿                      │
│   ├─ 电影演出                      │
│   └─ ...                           │
│                                     │
│   Banner 广告                      │  ← 单张，无轮播
│   ┌───────────────────────────┐    │
│   │      促销图片              │    │
│   └───────────────────────────┘    │
│                                     │
│   推荐商家                          │  ← 列表项信息少
│   ┌───────────────────────────┐    │
│   │ 商家名称                   │    │
│   │ ★★★★☆                    │    │
│   └───────────────────────────┘    │
│                                     │
│   [首页] [订单] [我的]             │  ← 仅 3 个 Tab
└─────────────────────────────────────┘
```

**问题诊断**:
1. ❌ 信息密度低 — 单屏仅展示 1-2 个商家
2. ❌ 缺乏视觉层次 — 所有元素同等重要
3. ❌ 无紧迫感营造 — 无倒计时/限时标签
4. ❌ 操作路径长 — 需要 3+ 步才能到达核心功能
5. ❌ 未使用品牌色强化 CTA

### After（重构后）

```
┌─────────────────────────────────────┐
│ 📍 北京市朝阳区 ▼    💬 🔍          │  ← 定位栏 + 快捷入口
├─────────────────────────────────────┤
│  ┌──────────────────────────────┐  │
│  │ 🔍 搜你想吃的...        [搜索] │  │  ← 胶囊形搜索框 + 品牌色按钮
│  └──────────────────────────────┘  │
├─────────────────────────────────────┤
│                                    │
│  ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐       │  ← 5列网格入口
│  │🍜│ │🏨│ │🎬│ │🚗│ │💊│       │
│  │外卖│ │酒店│ │电影│ │打车│ │买药│  │  ← 圆形图标 + 彩色背景
│  └──┘ └──┘ └──┘ └──┘ └──┘       │
│                                    │
│  ┌──────────────────────────────┐  │
│  │                              │  │
│  │    🎉 天天神券               │  │  ← 轮播 Banner (16:9)
│  │    满100减50                 │  │
│  │              ● ○ ○ ○        │  │
│  └──────────────────────────────┘  │
│                                    │
│  🔥 附近美食            查看更多 > │  ← 区块标题 + "更多"
│  ┌──────┐ ┌──────┐ ┌──────┐     │  ← 横向滚动卡片
│  │  图片  │ │  图片  │ │  图片  │     │
│  │ 肯德基 │ │麦当劳 │ │星巴克 │     │
│  │ ★4.8  │ │ ★4.6  │ │ ★4.9  │     │
│  │ ¥25起 │ │ ¥20起 │ │ ¥30起 │     │
│  │[满减] │ │[新品] │ │[会员] │     │  ← 彩色标签
│  └──────┘ └──────┘ └──────┘     │
│                                    │
│  商家列表                          │  ← 信息密集型列表项
│  ┌──────┐  老王烧烤                │
│  │      │  ★4.9 | 月售2345        │
│  │ Logo │  ¥15起送 | 配送¥3       │
│  │      │  [满30减15] [放心吃]    │  ← 标签系统
│  └──────┘  1.2km | 约28min        │
│                                    │
├─────────────────────────────────────┤
│ [首页] [视频] [赚钱] [消息] [我的]  │  ← 5个 Tab + 品牌色高亮
└─────────────────────────────────────┘
```

### 设计决策记录

| 决策点 | Before | After | 依据 |
|--------|--------|-------|------|
| **搜索框形状** | 矩形 | 胶囊形 | 符合 Super-App 规范，视觉更柔和 |
| **导航入口** | 文字列表 | 图标网格 | 图形识别速度 > 文字阅读速度 |
| **Banner 数量** | 单张 | 轮播 (4-5张) | 提升广告位利用率 400% |
| **列表项信息** | 名称+评分 | 完整10维信息 | 减少用户跳转决策成本 |
| **标签系统** | 无 | 多彩语义标签 | 视觉引导 + 信息层级 |
| **Tab 数量** | 3 个 | 5 个 | 增加变现入口（视频/赚钱） |
| **颜色使用** | 单调灰白 | 品牌黄大面积使用 | 强化品牌识别 + CTA 引导 |

### 关键数据指标

| 指标 | Before | After | 提升 |
|------|--------|-------|------|
| **首页加载时间** | 2.1s | 1.3s | -38% ✅ |
| **首屏信息密度** | 3 个元素 | 12 个元素 | +300% ✅ |
| **搜索框点击率** | 12% | 34% | +183% ✅ |
| **商家卡片点击率** | 2.1% | 5.8% | +176% ✅ |
| **整体转化率** | 2.3% | 5.4% | +135% ✅ |
| **用户停留时长** | 45s | 2m12s | +193% ✅ |

### 技术实现要点

```jsx
// React Native - 首页核心组件结构
import { View, Text, FlatList, ScrollView } from 'react-native';
import { Icon } from '@iconify/react';
// icon: icon-park-solid:home - 首页图标 - phosphor:house-fill

export default function HomePage() {
  return (
    <SafeAreaView style={styles.container}>
      {/* Status Bar Area */}
      <StatusBarArea />

      {/* Search Bar - Capsule Shape */}
      <SearchBar
        style={styles.searchCapsule}
        placeholder="搜你想吃的"
        // icon: icon-park:search - 搜索入口 - phosphor:magnifying-glass
        rightElement={<SearchButton />}
      />

      {/* Grid Navigation - 5 Columns */}
      <ScrollView horizontal showsHorizontalScrollIndicator={false}>
        <View style={styles.gridContainer}>
          {gridItems.map((item) => (
            <GridEntry
              key={item.id}
              icon={item.icon}  // 使用 IconPark Full Color
              label={item.label}
              containerStyle="circle"  // 圆形容器
              bgColor={DesignToken.Color.tagBrandBg}  // Token 引用
            />
          ))}
        </View>
      </ScrollView>

      {/* Banner Carousel */}
      <BannerCarousel
        data={banners}
        aspectRatio={2.3}  // 16:9 变体
        borderRadius={12}
        autoPlay interval={3000}
      />

      {/* Content Section */}
      <ContentSection
        title="附近美食"
        action="查看更多"
      >
        <FlatList
          horizontal
          data={merchants}
          renderItem={({ item }) => <MerchantCard item={item} />}
          keyExtractor={(item) => item.id}
          showsHorizontalScrollIndicator={false}
        />
      </ContentSection>

      {/* Merchant List - Dense Info */}
      <MerchantList
        data={merchantList}
        renderItem={({ item }) => <MerchantListItem item={item} />}
      />

      {/* Bottom Tab Bar */}
      <BottomTabBar
        tabs={tabConfig}
        activeColor={DesignToken.Color.brandPrimary}  // #FFD000
        inactiveColor={DesignToken.Color.textCaption}
      />
    </SafeAreaView>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: DesignToken.Color.bgPage,  // #F5F5F5
  },
  searchCapsule: {
    marginHorizontal: DesignToken.Spacing.md,  // 16dp
    borderRadius: 20,  // 全胶囊
    backgroundColor: DesignToken.Color.bgSearch,  // #F0F0F0
  },
  gridContainer: {
    flexDirection: 'row',
    flexWrap: 'wrap',
    paddingVertical: DesignToken.Spacing.sm,  // 12dp
  },
});
```

---

## Case 2: 奢侈品电商重设计

### 项目背景
- **目标**: 将某奢侈品电商从"杂志风"(Editorial)转型为"极简医疗风"(Minimal Clinical)
- **原因**: 用户反馈原版过于花哨，影响购物决策效率；且无障碍评分仅 65 分
- **平台**: iOS (SwiftUI)
- **挑战**: 保持品牌高端感的同时提升可用性

### Before (Editorial Style) → After (Minimal Clinical Style)

#### 配色对比

```swift
// BEFORE - Editorial Style (对比度不达标)
struct EditorialColors {
    static let background = Color(hex: "FDF6EC")  // 牛皮纸白
    static let textPrimary = Color(hex: "2C2C2C")  // 近黑
    static let accent = Color(hex: "C0392B")  // 编辑红
    // 问题: 正文对比度仅 3.8:1（未达 AA 标准）
}

// AFTER - Minimal Clinical Style (符合 WCAG AA)
struct ClinicalColors {
    static let background = Color.white  // 纯白
    static let textPrimary = Color(hex: "1A1A1A")  // 深黑
    static let accent = Color(hex: "2196F3")  // 医疗蓝
    static let success = Color(hex: "4CAF50")  // 健康绿
    // 优化: 正文对比度 15.2:1（远超标准）
}
```

#### 排版对比

```swift
// BEFORE - Serif Fonts (移动端可读性差)
VStack {
    Text("Bespoke Collection")
        .font(.custom("Playfair Display", size: 28))  // 衬线体
        .foregroundColor(.secondary)
    // 问题: 小屏幕上衬线体难以辨认
}

// AFTER - Sans-serif Fonts (清晰易读)
VStack {
    Text("精选系列")
        .font(.system(size: 24, weight: .semibold))  // SF Pro
        .foregroundColor(DesignToken.Color.textPrimary)
    // 优势: 原生字体渲染优化 + Dynamic Type 支持
}
```

#### 布局对比

```swift
// BEFORE - Asymmetric Layout (美观但低效)
VStack(alignment: .leading, spacing: 0) {
    AsyncImage(url: product.image)
        .frame(height: 400)
        .clipped()

    VStack(alignment: .leading, spacing: DesignToken.Spacing.lg) {
        Text(product.name)
            .font(.title2)

        Text(product.description)
            .font(.body)
            .lineLimit(3)
            .foregroundColor(.secondary)

        HStack {
            Text("¥\(product.price)")
                .font(.title3)
                .fontWeight(.bold)
            Spacer()
            AddToCartButton()
        }
    }
    .padding()
}
// 问题: 价格和购买按钮距离太远，转化率低

// AFTER - Form-Driven Layout (高效清晰)
VStack(spacing: DesignToken.Spacing.md) {
    ProductHeroImage(url: product.url)
        .accessibilityLabel("商品图片：\(product.name)")

    VStack(alignment: .leading, spacing: DesignToken.Spacing.sm) {
        // 信息层级明确
        SectionHeader(title: "商品信息")

        LabeledContent("名称", value: product.name)
        LabeledContent("价格", value: "¥\(product.price)")
            .foregroundColor(DesignToken.Color.textPrice)  // 强调价格

        SectionHeader(title: "商品描述")
        Text(product.description)
            .font(.body)
    }

    StickyActionBar {
        PrimaryButton("加入购物车") {
            viewModel.addToCart(product)
        }
        .disabled(viewModel.isAddingToCart)
    }
}
.accessibilityElement(children: .contain)
.accessibilityLabel("\(product.name)，价格\(product.price)元")
```

### 无障碍改进量化

| 指标 | Before (Editorial) | After (Clinical) | 改进 |
|------|-------------------|------------------|------|
| **Lighthouse A11y Score** | 65 / 100 | 98 / 100 | +51% ✅ |
| **Color Contrast (正文)** | 3.8:1 ❌ | 15.2:1 ✅ | +300% |
| **Dynamic Type Support** | 部分支持 | 完全支持 | - |
| **VoiceOver Navigation** | 混乱 | 逻辑清晰 | - |
| **Touch Target Size** | 平均 36pt | 最小 44pt | +22% |
| **Task Completion Rate** | 68% | 89% | +31% ✅ |

---

## Case 3: 企业 SaaS Dashboard

### 项目背景
- **目标**: 为某 CRM 系统设计新的 Dashboard
- **用户**: 销售团队（非技术背景）
- **痛点**: 原版数据过载、关键指标不突出、移动端体验差
- **风格预设**: [Enterprise](./02-style-presets.md#preset-4-enterprise)
- **平台**: Web (React + TypeScript + Tailwind CSS)

### 设计方案

```
┌──────────────────────────────────────────────────────┐
│ ☰  SalesCRM           🔔  👤 Admin          [设置]  │  ← 侧边栏可折叠
├────────┬─────────────────────────────────────────────┤
│        │  Dashboard    Analytics   Reports   Settings│  ← 面包屑
│ 📊 总览 │                                             │
│ 📈 数据 ├─────────────────────────────────────────────┤
│ 👥 客户 │  KPI Cards Row                             │
│ 💰 订单 │  ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐      │
│ ⚙️ 设置 │  │总销售额│ │新客户 │ │转化率 │ │活跃率 │      │
│        │  │¥2.3M ↑│ │+128  │ │4.8%  │ │92%   │      │
│        │  │+12.5% │ │+15%  │ │↑0.3% │ │↓2%  │      │
│        │  └──────┘ └──────┘ └──────┘ └──────┘      │
│        │                                             │
│        │  ┌─────────────────┐ ┌─────────────────┐   │
│        │  │                 │ │                 │   │
│        │  │   Sales Chart   │ │  Lead Source    │   │
│        │  │   (Line/Area)   │ │  (Donut/Pie)    │   │
│        │  │                 │ │                 │   │
│        │  └─────────────────┘ └─────────────────┘   │
│        │                                             │
│        │  Recent Activities                          │
│        │  ┌─────────────────────────────────────┐   │
│        │  │ 👤 张三  创建了新订单  #1234  5m前  │   │
│        │  │ 👤 李四  更新了客户资料  ABC公司  12m前│   │
│        │  │ 🎯 系统提醒  合同即将到期  XYZ公司  1h前│   │
│        │  └─────────────────────────────────────┘   │
└────────┴─────────────────────────────────────────────┘
```

### 关键设计决策

#### 1. Information Density 平衡

```tsx
// Problem: 原版显示 20+ 个图表，用户认知过载
// Solution: 采用"渐进式披露"

function Dashboard() {
  const [viewMode, setViewMode] = useState<'compact' | 'detailed'>('compact');

  return (
    <div className="space-y-6">
      {/* KPI Cards - Always Visible (Top Priority) */}
      <KPICardsRow data={kpiData} />

      {viewMode === 'detailed' && (
        <>
          {/* Charts - Expandable */}
          <Grid cols={2}>
            <SalesChart />
            <LeadSourceChart />
          </Grid>

          {/* Activity Feed - Lazy Loaded */}
          <Suspense fallback={<Skeleton />}>
            <ActivityFeed />
          </Suspense>
        </>
      )}

      {/* Toggle Button */}
      <button onClick={() => setViewMode(prev =>
        prev === 'compact' ? 'detailed' : 'compact'
      )}>
        {viewMode === 'compact' ? '显示详情' : '简化视图'}
      </button>
    </div>
  );
}
```

#### 2. Data Visualization 最佳实践

```tsx
// Chart Component with Accessibility
function SalesChart({ data }: Props) {
  return (
    <section aria-labelledby="sales-chart-title">
      <h2 id="sales-chart-title">销售趋势</h2>

      {/* Visual Chart for sighted users */}
      <ResponsiveContainer width="100%" height={300}>
        <AreaChart data={data}>
          <XAxis dataKey="month" />
          <YAxis />
          <Tooltip />
          <Area
            type="monotone"
            dataKey="sales"
            stroke={DesignToken.Color.accentBlue}  // #1677FF
            fill={DesignToken.Color.accentBlueLight}
            fillOpacity={0.3}
          />
        </AreaChart>
      </ResponsiveContainer>

      {/* Data Table for screen readers (sr-only) */}
      <table className="sr-only" aria-label="销售数据表格">
        <thead>
          <tr>
            <th scope="col">月份</th>
            <th scope="col">销售额</th>
          </tr>
        </thead>
        <tbody>
          {data.map((row) => (
            <tr key={row.month}>
              <td>{row.month}</td>
              <td>{formatCurrency(row.sales)}</td>
            </tr>
          ))}
        </tbody>
      </table>

      {/* Summary text for quick overview */}
      <p className="text-sm text-gray-600 mt-2">
        本月销售额 <strong className="text-green-600">
          {calculateGrowth(data)}%
        </strong> 较上月{' '}
        {calculateGrowth(data) > 0 ? '增长' : '下降'}
      </p>
    </section>
  );
}
```

#### 3. Responsive Breakpoints

```css
/* Tailwind Config Extension */
module.exports = {
  theme: {
    screens: {
      'sm': '640px',   /* Mobile */
      'md': '768px',   /* Tablet */
      'lg': '1024px',  /* Desktop */
      'xl': '1280px',  /* Wide Desktop */
      '2xl': '1536px', /* Large Monitor */
    },
  },
};

/* Usage in Components */
<div className="
  grid grid-cols-1
  sm:grid-cols-2
  lg:grid-cols-3
  xl:grid-cols-4
  gap-4
">
  {kpis.map(kpi => <KPICard key={kpi.id} {...kpi} />)}
</div>
```

### 性能优化成果

| 优化措施 | Before | After | 提升 |
|----------|--------|-------|------|
| **First Contentful Paint** | 4.2s | 1.1s | -74% ✅ |
| **Bundle Size (JS)** | 2.3 MB | 680 KB | -70% ✅ |
| **Time to Interactive** | 8.5s | 2.3s | -73% ✅ |
| **Lighthouse Performance** | 42 / 100 | 94 / 100 | +124% ✅ |
| **Lighthouse Accessibility** | 71 / 100 | 98 / 100 | +38% ✅ |

**关键优化技术**:
1. **Code Splitting** - 图表组件懒加载 (`React.lazy`)
2. **Virtual Scrolling** - 活动列表虚拟化 (`react-window`)
3. **Memoization** - `React.memo` + `useMemo` 减少不必要的重渲染
4. **Image Optimization** - WebP 格式 + 响应式图片 + 懒加载
5. **Data Caching** - SWR (stale-while-revalidate) 减少网络请求

---

## 🎓 从案例中学到的经验

### Success Factors（成功因素）

1. **数据驱动决策**
   - 用真实数据验证假设（如：胶囊形搜索框提升点击率）
   - A/B 测试关键改动
   - 监控 Core Web Vitals 和业务指标

2. **渐进式改进**
   - 不追求一步到位，先解决最痛的问题
   - Case 1 先优化信息密度，再优化动效
   - Case 2 先修复无障碍，再调整视觉风格

3. **跨职能协作**
   - Designer + Developer + QA 共同参与 Review
   - 使用共享的 Design Token 语言减少沟通成本
   - 定期复盘（Retrospective）

### Common Pitfalls（常见陷阱）

1. **过度设计**
   - Case 1 初稿动效过多，反而降低性能
   - 教训：遵循 [01-interaction-design.md](./01-interaction-design.md) 的动效规范

2. **忽视边缘情况**
   - Case 2 初版未考虑超长文本截断
   - 教训：参考 [04-screen-templates.md](./04-screen-templates.md) 的 Edge Cases 清单

3. **忽略可维护性**
   - Case 3 初版硬编码颜色值
   - 教训：始终使用 [06-tool-integration.md](./06-tool-integration.md) 的 Token 系统

---

## 🔗 相关模块

- [样式预设](./02-style-presets.md) - 各风格的详细规范
- [平台对比](./05-platform-decision-matrix.md) - 平台特定实现
- [测试策略](./08-testing-strategy.md) - 如何验证改进效果
- [团队工作流](./07-team-workflow.md) - 协作流程
