# Team Workflow - 团队协作流程

> **模块定位**: 多角色协作的标准工作流和 Review 机制
> **依赖关系**: [06-tool-integration.md](./06-tool-integration.md)（工具链）
> **预估行数**: ~160 行

---

## 👥 角色定义

### Core Roles（核心角色）

| 角色 | 职责 | 技能要求 |
|------|------|----------|
| **Product Designer (PD)** | UI/UX 设计、交互原型、设计规范 | Figma/Sketch、用户研究、设计系统 |
| **Mobile Developer (MD)** | iOS/Android 原生开发 | SwiftUI/Compose、平台 API、性能优化 |
| **Cross-Platform Dev (CPD)** | React Native / Flutter 开发 | 框架生态、桥接原生模块 |
| **Design Engineer (DE)** | Design Token 管理、组件库维护 | Style Dictionary、Storybook、CSS-in-JS |
| **QA Engineer** | 视觉回归测试、无障碍测试 | 自动化测试工具、屏幕阅读器 |

### Extended Roles（扩展角色）

- **Product Manager (PM)**: 需求优先级、验收标准
- **User Researcher (UR)**: 可用性测试、用户反馈
- **Content Strategist (CS)**: 文案、多语言适配

---

## 🔄 Standard Workflow（标准工作流）

### Phase 1: Discovery & Planning（发现与规划）

```
PM 提出需求
    ↓
PD 进行用户研究和竞品分析
    ↓ （1-2 天）
PD 输出：
  ✅ User Story Map
  ✅ Information Architecture
  ✅ 初步线框图（低保真）
    ↓
Team Review Meeting（30 min）
  ├── PM 确认需求范围
  ├── DE 评估技术可行性
  └── MD/CPD 确认平台限制
    ↓
确认设计方案 → 进入 Phase 2
```

**交付物清单**:
- [ ] 需求文档（PRD）v0.1
- [ ] 用户故事地图
- [ ] 信息架构图
- [ ] 低保真原型（Figma）

---

### Phase 2: Design & Specification（设计与规格）

```
PD 创建高保真设计
    ↓ （2-5 天，视复杂度而定）
PD 输出：
  ✅ 完整的 UI 设计稿（所有状态）
  ✅ 交互原型（可点击）
  ✅ Design Token 更新（如有变更）
  ✅ 标注说明（Zeplin/Figma Dev Mode）
    ↓
Design Review（1 hour）
  参与者：PD + DE + PM + MD/CPD
    ↓
Review Checklist:
  [ ] 是否符合本 Skill 的设计原则？
  [ ] 所有状态是否完整（Loading/Error/Empty/Success）？
  [ ] 无障碍是否满足 WCAG AA？
  [ ] 暗色模式是否支持？
  [ ] 动效是否符合 [01-interaction-design.md](./01-interaction-design.md)？
  [ ] 图标选型是否符合 [03-icon-system.md](./03-icon-system.md)？
    ↓
通过 → 进入 Phase 3
修改 → 返回 PD 修订
```

**Design Review 模板**:

```markdown
## Design Review: [Feature Name]

### 基本信息
- **设计师**: @designer_name
- **评审日期**: YYYY-MM-DD
- **涉及页面**: X 个屏幕
- **链接**: [Figma Link]

### 评审项

#### 1. 设计一致性 ⭐⭐⭐⭐⭐
- [ ] 符合 Style Preset（如：Super-App）
- [ ] 使用语义化颜色 Token
- [ ] 字体层级正确
- **问题**: _______________

#### 2. 交互完整性 ⭐⭐⭐⭐⭐
- [ ] 所有手势已定义
- [ ] 动效时长符合规范
- [ ] Loading/Empty/Error 状态完整
- **问题**: _______________

#### 3. 平台适配 ⭐⭐⭐⭐
- [ ] iOS/Android 差异已处理
- [ ] 尊重平台导航模式
- [ ] 安全区域处理正确
- **问题**: _______________

#### 4. 无障碍 ⭐⭐⭐⭐
- [ ] 对比度达标（4.5:1）
- [ ] 支持动态字体
- [ ] 有无障碍标签
- **问题**: _______________

#### 5. 性能考量 ⭐⭐⭐
- [ ] 图片已优化（WebP/AVIF）
- [ ] 列表使用虚拟滚动
- [ ] 动画性能可接受
- **问题**: _______________

### 结论
- ✅ **通过** - 可以进入开发
- ⚠️ **有条件通过** - 需修复以下小问题：...
- ❌ **需要大改** - 主要问题：...

### 行动项
- [ ] @person 任务描述 (截止日期)
```

