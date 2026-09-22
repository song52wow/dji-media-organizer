# DJI Media Organizer

一个 Mavis skill：把 DJI Osmo / Action 等设备导出的原始素材按实际拍摄日期整理为 `YYYYMMDD` 命名的子目录，移动到相机所在的项目根目录。

每次执行前会询问是否清理 `.LRF` 低分辨率预览文件。

## 适用场景

- 你把 SD 卡插到 Mac，挂载后目录长这样：`/Volumes/<设备名>/DJI_001/` 里面有几百个 `DJI_20260920..._D.MP4`
- 你希望把它们按拍摄日期归档成 `20260920/`、`20260921/` 这种目录，放在 SD 卡根目录而不是 `DJI_001/` 内
- 你不再需要 `.LRF`（DJI Mimo APP 用的 720p 预览文件，对长期归档没用）

## 安装

### 方式一：让 LLM 自动安装（推荐）

把下面这段话直接复制发给一个 AI 编程助手（**Mavis**、Claude Code、Codex、Cursor、Aider 等任何一个能执行 shell 命令的都行），它会自动帮你装好：

---

请帮我从 GitHub 安装 Mavis skill `dji-media-organizer`：

1. 在临时目录克隆仓库（macOS / Linux）：
   `git clone https://github.com/song52wow/dji-media-organizer.git /tmp/dji-media-organizer`

   Windows PowerShell 等价：
   `git clone https://github.com/song52wow/dji-media-organizer.git "$env:TEMP\dji-media-organizer"`

2. 确保 Mavis 扫描目录存在（macOS / Linux）：
   `mkdir -p ~/.minimax/skills`

   Windows：
   `New-Item -ItemType Directory -Force -Path "$HOME\.minimax\skills"`

3. 复制到 Mavis 扫描路径下（macOS / Linux）：
   `cp -r /tmp/dji-media-organizer ~/.minimax/skills/dji-media-organizer`

   Windows：
   `Copy-Item -Recurse -Force "$env:TEMP\dji-media-organizer" "$HOME\.minimax\skills\dji-media-organizer"`

4. 验证安装成功：
   `ls -la ~/.minimax/skills/dji-media-organizer/SKILL.md`
   （Windows：`Get-Item "$HOME\.minimax\skills\dji-media-organizer\SKILL.md"`）

5. 清理临时目录：`rm -rf /tmp/dji-media-organizer`（Windows：`Remove-Item -Recurse -Force "$env:TEMP\dji-media-organizer"`）

**完成后回报**：
- 目标文件 `~/.minimax/skills/dji-media-organizer/SKILL.md` 是否存在
- 该文件首行是否包含 `name: dji-media-organizer`（说明 frontmatter 正确）
- 提醒我下次启动 Mavis 即可用「扫描 DJI_001」激活

**重要约束**：
- 任何一步失败立即停下来报告，不要继续
- 不要修改 SKILL.md 的内容
- 不要把仓库克隆到 `~/.minimax/skills/dji-media-organizer`（避免覆盖已有内容；如果你要这样做，请先确认该目录不存在）

---

### 方式二：手动安装（不通过 LLM）

如果你想自己动手，按顺序做三步：

1. 在终端进入你想放仓库的目录，运行 `git clone https://github.com/song52wow/dji-media-organizer.git`，把仓库克隆到当前文件夹。
2. 创建 `~/.minimax/skills` 目录（如果已经存在就跳过这步）。
3. 把克隆下来的 `dji-media-organizer` 整个文件夹复制（或移动）到 `~/.minimax/skills/` 下。

完成后下次启动 Mavis 时，它会自动扫描到 `~/.minimax/skills/dji-media-organizer/SKILL.md` 并加载。

### 方式三：手动复制 `SKILL.md`

只需要 `SKILL.md` 一个文件。把它放到任何 Mavis 能扫描到的 skill 目录下即可。无需 `git clone`，也无需复制整个仓库。

## 使用

启动 Mavis 后，说以下任意一句即可激活：

- "扫描 DJI_001"
- "整理 DJI 素材"
- "把 DJI_001 按日期分类"
- "按年月日归档 DJI 文件"

skill 会自动：

1. 找到 `<项目根>/DJI_001/`（可能还有 `DJI_002`、`DJI_003`）
2. 扫描 LRF 文件，弹卡片问你怎么处理（删除 / 归档 / 取消）
3. 用 `ffprobe` / `sips` 校验每个文件的实际拍摄时间
4. 在项目根建 `YYYYMMDD/` 日期目录
5. 把 MP4 / JPG 按拍摄日期移到对应目录
6. 清理 macOS 的 `._` AppleDouble 影子文件
7. 输出最终报告（每个目录文件数 + 大小 + 总数 + LRF 处置摘要）

## 工作流示例

```
原始（SD 卡挂载点 /Volumes/dji 共享给我/DJI Device/OsmoNano-CBF0/）：

  DJI_001/
    DJI_20260920150156_0001_D.MP4
    DJI_20260920170715_0002_D.MP4
    ...
    DJI_20260920150156_0001_D.LRF
    DJI_20260920170715_0002_D.LRF
    ...

整理后：

  OsmoNano-CBF0/
    20260920/
      DJI_20260920150156_0001_D.MP4
      DJI_20260920170715_0002_D.MP4
      ...
    20260921/
      ...
    DJI_001/        ← 清空，只剩 .DS_Store
```

## 触发短语

| 你说的 | 是否会激活 |
|---|---|
| 扫描 DJI_001 | ✅ |
| 整理 DJI 素材 | ✅ |
| 把 DJI_001 按日期分类 | ✅ |
| 按年月日归档 DJI | ✅ |
| DJI 文件按日期分类 | ✅ |
| 帮我清理一下相机素材 | ✅（隐含） |

## 不会触发的场景

- ❌ 视频转码 / 剪辑（用专门的视频处理工具）
- ❌ 素材去重（用 dedup 工具）
- ❌ 非 DJI 相机且文件名不带 `YYYYMMDD_HHMMSS` 的素材
- ❌ 上传到云盘

## 系统依赖

| 工具 | 用途 |
|---|---|
| `ffprobe` | 读 MP4 元数据中的拍摄时间 |
| `sips` | 读 JPG EXIF 中的拍摄时间（macOS 自带） |
| `dot_clean` | 清理 macOS `._` 影子文件（macOS 自带） |
| SSH 客户端 | 跟 Mavis 自己的工具链 |

如果 `ffprobe` 不存在，可以用 Homebrew 安装：`brew install ffmpeg`。

## 许可

MIT License — 见 [LICENSE](./LICENSE)。