<!--
main 受保护：PR 需要至少 1 个 approving review，且必须包含 code owner（@LaT-SKY）的审核。
本文件里的 HTML 注释不会显示在渲染后的正文中，但下面的标题请保留。
填不出来的项请写「未验证」或「N/A」，不要留空。
-->

## 改了什么

<!-- 一两句话，列出涉及的组件 / 文件。 -->

## 为什么

<!-- 解决什么问题、依据哪条 issue 或已有结论。关联 issue 请在最下方写 Closes #编号。 -->

## 验证方式

<!-- 只写真实跑过的命令与结果。纯文档改动可整段写 N/A。 -->

| 项 | 结果 |
|---|---|
| 静态检查 | 例如 `git diff --check`、`luac -p hypr/hyprland.lua`、`jq empty …` |
| 安装 / 更新 | 例如在测试用户执行 `caelestia update`，或手动复制后重登 |
| 实际效果 | 例如 `hyprctl configerrors` 无输出；打开对应组件确认行为 |

## 检查项

- [ ] 本次 PR 只做一件事（例如没有把「搬文件」和「改文件」混在一起）
- [ ] 改动范围与关联 issue 的「实现」一致
- [ ] 新增 / 删除组件或包时已同步 `manifest.toml`
- [ ] 没有提交生成文件（`hypr/scheme/current.*`、`fish/conf.d/`、`.SRCINFO` 等）或任何密钥
- [ ] 没有把用户可覆盖的配置写死（`~/.config/caelestia/hypr-user.lua` / `hypr-vars.lua`）
- [ ] 改了行为或结论时，相关文档已同步

<!-- 关联 Issue 写 Closes #编号，合并后会自动关闭。 -->
