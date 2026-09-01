# ghostty-config

本仓库是 [Ghostty](https://ghostty.org/) 终端模拟器在 macOS 下的个人配置文件，也是一份可直接照抄的配置示例。全部设置集中在 `config.ghostty` 单一文件，随用随改。更新历史见 [CHANGELOG.md](CHANGELOG.md)。

## 功能特性

### 外观

- 字体：JetBrainsMono Nerd Font Mono；中文字符回退 PingFang SC，解决中文渲染发虚；新窗口/分屏继承当前字号
- 主题随系统深浅模式自动切换：浅色 Catppuccin Latte、深色 Catppuccin Mocha
- 透明毛玻璃窗口，支持自定义内边距

### 交互

- 光标为竖线带闪烁；滚动缓冲区 `scrollback-limit = 10000000`（按字节计，限 10MB）
- 输入时自动隐藏鼠标、选取即复制；触控板滚动速度微调
- 剪贴板粘贴保护开启

### 工作区

- Shell 集成自动检测 zsh / fish，窗口标题随当前命令/目录更新
- 新窗口/标签默认打开到 `~/Documents/github:kesonglab`（`working-directory`）
- 快捷键布局参考 iTerm2 习惯，含 **Tab 与分屏操作**；macOS Option 键作为 Alt，便于按词跳转

### 快捷键速览（iTerm2 习惯）

| 快捷键 | 动作 |
| --- | --- |
| `cmd + t` / `cmd + w` | 新建 / 关闭 Tab |
| `cmd + shift + left / right` | 上一个 / 下一个 Tab |
| `cmd + d` / `cmd + shift + d` | 新建右侧 / 底部（上下）分屏 |
| `cmd + alt + 方向键` | 在分屏间切换 |
| `cmd + shift + ,` | 重载配置 |

## CI 自动化检查

通过 [GitHub Actions](https://github.com/kesonglab/ghostty-config/actions)，每次 push / PR 自动运行：

- **validate-config**：macOS 上 `ghostty +validate-config --config-file=config.ghostty`，校验语法与键值（配置为 macOS 专属，故用 macOS runner）
- **markdownlint**：检查 `*.md` 是否符合规范（见 `.markdownlint-cli2.jsonc`）

合入前确保 CI 全部通过。每处配置改动前应先本地 `ghostty +validate-config` 自检。

## 目录结构

```
ghostty-config
├── config.ghostty    # 主配置
├── CHANGELOG.md      # 更新日志
├── README.md
├── .github/workflows/ci.yml   # CI
├── .markdownlint-cli2.jsonc   # markdownlint 规则
├── .gitignore
└── LICENSE
```

## 使用

### 安装字体

```bash
brew install --cask font-jetbrains-mono-nerd-font
```

### 复制配置到生效路径

macOS 上生效路径为 `~/Library/Application Support/com.mitchellh.ghostty/config.ghostty`：

```bash
cp config.ghostty "$HOME/Library/Application Support/com.mitchellh.ghostty/config.ghostty"
```

`~/.config/ghostty/config` 同样可读，两路径并存会合并——本机用 `Cmd + ,` 维护 Library 路径那份，故推荐前者。

### 重载配置

Ghostty 内按 `Cmd + Shift + ,` 即刻生效。

### 验证配置

```bash
# CLI 未在 PATH 时先建软链
ln -s /Applications/Ghostty.app/Contents/MacOS/ghostty /usr/local/bin/ghostty

ghostty +validate-config
```

### 查看主题

```bash
ghostty +list-themes
```

## 许可证

MIT，详见 [LICENSE](LICENSE)。
