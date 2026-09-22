# Changelog

All notable changes to this skill are documented here. Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [1.2.0] - 2026-09-22

### Changed
- **不再限定 `DJI_001`**：扫描目标改为「用户指定的目录，未指定则当前项目目录」，
  `YYYYMMDD/` 建在该目录下。素材可以直接散在目标目录里，也可以分散在 `DJI_001` / `DJI_002`
  等多个编号子目录中——**递归**扫描全树并汇总，多个编号目录不再需要逐个询问
- 每个文件按 `find` 递归收集，归档根的「来源分布」会写进计划表
- frontmatter `description` 与 README 同步改为不限定目录的说法
- Procedure 重排为 9 步

### Added
- **无日期文件的处理**：文件名读不出日期时，先让同名 sidecar 继承主体日期；非 DJI 格式但含
  `YYYYMMDD` 的按该日期归档并用元数据校验；完全读不出日期的用元数据 `creation_time` / EXIF
  定日期并标注为「由元数据推断」；元数据也没有时让用户选「跳过 / 移到 `未标注日期/` /
  用文件系统时间」。此前这类文件一律被当作异常丢弃不归档
- 识别**伪日期**（如 `DJI_20261332_...`）：`case` 模式只保证 8 位数字，需额外校验月日合法性
- **防止二次归档**：递归扫描时剪掉所有 `YYYYMMDD` 形式的目录，避免重跑时把归档产物再归档一次
- **先报计划再动文件**：归档计划表（每个日期目录的文件数 + 大小）+ 用户确认
- **同名冲突检查**：列出冲突清单让用户选跳过 / 加后缀 / 覆盖，绝不静默覆盖
- **跨卷与磁盘空间检查**：跨卷改用 `rsync -a --remove-source-files --ignore-existing`
- **字节数校验**：除文件数外用 `du -sk` 校验字节数
- 每步的**退出条件**
- sidecar（`.SRT` / `.THM`）跟随主体归档
- 依赖缺失时的降级路径表

### Fixed
- **越界搜索**：补上「目标里没有可归档素材」的规则。此前只规定了「目标不存在/不可读 → 停下」，
  没有覆盖「目标可读但没有素材」，导致执行时会自行去翻别的目录、别的挂载卷找素材。
  现在明确：只在扫描目标这一棵树里找，没有就地停下报告，别处的路径只能作为**建议**列出；
  空手而归是合法结果。frontmatter 也补了「只用于归档已有素材，不用于找素材」。
- 去掉对特定 agent 运行时的依赖：正文不再出现专有工具名或安装路径
- 时区不再写死「UTC + 8h」：改为探测候选偏移、取与文件名日期一致率最高者，
  一致率必须 100% 才继续；跨 DST 用整小时 `-v+8H` 而非 `-v+480M`
- `dot_clean -m` 会递归且默认只合并不删除 `._*`，改为 `dot_clean -fm -- <日期目录>`
- `trash` 默认「部分文件删不掉也返回成功」，需加 `-s` 才能暴露失败；
  且 macOS 自带的 `trash` **不认 `--`**（`trash -s -- "$f"` 会去找字面名为 `--` 的文件），已改正
- `mv` 示例补双引号（处理含空格/中文的路径），用 `-n` 避免静默覆盖
- 不再假设后缀固定为 `_D`，兼容 `_001` / `_002` 等

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

[Unreleased]: https://github.com/song52wow/dji-media-organizer/compare/6c93c51...HEAD
[1.2.0]: https://github.com/song52wow/dji-media-organizer/compare/719be028...6c93c51
[1.1.0]: https://github.com/song52wow/dji-media-organizer/compare/9e4ae13b...719be028
[1.0.0]: https://github.com/song52wow/dji-media-organizer/commit/9e4ae13b