---

### Phase 3: Development & Implementation（开发与实现）

```
开发者领取任务
    ↓
Code Review 准备
  ├── 阅读 Design Spec
  ├── 搭建本地开发环境
  └── 创建 Feature Branch
    ↓
实现代码（遵循本 Skill 的输出格式）
    ↓
开发者自测：
  ✅ 功能正常
  ✅ 视觉还原度 > 95%
  ✅ 无控制台报错
  ✅ 性能在预算内
    ↓
提交 Pull Request
    ↓
Code Review（见下方流程）
```

---

### Phase 4: Code Review（代码评审）

#### Code Review Checklist

```markdown
## Code Review: [Feature Name/PR #]

### 基本信息
- **开发者**: @developer_name
- ** reviewers**: @reviewer_1, @reviewer_2
- **PR Link**: [GitHub/GitLab Link]

### 评审维度

#### A. 设计还原度 (40%)
- [ ] 像素级精准？误差 < 2px?
- [ ] 颜色使用 Token？无硬编码 hex?
- [ ] 间距符合 Grid 系统？
- [ ] 字体大小/粗细正确？
- [ ] 圆角值正确？

**截图对比**:
| 设计稿 | 实现 | 差异 |
|--------|------|------|
| [截图] | [截图] | 说明 |

#### B. 代码质量 (25%)
- [ ] 组件拆分合理？
- [ ] 命名清晰有意义？
- [ ] 无重复代码（DRY）？
- [ ] 类型安全（TypeScript/Kotlin/Swift）？
- [ ] 注释充分但不冗余？

#### C. 性能 (15%)
- [ ] 列表使用虚拟化（VirtualizedList/LazyColumn）？
- [ ] 图片懒加载？
- [ ] 避免不必要的重渲染？
- [ ] Bundle size 影响评估？

#### D. 无障碍 (10%)
- [ ] 所有交互元素有 `accessibilityLabel`？
- [ ] 颜色不是唯一信息载体？
- [ ] Focus order 正确？
- [ ] 支持 VoiceOver/TalkBack？

#### E. 图标规范 (10%)（参考 [03-icon-system.md](./03-icon-system.md)）
- [ ] 图标库统一？（最多混用 2 个）
- [ ] 大小一致？（最多 3 种尺寸）
- [ ] 有 fallback 标注注释？
- [ ] 容器样式符合预设？

### 评审结果
- ✅ **LGTM** (Looks Good To Me) - 可合并
- ⚠️ **Request Changes** - 需修改后重新 Review
- 💬 **Comment** - 建议性意见，非阻塞

### 统计
- 代码行数: +XXX / -XX
- 文件数: X
- 评审耗时: X hours
```

---

### Phase 5: QA & Launch（测试与发布）

```
QA 接收版本
    ↓
执行测试用例：
  ├── 功能测试（Happy Path + Edge Cases）
  ├── 视觉回归测试（见 [08-testing-strategy.md](./08-testing-strategy.md)）
  ├── 无障碍测试（屏幕阅读器）
  ├── 性能测试（Lighthouse/PerfDog）
  └── 多设备/多系统兼容性测试
    ↓
Bug 分类：
  🔴 P0 - Blocking（阻断发布）
  🟡 P1 - Major（应修复但可延后）
  🟢 P2 - Minor（下次迭代修复）
    ↓
Bug 修复循环（如有 P0/P1）
    ↓
最终验收（Sign-off）
  ├── PM 确功能完整
  ├── PD 确视觉质量
  └── DE 确代码质量
    ↓
发布到生产环境 🚀
```

