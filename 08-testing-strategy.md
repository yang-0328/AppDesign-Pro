# Testing Strategy - 测试策略

> **模块定位**: 自动化测试方案、工具推荐、质量保障体系
> **依赖关系**: [04-screen-templates.md](./04-screen-templates.md)（输出格式）、[07-team-workflow.md](./07-team-workflow.md)（QA 流程）
> **预估行数**: ~170 行

---

## 🎯 测试金字塔

```
        ┌─────────┐
       /    E2E    \          10% - 端到端测试
      /   (用户流程)  \
     ├───────────────┤
    /    Integration   \       20% - 集成测试
   /   (组件交互)      \
  ├───────────────────┤
 /      Unit           \     70% - 单元测试
/   (纯函数/逻辑)       \
└───────────────────────┘
```

**原则**: 底层测试多且快，顶层测试少但慢。

---

## 1. Visual Regression Testing（视觉回归测试）

### 核心概念

**"截图对比"** — 在每次代码变更后，自动截图并与基准图对比，检测视觉差异。

### 工具选择矩阵

| 工具 | 适用平台 | 优势 | 劣势 | 推荐度 |
|------|----------|------|------|--------|
| **Percy** | Web/React Native | 云服务、智能忽略 | 付费、依赖网络 | ⭐⭐⭐⭐⭐ |
| **Chromatic** | Web (Storybook) | 与 Storybook 深度集成 | 仅限 Web | ⭐⭐⭐⭐⭐ |
| **Applitools** | 全平台 | AI 驱动、跨设备 | 昂贵、企业级 | ⭐⭐⭐⭐ |
| **Appium + ScreenGrab** | iOS/Android | 开源免费 | 配置复杂、维护成本高 | ⭐⭐⭐ |
| **Detox** (RN) | React Native | 专为 RN 设计、灰盒测试 | 仅 RN | ⭐⭐⭐⭐ |

### Percy 配置示例（Web 项目）

```javascript
// percy.config.js
module.exports = {
  version: 2,
  snapshots: {
    width: [375, 768, 1280], // 移动端、平板、桌面
    minHeight: 1024,
    percyCSS: `
      /* 忽略动态内容 */
      .timestamp,
      .ad-banner,
      .randomized-content {
        visibility: hidden;
      }
    `
  },
  statics: {
    // 静态资源目录
    baseDir: '__snapshots__'
  }
}
```

```bash
# package.json scripts
{
  "scripts": {
    "test:visual": "percy exec -- npm run test",
    "test:visual:update": "percy exec -- npm run test -- --updateSnapshot"
  }
}
```

### 视觉回归测试最佳实践

#### ✅ 应该做的

1. **分层快照**
   ```
   Atom Level（原子组件）: Button, Input, Icon → 每次 PR 都测
   Molecule Level（分子组件）: Card, List Item → 关键路径测试
   Organism Level（有机体）: Header, Feed → 发布前必测
   Page Level（页面）: Home, Detail → 回归测试
   ```

2. **智能忽略策略**
   ```css
   /* Percy CSS - 忽略不稳定元素 */
   .skeleton-loader { opacity: 0; } /* 骨架屏 */
   .loading-spinner { display: none; } /* 加载动画 */
   .countdown-timer { content: "00:00"; } /* 倒计时 */
   .map-canvas { background: #E5E3DF; } /* 地图 */
   ```

3. **多分辨率覆盖**
   - iPhone SE (375px) - 小屏适配
   - iPhone 14 Pro (393px) - 主流设备
   - iPad (768px) - 平板适配
   - Desktop (1280px+)

4. **CI/CD 集成**
   ```yaml
   # .github/workflows/visual-regression.yml
   name: Visual Regression Test
   on: [pull_request]
   jobs:
     percy:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v3
         - uses: actions/setup-node@v3
         - run: npm ci
         - run: npm run build
         - run: npx percy exec -- npm run test:visual
           env:
             PERCY_TOKEN: ${{ secrets.PERCY_TOKEN }}
   ```

#### ❌ 应该避免的

1. **过度测试细节** - 不需要测试每个像素，关注布局和关键元素
2. **忽略性能** - 截图测试不应成为性能瓶颈
3. **手动审查所有差异** - 设置合理的 diff threshold，只 review 显著变化
4. **不更新基线** - 当设计有意变更时，及时更新 baseline

---

## 2. Accessibility Testing（无障碍测试）

### 工具推荐

