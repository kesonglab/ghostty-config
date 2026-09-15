# Changelog

本项目的所有显著变更记录于此。
格式基于 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/)，版本号遵循 [Semantic Versioning](https://semver.org/lang/zh-CN/)。

## [0.4.0] - 2026-09-15

### 新增

- 分屏：新增 `unfocused-split-opacity = 0.85`（默认 0.7），非焦点分屏变暗幅度更小，便于分辨焦点。
- README 新增「字体文件在，但系统没激活」小节：字体放入 `~/Library/Fonts` 后若 `fontd` 未登记，Ghostty 会静默回退默认字体；给出检测方法与 `killall fontd` 修复。

### 修复

- `working-directory` 路径更正：由 `~/Documents/github:kesonglab` 改为实际存在的 `~/Documents/github-kesonglab`，并同步更新 README。
- `working-directory` 补注释：目录必须存在，否则新窗口会落到 `/` 而非 `~`（本次「默认目录显示 `/`」即由此引起）。
- README 更正：Ghostty 只读取**第一个存在**的配置文件，多份配置不会合并。

### 文档

- 说明 `cmd+t`、`cmd+w`、`cmd+d`、`cmd+shift+d`、`cmd+alt+方向键` 等本已为 Ghostty 默认快捷键，配置中保留仅为显式声明。

## [0.3.0] - 2026-09-14

### 改进

- **可读性**：`background-opacity` 由 `0.72` 提到 `0.9`，减少桌面内容透出对文字对比度的稀释。
- 新增 `minimum-contrast = 3`：自动抬升过淡的前景。内置调色板里 Mocha 的 black/bright black 对比度仅 1.80:1 / 2.46:1，Latte 的 white/bright white 仅 1.91:1 / 1.61:1，注释、dim 提示、git 高亮常踩这几个槽位。
- 新增 `faint-opacity = 0.7`，dim 文本不再那么糊。
- 中文字体回退改用 `font-family` 列表（`JetBrainsMono Nerd Font Mono` → `PingFang SC` → `Hiragino Sans GB`），替换原 `font-codepoint-map` 单行写法，回退范围更通用。
- `adjust-cell-height` 由 `4` 改为 `8%`，行间距更宽，长文本更好扫读。

### 变更

- `confirm-close-surface` 由 `false` 改为 `true`：有进程运行时关闭窗口会先确认，避免误杀 build / ssh。
- CI 的 actions 升级到 Node.js 24 兼容版本（`actions/checkout@v7`、`markdownlint-cli2-action@v24`），消除弃用警告。

> 说明：macOS 下改 `background-opacity` 需完全退出 Ghostty 重启，`Cmd + Shift + ,` 热重载不生效。

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

[0.4.0]: https://github.com/kesonglab/ghostty-config/compare/v0.3.0...v0.4.0
[0.3.0]: https://github.com/kesonglab/ghostty-config/compare/v0.2.1...v0.3.0
[0.2.1]: https://github.com/kesonglab/ghostty-config/compare/v0.2.0...v0.2.1
[0.2.0]: https://github.com/kesonglab/ghostty-config/compare/v0.1.0...v0.2.0
[0.1.0]: https://github.com/kesonglab/ghostty-config/releases/tag/v0.1.0
[0.0.1]: https://github.com/kesonglab/ghostty-config/releases/tag/v0.0.1
