# Platform Decision Matrix - 平台差异化决策矩阵

> **模块定位**: iOS vs Android vs Cross-platform 的详细对比和选型指南
> **依赖关系**: [00-core-foundations.md](./00-core-foundations.md)（通用原则）
> **预估行数**: ~200 行

---

## 🎯 核心原则

**"Adapt, Don't Clone"** — 适配平台，而非简单复制。

每个平台都有其独特的设计语言和用户期望。跨平台开发时，应在**保持品牌一致性**的同时，**尊重平台惯例**。

---

## 1. Navigation Patterns（导航模式对比）

### Stack Navigation（栈导航）

| 特性 | iOS (SwiftUI) | Android (Jetpack Compose) | React Native |
|------|---------------|---------------------------|--------------|
| **API** | `NavigationStack` | `NavController` / `NavHost` | `@react-navigation/native` |
| **返回手势** | 左边缘滑动（系统级） | 系统返回手势/按钮 | 平台自适应 |
| **转场动画** | 默认从右侧滑入 | 默认从底部滑入 | 平台默认 |
| **导航栏** | `.navigationBarTitleDisplayMode(.large)` | `TopAppBar` / `Scaffold` | `<Stack.Screen>` |
| **大标题** | ✅ Large Title（iOS 特色） | ❌ 不适用 | 仅 iOS |

#### 代码对比：页面跳转

```swift
// iOS - SwiftUI
NavigationStack {
    List(items) { item in
        NavigationLink(value: item) {
            ItemRow(item)
        }
    }
    .navigationDestination(for: Item.self) { item in
        DetailView(item: item)
    }
    .navigationTitle("列表")
    .navigationBarTitleDisplayMode(.large) // 大标题
}
```

```kotlin
// Android - Jetpack Compose
NavHost(navController = navController, startDestination = "list") {
    composable("list") {
        Scaffold(
            topBar = { TopAppBar(title = { Text("列表") }) }
        ) {
            LazyColumn {
                items(items) { item ->
                    ListItem(
                        headlineContent = { Text(item.title) },
                        onClick = { navController.navigate("detail/${item.id}") }
                    )
                }
            }
        }
    }
    composable("detail/{itemId}") { backStackEntry ->
        val itemId = backStackEntry.arguments?.getString("itemId")
        DetailView(itemId = itemId)
    }
}
```

```jsx
// React Native
<Stack.Navigator>
  <Stack.Screen
    name="List"
    component={ListScreen}
    options={{ headerLargeTitle: true }} // iOS only
  />
  <Stack.Screen
    name="Detail"
    component={DetailScreen}
    options={({ route }) => ({ title: route.params.title })}
  />
</Stack.Navigator>
```

---

### Tab Navigation（标签导航）

| 特性 | iOS | Android | Cross-platform |
|------|-----|---------|----------------|
| **位置** | 底部固定 | 底部固定（Material 3） | 底部固定 |
| **图标状态** | Fill/Line 切换 | Fill/Outline 切换 | 双态切换 |
| ** badge** | 右上角红点/数字 | 同左 + 可带动效 | 统一处理 |
| **中间凸起** | 不常见 | 较常见（FAB 模式） | 可选 |
| **标签数** | 3-5 个 | 3-5 个 | 3-5 个 |

#### 代码对比：底部 Tab 栏

```swift
// iOS - SwiftUI (TabView)
TabView(selection: $selectedTab) {
    HomeView()
        .tabItem {
            Image(systemName: selectedTab == .home ? "house.fill" : "house")
            Text("首页")
        }
        .tag(Tab.home)

    ProfileView()
        .tabItem {
            Image(systemName: selectedTab == .profile ? "person.fill" : "person")
            Text("我的")
        }
        .tag(Tab.profile)
}
.tint(.orange) // iOS 15+ 自定义选中色
```

