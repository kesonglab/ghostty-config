# ghostty-config

[![CI](https://github.com/kesonglab/ghostty-config/actions/workflows/ci.yml/badge.svg)](https://github.com/kesonglab/ghostty-config/actions/workflows/ci.yml)
[![Release](https://img.shields.io/github/v/release/kesonglab/ghostty-config)](https://github.com/kesonglab/ghostty-config/releases)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![macOS 13+](https://img.shields.io/badge/macOS-13%2B-black)](https://www.apple.com/macos/)
[![Ghostty 1.4+](https://img.shields.io/badge/Ghostty-1.4%2B-8a2be2)](https://ghostty.org/)

> 把 [Kaku](https://github.com/tw93/Kaku) 的终端体验搬到 [Ghostty](https://ghostty.org/)：Kaku Dark / Kaku Light 配色、JetBrains Mono 17 pt 与 1.28 行高、Kaku 默认键位与交互，同时保留 macOS 毛玻璃模糊。

![Kaku Dark / Kaku Light 预览](assets/preview.png)

## ✨ 特性

- 🎨 **两套主题**：Kaku Dark / Kaku Light 跟随 macOS 深浅色自动切换，ANSI 0–15 调色板逐项移植
- 🔤 **Kaku 字体排版**：JetBrains Mono 17 pt、行高 1.28、关闭连字、直立斜体、Medium 粗体，中文回退 PingFang SC
- ⌨️ **Kaku 键位**：`Cmd + T/D/W`、`Cmd + Opt + 方向键`、`Cmd + Enter` 换行不执行、`Opt + 方向键` 按词跳转，开箱即用
- 🪟 **Kaku 式窗口**：20 pt 内边距、110 × 22 初始尺寸、透明标题栏 + 系统红绿灯、10000 行滚动缓冲
- 🧊 **毛玻璃**：保留 Ghostty 的高斯模糊（`background-opacity = 0.9`、`background-blur = 18`）
- ✅ **可验证**：CI 对主配置与两个主题文件执行 `ghostty +validate-config`，并运行 markdownlint

## 🚀 快速开始

需要 **Ghostty 1.4+** 与 **macOS 13+**。

```bash
# 1. 安装 Ghostty 与字体
brew install --cask ghostty
brew install --cask font-jetbrains-mono-nerd-font

# 2. 安装主配置
cp config.ghostty "$HOME/Library/Application Support/com.mitchellh.ghostty/config.ghostty"

# 3. 安装主题（Ghostty 只从该目录读取自定义主题）
mkdir -p "$HOME/.config/ghostty/themes"
cp themes/"Kaku Dark" themes/"Kaku Light" "$HOME/.config/ghostty/themes/"

# 4. 校验并在 Ghostty 内按 Cmd + Shift + , 热重载
ghostty +validate-config
```

`ghostty` 不在 `PATH` 时先建软链：

```bash
ln -s /Applications/Ghostty.app/Contents/MacOS/ghostty /usr/local/bin/ghostty
```

> 截图使用不透明背景以方便对比配色；默认配置保留毛玻璃模糊。

## 🎨 主题

| | Kaku Dark | Kaku Light |
| :--- | :--- | :--- |
| 背景 | `#15141b` | `#fffcf0` |
| 前景 | `#d5d4d6` | `#100f0f` |
| 光标 | `#8e6ad9` | `#343331` |
| 选中 | `#584384` | `#e8e6db` |

- 主题文件位于 `themes/`，文件名即主题名，需与 `theme = light:Kaku Light,dark:Kaku Dark` 完全一致（含空格与大小写）
- Kaku 的选中色为 `rgba(142,106,217,0.55)`，Ghostty 主题不支持 alpha，按 Kaku Dark 背景合成成 `#584384`
- 自定义主题目录固定在 `~/.config/ghostty/themes/`，**不是** `~/Library/Application Support/com.mitchellh.ghostty/themes/`

## ⌨️ 快捷键

| 快捷键 | 动作 |
| :--- | :--- |
| `Cmd + T` / `Cmd + W` / `Cmd + N` | 新建标签 / 关闭当前分屏 / 新建窗口 |
| `Cmd + Shift + W` | 关闭当前标签页 |
| `Cmd + 1..8` / `Cmd + 9` | 切到第 N 个标签 / 最后一个标签 |
| `Cmd + Shift + [` / `]` | 上一个 / 下一个标签 |
| `Cmd + D` / `Cmd + Shift + D` | 左右分屏 / 上下分屏 |
| `Cmd + Alt + 方向键` | 在分屏间跳转 |
| `Cmd + Ctrl + 方向键` | 调整分屏尺寸（步长 5） |
| `Cmd + Shift + Enter` | 最大化 / 还原当前分屏 |
| `Cmd + Shift + ↑` | 当前窗口置顶 |
| `Cmd + Ctrl + F` | 全屏 |
| `Cmd + K` / `Cmd + R` | 清屏并清空滚动缓冲 |
| `Cmd + Enter` / `Shift + Enter` | 换行但不执行 |
| `Opt + ←/→` / `Cmd + ←/→` | 按词跳转 / 跳到行首行尾 |
| `Cmd + Backspace` / `Opt + Backspace` | 删除到行首 / 删除前一个词 |
| `Cmd + Shift + I` | 重命名标签页 |
| `Cmd + Opt + Ctrl + K` | 全局呼出 / 收起 Quick Terminal |
| `Cmd + Shift + ,` | 重载配置 |

## 🔧 配置速览

| 项目 | 取值 |
| :--- | :--- |
| 主题 | `theme = light:Kaku Light,dark:Kaku Dark` |
| 字体 | `font-family = JetBrains Mono`（回退 Nerd Font / PingFang SC）、`font-size = 17` |
| 行高与连字 | `adjust-cell-height = 28%`、`font-feature = -calt,-clig,-liga` |
| 窗口 | `window-padding-x = 20`、`window-padding-y = 20,0`、`window-width = 110`、`window-height = 22` |
| 光标 | `cursor-style = bar`、`cursor-style-blink = true` |
| 滚动 | `scrollback-limit-lines = 10000`、`scrollbar = never` |
| 鼠标与选择 | `copy-on-select = clipboard`、`mouse-hide-while-typing = true` |
| 可读性 | `minimum-contrast = 3`、`unfocused-split-opacity = 1` |
| 关闭保护 | `confirm-close-surface = true` |
| 毛玻璃 | `background-opacity = 0.9`、`background-blur = 18` |

所有配置项的原理与对照都写在 [`config.ghostty`](config.ghostty) 的注释里。

## 🛠 自定义

在 `config.ghostty` 末尾追加覆盖项即可，例如恢复不透明背景：

```ini
background-opacity = 1
background-blur = false
```

隐藏标题栏文字（更接近 Kaku 的观感，代价是 Mission Control 与标签页失去标题）：

```ini
title = " "
```

低分辨率屏幕可把字号改回 Kaku 的 15 pt：

```ini
font-size = 15
```

## 🧊 与 Kaku 的差异

- **标签栏**：Kaku 是底部自绘标签栏；Ghostty 仅支持原生 / 标题栏标签（单标签时自动隐藏）。配置层无法实现底部标签栏，需要改 Ghostty 源码。
- **标题文字**：Kaku 标题栏只有红绿灯；Ghostty 默认显示窗口标题，可用上面的 `title = " "` 隐藏。
- **主题字重与颜色映射**：Kaku 按深浅色切换字重（Regular/Medium 与 Medium/SemiBold）、并用 `color_overrides` 把 ANSI 黑/白按前景与背景分别映射；Ghostty 无对应机制，本配置按深色主题对齐。
- **Shell 提示符**：Kaku 自带托管 zsh（语法高亮、自动建议、自定义提示符）；Ghostty 只注入 shell integration，提示符沿用你现有的 `.zshrc`。
- **Kaku 专属功能**：AI Chat、lazygit、yazi、远程文件、Doctor、标签导航等无 Ghostty 对应功能。

## 🧪 验证

- `ghostty +validate-config`：主配置与两个主题文件均为 0 error（CI 同步执行）
- 渲染实测：单元格 20 × 58 设备像素（@2×）、Kaku Dark/Light 调色板逐项一致（±1 舍入）、深浅色自动切换正常
- 文档：markdownlint 0 issue

## 📁 项目结构

```text
ghostty-config
├── config.ghostty          # 主配置（Kaku 0.30 → Ghostty 1.4）
├── themes/
│   ├── Kaku Dark           # Kaku 深色主题
│   └── Kaku Light          # Kaku 浅色主题
├── assets/preview.png      # README 预览图
├── CHANGELOG.md            # 版本记录
├── CITATION.cff            # 引用元数据
├── .github/workflows/ci.yml
├── .markdownlint-cli2.jsonc
├── LICENSE
└── README.md
```

## 📚 参考

- [Kaku](https://github.com/tw93/Kaku)：配色与默认行为的来源
- [Ghostty](https://ghostty.org/docs/config/reference)：配置参考
- [JetBrains Mono](https://www.jetbrains.com/lp/mono/) 与 [Nerd Fonts](https://www.nerdfonts.com/)
- 引用本工程请使用 [`CITATION.cff`](CITATION.cff)；完整变更见 [CHANGELOG.md](CHANGELOG.md)

## 📄 License

[MIT](LICENSE) © kesonglab
