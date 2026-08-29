# ghostty-config

本项目为 [Ghostty](https://ghostty.org/) 终端模拟器在 macOS 平台下的个人配置文件。配置以自用习惯为导向，兼顾中文字体渲染、主题自适应、窗口外观与操作效率。全部内容集中于 `config.ghostty` 单一文件，可直接使用。

## 功能特性

- 字体使用 JetBrainsMono Nerd Font Mono，并针对中文字符设置 PingFang SC 回退，改善中文渲染的清晰度。
- 主题随系统深浅模式自动切换（浅色使用 Catppuccin Latte，深色使用 Catppuccin Mocha），窗口主题亦自动跟随。
- 窗口采用透明毛玻璃效果，支持自定义内边距。
- 光标样式为竖线且带闪烁，滚动缓冲区设置为 50000 行。
- 输入状态下自动隐藏鼠标，选取即复制到剪贴板。
- 开启剪贴板粘贴保护及相关安全功能。
- 自动检测并集成 zsh / fish 等 Shell。
- 快捷键布局参考 iTerm2 使用习惯，并配置 macOS Option 键作为 Alt 使用，便于按词跳转。

## 目录结构

```
ghostty-config
├── config.ghostty   # Ghostty 主配置文件
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

2. 将 `config.ghostty` 复制到 Ghostty 的配置目录：

   ```bash
   cp config.ghostty ~/.config/ghostty/config
   ```

3. 重载配置。在 Ghostty 中执行快捷命令 `Cmd + Shift + ,` 即可生效。

## 验证配置

```bash
ghostty +validate-config
```

## 主题查看

```bash
ghostty +list-themes
```

## 许可证

本项目以 MIT 许可证发布，详见 [LICENSE](LICENSE) 文件。
