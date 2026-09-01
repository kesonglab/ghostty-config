# ghostty-config

本项目为 [Ghostty](https://ghostty.org/) 终端模拟器在 macOS 平台下的个人配置文件。配置以自用习惯为导向，兼顾中文字体渲染、主题自适应、分屏操作效率、窗口外观与安全。全部内容集中于 `config.ghostty` 单一文件，可直接使用。更新历史见 [CHANGELOG.md](CHANGELOG.md)。

## 功能特性

- 字体使用 JetBrainsMono Nerd Font Mono，并针对中文字符设置 PingFang SC 回退，改善中文渲染的清晰度；新窗口/分屏继承当前字号（`window-inherit-font-size`）。
- 主题随系统深浅模式自动切换（浅色使用 Catppuccin Latte，深色使用 Catppuccin Mocha），窗口主题亦自动跟随。
- 窗口采用透明毛玻璃效果，支持自定义内边距。
- 光标样式为竖线且带闪烁，滚动缓冲区设置为 10MB（`scrollback-limit`，按字节计）。
- 输入状态下自动隐藏鼠标，选取即复制到剪贴板。
- 触控板滚动速度微调（`mouse-scroll-multiplier`）。
- 开启剪贴板粘贴保护及相关安全功能。
- 自动检测并集成 zsh / fish 等 Shell，窗口标题随当前命令/目录更新（`title` 特性）。
- 快捷键布局参考 iTerm2 使用习惯，包含 **Tab 与分屏操作**，并配置 macOS Option 键作为 Alt 使用，便于按词跳转。

### 快捷键速览（iTerm2 习惯）

| 快捷键 | 动作 |
| --- | --- |
| `cmd + t` / `cmd + w` | 新建 / 关闭 Tab |
| `cmd + shift + left / right` | 上一个 / 下一个 Tab |
| `cmd + d` / `cmd + shift + d` | 新建右侧 / 底部（上下）分屏 |
| `cmd + alt + 方向键` | 在分屏间切换 |
| `cmd + shift + ,` | 重载配置 |

## 目录结构

```
ghostty-config
├── config.ghostty   # Ghostty 主配置文件
├── CHANGELOG.md     # 更新日志
├── .gitignore
├── LICENSE
└── README.md
```

## 环境要求

- macOS 操作系统
- Ghostty 终端模拟器
- 字体 JetBrainsMono Nerd Font（推荐通过 Homebrew 安装）

## 安装步骤

1. 安装所需字体：

   ```bash
   brew install --cask font-jetbrains-mono-nerd-font
   ```

2. 将 `config.ghostty` 复制到 Ghostty 的配置目录。macOS 上实际生效路径为：

   ```bash
   cp config.ghostty "$HOME/Library/Application Support/com.mitchellh.ghostty/config.ghostty"
   ```

   > 也可以使用 `~/.config/ghostty/config`，两个路径 Ghostty 都会读取；若都存在会合并。因本机使用 `Cmd + ,` 维护的是 Library 路径那份，故推荐前者。注意 `Cmd + ,`（配合 `Cmd + Shift + ,` 重载）编辑的即是生效文件。

3. 重载配置。在 Ghostty 中执行快捷命令 `Cmd + Shift + ,` 即可生效。

## 验证配置

```bash
# 若 CLI 未在 PATH，先建立软链：
ln -s /Applications/Ghostty.app/Contents/MacOS/ghostty /usr/local/bin/ghostty

ghostty +validate-config
```

## 主题查看

```bash
ghostty +list-themes
```

## 许可证

本项目以 MIT 许可证发布，详见 [LICENSE](LICENSE) 文件。