```kotlin
// Android - Jetpack Compose (NavigationBar)
NavigationBar(selectedItem = selectedItem) {
    items.forEach { item ->
        NavigationBarItem(
            icon = {
                Icon(
                    imageVector = if (selectedItem == item) item.selectedIcon else item.icon,
                    contentDescription = item.label
                )
            },
            label = { Text(item.label) },
            selected = selectedItem == item,
            onClick = { selectedItem = item }
        )
    }
}
```

---

### Modal Presentation（模态呈现）

| 特性 | iOS | Android | 说明 |
|------|-----|---------|------|
| **全屏模态** | `.fullScreenCover` | `Popup` / Dialog | 覆盖整个屏幕 |
| **表单模态** | `.sheet` (Bottom Sheet) | `BottomSheetScaffold` | 从底部滑出 |
| **居中弹窗** | `.alert` / `.confirmationDialog` | `AlertDialog` | 确认操作 |
| **关闭方式** | 下拉关闭 / 点击外部 | 下拉关闭 / 返回按钮 | 平台自适应 |

---

## 2. UI Components（UI 组件对比）

### Button Styles（按钮样式）

| 按钮类型 | iOS (SwiftUI) | Android (Compose) | 差异点 |
|----------|---------------|-------------------|--------|
| **Filled** | `.buttonStyle(.borderedProminent)` | `Button(defaultElevation = ...)` | iOS 圆角更大 |
| **Outlined** | `.buttonStyle(.bordered)` | `OutlinedButton` | 边框粗细不同 |
| **Text** | `.buttonStyle(.plain)` | `TextButton` | 文字颜色不同 |
| **FAB** | 浮动按钮不常用 | `FloatingActionButton` | Android 特色组件 |
| **Icon Toggle** | `Toggle` / `Picker` | `IconButton` / `Switch` | 交互方式差异 |

### Input Fields（输入框）

| 特性 | iOS | Android | 处理建议 |
|------|-----|---------|----------|
| **键盘类型** | `.keyboardType(.emailAddress)` | `KeyboardType.Email` | 映射关系明确 |
| **Return 键** | `.submitLabel(.done)` | `imeAction = ImeAction.Done` | 功能一致，API 不同 |
| **清除按钮** | `.textFieldStyle(.roundedBorder)` 自动显示 | `trailingIcon = { IconButton(...) }` | 需手动实现 |
| **密码可见** | `SecureField` | `TextField(visualTransformation = PasswordVisualTransformation())` | API 名称不同 |
| **错误提示** | 底部红色文字或 Field 内 icon | `TextField` 的 `supportingText` + `isError` | Material 有内置支持 |

---

## 3. System Integration（系统集成对比）

### Permissions（权限请求）

| 权限类型 | iOS | Android | 最佳实践 |
|----------|-----|---------|----------|
| **相机** | `NSCameraUsageDescription` | `CAMERA` (Runtime) | iOS 在 Info.plist 声明，Android 运行时请求 |
| **相册** | `NSPhotoLibraryUsageDescription` (iOS 14+) 分读写 | `READ_EXTERNAL_STORAGE` | iOS 14+ 需要分权限级别 |
| **定位** | `NSLocationWhenInUseUsageDescription` | `ACCESS_FINE_LOCATION` | 都需要说明用途 |
| **通知** | 用户在设置中控制 | 运行时请求 + Channel | Android 需要创建 Channel |
| **麦克风** | `NSMicrophoneUsageDescription` | `RECORD_AUDIO` | 必须提供拒绝后的降级方案 |

**⚠️ 关键差异**:
- iOS: **一次性授权**（用户选择"允许"或"不允许"）
- Android: **运行时动态请求**（可再次询问）
- 建议：Android 使用 **"温和引导"** 策略，避免频繁弹窗

### Biometric Auth（生物识别认证）

