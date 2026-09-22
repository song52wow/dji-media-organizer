# Changelog

All notable changes to this skill are documented here. Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [1.1.0] - 2026-09-22

### Added
- Step 3: 每次执行前弹 ask_user 询问是否删除 `.LRF` 低分辨率预览文件（必问，不能跳过）
- 文档里加了 macOS AppleDouble 影子文件清理步骤

### Changed
- README 安装段落回滚为最初版本（带 bash 代码块）
- 验证流程：先用 `ffprobe` / `sips` 校验实际拍摄时间，再决定归档目录

### Notes
- 项目从 `~/.minimax/skills/` 拆出来作为独立 repo（`/Users/hari/Project/dji-media-organizer/`）
- GitHub 仓库地址：https://github.com/song52wow/dji-media-organizer

## [1.0.0] - 2026-09-20

### Added
- 初始版本：扫描 `DJI_001/`，按拍摄日期归档成 `YYYYMMDD/` 子目录
- 支持 MP4 / JPG
- 通过 `ffprobe` 读取 MP4 元数据中的 `creation_time`，加 8 小时换算到本地（CST）
- 通过 `sips` 读取 JPG EXIF
- macOS 清理 `._` AppleDouble 影子文件
- 处理 SD 卡挂载在 `/Volumes/.../DJI Device/<设备名>/` 下的常见路径

[1.1.0]: https://github.com/song52wow/dji-media-organizer/compare/9e4ae13b...719be028
[1.0.0]: https://github.com/song52wow/dji-media-organizer/commit/9e4ae13b