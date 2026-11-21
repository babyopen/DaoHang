# DaoHang · 液态玻璃移动端 UI 模板

一套 iOS 26 Liquid Glass 风格的移动端界面基座：底部 4 项主胶囊 + 独立搜索胶囊，带完整「设置」页可实时调节外观，并已沉淀出可复用的组件层与开发规约。

- **单文件交付**：`index.html`，零外部依赖，断网可打开
- **仓库**：https://github.com/babyopen/DaoHang
- **版本**：1.5.0
- **风格参考**：rdev/liquid-glass-react（本项目为 CSS + 自写 WebGL 双实现，未引入该库）

---

## 快速开始

直接用浏览器打开 `index.html` 即可；手机端建议「添加到主屏幕」以获得全屏与安全区体验。

部署到 GitHub Pages：仓库 Settings → Pages → 选择 `main` 分支根目录，保存后即可通过 Pages 地址访问。

---

## 功能一览

| 模块 | 说明 |
| --- | --- |
| 底部导航 | 4 项主胶囊 + 独立搜索胶囊，选中指示器弹性滑动，支持负值悬浮高度，支持角标 |
| 滑轮交互 | 左右滑动切换页面（可关），滚动可收起导航 |
| 设置页 | 35 项可调配置；19 个滑块均为「±偏移」制，0 = 默认外观，可左右拖动调正负、双击回 0、分组折叠 |
| 深浅双套 | 着色/透明度/模糊/饱和度/边框/高光/噪点/投影/指示器色 分深浅两套独立记忆，可分别重置 |
| 玻璃质感 | CSS `backdrop-filter` + 内高光、噪点颗粒、RGB 边缘色散、双层背板、投影 |
| WebGL 折射 | 自写 shader 做边缘位移折射（SDF 环带采样 + 色散 + 厚度明暗），设置页可开关 |
| 壁纸 | 本地上传（自动压缩 1600px）、5 套预设渐变、模糊/暗化、主色提取 |
| 账户 | 头像上传（居中裁切 256px）、名称/邮箱编辑校验，本地持久化；卡片上可一键切换显示模式 |
| 弹窗 | 底部上滑 Sheet（编辑类）+ 居中确认弹窗（二次确认/输入类），均含遮罩、Esc、焦点管理 |
| 提示 | Toast 四态（成功/警告/错误/信息），玻璃胶囊，自动消失 |
| 搜索 | 设置项关键词检索，命中后自动展开折叠分组并高亮直达 |
| 按钮体系 | primary/secondary/ghost/danger/text、尺寸、加载、禁用、图标、胶囊、悬浮按钮 |
| 基础组件 | 分割线、徽标（数字/红点）、空状态、列表项（图标+标题+副标题+箭头） |
| 字体 | 系统 / 圆润 / 衬线 三套字族切换 |
| 无障碍 | 对话框语义、焦点环（仅键盘触发）、`prefers-reduced-motion` 适配 |

---

## 参数模型：一切滑块都是「偏移量」

所有数值参数存的是**相对默认外观的偏移**，不是绝对值：

- **0 = 默认外观**（滑块停在中间，界面与出厂状态一致）
- **负值 = 减弱**（向左拖），**正值 = 增强**（向右拖）
- 双击参数名回到 0；深浅两套材质的偏移各自独立记忆

渲染时统一走 `eff(key)`：基准值 + 偏移，再按合法区间夹紧。基准与区间：

