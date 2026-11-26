# ghostty-config

[![CI](https://github.com/kesonglab/ghostty-config/actions/workflows/ci.yml/badge.svg)](https://github.com/kesonglab/ghostty-config/actions/workflows/ci.yml)
[![Release](https://img.shields.io/github/v/release/kesonglab/ghostty-config)](https://github.com/kesonglab/ghostty-config/releases)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

> **摘要**：本工程面向 macOS 上的 [Ghostty](https://ghostty.org/) 终端，以 [Kaku](https://github.com/tw93/Kaku) 0.30 的内置默认配置为唯一事实来源，通过逐项语义映射，将主题、字体度量、窗口内外边距、光标、滚动、鼠标、关闭保护与键位迁移到 Ghostty 1.4 的配置模型；同时给出静态校验、渲染测量与已知差异分析。当前版本 **v1.0.0**。

## 1. 引言

Kaku 是基于 [WezTerm](https://wezfurlong.org/wezterm/) 的 macOS 终端发行版，其默认配置以 Lua 形式随应用发布，涵盖配色方案、字体栈、窗口布局与键位等；Ghostty 是采用平台原生 UI 与 GPU 渲染的终端模拟器，配置模型与之不同，但提供了覆盖面相近的公开配置项。

本工程的研究问题是：**在不修改 Ghostty 源码的前提下，其公开配置能力能在多大程度上复现 Kaku 的默认终端体验？** 为回答这一问题，本工程将 Kaku 0.30 的内置默认配置（`/Applications/Kaku.app/Contents/Resources/kaku.lua`）逐项映射至 Ghostty 配置语汇，并对映射结果进行分类、验证与记录：

- **等价映射**：语义与数值可直接对应，如滚动缓冲行数、选中即复制、关闭确认；
- **近似映射**：语义可对齐但单位或实现不同，如物理像素内边距到 point、行高倍数到单元格高度百分比；
- **不可映射**：Ghostty 不提供对应能力，如底部自绘标签栏、WezTerm 的前景/背景分离色映射（`color_overrides`）。

该分类构成第 5 节差异分析的基础。工程遵循三条设计原则：以 Kaku 内置默认配置为唯一基准、不引入 Kaku 未声明的个人偏好、Ghostty 默认值已一致者不重复声明。

## 2. 方法

### 2.1 事实来源

- **基准配置**：Kaku 0.30 应用内 `Resources/kaku.lua`（随应用发布，不受用户覆盖层影响）；
- **规范文档**：Kaku 官方 configuration / keybindings 文档，以及 Ghostty 1.4.0 的 `ghostty +show-config --docs` 与源码；
- **渲染实测**：Ghostty 1.4.0 在目标机器上的真实窗口截图与像素测量（见第 3 节）。

### 2.2 主题移植

Kaku 内置两套调色板（Kaku Dark / Kaku Light）。本工程将背景、前景、光标、选中与 ANSI 0–15 全量移植到 Ghostty 主题文件 `themes/Kaku Dark` 与 `themes/Kaku Light`，并以 `theme = light:Kaku Light,dark:Kaku Dark` 实现随 macOS 深浅色自动切换。

Kaku 的选中背景为半透明紫 `rgba(142,106,217,0.55)`；Ghostty 主题不接受 alpha 通道，故按 alpha 合成规则在 Kaku Dark 背景 `#15141b` 上求得等效不透明色 `#584384`：

```text
R = 0.55 × 142 + 0.45 × 21 ≈ 88  (#58)
G = 0.55 × 106 + 0.45 × 20 ≈ 67  (#43)
B = 0.55 × 217 + 0.45 × 27 ≈ 132 (#84)
```

### 2.3 字体与度量

Kaku 在应用内内置 JetBrains Mono，并按屏幕分辨率自适应 15 pt / 17 pt；中文回退 PingFang SC，符号回退 Apple Symbols。本工程取目标机器（高分屏）对应的 17 pt，并按下述方式对齐度量：

- **行高**：Kaku `line_height = 1.28` → Ghostty `adjust-cell-height = 28%`；
- **字体回退链**：`JetBrains Mono` → `JetBrainsMono Nerd Font Mono` → `PingFang SC` → `Apple Symbols`（已安装原版时用原版，否则自动落到 Nerd Font 版）；
- **排版特性**：关闭 `calt` / `clig` / `liga` 连字（Kaku `harfbuzz_features`），斜体保持直立（Kaku 显式禁用斜体），粗体使用 Medium 字重（对齐 Kaku 深色主题）。

在上述参数下，理论单元格为 10 × 29 pt，即 2× 屏幕上的 20 × 58 设备像素，与 Kaku 实测一致（见 3.2 节）。

### 2.4 窗口与装饰

Kaku 的内边距以物理像素声明（左右上 40 px、下 0），Ghostty 以 point 声明，故在 2× 屏幕上换算为 `window-padding-x = 20`、`window-padding-y = 20,0`；初始窗口对齐 Kaku 的 110 列 × 22 行；标题栏采用透明样式并保留系统红绿灯。

**唯一刻意偏离基准的配置项**是窗口透明与模糊：Kaku 默认不透明（`window_background_opacity = 1.0`），本工程保留 Ghostty 的毛玻璃高斯模糊（`background-opacity = 0.9`、`background-blur = 18`），以保留 Ghostty 的视觉特性。

### 2.5 键位映射

按与 Ghostty 默认绑定的关系将键位分为三类：Ghostty 默认已与 Kaku 一致者不重复声明；冲突者以 Kaku 行为覆盖；Kaku 独有而 Ghostty 无对应动作者记为不可映射（见 5.3 节）。表 1 列出其中节选。

表 1　键位映射（节选）

| 快捷键 | 行为 | 映射方式 |
| --- | --- | --- |
| `cmd + t` / `cmd + w` / `cmd + n` | 新标签 / 关闭当前分屏 / 新窗口 | Ghostty 默认 |
| `cmd + shift + w` | 关闭当前标签页 | 覆盖为 `close_tab` |
| `cmd + 1..8` / `cmd + 9` | 第 N 个标签 / 最后一个标签 | Ghostty 默认 |
| `cmd + d` / `cmd + shift + d` | 左右分屏 / 上下分屏 | Ghostty 默认 |
| `cmd + alt + 方向键` | 在分屏间跳转 | Ghostty 默认 |
| `cmd + ctrl + 方向键` | 调整分屏尺寸（步长 5） | 覆盖为 `resize_split:*` |
| `cmd + shift + ↑` | 当前窗口置顶 | 覆盖为 `toggle_window_float_on_top` |
| `cmd + k` / `cmd + r` | 清屏并清空滚动缓冲 | 默认 / 补映射 `clear_screen` |
| `cmd + enter` / `shift + enter` | 换行但不执行 | 补映射 `text:\n` |
| `opt + ←` / `→` | 按词跳转 | Ghostty 默认 |
| `cmd + ←` / `→` | 跳到行首 / 行尾 | Ghostty 默认 |
| `cmd + backspace` / `opt + backspace` | 删除到行首 / 删除前一个词 | 默认 / 补映射 |
| `cmd + shift + i` | 重命名标签页 | 补映射 `prompt_tab_title` |
| `cmd + opt + ctrl + k` | 全局呼出 / 收起抽屉终端 | 补映射 Quick Terminal |

完整键位以 `config.ghostty` 内注释为准，维度级对照见表 2。

### 2.6 实现规范

表 2　配置映射总览

| 维度 | Kaku 0.30 默认 | 本工程（Ghostty 1.4） | 映射类型 |
| --- | --- | --- | --- |
| 主题 | Kaku Dark / Kaku Light 跟随系统 | `theme = light:Kaku Light,dark:Kaku Dark` | 等价 |
| 字体 | JetBrains Mono 17 pt，PingFang SC 回退 | `font-family` 列表 + `font-size = 17` | 等价 |
| 行高 | `line_height = 1.28` | `adjust-cell-height = 28%` | 近似（单位换算） |
| 连字 / 斜体 / 粗体 | 关闭连字、直立斜体、深色 Medium | `font-feature` / `font-style-*` | 等价 |
| 内边距 | 左右上 40 px、下 0 | `window-padding-x = 20`、`window-padding-y = 20,0` | 近似（单位换算） |
| 初始窗口 | 110 列 × 22 行 | `window-width = 110`、`window-height = 22` | 等价 |
| 标题栏 | 内嵌红绿灯、无标题文字 | `macos-titlebar-style = transparent` | 近似（见 5.1） |
| 光标 | 竖线闪烁，2 px，500 ms | `cursor-style = bar`、`cursor-style-blink = true` | 近似（无粗细/间隔） |
| 滚动缓冲 | 10000 行 | `scrollback-limit-lines = 10000`、`scrollback-limit-bytes = unlimited` | 等价 |
| 滚动条 | 关闭 | `scrollbar = never` | 等价 |
| 选中即复制 | 开 | `copy-on-select = clipboard` | 等价 |
| 选择分词字符 | 空格 / `\t` / `\n` / `{}[]()"'-` | `selection-word-chars` | 等价 |
| 最小对比度 | 3.0 | `minimum-contrast = 3` | 等价 |
| 非焦点分屏 | 不压暗 | `unfocused-split-opacity = 1` | 等价 |
| 关闭确认 | `SmartPrompt` | `confirm-close-surface = true` | 近似 |
| Option 键 | 左 Meta、右 Compose | `macos-option-as-alt = left` | 等价 |
| 会话恢复 | `restore_previous_session = true` | `window-save-state = always` | 近似 |
| 全局抽屉 | Cmd + Opt + Ctrl + K，顶部 1/3 | Quick Terminal（`quick-terminal-*`） | 近似 |
| 毛玻璃模糊 | 无（不透明） | `background-opacity = 0.9`、`background-blur = 18` | 保留偏离 |

## 3. 验证

### 3.1 静态验证

配置与主题均通过 Ghostty 1.4.0 的配置校验器：

```bash
ghostty +validate-config --config-file=config.ghostty
ghostty +validate-config --config-file="themes/Kaku Dark"
ghostty +validate-config --config-file="themes/Kaku Light"
```

CI 在 macOS runner 上执行同一组校验（先将 `themes/` 复制到 `~/.config/ghostty/themes/`，否则主题解析失败会使校验返回非零），并以 markdownlint 检查文档。

### 3.2 渲染一致性测量

**测量方法**：在目标机器（Apple Silicon，2× 屏幕，macOS 27）上以 Ghostty 1.4.0 运行本配置，截图后由 Display P3 转换至 sRGB，再对 ANSI 背景色块与全块字符（U+2588）进行像素采样，测单元格几何与调色板。

表 3　渲染测量结果

| 指标 | Kaku 0.30 实测 | Ghostty 1.4.0 实测 | 结论 |
| --- | --- | --- | --- |
| 单元格（设备像素 @2×） | 20 × 58 | 20 × 58 | 一致 |
| 背景 / 前景（Kaku Dark） | `#15141b` / `#d5d4d6` | `#15141b` / `#d5d4d6` | 一致 |
| ANSI 0–15 调色板 | 见主题文件 | 与主题文件逐项一致（±1 舍入） | 一致 |
| 深浅色自动切换 | 跟随 macOS 外观 | 跟随 macOS 外观 | 一致 |
| 主题解析 | — | `+list-themes` 可见两个用户主题 | 通过 |

### 3.3 复现

```bash
brew install --cask ghostty
ghostty +version                     # 需 ≥ 1.4.0
ghostty +validate-config --config-file=config.ghostty
ghostty +list-themes | grep Kaku     # 需先安装主题文件
```

## 4. 使用

### 4.1 环境要求

- macOS 13 及以上（本工程验证于 macOS 27 / Apple Silicon）；
- Ghostty 1.4.0 及以上（依赖 `scrollback-limit-lines`、`selection-word-chars`、`font-style-*`）；
- [JetBrains Mono Nerd Font](https://www.nerdfonts.com/)（或原版 JetBrains Mono）。

### 4.2 安装

```bash
# 1. 安装 Ghostty 与字体
brew install --cask ghostty
brew install --cask font-jetbrains-mono-nerd-font

# 2. 安装主配置（macOS 生效路径）
cp config.ghostty "$HOME/Library/Application Support/com.mitchellh.ghostty/config.ghostty"

# 3. 安装主题（Ghostty 自定义主题只从该目录读取）
mkdir -p "$HOME/.config/ghostty/themes"
cp themes/"Kaku Dark" themes/"Kaku Light" "$HOME/.config/ghostty/themes/"
```

### 4.3 校验与重载

```bash
ghostty +validate-config
```

随后在 Ghostty 内按 `Cmd + Shift + ,` 热重载。`ghostty` 不在 `PATH` 时可建立软链：

```bash
ln -s /Applications/Ghostty.app/Contents/MacOS/ghostty /usr/local/bin/ghostty
```

> 注意：主配置的生效路径在 `~/Library/Application Support/com.mitchellh.ghostty/`，而自定义主题目录固定在 `~/.config/ghostty/themes`；两处需分别安装。

## 5. 已知限制

### 5.1 架构性差异（不可映射）

- **标签栏**：Kaku 为自绘底部标签栏（单标签自动隐藏、可显示提醒圆点）；Ghostty macOS 端标签栏仅支持原生 / 标题栏两种位置，`gtk-tabs-location` 仅 Linux，且无自绘窗口 UI 的扩展机制。配置层无法实现底部标签栏，如需一致只能修改 Ghostty 源码（macOS Swift 前端）。
- **标题文字**：Kaku 标题栏只有红绿灯；Ghostty 透明标题栏仍显示窗口标题。可通过 `title = " "` 隐藏（代价是 Mission Control 与标签页失去标题），默认未启用。
- **ANSI 黑语义**：Kaku Dark 将 ANSI 0 前景设为浅色，并以 `color_overrides` / `foreground_color_overrides` 将黑色背景、ANSI 7/15 白底等映射回深色；Ghostty 无「同一颜色按前景/背景分别映射」的机制，故以 ANSI 黑/白作背景的应用（如 Claude Code 的引用块）观感不同。
- **Shell 提示符**：Kaku 托管 zsh（`~/.config/kaku/zsh`，内置语法高亮、自动建议、补全与自定义 starship 提示符）；Ghostty 仅注入 shell integration，提示符沿用用户既有配置。

### 5.2 参数性差异（近似映射）

- 光标粗细（2 px）与闪烁间隔（500 ms）无对应配置，仅支持闪烁开关；
- 字号与内边距按屏幕自适应（15 pt / 26 px 与 17 pt / 40 px）在静态配置中固定为 17 pt / 20 pt；
- 分屏间隙（`split_pane_gap = 2`）无对应配置；
- 会话恢复（`restore_previous_session`）以 `window-save-state = always` 近似；
- 智能关闭（`cmd + w` 按「分屏 → 标签 → 隐藏应用」）以关闭当前分屏/窗口近似；
- 字重随主题切换（深色 Regular/Medium、浅色 Medium/SemiBold）无法复现，按深色主题对齐。

### 5.3 功能性差异（不可映射）

Kaku 专属能力在 Ghostty 中无对应动作，包括 AI Chat（`cmd + l`）、AI 设置（`cmd + shift + a`）、修复建议（`cmd + shift + e`）、lazygit（`cmd + shift + g`）、yazi（`cmd + shift + y`）、远程文件（`cmd + shift + r`）、Doctor（`ctrl + shift + l`）、标签导航（`cmd + shift + o`；Ghostty 的 `toggle_tab_overview` 仅 Linux）与窗口快照恢复（`cmd + opt + shift + t`）。

## 6. 仓库结构

```text
ghostty-config
├── config.ghostty              # 主配置（Kaku 0.30 → Ghostty 1.4 映射）
├── themes/
│   ├── Kaku Dark               # Kaku 深色主题（bg/fg/cursor/selection/ANSI 0–15）
│   └── Kaku Light              # Kaku 浅色主题
├── CHANGELOG.md                # 版本记录（v1.0.0 起为规范冻结版本）
├── CITATION.cff                # 引用元数据
├── README.md                   # 本文件（方法、规范、验证与限制）
├── .github/workflows/ci.yml    # CI：配置/主题校验与 markdownlint
├── .markdownlint-cli2.jsonc    # markdownlint 规则
├── .gitignore
└── LICENSE
```

## 7. 参考文献

1. tw93. *Kaku: a fast, out-of-the-box terminal built for AI coding* [EB/OL]. <https://github.com/tw93/Kaku>.
2. Ghostty. *Configuration Reference* [EB/OL]. <https://ghostty.org/docs/config/reference>.
3. Ghostty. *Ghostty v1.4.0 source* [EB/OL]. <https://github.com/ghostty-org/ghostty/tree/v1.4.0>.
4. WezTerm. *Configuration* [EB/OL]. <https://wezfurlong.org/wezterm/config/>.
5. W3C. *Web Content Accessibility Guidelines (WCAG) 2.0* [S]. 2008.
6. JetBrains. *JetBrains Mono* [EB/OL]. <https://www.jetbrains.com/lp/mono/>.
7. Nerd Fonts. *Nerd Fonts* [EB/OL]. <https://www.nerdfonts.com/>.
8. Keep a Changelog. <https://keepachangelog.com/>；Semantic Versioning. <https://semver.org/>.

若在研究中引用本工程，请使用仓库中的 [`CITATION.cff`](CITATION.cff)。

## 8. 许可

本项目采用 MIT 许可，详见 [LICENSE](LICENSE)。
