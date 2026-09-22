# 贡献指南

感谢你考虑为本 skill 贡献！🎉

## 这是什么

这是一个 **agent skill** —— 不是命令行工具，而是让 AI agent 按 procedure 自动执行任务的协议文档。
核心是 `SKILL.md`：frontmatter 决定何时触发，Procedure 写执行步骤，Output contract 定输出格式，
Failure handling 记踩过的坑。

不绑定任何 agent，所以正文里不出现任何 runtime 的名字、专有工具名或专有安装路径——
那些只写在 README 里。

## 如何贡献

### 提 Issue

- 🐛 **Bug**：复现步骤、预期行为、实际行为、终端输出
- 💡 **新场景**：比如同时支持 Sony / Insta360 相机？
- 📖 **文档不清楚**：哪里读起来别扭？

### 改 PR

1. Fork 这个仓库
2. 改 `SKILL.md`（核心）或 `README.md` / `CHANGELOG.md`
3. 跑一遍你自己的场景，确认 procedure 还 work
4. 提交 PR，标题写清楚改了什么

### PR 评审重点

- **description 是否清晰**：触发条件 + 不该触发的相邻场景，不要塞步骤细节或工具名
- **步骤是否可执行**：祈使句要能让另一个模型照做，且带退出条件
- **有没有硬编码假设**：路径、时区、后缀、工具名写死都算缺陷，
  要写成「先探测，探测不出就问用户」
- **破坏性操作前是否确认**：删文件、覆盖同名文件都要显式问，并在 Failure handling 留兜底
- **是否与 agent 无关**：`grep -niE 'dsh|mavis|minimax|claude|codex|cursor' SKILL.md` 应无输出

## 开发约定

- 文件用 UTF-8，缩进用 2 空格
- 链接用相对路径
- 中文标点用全角，英文标点用半角
- 正文里的命令要**实际跑过**再写进去，别凭印象抄 flag

## 不收什么

- ❌ 改成要 `pip install` 的 Python 包 —— 它只是个 markdown 协议
- ❌ 加 CI / 测试覆盖 —— skill 没有代码可测，验证靠真实场景跑一遍
- ❌ 改 frontmatter 的 `name` —— 改了之后所有引用都会断

## 许可

贡献的代码同样按 MIT 协议发布。