| 工具 | 类型 | 平台 | 功能 |
|------|------|------|------|
| **axe DevTools** | 浏览器扩展 | Web | 实时 WCAG 违规检测 |
| **Lighthouse** | CLI/API | Web | 综合审计（含 a11y） |
| **VoiceOver** (macOS) | 屏幕阅读器 | iOS Simulator | 手动测试 |
| **TalkBack** (Android) | 屏幕阅读器 | Android Emulator | 手动测试 |
| **Accessibility Inspector** (Xcode) | 审计工具 | iOS/macOS | 元素属性检查 |
| **Accessibility Scanner** (Google) | 扫描工具 | Android | 自动扫描 UI 问题 |

### Lighthouse CI 配置示例

```javascript
// lighthouse.config.js
module.exports = {
  ci: {
    assert: {
      'categories:accessibility': ['error', { minScore: 1.0 }],
      'categories:performance': ['warn', { minScore: 0.9 }],
      'categories:best-practices': ['warn', { minScore: 0.9 }],
      'categories:seo': ['off'],
    },
    upload: {
      target: 'temporary-public-storage',
    },
  },
};
```

```bash
# 运行无障碍测试
npx lighthouse http://localhost:3000 \
  --preset=desktop \
  --only-categories=accessibility \
  --output=json \
  --output-path=./lighthouse-a11y-results.json
```

### 无障碍测试清单

#### A. 键盘导航测试（Web/Desktop）

- [ ] Tab 键可以到达所有可交互元素？
- [ ] Focus 顺序符合视觉顺序？
- [ ] Focus indicator 清晰可见？
- [ ] Modal 打开时焦点被 trap 在其中？
- [ ] Escape 键可以关闭 Modal/Dropdown？

#### B. 屏幕阅读器测试（Mobile）

**iOS VoiceOver 测试步骤**:

1. 启用 Xcode Simulator 的 VoiceOver (Cmd+F5)
2. 用单指滑动浏览界面
3. 验证：
   - 所有按钮都有有意义的标签？
   - 图片有 alt text 或 `accessibilityLabel`？
   - 自定义组件实现了 `UIAccessibility` 协议？
   - 状态变更会触发 announcement？

**Android TalkBack 测试步骤**:

1. 启用 TalkBack（设置 → 无障碍 → TalkBack）
2. 用手指滑动探索界面
3. 验证：
   - 所有 View 有 `contentDescription`？
   - `importantForAccessibility` 设置正确？
   - 自定义 View 覆盖了 `onInitializeAccessibilityNodeInfo()`？
   - LiveRegion 用于动态内容更新？

#### C. 对比度与颜色测试