| 参数 | key | 基准（深 / 浅） | 偏移范围 | 生效区间 |
| --- | --- | --- | --- | --- |
| 基底透明度 | opacity | 0.35 / 0.55 | -0.32 ~ +0.32 | 0.03 ~ 0.90 |
| 模糊强度 | blur | 28 / 26 px | -28 ~ +28 | 0 ~ 60 |
| 饱和度 | saturation | 120 / 135 % | -110 ~ +110 | 0 ~ 250 |
| 边框强度 | borderAlpha | 0.10 / 0.18 | -0.18 ~ +0.20 | 0 ~ 0.5 |
| 内高光 | highlight | 0.55 / 0.78 | -0.55 ~ +0.45 | 0 ~ 1 |
| 噪点颗粒 | grain | 0.12 / 0.07 | -0.12 ~ +0.30 | 0 ~ 0.45 |
| 投影强度 | shadow | 0.32 / 0.18 | -0.32 ~ +0.50 | 0 ~ 0.9 |
| 模糊色散 | chroma | 0 | -6 ~ +6 | 0 ~ 8 |
| 胶囊圆角 | radius | 999 px | -999 ~ +120 | 0 ~ 999 |
| 卡片圆角 | cardRadius | 20 px | -20 ~ +20 | 0 ~ 40 |
| 悬浮高度 | bottom | 16 px | -76 ~ +60 | -60 ~ 90 |
| 胶囊间距 | gap | 12 px | -12 ~ +18 | 0 ~ 32 |
| 图标尺寸 | iconScale | 1.0 × | -0.60 ~ +0.80 | 0.4 ~ 1.9 |
| 导航最小宽度 | tabMin | 72 px | -32 ~ +48 | 40 ~ 130 |
| 动画速度 | animSpeed | 1.0 × | -0.8 ~ +1.5 | 0.2 ~ 2.5 |
| 界面字号 | fontScale | 1.0 × | -0.14 ~ +0.30 | 0.85 ~ 1.35 |
| 壁纸模糊 | wallBlur | 0 px | -40 ~ +44 | 0 ~ 44 |
| 壁纸暗化 | wallDim | 0.35 | -0.35 ~ +0.55 | 0 ~ 0.9 |

新增数值参数时：在 `DEFAULT_CONF` 登记 `0`、在 `BASE_CONF` 写基准、在 `LIMIT` 写区间，滑块 `min/max/step` 用对称偏移，`applyConfig` 里用 `eff(key)` 取值即可。

## 组件调用示例

### 提示 Toast

```js
toast('保存成功');          // 普通
toast('保存成功', 'ok');     // 成功（✓）
toast('空间不足', 'warn');   // 警告（!）
toast('上传失败', 'err');    // 错误（✕）
toast('显示模式：浅色', 'info'); // 信息（i）
```

### 居中确认弹窗

```js
// 二次确认
confirmDialog({
  title: '删除这条记录？',
  desc: '删除后无法恢复，请确认。',
  okText: '删除',
  danger: true,               // 确认按钮变红
  onOk: function(){ toast('已删除', 'ok'); }
});

// 带输入框
confirmDialog({
  title: '新建模块',
  desc: '给新模块起个名字。',
  input: true,
  placeholder: '例如：每日复盘',
  okText: '创建',
  onOk: function(name){ toast('已创建「' + name + '」', 'ok'); }
});
```

### 底部 Sheet（编辑类）

复用 `#profile-sheet` 的结构与 `openProfile / closeProfile`；新增 Sheet 时只需换内容区，遮罩、手势下拉、焦点锁定、滚动锁定都已内置。

### 按钮

```html
<button class="btn primary">主要</button>
<button class="btn secondary">次要</button>
<button class="btn ghost">取消</button>
<button class="btn danger">删除</button>
<button class="btn text">文字</button>
<button class="btn icon ghost">＋</button>

<button class="btn primary sm">小号</button>
<button class="btn primary lg">大号</button>
<button class="btn primary block">整宽</button>

<button class="btn secondary is-disabled">禁用</button>
<button class="btn secondary is-loading">加载中</button>
<button class="pill-btn">胶囊标签</button>
```

### 卡片 / 列表行

```html
<div class="liquid-glass card">
  <div class="row"><span class="row-label">标题</span><span class="row-val">值</span></div>
  <div class="row tap"><span class="row-label">可点击行</span></div>
  <div class="row tap danger"><span class="row-label">危险操作</span></div>
</div>
```

### 列表项

```html
<div class="list-item">
  <div class="li-icon">📄</div>
  <div class="li-body">
    <div class="li-title">标题</div>
    <div class="li-sub">副标题，过长自动省略</div>
  </div>
  <svg class="chev" viewBox="0 0 24 24"><path d="M9 5l7 7-7 7"/></svg>
</div>
```

### 空状态

