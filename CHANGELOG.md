# Changelog

本项目的所有显著变更记录在此文件。
格式基于 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/)，
版本号遵循 [Semantic Versioning](https://semver.org/lang/zh-CN/)。

## [未发布]

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

[未发布]: https://github.com/kesonglab/ghostty-config/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/kesonglab/ghostty-config/releases/tag/v0.1.0
[0.0.1]: https://github.com/kesonglab/ghostty-config/releases/tag/v0.0.1
