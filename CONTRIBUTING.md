# 贡献指南

感谢你考虑为本 skill 贡献！🎉

## 这是什么

这是一个 [Mavis](https://github.com) skill —— 不是普通的命令行工具，而是一个让 AI agent 自动按照 procedure 执行任务的协议文档（`SKILL.md`）。

skill 的核心是 `SKILL.md`：
- **frontmatter**：告诉 LLM 何时触发这个 skill
- **Procedure**：分步骤的执行规则
- **Output contract**：期望的输出格式
- **Failure handling**：失败时怎么办

## 如何贡献

### 提 Issue

- 🐛 **Bug**：复现步骤、预期行为、实际行为、终端输出、macOS 版本
- 💡 **新场景**：你的 DJI 工作流有什么需求？比如同时支持 Sony / Insta360 相机？
- 📖 **文档不清楚**：哪里读起来别扭？

### 改 PR

1. Fork 这个仓库
2. 改 `SKILL.md`（核心）或 `README.md` / `CHANGELOG.md`
3. 跑一遍你自己的场景验证 procedure 还 work
4. 提交 PR，标题写清楚改了什么

### PR 评审重点

我会重点看：

- **frontmatter 的 description 是否清晰**：触发条件 + 不该触发的相邻场景
- **procedure 步骤是否可执行**：每步的祈使句都要能让另一个模型照做
- **failure handling 是否覆盖**：踩过的坑有没有写进 Failure handling 防止下次再踩
- **风格一致**：跟现有 SKILL.md 的语气保持一致（中文，简洁，不写 README 式废话）

## 开发约定

- 文件用 UTF-8
- 缩进用 2 空格（markdown 不缩进）
- 链接用相对路径（`./LICENSE` 不是绝对 URL）
- 中文标点用全角，英文标点用半角

## 不收什么

- ❌ 把 skill 改成要 `pip install` 的 Python 包 —— 它只是个 markdown 协议
- ❌ 加 CI / 测试覆盖 —— skill 没有代码可测，验证靠真实场景跑一遍
- ❌ 改 frontmatter 的 `name` —— 改了之后所有引用都会断

## 许可

贡献的代码同样按 MIT 协议发布。