```html
<div class="empty">
  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6">
    <rect x="3" y="5" width="18" height="14" rx="3"/><path d="M8 11h8M8 15h5"/>
  </svg>
  <h3>还没有自定义模块</h3>
  <p>创建第一个模块，把常用操作收进同一个入口。</p>
  <button class="btn sm primary">新建模块</button>
</div>
```

### 徽标 / 分割线

```html
<span class="badge">3</span>      <!-- 数字徽标 -->
<span class="badge dot"></span>   <!-- 红点 -->
<div class="divider"></div>       <!-- 分割线 -->

<!-- 导航角标：放在 .tab-item 内 -->
<div class="tab-item" data-nav="app">…<div class="badge dot"></div></div>
```

### 点击复制

给行加 `data-copy` 即可，点击后自动 Toast 提示（不支持剪贴板 API 时回退 `execCommand`）：

```html
<div class="row tap" data-copy="v1.4.0">
  <span class="row-label">当前版本号</span><span class="row-val">点击复制</span>
</div>
```

### 新增一个页面

1. 在 `main` 内追加 `<section class="view" data-view="xxx">…</section>`
2. 底部导航追加 `<div class="tab-item" data-nav="xxx">…</div>`（主胶囊内）
3. 内容直接用 `.liquid-glass.card` / `.row` / `.list-item` / `.btn` 组合，风格自动继承
4. 若需新弹窗，编辑类复用 Sheet，确认类用 `confirmDialog`
5. 若加设置项，只需在设置页按现有结构加 `.row`，`applyConfig` 里补一行同步即可自动接入搜索索引与双击重置

---

## 本地存储键

| Key | 内容 |
| --- | --- |
| `lgSettingsV3` | 外观配置（含深浅两套变体、折叠状态、字体、滑动开关，不含壁纸） |
| `lgWallpaperV1` | 壁纸图片 dataURL（单独存放，避免拖动滑块时重复序列化） |
| `lgProfileV1` | 账户名称、邮箱 |
| `lgAvatarV1` | 头像图片 dataURL |

「清除缓存」只清业务临时数据，以上四项均保留。

---

## 开发规约（硬性）

1. **视图与数据分离**：渲染、事件、DOM 操作留在视图区块；存储、计算、校验集中在数据区块，数据区块内不出现 `innerHTML` / `classList` / 样式操作。
2. **样式只走变量**：新组件复用 `--lg-*` 变量与既有 class，不新增独立的 `backdrop-filter` 组合。
3. **切换不加淡入**：`.view` 容器不做 opacity 过渡，避免玻璃层逐帧重绘造成闪动；过渡只给标题等无玻璃元素。
4. **弹窗二选一**：编辑内容用底部 Sheet，二次确认/输入用 `confirmDialog`，不自造第三种弹窗动画。
5. **焦点不留在容器上**：程序化聚焦弹窗容器时须 `outline:none`（否则浏览器默认 focus ring 会在玻璃边缘画出一圈白线）。
6. **性能**：滑块输入只渲染，落盘走 200ms 防抖；提供性能模式关闭实时模糊。
7. **提交**：每次改动完成即提交并推送，提交信息用 `feat: / fix: / chore:` 前缀说明改了什么。

---

## 版本记录

| 版本 | 内容 |
| --- | --- |
| 1.5.0 | 参数模型改为「偏移制」：全部 19 个滑块默认 0，可左右拖动调正负，实际值 = 基准 + 偏移并夹紧；深浅两套偏移独立；滑块轨道加中点标记；文案与 README 同步 |
| 1.4.0 | 基础组件补齐：Toast 四态、居中确认弹窗（支持输入）、空状态、列表项、徽标、分割线、点击复制；设置页分组折叠、显示模式快切、字体三选、分主题重置、滑动切页开关、导出备注；组件预览区同步展示 |
| 1.3.2 | 7 项优化（搜索直达、深浅双套、壁纸独立存储、写入防抖、单项重置、通知权限、减弱动效）、切换闪动修复、账户可编辑、负悬浮高度、完整弹窗交互、按钮体系、组件预览 |
| 1.3.1 | 设置页独立视图、壁纸与预设、性能模式 |
| 1.3.0 | 玻璃参数面板 → 独立设置页 |
