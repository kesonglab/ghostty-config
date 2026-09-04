# Changelog

本项目的所有显著变更记录于此。
格式基于 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/)，版本号遵循 [Semantic Versioning](https://semver.org/lang/zh-CN/)。

## [0.2.1] - 2026-09-04

### 新增

- README「中文字体回退」小节：列出 `PingFang SC`、`Hiragino Sans GB`、`Noto Sans CJK SC` 等可选回退字体及适用场景，并给出 Ghostty 1.2+ 推荐的 `font-family` 列表写法。当系统未装 PingFang SC（精简镜像/字体清理工具）导致中文显示为方框时，可按表替换。

### 说明

- 仓库 `config.ghostty` 主配置**未改动**，仍以 PingFang SC 为默认；该次更新仅补全文档。

## [0.2.0] - 2026-09-01

### 新增

- `working-directory = "~/Documents/github:kesonglab"`：新窗口/标签默认打开到该工作目录。路径含冒号（`github:kesonglab`），值用引号包裹以避免解析歧义。

## [0.1.0] - 2026-09-01

Ghostty macOS 配置文件首次开源发布。

### 修复

- `scrollback-limit` 单位更正：Ghostty 按**字节**计（非行数），由误设的 `50000`（50KB）修正为 `10000000`（10MB）。此前滚动历史几乎无法保留。

### 新增

- **分屏快捷键（iTerm2 习惯）**：
  - `cmd+d` 新建右侧分屏，`cmd+shift+d` 新建底部（上下）分屏
  - `cmd+alt+方向键` 在分屏间切换
- **Shell 集成增加 `title` 特性**：窗口标题随当前命令/目录更新（此前仅有 `cursor,sudo,ssh-env,path`）
- `window-inherit-font-size = true`：新窗口/分屏继承当前窗口字号
- `mouse-scroll-multiplier = precision:1.5`：触控板滚动速度微调

### 说明

- macOS 实际生效配置位于 `~/Library/Application Support/com.mitchellh.ghostty/config.ghostty`
- 本文件（`config.ghostty`）为 GitHub 开源备份，与本机生效配置保持同步

## [0.0.1] - 2026-08-29

### 新增

- 初始版本：字体（JetBrainsMono Nerd Font + 中文 PingFang SC 回退）、Catppuccin 深浅主题自适应、毛玻璃透明窗口、光标/滚动、粘贴保护、Shell 集成、iTerm2 Tab 快捷键、Option 键作为 Alt

[0.2.0]: https://github.com/kesonglab/ghostty-config/compare/v0.1.0...v0.2.0
[0.1.0]: https://github.com/kesonglab/ghostty-config/releases/tag/v0.1.0
[0.0.1]: https://github.com/kesonglab/ghostty-config/releases/tag/v0.0.1