- [ ] 正文文字对比度 ≥ 4.5:1？（使用 [Contrast Checker](https://webaim.org/resources/contrastchecker/)）
- [ ] 大文字（18pt+/24px+）对比度 ≥ 3:1？
- [ ] 交互元素（按钮、链接）对比度 ≥ 3:1？
- [ ] 图标单独使用时有文字标签或 tooltip？
- [ ] 信息不只通过颜色传达？（如：错误状态用图标+颜色+文字）

#### D. 动态字体测试

**iOS Dynamic Type**:
```swift
// 在 Xcode 中测试：Editor → Size Class → 切换字体大小
// 验证：
// - 文字在 200% 大小时不截断
// - 布局自适应调整
// - StackView 正确换行
```

**Android Font Scale**:
```kotlin
// 在开发者选项中切换字体大小（Small → Huge）
// 验证：
// - TextView 使用 sp 单位（非 dp）
// - ConstraintLayout 约束正确
// - RecyclerView item 高度自适应
```

---

## 3. Performance Testing（性能测试）

### 工具矩阵

| 工具 | 平台 | 指标 | 使用场景 |
|------|------|------|----------|
| **Lighthouse** | Web | FCP, LCP, CLS, TTFB | Web 性能审计 |
| **Xcode Instruments** | iOS | CPU, Memory, GPU, Time Profiler | iOS 性能分析 |
| **Android Profiler** | Android | CPU, Memory, Network | Android 性能分析 |
| **Flipper** | Cross-platform | Layout, Network, Database | React Native 调试 |
| **PerfDog** | Mobile Real Devices | FPS, Memory, CPU, GPU | 真机性能监控 |
| **Sentry Performance** | All | Transaction tracing | 生产环境监控 |

### Core Web Vitals 监控

```javascript
// 使用 web-vitals 库
import { getCLS, getFID, getFCP, getLCP, getTTFB } from 'web-vitals';

getCLS(console.log);       // Cumulative Layout Shift (< 0.1)
getFID(console.log);       // First Input Delay (< 100ms)
getFCP(console.log);       // First Contentful Paint (< 1.8s)
getLCP(console.log);       // Largest Contentful Paint (< 2.5s)
getTTFB(console.log);      // Time to First Byte (< 800ms)

// 上报至监控平台
function sendToAnalytics(metric) {
  const body = JSON.stringify({
    name: metric.name,
    value: metric.value,
    rating: metric.rating, // 'good' | 'needs-improvement' | 'poor'
    ...metric.entries[0]
  });

  // Send to your analytics service
  navigator.sendBeacon('/analytics', body);
}

getCLS(sendToAnalytics);
getFID(sendToAnalytics);
getLCP(sendToAnalytics);
```

### 移动端性能预算

| 指标 | iOS 目标值 | Android 目标值 | 测量方式 |
|------|-----------|---------------|----------|
| **App Cold Start** | < 1.5s | < 2s | Instruments/Profiler |
| **Frame Rate** | 60 FPS | 60 FPS | CADisplayLink / Choreographer |
| **Memory Usage** | < 150 MB | < 200 MB | Memory Graph |
| **CPU Usage** | < 50% (idle) | < 50% (idle) | Time Profiler |
| **Battery Drain** | < 5%/hour | < 8%/hour | Energy Impact |
| **APK/IPA Size** | < 50 MB | < 100 MB | Build Output |
| **JS Bundle Size** (RN) | - | < 20 MB (base) | Metro Bundler |

---

## 4. Component Testing（组件测试）

### React Native Testing (Detox + Jest)

```javascript
// __tests__/Button.e2e.js
describe('Button Component', () => {
  beforeEach(async () => {
    await device.launchApp();
  });

  it('should render with correct label', async () => {
    const component = await element(by.id('primary-button'));
    await expect(component).toBeVisible();
    await expect(component).toHaveText('Submit');
  });

  it('should handle press event', async () => {
    const button = await element(by.id('primary-button'));
    await button.tap();

    // 验证后续行为
    const successMessage = await element(by.id('success-toast'));
    await expect(successMessage).toBeVisible();
  });

  it('should be disabled when loading prop is true', async () => {
    const button = await element(by.id('disabled-button'));
    await expect(button).toBeNotEnabled();
  });
});
```

### SwiftUI Unit Testing (XCTest)

```swift
// ButtonTests.swift
@testable import YourApp
import XCTest
import SwiftUI
import ViewInspector

final class ButtonTests: XCTestCase {

    func testButtonRendersCorrectText() throws {
        let view = Button(action: {}) {
            Text("Click Me")
        }

        let text = try view.inspect().text().string()
        XCTAssertEqual(text, "Click Me")
    }

    func testButtonCallsActionOnTap() throws {
        var actionCalled = false
        let view = Button(action: { actionCalled = true }) {
            Text("Tap Me")
        }

        try view.tap()
        XCTAssertTrue(actionCalled)
    }

    func testButtonAppliesDesignToken() throws {
        let view = PrimaryButton(label: "Submit")
            .background(DesignToken.Color.brandPrimary)

        let backgroundColor = try view
            .inspect()
            .view(PrimaryButton.self)
            .backgroundShapeStyle(0)

        // 验证使用了正确的 Token
        XCTAssertNotNil(backgroundColor)
    }
}
```

### Jetpack Compose Testing

```kotlin
// ButtonTest.kt
class ButtonTest {

    @get:Rule
    val composeTestRule = createComposeRule()

    @Test
    fun buttonDisplaysCorrectText() {
        composeTestRule.setContent {
            AppTheme {
                Button(onClick = {}) {
                    Text("Submit")
                }
            }
        }

        composeTestRule
            .onNodeWithText("Submit")
            .assertIsDisplayed()
    }

    @Test
    fun buttonHandlesClick() {
        var clicked = false
        composeTestRule.setContent {
            AppTheme {
                Button(onClick = { clicked = true }) {
                    Text("Tap Me")
                }
            }
        }

        composeTestRule.onNodeWithText("Tap Me").performClick()

        Truth.assertThat(clicked).isTrue()
    }

    @Test
    fun buttonUsesCorrectColor() {
        composeTestRule.setContent {
            AppTheme {
                Button(onClick = {}) {
                    Text("Styled Button")
                }
            }
        }

        // 验证背景色使用 Token
        composeTestRule
            .onNodeWithText("Styled Button")
            .assertBackgroundColorEquals(
                Color(0xFFFFD000.toInt()) // brand-primary
            )
    }
}
```

---

## 5. End-to-End (E2E) Testing（端到端测试）

### 典型用户流程测试用例

```yaml
# e2e/user-flows.yaml

- name: "User Login Flow"
  priority: P0
  steps:
    - open_app
    - tap: "Login Button"
    - enter_text:
        field: "Email"
        value: "user@example.com"
    - enter_text:
        field: "Password"
        value: "securepassword123"
    - tap: "Submit"
    - wait_for: "Home Screen"
    - verify: "Welcome message visible"

- name: "Add to Cart Flow"
  priority: P0
  steps:
    - navigate_to: "Product Detail"
    - select_option: "Size: M"
    - tap: "Add to Cart"
    - verify: "Cart badge shows 1"
    - navigate_to: "Cart"
    - verify: "Product in cart"

- name: "Checkout Flow"
  priority: P0
  steps:
    - precondition: "Cart has items"
    - navigate_to: "Cart"
    - tap: "Checkout"
    - fill_shipping_address
    - select_payment_method
    - confirm_order
    - verify: "Order confirmation screen"
```

### E2E 测试框架选型

| 框架 | 适用平台 | 特点 | 学习曲线 |
|------|----------|------|----------|
| **Detox** | React Native | 灰盒测试、稳定、快速 | 中等 |
| **Maestro** | iOS/Android | 声明式 YAML、无需编码 | 低 |
| **Appium** | 全平台 | 标准 WebDriver 协议 | 高 |
| **Cypress** | Web | 快速、实时重载 | 低 |
| **Playwright** | Web | 多浏览器支持、自动等待 | 中等 |

---

## 6. CI/CD Integration（持续集成配置）

### GitHub Actions 示例

```yaml
# .github/workflows/test.yml
name: Test Suite

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  # 单元测试
  unit-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
      - run: npm ci
      - run: npm run test:unit -- --coverage
      - uses: codecov/codecov-action@v3
        with:
          files: ./coverage/lcov.info

  # 视觉回归测试
  visual-regression:
    runs-on: ubuntu-latest
    needs: unit-tests
    if: github.event_name == 'pull_request'
    steps:
      - uses: actions/checkout@v3
      - run: npm ci
      - run: npm run build
      - run: npx percy exec -- npm run test:visual
        env:
          PERCY_TOKEN: ${{ secrets.PERCY_TOKEN }}

  # E2E 测试
  e2e-tests:
    runs-on: ubuntu-latest
    needs: visual-regression
    if: github.ref == 'refs/heads/main'
    steps:
      - uses: actions/checkout@v3
      - run: npm ci
      - run: npm run test:e2e

  # 无障碍审计
  accessibility-audit:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: npm ci
      - run: npm run build
      - run: npx lighthouse http://localhost:3000 --only-categories=accessibility --output=json
      - name: Check a11y score
        run: |
          SCORE=$(node -e "const r=require('./lighthouse-result.json'); console.log(r.categories.accessibility.score)")
          if (( $(echo "$SCORE < 1.0" | bc -l) )); then
            echo "::error::Accessibility score is $SCORE (must be 1.0)"
            exit 1
          fi
```

---

## ✅ 测试策略总结清单

在发布新功能前，确认：

### 必须通过的测试 (P0)
- [ ] 单元测试覆盖率 > 80%（核心逻辑）
- [ ] 关键用户流程的 E2E 测试通过
- [ ] 视觉回归测试无 unexpected changes
- [ ] Lighthouse Accessibility = 100
- [ ] Lighthouse Performance > 90

### 应该通过的测试 (P1)
- [ ] 组件库 Storybook 快照测试通过
- [ ] 真机性能测试达标（FPS ≥ 55）
- [ ] 内存泄漏检测通过
- [ ] 多语言/i18n 测试通过

### 可以延后的测试 (P2)
- [ ] 边缘设备的兼容性测试
- [ ] 弱网环境的降级测试
- [ ] 辅助功能（如 Voice Control）的高级场景

---

## 🔗 相关模块

- [屏幕模板](./04-screen-templates.md) - 输出格式与质量标准
- [团队工作流](./07-team-workflow.md) - QA 流程与 Review 机制
- [工具集成](./06-tool-integration.md) - CI/CD 配置
