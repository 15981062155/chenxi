---
name: submit-project
description: 提交并推送当前项目到 Gitee 仓库 https://gitee.com/chen-chen-xixi/chenxi2026。Use when the user asks to 提交项目、commit、push、推送代码、同步到 Gitee。
metadata:
  opencode/slash: "true"
---

# 提交项目到 Gitee

将当前项目的改动用 `git commit` 提交，并 `git push` 推送到
https://gitee.com/chen-chen-xixi/chenxi2026

## 凭据

私人令牌存放在 `.opencode/gitee-token.txt`（已被 `.gitignore` 排除，不进 git 历史）。

- 文件存在：读取其中的令牌用于推送。
- 文件缺失：向用户索取令牌，写入该文件（单行、无换行）后继续。
- 令牌只在推送命令中临时使用，不写入 `.git/config`、不写入提交内容、不回显到报告中。

## 工作流程

按顺序执行，每步失败先修复再继续：

1. **检查状态**：运行 `git status` 和 `git diff --stat`，了解将要提交的改动。
2. **配置远程**：运行 `git remote -v`。
   - 没有远程或 URL 不匹配时：`git remote add origin https://gitee.com/chen-chen-xixi/chenxi2026.git`（已存在则 `git remote set-url origin <url>`）。
3. **检查敏感信息**：扫描将要暂存的内容（`git diff`、`git status` 中的新文件），确认没有密钥、密码、token 被暂存。特别确认 `.opencode/gitee-token.txt` 未被跟踪（`git check-ignore .opencode/gitee-token.txt` 应有输出）。发现敏感文件被跟踪时停止并向用户报告，不得提交。
4. **暂存**：默认 `git add -A`。若用户指定了文件，只暂存指定文件。
5. **提交**：`git commit -m "<message>"`。
   - message 用中文简述改动内容；首次提交可用 `初始化项目`。
6. **推送**：读取令牌后执行（PowerShell）：
   ```powershell
   $t = (Get-Content .opencode\gitee-token.txt -Raw).Trim()
   git push "https://chen-chen-xixi:${t}@gitee.com/chen-chen-xixi/chenxi2026.git" master
   ```
   - 首次推送可加 `-u` 建立跟踪；注意 `-u` 会把带令牌的 URL 写入 `.git/config`，推送成功后改回：`git remote set-url origin https://gitee.com/chen-chen-xixi/chenxi2026.git`。
   - 推送被拒绝（远程有新提交）：先 `git pull --rebase origin master`，解决冲突后再推送；有冲突时停下询问用户。
   - 认证失败：令牌可能过期，向用户索取新令牌并更新 `gitee-token.txt` 后重试。
7. **验证**：确认 `git status` 显示干净、`git log --oneline -1` 显示新提交，向用户报告推送结果。

## 注意事项

- 遵循仓库既有提交信息风格；不修改 git 配置、不使用 `--force`。
- 提交后如发现令牌或其他密钥进入提交，停止并报告用户，建议作废该令牌。
- `img/`、`*.html`、`css.css` 等项目文件正常提交；忽略规则以 `.gitignore` 为准。
