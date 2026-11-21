# DaoHang · 液态玻璃移动端 UI 模板

一套 iOS 26 Liquid Glass 风格的移动端界面基座：底部 4 项主胶囊 + 独立搜索胶囊，带完整「设置」页可实时调节外观，并已沉淀出可复用的组件与开发规约。

- **单文件交付**：`index.html`，零外部依赖，断网可打开
- **仓库**：https://github.com/babyopen/DaoHang
- **版本**：1.3.2
- **风格参考**：rdev/liquid-glass-react（本项目为 CSS + 自写 WebGL 双实现，未引入该库）

---

## 快速开始

直接用浏览器打开 `index.html` 即可；手机端建议「添加到主屏幕」以获得全屏与安全区体验。

部署到 GitHub Pages：仓库 Settings → Pages → 选择 `main` 分支根目录，保存后即可通过 Pages 地址访问。

---

## 功能一览

| 模块 | 说明 |
| --- | --- |
| 底部导航 | 4 项主胶囊 + 独立搜索胶囊，选中指示器弹性滑动，支持负值悬浮高度 |
| 设置页 | 32 项可调配置，实时预览、防抖持久化、双击单项恢复默认 |
| 深浅双套 | 着色/透明度/模糊/饱和度/边框/高光/噪点/投影/指示器色 分深浅两套独立记忆 |
| 玻璃质感 | CSS `backdrop-filter` + 内高光、噪点颗粒、RGB 边缘色散、双层背板、投影 |
| WebGL 折射 | 自写 shader 做边缘位移折射（SDF 环带采样 + 色散 + 厚度明暗），设置页可开关 |
| 壁纸 | 本地上传（自动压缩 1600px）、5 套预设渐变、模糊/暗化、主色提取 |
| 账户 | 头像上传（居中裁切 256px）、名称/邮箱编辑校验，本地持久化 |
| 弹窗 | 底部上滑 Sheet：遮罩关闭、Esc、下拉手势、焦点锁定与归还、滚动锁定 |
| 搜索 | 设置项关键词检索，命中结果点击直达并高亮 |
| 按钮体系 | primary/secondary/ghost/danger/text、尺寸、加载、禁用、图标、胶囊、悬浮按钮 |
| 无障碍 | 对话框语义、焦点环（仅键盘触发）、`prefers-reduced-motion` 适配 |

---

## 核心参数（设置页可调）

玻璃材质：着色、基底透明度、模糊强度、饱和度、边框强度、内高光、模糊色散、噪点颗粒、投影强度

布局：胶囊圆角、卡片圆角、悬浮高度（-40~40）、胶囊间距、图标尺寸、导航项最小宽度、选中项放大、文字标签、双层背板

背景：壁纸上传、壁纸模糊、壁纸暗化、预设渐变、主色提取

指示器：颜色、样式（填充胶囊 / 描边环）

其它：显示模式、动画速度、界面字号、性能模式、通知、触感反馈、滑动动效、滚动收起导航

> 单文件内这些参数集中定义在脚本 A 区块 `DEFAULT_CONF` 与 `DEFAULT_VARIANTS`，新增参数只需在此登记，设置页加一行控件即可自动接入渲染、持久化、搜索与双击重置。

---

## 组件用法

### 按钮

```html
<button class="btn primary">主要</button>
<button class="btn secondary">次要</button>
<button class="btn ghost">取消</button>
<button class="btn danger">删除</button>
<button class="btn text">文字</button>

<button class="btn primary sm">小号</button>
<button class="btn primary lg">大号</button>
<button class="btn primary block">整宽</button>

<button class="btn secondary is-disabled">禁用</button>
<button class="btn secondary is-loading">加载中</button>

<button class="pill-btn">胶囊标签</button>
<button class="btn icon ghost">＋</button>
```

设置页底部「组件预览」区块即为这些样式的实时对照，可随主题与玻璃参数变化。

### 玻璃容器与列表

```html
<div class="liquid-glass card">
  <div class="row"><span class="row-label">标题</span><span class="row-val">值</span></div>
  <div class="row tap"><span class="row-label">可点击行</span></div>
  <div class="row tap danger"><span class="row-label">危险操作</span></div>
</div>
```

### 新增一个页面

1. 在 `main` 内追加 `<section class="view" data-view="xxx">…</section>`
2. 底部导航追加 `<div class="tab-item" data-nav="xxx">…</div>`（主胶囊内）
3. 内容直接用 `.liquid-glass.card` / `.row` / `.btn` 组合，风格自动继承
4. 若需新弹窗，复用 `.sheet` 结构与 H 区块的开关函数，不要自造弹窗动画

---

## 本地存储键

| Key | 内容 |
| --- | --- |
| `lgSettingsV3` | 外观配置（含深浅两套变体，不含壁纸） |
| `lgWallpaperV1` | 壁纸图片 dataURL（单独存放，避免拖动滑块时重复序列化） |
| `lgProfileV1` | 账户名称、邮箱 |
| `lgAvatarV1` | 头像图片 dataURL |

「清除缓存」只清业务临时数据，以上四项均保留。

---

## 开发规约（硬性）

1. **视图与数据分离**：渲染、事件、DOM 操作留在视图区块；存储、计算、校验集中在数据区块，数据区块内不出现 `innerHTML` / `classList` / 样式操作。
2. **样式只走变量**：新组件复用 `--lg-*` 变量与既有 class，不新增独立的 `backdrop-filter` 组合。
3. **切换不加淡入**：`.view` 容器不做 opacity 过渡，避免玻璃层逐帧重绘造成闪动；过渡只给标题等无玻璃元素。
4. **弹窗唯一实现**：统一使用 Sheet 组件（焦点锁定、Esc、手势、滚动锁定已内置）。
5. **性能**：滑块输入只渲染，落盘走 200ms 防抖；提供性能模式关闭实时模糊。
6. **提交**：每次改动完成即提交并推送，提交信息用 `feat: / fix: / chore:` 前缀说明改了什么。

---

## 版本记录

| 版本 | 内容 |
| --- | --- |
| 1.3.2 | 7 项优化（搜索直达、深浅双套、壁纸独立存储、写入防抖、单项重置、通知权限、减弱动效）、切换闪动修复、账户可编辑、负悬浮高度、完整弹窗交互、按钮体系、组件预览 |
| 1.3.1 | 设置页独立视图、壁纸与预设、性能模式 |
| 1.3.0 | 玻璃参数面板 → 独立设置页 |
