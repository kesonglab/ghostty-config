# Changelog

本文件记录本工程的所有显著变更。格式参考 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/)，版本号遵循[语义化版本](https://semver.org/lang/zh-CN/)。

自 **v1.0.0** 起，本工程将「与 Kaku 的配置一致性」作为规范性目标：任何配置变更都应同步更新 README 的映射表（表 2）与验证章节，并通过 CI 的配置校验。

## [1.0.0] - 2026-11-26

首个规范冻结版本（major release）。以 Kaku 0.30 内置默认配置（`/Applications/Kaku.app/Contents/Resources/kaku.lua`）为唯一基准，重建配置、主题、CI 与文档，并在 Ghostty 1.4.0 / macOS 27 上完成静态与渲染两级验证。

### 配置（Changed）

- **主题**：由 Catppuccin Latte / Mocha 改为 Kaku Light / Kaku Dark；新增两个自定义主题文件，以 `theme = light:Kaku Light,dark:Kaku Dark` 随系统深浅色切换。
- **字体**：由 JetBrainsMono Nerd Font Mono 14 pt 改为 Kaku 的 JetBrains Mono 17 pt；`adjust-cell-height` 由 8% 改为 28%（对应 Kaku `line_height = 1.28`）；新增关闭连字（`-calt,-clig,-liga`）、直立斜体（`font-style-italic = Regular`）与 Medium 粗体（`font-style-bold = Medium`）。
- **窗口**：内边距由 12 / 10 改为 Kaku 的 20（左右）与 20,0（上下），关闭 padding 自动均衡；初始窗口 110 × 22；标题栏改为透明样式 + 系统红绿灯；保留毛玻璃（`background-opacity = 0.9`、`background-blur = 18`），作为唯一刻意偏离基准的配置项。
- **交互**：`copy-on-select = clipboard`、`mouse-hide-while-typing = true`、`selection-word-chars`、`minimum-contrast = 3`、`unfocused-split-opacity = 1`、`confirm-close-surface = true`、`macos-option-as-alt = left`、`scrollbar = never`。
- **滚动**：`scrollback-limit-lines = 10000`（对齐 Kaku `scrollback_lines`），`scrollback-limit-bytes = unlimited` 使行数成为唯一限制。
- **移除**：`faint-opacity`、`mouse-scroll-multiplier`、`working-directory`、`shell-integration-features` 显式列表、`clipboard-paste-*` 显式声明等非 Kaku 默认项，以及 iTerm2 习惯的 `cmd+shift+left/right` 标签切换。

### 键位（Added）

- 新增 `cmd+r`（清屏并清空滚动缓冲，Kaku 兼容绑定）、`cmd+shift+w`（关闭标签页）、`cmd+shift+↑`（窗口置顶）、`cmd+shift+i`（重命名标签页）、`cmd+enter` / `shift+enter`（换行不执行）、`opt+backspace`（删除前一个词）、`cmd+ctrl+方向键`（调整分屏尺寸，步长 5）。
- Quick Terminal 映射 Kaku 全局抽屉：`quick-terminal-position = top`、高度 1/3、200 ms 动画、失焦自动隐藏，全局快捷键 `cmd+opt+ctrl+k`。
- 新增注释示例 `title = " "`：隐藏 Ghostty 标题栏文字以更接近 Kaku 的观感（默认未启用）。

### 主题（Added）

- `themes/Kaku Dark`、`themes/Kaku Light`：Kaku 内置调色板的背景、前景、光标、选中与 ANSI 0–15 全量移植；半透明选中色 `rgba(142,106,217,0.55)` 按 alpha 合成规则换算为 `#584384`。

### 验证与 CI（Verification）

- 配置与两个主题文件均通过 `ghostty +validate-config`（0 error）。
- 渲染实测：单元格 20 × 58 设备像素（@2×）与 Kaku 一致；Kaku Dark / Light 调色板经 sRGB 转换后逐项一致（±1 舍入）；深浅色自动切换正常。
- CI 增加主题目录准备与主题文件独立校验（缺少主题时校验返回非零，防止回退默认配色未被发现）。

### 文档（Docs）

- README 重写为「摘要—方法—规范—验证—限制—参考」结构，新增映射类型分类、表 1–3 与复现命令。
- 新增 `CITATION.cff` 引用元数据；CHANGELOG 重写并压缩 v0.0.1–v0.5.0 的历史条目（见「版本历史」）。

### 迁移（Migration）

- **破坏性变更**：自定义主题必须安装（`~/.config/ghostty/themes/`），否则配色回退 Ghostty 默认；字号、行高、内边距、主题与多数键位与 0.5.0 不兼容。
- 若仍需 0.5.0 的个人定制（如 `working-directory`），建议在 `config.ghostty` 末尾追加覆盖项，而非修改基准段。
- 回退：`git checkout v0.5.0` 并重新安装旧配置。

## 版本历史（0.0.1–0.5.0）

> 0.x 版本未打标签，链接指向引入对应版本记录的提交；v1.0.0 起恢复「标签 + GitHub Release」发行方式。

| 版本 | 日期 | 摘要 |
| --- | --- | --- |
| [v0.5.0](https://github.com/kesonglab/ghostty-config/releases/tag/v0.5.0) | 2026-09-16 | 增加 `ssh-terminfo`；`copy-on-select` 改 `true`；滚动条改为跟随系统；显式声明分屏与跳提示符键位；Quick Terminal 全局绑定 |
| [v0.4.0](https://github.com/kesonglab/ghostty-config/commit/2f44687) | 2026-09-15 | 分屏 `unfocused-split-opacity = 0.85`；README 增加字体未激活排查；修正 `working-directory` 路径 |
| [v0.3.0](https://github.com/kesonglab/ghostty-config/commit/31dff68) | 2026-09-14 | 可读性优化：`background-opacity = 0.9`、`minimum-contrast = 3`、`faint-opacity = 0.7`；字体回退改为多行列表；CI 升级 |
| [v0.2.1](https://github.com/kesonglab/ghostty-config/commit/d054d22) | 2026-09-04 | 补充中文字体回退文档；主配置未改动 |
| [v0.2.0](https://github.com/kesonglab/ghostty-config/commit/710eedd) | 2026-09-01 | 新增 `working-directory` 默认工作目录 |
| [v0.1.0](https://github.com/kesonglab/ghostty-config/commit/aa363a3) | 2026-09-01 | 首次开源发布；修正 `scrollback-limit` 单位；加入 iTerm2 习惯的分屏键位 |
| [v0.0.1](https://github.com/kesonglab/ghostty-config/commit/0abd5a4) | 2026-08-29 | 初始版本：字体、Catppuccin 深浅主题、毛玻璃、光标/滚动、粘贴保护、Shell 集成 |

[1.0.0]: https://github.com/kesonglab/ghostty-config/compare/v0.5.0...v1.0.0
