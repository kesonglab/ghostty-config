# 变更记录

本文档记录本工程的规范与实现变更。版本号遵循 [Semantic Versioning](https://semver.org/lang/zh-CN/)，记录结构参考 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/)。

自 v1.0.0 起，「与 Kaku 0.30 默认配置的一致性」为本工程的规范性目标：配置变更须同步更新 README 第 3 节的逐项映射与第 4 节的验证记录。

## [1.0.0] - 2026-11-26

### 摘要

首个规范冻结版本（specification freeze），属破坏性主版本。以 Kaku 0.30 应用内置默认配置为唯一事实来源，重建配色、排版、窗口、终端行为与键位配置。在 31 个可识别配置语义条目中，13 项为等价映射、11 项为近似映射、1 项为保留偏离、6 项为不可映射。

### 规范变更（按维度）

- **配色系统**：新增 Kaku Dark 与 Kaku Light 用户主题文件，覆盖背景、前景、光标、选中与 ANSI 0–15；以 `theme = light:Kaku Light,dark:Kaku Dark` 随系统外观切换；半透明选中色按 alpha 合成规则换算为等效不透明色 `#584384`。
- **排版系统**：字族改为 JetBrains Mono，字号 17 pt，行高通过 `adjust-cell-height = 28%` 表达；关闭编程连字；斜体保持直立；粗体采用 Medium 字重。
- **窗口系统**：内边距按物理像素与 point 换算为 `20` 与 `20,0`；初始窗口 110 × 22；标题栏改为透明样式并保留系统红绿灯；保留毛玻璃（`background-opacity = 0.9`、`background-blur = 18`），为本版本唯一保留偏离。
- **终端行为**：光标改为竖线闪烁；滚动缓冲改为 10000 行并解除字节上限；关闭滚动条；启用选中即复制；选择分词字符、最小对比度、非焦点分屏、关闭确认与 Option 键对齐 Kaku。
- **键位系统**：新增或覆盖 12 条绑定，包括 `cmd+r`、`cmd+shift+w`、`cmd+shift+↑`、`cmd+shift+i`、`cmd+enter`、`shift+enter`、`opt+backspace`、`cmd+ctrl+方向键` 与全局 Quick Terminal 绑定。
- **移除项**：Catppuccin 主题、`faint-opacity`、`mouse-scroll-multiplier`、`working-directory`、`shell-integration-features` 显式列表与 iTerm2 习惯的标签切换键位等非 Kaku 默认配置。

### 工程与验证

- 持续集成对主配置与两个主题文件执行 `ghostty +validate-config`；主题文件缺失时校验返回非零，避免配色静默回退。
- 渲染测量：单元格 20 × 58 设备像素（2× 屏幕）；ANSI 0–15 与主题文件逐项一致（偏差不超过 1/255）；深浅色切换正常。
- 文档：markdownlint 通过；新增 `CITATION.cff` 与预览截图 `assets/preview-dark.png`、`assets/preview-light.png`。

### 迁移

- 破坏性变更：自定义主题须安装至 `~/.config/ghostty/themes/`；字号、行高、主题与多数键位与 0.5.0 不兼容。
- 旧版个性化配置建议以追加覆盖项表达，而非修改基准配置段；亦可检出 `v0.5.0` 回退。

### 早期版本

| 版本 | 日期 | 摘要 |
| :--- | :--- | :--- |
| [v0.5.0](https://github.com/kesonglab/ghostty-config/releases/tag/v0.5.0) | 2026-09-16 | 增加 `ssh-terminfo`；`copy-on-select` 改为 `true`；滚动条跟随系统；补充分屏与跳提示符键位；Quick Terminal 全局绑定 |
| [v0.4.0](https://github.com/kesonglab/ghostty-config/commit/2f44687) | 2026-09-15 | 分屏 `unfocused-split-opacity = 0.85`；补充字体排查文档；修正 `working-directory` |
| [v0.3.0](https://github.com/kesonglab/ghostty-config/commit/31dff68) | 2026-09-14 | 可读性优化：`minimum-contrast = 3`、`faint-opacity = 0.7`；字体回退改为多行列表 |
| [v0.2.1](https://github.com/kesonglab/ghostty-config/commit/d054d22) | 2026-09-04 | 补充中文字体回退文档 |
| [v0.2.0](https://github.com/kesonglab/ghostty-config/commit/710eedd) | 2026-09-01 | 新增 `working-directory` 默认工作目录 |
| [v0.1.0](https://github.com/kesonglab/ghostty-config/commit/aa363a3) | 2026-09-01 | 首次发布；修正 `scrollback-limit` 单位；加入分屏键位 |
| [v0.0.1](https://github.com/kesonglab/ghostty-config/commit/0abd5a4) | 2026-08-29 | 初始版本：字体、Catppuccin 深浅主题、毛玻璃 |

0.x 版本未打标签，链接指向引入对应版本记录的提交。

[1.0.0]: https://github.com/kesonglab/ghostty-config/compare/v0.5.0...v1.0.0
