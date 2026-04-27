[ARCHITECTURE.md](https://github.com/user-attachments/files/27119043/ARCHITECTURE.md)
# 共生宠物 · PetLife — 项目架构说明

## 目录结构

```
代办/
├── index.html          # 页面骨架（仅 HTML 结构）
├── ARCHITECTURE.md     # 本文档
├── 设计方案.md          # 原始设计稿（参考文档）
├── css/
│   ├── base.css        # 全局重置 & CSS 变量 & 屏幕切换
│   ├── onboarding.css  # 引导流程（选蛋 + 命名）
│   ├── app.css         # 主应用布局（Header、Tab 区、底部导航）
│   ├── tasks.css       # 任务列表 & 进度条 & 打卡横幅
│   ├── pet.css         # 宠物展示卡 & 进化路线 & 属性条
│   ├── bag.css         # 背包网格 & 食物/道具卡片
│   ├── stats.css       # 统计页（周报、图表、成就墙）
│   ├── modals.css      # 底部抽屉弹窗 & 表单控件
│   └── fx.css          # Toast、彩带粒子、升级弹窗
└── js/
    ├── data.js         # 静态数据常量（只读）
    ├── state.js        # 全局状态 S & 持久化
    ├── router.js       # 屏幕 & Tab 切换
    ├── fx.js           # 视觉特效函数
    ├── onboarding.js   # 引导流程逻辑
    ├── pet.js          # 宠物渲染 & 升级逻辑
    ├── tasks.js        # 任务 CRUD & 完成逻辑
    ├── bag.js          # 背包渲染 & 喂食/道具使用
    ├── stats.js        # 统计渲染 & 日期工具
    ├── modals.js       # 弹窗开关 & 表单构建
    └── app.js          # 入口：初始化 & 事件绑定
```

---

## CSS 文件职责

### `css/base.css`
全局基础层，所有其他样式的依赖前提。

- **CSS 变量**：定义全站设计令牌，如 `--or:#FF9F43`（橙）、`--co:#FF6B6B`（红）、`--mn:#FFF9F0`（米白背景）等
- **全局重置**：`box-sizing: border-box`，`body` 字体、背景色、防溢出
- **屏幕切换机制**：`.screen` 默认 `display:none`；`.screen.active` 设为 `display:flex` 并触发 `fadeUp` 入场动画
- **滚动条美化**：WebKit 自定义滚动条样式

### `css/onboarding.css`
引导流程两个页面的专属样式。

- **选蛋页** (`#s-ob`)：星空渐变背景、标题排版、`.eggs-grid` 2×2 网格、`.egg-card` 卡片（含 `.sel` 选中态：白底 + 绿色对勾）、确认按钮的激活态 (``.ob-btn.on``)
- **命名页** (`#s-nm`)：居中卡片、输入框行、快速建议 Chip 列表

### `css/app.css`
主应用 (`#s-main`) 的骨架布局。

- **Header** (`.hdr`)：渐变背景、日期文本、宠物徽章（表情 + 名字 + 等级 + XP 细条）
- **Tab 容器** (`.tab-area`)：`position:relative`，内部 `.tab` 用 `display:none/block` 切换，切入时触发 `fadeUp`
- **底部导航** (`.bnav`)：固定在底部的 4 按钮导航栏，`.nb.active` 高亮样式

### `css/tasks.css`
任务标签页的所有组件样式。

- **进度条** (`.dprog`)：今日进度文字 + 橙色填充条
- **打卡横幅** (`#streak-area`)：连续打卡天数展示
- **任务卡片** (`.tc`)：左侧彩色分类竖条（`.cw/.cs/.cp/.cl/.co/.cr`）、勾选按钮、任务信息、完成时的划线动画
- **添加按钮** (`.add-btn`)：橙色渐变 + 阴影悬浮效果

### `css/pet.css`
宠物标签页的展示组件。

- **宠物展示区** (`.pet-disp`)：大号表情 + `petIdle` 上下漂浮动画、装饰性背景圆圈
- **进化路线** (`.evo-stages`)：横向 5 阶段，当前阶段带橙色光晕 + 放大效果
- **属性条** (`.sf`)：5 种属性（饱腹/活力/心情/洁净/羁绊）各有独立颜色
- **性格标签** (`.tr-list`)：圆角 Chip 列表

### `css/bag.css`
背包标签页样式。

- **网格布局** (`.food-grid`)：3 列等宽网格
- **物品卡片** (`.fi`)：圆角卡片、大号表情、名称、数量角标 (`.fi-cnt`)
- **稀有度标签** (`.rc/.rr/.re/.rl`)：普通灰 / 稀有蓝 / 史诗紫 / 传说橙

### `css/stats.css`
统计标签页的图表与成就组件。

- **英雄卡** (`.st-hero`)：深色渐变背景、4 宫格数字统计
- **宠物评语气泡** (`.pet-comment`)：表情 + 圆角对话框
- **周柱状图** (`.bar-chart`)：7 列等宽，今日列橙色高亮
- **分类横条图** (`.cat-row`)：标签 + 彩色填充条 + 百分比
- **成就墙** (`.ach-grid`)：4 列网格，未解锁呈灰色模糊

### `css/modals.css`
所有弹窗的通用基础层。

- **遮罩** (`.overlay`)：全屏半透明黑底，点击空白处关闭
- **底部抽屉** (`.sheet`)：白色圆角卡片，打开时触发 `slideUp` 动画
- **表单控件**：`.cbt`（分类/时段按钮）、`.dbt`（难度按钮）、`.finp`（输入框）、`.sbmt`（提交按钮）
- **完成弹窗**：食物奖励区的弹跳动画

### `css/fx.css`
全局视觉特效，叠加在最顶层。

- **Toast** (`.toast`)：底部浮现的黑色提示条，`.show` 时淡入，2.6 秒后自动消失
- **彩带** (`.cf`)：绝对定位粒子，`cff` 关键帧实现从顶部坠落 + 旋转
- **升级弹窗** (`.lvup`)：居中弹出，弹簧缩放动画，3 秒后自动消失

---

## JS 文件职责

### `js/data.js`
**纯静态数据，不含任何逻辑，无任何依赖。**

| 常量 | 内容 |
|------|------|
| `PET_TYPES` | 4 种宠物类型（cat/fish/rabbit/bird），每种含 5 阶段表情、阶段名、性格标签、推荐名字 |
| `XP_THRESHS` | 10 个等级的 XP 阈值数组 |
| `FOODS_DEF` | 8 种食物的默认定义（属性增量、初始数量、稀有度） |
| `TOOLS_DEF` | 4 种道具的定义 |
| `CATS` | 6 个任务分类（含关联属性 sk、属性增量 sv） |
| `DIFFS` | 3 档难度（含 XP 奖励、食物奖励列表） |
| `SLOTS` | 4 个时间段 |
| `DEFAULT_TASKS` | 5 个预置任务 |
| `ACHS` | 12 个成就定义 |
| `RAR_CLS` / `RAR_LBL` | 稀有度 CSS 类名 & 显示文字映射 |

### `js/state.js`
**全局状态单例 `S` 与持久化逻辑。**

- `S`：应用运行时的唯一数据源，包含宠物数据、任务列表、食物/道具库存、连击数、表单缓存等
- `initInventory()`：从 `FOODS_DEF`/`TOOLS_DEF` 初始化默认库存
- `save()`：将 `S` 序列化为 JSON 存入 `localStorage`（键名 `'pl4'`）
- `loadState()`：从 `localStorage` 恢复 `S`，返回 `true` 表示有存档

### `js/router.js`
**页面导航，管理屏幕与标签的切换。**

- `goTo(id)`：隐藏所有 `.screen`，显示目标屏幕（切换 `.active` 类）
- `switchTab(tab)`：切换底部导航高亮 + Tab 内容区显示；Tab 切入时懒触发对应渲染函数

  懒渲染映射：
  ```
  tasks  → renderTasks()
  pet    → renderPet()
  bag    → renderBag()
  stats  → renderStats()
  ```

### `js/fx.js`
**视觉特效工具函数（无状态）。**

- `showToast(msg)`：在 `#toast` 显示文字提示，自动消失
- `spawnConfetti(n)`：动态创建 n 个彩带 DOM 节点，3 秒后自动清理
- `showLevelUp(prevLv)`：更新并显示 `#lvup` 升级弹窗，配合撒彩带

### `js/onboarding.js`
**引导流程（选蛋 → 命名）的全部逻辑。**

- `buildStars()`：随机生成 80 颗星星粒子，填充 `#stars`
- `bindEggCards()`：为每张蛋卡绑定点击事件，管理 `.sel` 选中态，激活确认按钮
- `goToNaming()`：校验已选蛋类型，切换至命名页并预填推荐名字 Chip
- `confirmName()`：读取输入框内容，保存至 `S.petName`/`S.petType`，调用 `save()`，跳转至主应用

### `js/pet.js`
**宠物数据计算与宠物相关 UI 渲染。**

- `stage()`：根据 `S.pet.lv` 返回当前进化阶段（0-4）
- `renderHeader()`：更新顶部徽章（表情、名字、等级、XP 进度条）
- `renderPet()`：渲染宠物标签页全部内容（大图、进化路线、XP 卡、属性条、性格标签）
- `petMsg()`：根据属性值返回宠物心情台词

  升级逻辑内嵌于 `completeTask()`（`tasks.js`）中调用 `renderHeader()` + `showLevelUp()`。

### `js/tasks.js`
**任务的增删查改与完成奖励。**

- `renderTasks()`：按时间段分组渲染所有任务，更新进度条文字与填充宽度
- `makeTaskCard(task)`：创建单张任务卡 DOM 节点，绑定勾选点击事件
- `completeTask(task, card)`：标记任务完成，计算 XP + 属性增量，检测升级，触发彩带，打开完成弹窗
- `showCompleteModal(task, foods)`：填充 `#m-done` 弹窗内容（奖励食物、属性变化）
- `submitTask()`：读取 `#m-add` 表单，创建新任务对象，追加至 `S.tasks`，保存，关闭弹窗，刷新视图

### `js/bag.js`
**背包渲染、喂食与道具使用。**

- `renderBag()`：遍历 `S.foods`/`S.tools`，调用 `makeFoodItem()` 渲染网格
- `makeFoodItem(k, item, type)`：创建食物/道具卡片 DOM，绑定点击事件
- `openFeedModal(k)`：缓存当前选中食物键至 `S.feedKey`，更新 `#m-feed` 弹窗内容（表情、描述、属性预览），打开弹窗
- `doFeed()`：执行喂食，扣减数量，增加属性，保存，更新视图，显示 Toast
- `useTool(k)`：执行道具效果（固定属性增量表），扣减数量，保存，刷新背包

### `js/stats.js`
**统计标签页渲染与日期工具。**

- `setDate()`：更新 `#hdr-date`（今日日期）与 `#st-per`（本周时间段文字）
- `renderStats()`：渲染完整统计页（宠物评语、数字卡、柱状图、分类横条图、成就墙）

  注：柱状图前 6 天为模拟演示数据，今日列使用真实完成率。

### `js/modals.js`
**弹窗生命周期管理与动态表单构建。**

- `openModal(id)`：为 `.overlay` 添加 `.open` 类，触发 `slideUp` 动画
- `closeModal(id)`：移除 `.open` 类；若关闭的是 `#m-done`，额外刷新 Header 与任务列表
- `buildForms()`：在应用初始化时一次性创建"分类/难度/时段"按钮组，绑定点击事件（更新 `S.form` 缓存）

### `js/app.js`
**应用入口，协调所有模块的初始化与全局事件绑定。**

- `init()`：DOMContentLoaded 后执行；有存档则直接进入主应用，否则启动引导流程
- `setupMain()`：进入主应用后一次性完整渲染所有模块
- `bindAllEvents()`：绑定所有全局 UI 事件（蛋选择、命名、底部导航、弹窗按钮、遮罩点击）；使用 `_bound` 标志保证幂等性

---

## 模块依赖关系

```
data.js
  └── state.js
        ├── router.js
        │     └── (被 onboarding.js / app.js 调用)
        ├── fx.js          (无依赖，独立工具层)
        ├── onboarding.js  → 依赖: state, router, fx, data
        ├── pet.js         → 依赖: state, data, fx
        ├── tasks.js       → 依赖: state, data, pet, fx, modals
        ├── bag.js         → 依赖: state, data, fx, modals
        ├── stats.js       → 依赖: state, data
        ├── modals.js      → 依赖: state, data, tasks, pet
        └── app.js         → 依赖: 所有模块（入口）
```

`index.html` 中的 `<script>` 加载顺序严格对应上述依赖链：
```
data → state → router → fx → onboarding → pet → tasks → bag → stats → modals → app
```

---

## 数据流

```
用户操作
  │
  ▼
事件处理函数（tasks.js / bag.js / onboarding.js 等）
  │
  ├─→ 修改全局状态 S（state.js）
  │     └─→ save() 持久化到 localStorage
  │
  ├─→ 调用特效（fx.js）
  │     showToast / spawnConfetti / showLevelUp
  │
  └─→ 调用渲染函数（pet.js / tasks.js / bag.js / stats.js）
        └─→ 更新 DOM（直接操作 innerHTML / classList）
```

**渲染均为"全量替换"**：每次调用 `renderXxx()` 会重建对应区域的全部 DOM，无虚拟 DOM diff。适合当前数据规模，保持代码简洁。

---

## 持久化

- **存储键**：`localStorage['pl4']`
- **存储内容**：`JSON.stringify(S)`（全量序列化）
- **恢复时机**：`init()` 首次调用 `loadState()`；若返回 `true` 则跳过引导流程
- **写入时机**：任意修改 `S` 的操作末尾调用 `save()`

---

## 关键设计决策

| 问题 | 决策 | 原因 |
|------|------|------|
| 屏幕切换 | `display:none/flex` + CSS `fadeUp` 动画 | `opacity+pointer-events` 方案在部分浏览器下点击穿透问题难以排查 |
| Tab 切换 | `display:none/block` + `fadeUp` | 同上；同时支持懒渲染（切换时才触发 render） |
| 事件绑定 | `el._bound` 标志防重复 | `setupMain()` 与 `init()` 均调用 `bindAllEvents()`，避免重复注册 |
| 状态管理 | 单一全局对象 `S` | 无框架约束下最简单可靠；数据量小，全量序列化无性能问题 |
| 渲染策略 | 全量 `innerHTML` 替换 | 任务数量有限，避免 diff 算法的复杂度；代码量极少 |
