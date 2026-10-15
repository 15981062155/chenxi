---
name: submit-project
description: 提交并推送当前项目到 Gitee 仓库 https://gitee.com/chen-chen-xixi/chenxi2026。Use when the user asks to 提交项目、commit、push、推送代码、同步到 Gitee。
metadata:
  opencode/slash: "true"
---

# 提交项目到 Gitee

将当前项目的改动用 `git commit` 提交，并 `git push` 推送到
https://gitee.com/chen-chen-xixi/chenxi2026

## 工作流程

按顺序执行，每步失败先修复再继续：

1. **检查状态**：运行 `git status` 和 `git diff --stat`，了解将要提交的改动。
2. **配置远程**：运行 `git remote -v`。
   - 没有远程或 URL 不匹配时：`git remote add origin https://gitee.com/chen-chen-xixi/chenxi2026.git`（已存在则 `git remote set-url origin <url>`）。
3. **检查敏感信息**：提交前扫描暂存内容中的密钥、密码、token。发现即停止并向用户报告，不得提交。
4. **暂存**：默认 `git add -A`。若用户指定了文件，只暂存指定文件。
5. **提交**：`git commit -m "<message>"`。
   - message 用中文简述改动内容；首次提交可用 `初始化项目`。
   - 仓库无提交历史时当前分支为 `master`，正常提交即可。
6. **推送**：
   - 首次推送：`git push -u origin master`
   - 已有远程提交（被拒绝）：先 `git pull --rebase origin master`，解决冲突后再推送；有冲突时停下询问用户。
7. **验证**：确认 `git status` 显示干净、`git log --oneline -1` 显示新提交，向用户报告推送结果。

## 注意事项

- 遵循仓库既有提交信息风格；不修改 git 配置、不使用 `--force`。
- 推送需要 Gitee 凭据（用户名/密码或 SSH key）。认证失败时提示用户配置凭据后重试。
- `img/`、`*.html`、`css.css` 等项目文件正常提交；忽略规则以 `.gitignore` 为准。
