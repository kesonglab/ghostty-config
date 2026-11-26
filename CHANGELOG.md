# Changelog

本项目的所有重要变更记录于此。格式参考 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/)，版本号遵循 [Semantic Versioning](https://semver.org/lang/zh-CN/)。

## [1.0.0] - 2026-11-26

Kaku 0.30 默认配置移植版：主题、字体、窗口、交互与键位对齐 Kaku 默认值，并保留 Ghostty 毛玻璃模糊。

### 新增

- `themes/Kaku Dark`、`themes/Kaku Light`：完整 Kaku 调色板（背景、前景、光标、选中、ANSI 0–15），随系统深浅色切换
- Kaku 键位：`cmd+r` 清屏、`cmd+shift+w` 关标签、`cmd+shift+↑` 置顶、`cmd+shift+i` 重命名标签、`cmd+enter` / `shift+enter` 换行不执行、`opt+backspace` 删词、`cmd+ctrl+方向键` 调整分屏
- Quick Terminal 映射 Kaku 全局抽屉：顶部、1/3 高度、200 ms、失焦隐藏，快捷键 `cmd+opt+ctrl+k`
- CI 校验两个主题文件；新增 `CITATION.cff` 与预览截图 `assets/preview-dark.png`、`assets/preview-light.png`

### 变更

- 主题由 Catppuccin Latte / Mocha 改为 Kaku Dark / Kaku Light
- 字体由 14 pt 改为 JetBrains Mono 17 pt，行高由 8% 改为 28%，关闭连字、直立斜体、Medium 粗体
- 内边距改为 20 / 20,0，初始窗口 110 × 22，标题栏改为透明样式，保留系统红绿灯
- 交互对齐 Kaku：选中即复制、选择分词字符、最小对比度 3、非焦点分屏不压暗、关闭确认、Option 左 Meta
- 滚动缓冲改为 10000 行，并解除字节上限
- 保留毛玻璃：`background-opacity = 0.9`、`background-blur = 18`

### 移除

- Catppuccin 主题、`faint-opacity`、`mouse-scroll-multiplier`、`working-directory`、iTerm2 习惯的标签切换键位等非 Kaku 默认项

### 说明

- 破坏性变更：需安装主题文件；字号、行高、主题与多数键位与 0.5.0 不兼容，迁移与回退见发行说明
- 验证：主配置 + 两个主题文件 `ghostty +validate-config` 均 0 error；单元格 20 × 58 设备像素；ANSI 调色板与 Kaku 一致（±1 舍入）

## 早期版本

| 版本 | 日期 | 摘要 |
| :--- | :--- | :--- |
| [v0.5.0](https://github.com/kesonglab/ghostty-config/releases/tag/v0.5.0) | 2026-09-16 | 增加 `ssh-terminfo`；`copy-on-select` 改 `true`；滚动条跟随系统；补充分屏与跳提示符键位；Quick Terminal 全局绑定 |
| [v0.4.0](https://github.com/kesonglab/ghostty-config/commit/2f44687) | 2026-09-15 | 分屏 `unfocused-split-opacity = 0.85`；字体排查文档；修正 `working-directory` |
| [v0.3.0](https://github.com/kesonglab/ghostty-config/commit/31dff68) | 2026-09-14 | 可读性优化：`minimum-contrast = 3`、`faint-opacity = 0.7`；字体回退改为多行列表 |
| [v0.2.1](https://github.com/kesonglab/ghostty-config/commit/d054d22) | 2026-09-04 | 补充中文字体回退文档 |
| [v0.2.0](https://github.com/kesonglab/ghostty-config/commit/710eedd) | 2026-09-01 | 新增 `working-directory` 默认工作目录 |
| [v0.1.0](https://github.com/kesonglab/ghostty-config/commit/aa363a3) | 2026-09-01 | 首次发布；修正 `scrollback-limit` 单位；加入分屏键位 |
| [v0.0.1](https://github.com/kesonglab/ghostty-config/commit/0abd5a4) | 2026-08-29 | 初始版本：字体、Catppuccin 深浅主题、毛玻璃 |

> 0.x 版本未打标签，链接指向引入对应版本记录的提交。

[1.0.0]: https://github.com/kesonglab/ghostty-config/compare/v0.5.0...v1.0.0
