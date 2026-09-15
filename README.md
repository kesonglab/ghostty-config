# ghostty-config

[![CI](https://github.com/kesonglab/ghostty-config/actions/workflows/ci.yml/badge.svg)](https://github.com/kesonglab/ghostty-config/actions/workflows/ci.yml)

本仓库是 [Ghostty](https://ghostty.org/) 终端模拟器在 macOS 下的个人配置文件，也是一份可直接照抄的配置示例。全部设置集中在 `config.ghostty` 单一文件，随用随改。更新历史见 [CHANGELOG.md](CHANGELOG.md)。

## 功能特性

### 外观

- 字体：JetBrainsMono Nerd Font Mono，中文依次回退 PingFang SC、Hiragino Sans GB（macOS 自带，毛玻璃/Retina 下渲染不发虚）；新窗口/分屏继承当前字号
- 主题随系统深浅模式自动切换：浅色 Catppuccin Latte、深色 Catppuccin Mocha
- 毛玻璃窗口（`background-opacity = 0.9`）；`minimum-contrast = 3` 与 `faint-opacity = 0.7` 保证注释、dim 文本可读
- 支持自定义内边距

### 交互

- 光标为竖线带闪烁；滚动缓冲区 `scrollback-limit = 10000000`（按字节计，限 10MB）
- 输入时自动隐藏鼠标、选取即复制；触控板滚动速度微调
- 剪贴板粘贴保护开启

### 工作区

- Shell 集成自动检测 zsh / fish，窗口标题随当前命令/目录更新
- 新窗口/标签默认打开到 `~/Documents/github-kesonglab`（`working-directory`）
- 快捷键布局参考 iTerm2 习惯，含 **Tab 与分屏操作**；macOS Option 键作为 Alt，便于按词跳转

### 快捷键速览（iTerm2 习惯）

| 快捷键 | 动作 |
| --- | --- |
| `cmd + t` / `cmd + w` | 新建 / 关闭 Tab |
| `cmd + shift + left / right` | 上一个 / 下一个 Tab |
| `cmd + d` / `cmd + shift + d` | 新建右侧 / 底部（上下）分屏 |
| `cmd + alt + 方向键` | 在分屏间切换 |
| `cmd + shift + ,` | 重载配置 |

> 其中 `cmd+t` / `cmd+w` / `cmd+d` / `cmd+shift+d` / `cmd+alt+方向键` 本就是 Ghostty 默认绑定，列在此处仅为显式声明。

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

`~/.config/ghostty/config` 同样可读，但 Ghostty 只读取**第一个存在**的文件，多份配置不会合并（确需合并可用 `config-file` 显式 include）。macOS 优先 Library 路径，本机也用 `Cmd + ,` 维护这份，故推荐前者。

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

### 中文字体回退

主配置用 `font-family` 列表做自动回退：英文走 JetBrainsMono Nerd Font Mono，中文依次落到 macOS 自带的 PingFang SC、Hiragino Sans GB（渲染不发虚）。列表写法需要 Ghostty 1.2+，本仓库按 1.3 维护。

如果你装了字体优化工具、或在 macOS Sequoia 之后某些精简镜像里发现 PingFang SC 不存在、中文显示为方框 □，把列表里的字体名换成下表任选一种已确认存在的即可：

| 字体名 | 适用场景 | 风格 |
| --- | --- | --- |
| `PingFang SC` | macOS 原生自带（默认） | 圆润，半弧线，发虚感最低 |
| `Hiragino Sans GB` | macOS 必带（GB 简体字符集） | 接近 PingFang，字符覆盖率足够日常 |
| `STSong` / `SimSun` | 需手动安装宋体后填入 | 衬线，传统印刷风 |
| `Noto Sans CJK SC` | `brew install --cask font-noto-sans-cjk` | 开源，覆盖最全 |

> **验证方法**：在 Ghostty 里执行 `echo 测试中文 👑`，若仍出现方框说明字体名未识别，可用 `ghostty +list-fonts | grep -i <font>` 查真实可用的字体名。

### 字体文件在，但系统没激活

macOS 下把字体拷进 `~/Library/Fonts` 后，偶尔 `fontd` 不会立即登记，导致 Ghostty 静默回退到默认字体（如 Menlo）——新装机、迁移数据后尤其常见。排查与修复：

```bash
# 查看系统已激活的字族里有没有目标字体（输出为空 = 没激活）
osascript -l JavaScript -e 'ObjC.import("AppKit"); console.log(ObjC.deepUnwrap($.NSFontManager.sharedFontManager.availableFontFamilies).filter(function(f){return /jetbrains/i.test(f);}))'

# 重启字体服务，触发重扫
killall fontd
```

`ghostty +list-fonts` 只在字体已激活后才会列出对应字族，可用来二次确认。

## 许可证

MIT，详见 [LICENSE](LICENSE)。
