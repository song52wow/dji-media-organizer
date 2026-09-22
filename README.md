# DJI Media Organizer

一个 Mavis skill：把 DJI Osmo / Action 等设备导出的原始素材按实际拍摄日期整理为 `YYYYMMDD` 命名的子目录，移动到相机所在的项目根目录。

每次执行前会询问是否清理 `.LRF` 低分辨率预览文件。

## 适用场景

- 你把 SD 卡插到 Mac，挂载后目录长这样：`/Volumes/<设备名>/DJI_001/` 里面有几百个 `DJI_20260920..._D.MP4`
- 你希望把它们按拍摄日期归档成 `20260920/`、`20260921/` 这种目录，放在 SD 卡根目录而不是 `DJI_001/` 内
- 你不再需要 `.LRF`（DJI Mimo APP 用的 720p 预览文件，对长期归档没用）

## 安装

### 方式一：放进 Mavis user skills 目录（推荐）

```bash
git clone https://github.com/song52wow/dji-media-organizer.git
mkdir -p ~/.minimax/skills
cp -r dji-media-organizer ~/.minimax/skills/
```

下次启动 Mavis 就会自动加载 `dji-media-organizer` skill。

### 方式二：手动复制 `SKILL.md`

只需要 `SKILL.md` 一个文件。把它放到任何 Mavis 能扫描到的 skill 目录下即可。

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