```swift
// iOS - Face ID / Touch ID
import LocalAuthentication

let context = LAContext()
var error: NSError?

if context.canEvaluatePolicy(.deviceOwnerAuthenticationWithBiometrics, error: &error) {
    context.evaluatePolicy(.deviceOwnerAuthenticationWithBiometrics, localizedReason: "验证身份以继续") { success, _ in
        if success {
            // 认证成功
        }
    }
}
```

```kotlin
// Android - BiometricPrompt
val biometricPrompt = BiometricPrompt(this, ContextCompat.getMainExecutor(this),
    object : BiometricPrompt.AuthenticationCallback() {
        override fun onAuthenticationSucceeded(result: BiometricPrompt.AuthenticationResult) {
            super.onAuthenticationSucceeded(result)
            // 认证成功
        }
    })

val promptInfo = BiometricPrompt.PromptInfo.Builder()
    .setTitle("身份验证")
    .setSubtitle("使用指纹或面容登录")
    .setNegativeButtonText("取消")
    .build()

biometricPrompt.authenticate(promptInfo)
```

---

## 4. Motion & Animation（动效系统对比）

### Spring Animation（弹簧动画）

| 参数 | iOS | Android | 说明 |
|------|-----|---------|------|
| **API** | `.spring(response:dampingFraction:)` | `spring(stiffness:dampingRatio:)` | 参数名称不同但概念一致 |
| **响应速度** | `response: 0.5` (秒) | `stiffness: StiffnessMedium` | iOS 用时间，Android 用刚度 |
| **阻尼比** | `dampingFraction: 0.7` | `dampingRatio: DampingRatioMediumBouncy` | 范围略有差异 |
| **默认曲线** | Ease-out | Ease-out | 基本一致 |

### Shared Element Transition（共享元素转场）

| 特性 | iOS | Android | 实现难度 |
|------|-----|---------|----------|
| **原生支持** | ❌ 需手动实现 (matchedGeometryEffect) | ✅ 原生支持 (Shared Element Transition) | Android 更成熟 |
| **API** | `.matchedGeometryEffect(id:)` | `sharedElement()` + `Transition` | iOS 14+ 才完善 |
| **复杂度** | 中等 | 低 | Android 有更好的工具链支持 |

---

## 5. Design Tokens Platform Mapping（设计 Token 平台映射）

### Spacing Token 映射

| Token Name | Value | iOS (pt) | Android (dp) | Web (rem) |
|------------|-------|----------|--------------|-----------|
| xxs | 4 | 4pt | 4dp | 0.25rem |
| xs | 8 | 8pt | 8dp | 0.5rem |
| sm | 12 | 12pt | 12dp | 0.75rem |
| md | 16 | 16pt | 16dp | 1rem |
| lg | 24 | 24pt | 24dp | 1.5rem |
| xl | 32 | 32pt | 32dp | 2rem |
| xxl | 48 | 48pt | 48dp | 3rem |

### Typography Token 映射

| 层级 | iOS (Font size) | Android (sp) | Web (px/rem) |
|------|------------------|--------------|--------------|
| Hero | 32pt (.largeTitle) | 32sp (displayLarge) | 32px / 2rem |
| Title | 24pt (.title) | 24sp (titleLarge) | 24px / 1.5rem |
| Subtitle | 18pt (.headline) | 18sp (titleMedium) | 18px / 1.125rem |
| Body | 16pt (.body) | 16sp (bodyLarge) | 16px / 1rem |
| Caption | 13pt (.callout) | 13sp (bodyMedium) | 13px / 0.8125rem |
| Micro | 11pt (.footnote) | 11sp (labelSmall) | 11px / 0.6875rem |

### Color Token 导出格式

```json
// tokens/colors.json (Design Token 标准)
{
  "brand": {
    "primary": { "value": "#FFD000" },
    "primary-light": { "value": "#FFF8E0" },
    "secondary": { "value": "#FF7E00" }
  },
  "bg": {
    "page": { "value": "#F5F5F5" },
    "card": { "value": "#FFFFFF" }
  },
  "text": {
    "primary": { "value": "#1A1A1A" },
    "secondary": { "value": "#666666" }
  }
}
```

