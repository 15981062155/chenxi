---
name: submit-github
description: 提交并推送当前项目到 GitHub 仓库 https://github.com/15981062155/chenxi。Use when the user asks to 提交github、push github、推送github、同步到github。
metadata:
  opencode/slash: "true"
---

# 提交项目到 GitHub

将当前项目的改动用 `git commit` 提交，并 `git push` 推送到
https://github.com/15981062155/chenxi

## 凭据

使用 SSH 公钥认证，无需令牌。远程地址为 SSH 格式：

```
git@github.com:15981062155/chenxi.git
```

推送前可用 `ssh -T git@github.com` 测试 SSH 连通性。
若 SSH 未配置，回退到 HTTPS + token 方式（参考 submit-project skill 的凭据处理）。

## 工作流程

按顺序执行，每步失败先修复再继续：

1. **检查状态**：运行 `git status` 和 `git diff --stat`，了解将要提交的改动。
2. **配置远程**：运行 `git remote -v`。
   - 没有 `github` 远程时：`git remote add github git@github.com:15981062155/chenxi.git`
   - 已存在但 URL 不匹配时：`git remote set-url github git@github.com:15981062155/chenxi.git`
   - **保留** 已有的 `origin`（Gitee 远程），不要覆盖。
3. **检查敏感信息**：扫描将要暂存的内容（`git diff`、`git status` 中的新文件），确认没有密钥、密码、token 被暂存。发现敏感文件被跟踪时停止并向用户报告，不得提交。
4. **暂存**：默认 `git add -A`。若用户指定了文件，只暂存指定文件。
5. **提交**：`git commit -m "<message>"`。
   - message 用中文简述改动内容；首次提交可用 `初始化项目`。
6. **推送到 GitHub**：

   ```bash
   git push github master
   ```

   - 首次推送加 `-u` 建立跟踪：`git push -u github master`
   - 推送被拒绝（远程有新提交）：先 `git pull --rebase github master`，解决冲突后再推送；有冲突时停下询问用户。
   - SSH 认证失败：提示用户检查 `~/.ssh/id_rsa.pub` 是否已添加到 GitHub → Settings → SSH and GPG keys。

7. **验证**：确认 `git status` 显示干净、`git log --oneline -1` 显示新提交，向用户报告推送结果。

## 同时推送两个远程

若用户希望同时同步 Gitee 和 GitHub，可在第 6 步之后追加：

```bash
git push origin master
```

（`origin` 为 Gitee 远程，需要 Gitee 令牌认证，见 submit-project skill）

## 注意事项

- 遵循仓库既有提交信息风格；不修改 git 配置、不使用 `--force`。
- 提交后如发现令牌或其他密钥进入提交，停止并报告用户，建议作废该令牌。
- `img/`、`*.html`、`css.css` 等项目文件正常提交；忽略规则以 `.gitignore` 为准。