---

## 📋 Communication Guidelines（沟通指南）

### Asynchronous Communication（异步沟通）

| 场景 | 工具 | 格式要求 |
|------|------|----------|
| 设计反馈 | Figma Comment | 具体位置 + 问题 + 建议 |
| Bug 报告 | GitHub Issue | 复现步骤 + 截图 + 期望结果 |
| 技术讨论 | Slack Thread / Discord | 上下文完整，@相关人员 |
| 文档更新 | Confluence / Notion | 版本号 + 变更摘要 |

### Synchronous Communication（同步沟通）

| 会议类型 | 频率 | 时长 | 参与者 |
|----------|------|------|--------|
| Daily Standup | 每日 | 15min | 全员 |
| Design Review | 按需 | 1h | PD + DE + PM |
| Sprint Planning | 双周 | 2h | 全员 |
| Retrospective | 双周 | 1h | 全员 |

### Feedback Principles（反馈原则）

**给予反馈时**:
1. **具体** - 指出具体元素/代码行，而非模糊印象
2. **建设性** - 不仅指出问题，提供解决方案或方向
3. **及时** - 越早反馈，修改成本越低
4. **尊重** - 对事不对人，聚焦产品而非个人

**接收反馈时**:
1. **倾听** - 先理解对方观点，不急于辩解
2. **澄清** - 不确定时提问，确保理解一致
3. **感谢** - 反馈是改进的机会
4. **行动** - 记录并跟进，形成闭环

---

## 🎯 Conflict Resolution（冲突解决）

### 常见冲突场景

| 冲突类型 | 示例 | 解决机制 |
|----------|------|----------|
| **设计 vs 技术** | 设计效果难以高性能实现 | PD + MD 共同寻找替代方案，必要时降低视觉效果 |
| **时间 vs 质量** | 截止临近但质量未达标 | PM 重新排期或削减 scope |
| **平台差异** | iOS/Android 实现难度不同 | 参考 [05-platform-decision-matrix.md](./05-platform-decision-matrix.md)，采用平台自适应策略 |
| **品牌一致性 vs 平台惯例** | 品牌色不符合平台无障碍标准 | 调整品牌色至合规范围，或申请例外 |

### Escalation Path（升级路径）

```
Level 1: 同级讨论（Developer ↔ Designer）
    ↓ 无法解决
Level 2: Tech Lead / Design Lead 仲裁
    ↓ 无法解决
Level 3: Product Manager 决策（权衡优先级）
    ↓ 特殊情况
Level 4: CTO / VP Design 最终决定
```

---

## 📊 Metrics & KPIs（度量指标）

### Process Metrics（过程指标）

| 指标 | 目标值 | 测量方法 |
|------|--------|----------|
| Design → Handoff 时间 | < 2 days | 从 Design Review 通过到开发开始 |
| Code Review 响应时间 | < 24 hours | PR 创建到首次 Review 评论 |
| Bug 发现率（QA vs 用户） | > 80% bugs found by QA | QA 发现数 / 总 bug 数 |
| 设计还原度 | > 95% | 自动化视觉回归测试通过率 |

### Quality Metrics（质量指标）

| 指标 | 目标值 | 测量方式 |
|------|--------|----------|
| 无障碍评分 | Lighthouse Accessibility = 100 | Lighthouse CI |
| 性能分数 | Lighthouse Performance > 90 | Lighthouse CI |
| 包体积增长 | < 5% per feature | Bundle 分析工具 |
| 崩溃率 | < 0.1% | Sentry/Firebase Crashlytics |

---

## 🔗 相关模块

- [工具集成](./06-tool-integration.md) - Design Token 工作流
- [测试策略](./08-testing-strategy.md) - QA 流程细节
- [实战案例](./09-case-studies.md) - 成功项目复盘