**📖 详细工具集成**: 请查看 [06-tool-integration.md](./06-tool-integration.md)

---

## 6. Platform-Specific Anti-Patterns（平台特有反模式）

### ❌ iOS 反模式

| 反模式 | 问题 | 正确做法 |
|--------|------|----------|
| 使用 Android 风格的 FAB | 不符合 iOS 设计语言 | 使用常规按钮或右上角 Bar Button |
| 隐藏状态栏 | 违反 Apple HIG | 保持状态栏可见，可透明融合 |
| 自定义返回按钮样式 | 破坏用户习惯 | 使用系统默认的 "< 返回" |
| 全局禁用左右滑动返回 | 降低操作效率 | 仅在特定场景（如游戏）禁用 |

### ❌ Android 反模式

| 反模式 | 问题 | 正确做法 |
|--------|------|----------|
| 使用 iOS 风格的大标题 | 不符合 Material Design | 使用 TopAppBar 或 CollapsingToolbar |
| 底部导航超过 5 个 Tab | 拥挤且难以点击 | 合并部分 Tab 或使用 Drawer |
| 全屏透明状态栏（非沉浸式） | 与系统 UI 冲突 | 使用 `windowInsets` 或 Edge-to-Edge |
| 忽略系统返回手势 | 让用户困惑 | 尊重系统导航模式 |

### ❌ Cross-platform 反模式

| 反模式 | 问题 | 正确做法 |
|--------|------|----------|
| 完全相同的 UI | 忽略平台差异 | 适配各平台的设计语言 |
| 硬编码平台判断代码 | 维护困难 | 使用抽象层或平台特定文件 |
| 忽略无障碍差异 | 各平台屏幕阅读器行为不同 | 分别测试 VoiceOver/TalkBack |
| 统一使用一种图标库 | 可能与原生风格冲突 | 参考 [03-icon-system.md](./03-icon-system.md) 选型 |

---

## 7. Decision Flowchart（决策流程图）

```
开始新功能开发
    ↓
是否需要平台差异化？
    ├── 否 → 使用统一组件库 (React Native / Flutter)
    │           ↓
    │       应用 [02-style-presets.md] 规范
    │           ↓
    │       完成 ✅
    │
    └── 是 → 目标平台？
                ├── iOS → 参考 Apple HIG
                │           ↓
                │       使用 SwiftUI/UIKit 原生组件
                │           ↓
                │       应用 iOS 特有模式（Large Title、SF Symbols...）
                │
                ├── Android → 参考 Material Design 3
                │           ↓
                │       使用 Jetpack Compose/XML 原生组件
                │           ↓
                │       应用 Android 特有模式（TopAppBar、FAB、Ripple...）
                │
                └── Web → 参考 Web Content Accessibility Guidelines (WCAG)
                            ↓
                        使用响应式布局 + CSS Grid/Flexbox
                            ↓
                        应用 Web 特有模式（Hover、Focus ring、Scroll snap...）
```

---

## 📚 参考资源

### 官方指南
- [Apple Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/)
- [Material Design 3](https://m3.material.io/)
- [React Native Documentation](https://reactnative.dev/)
- [Flutter Documentation](https://flutter.dev/docs)

### 社区资源
- [Mobile UX Patterns](https://mobileuxpatterns.com/) - 跨平台模式库
- [Platform Differences Wiki](https://github.com/react-native-community/wiki) - RN 社区维护

---

## 🔗 相关模块

- [核心基础](./00-core-foundations.md) - 通用设计原则
- [交互设计](./01-interaction-design.md) - 动效规范
- [样式预设](./02-style-presets.md) - 风格适配
- [工具集成](./06-tool-integration.md) - Design Token 导出
