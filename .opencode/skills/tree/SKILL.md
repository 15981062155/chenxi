---
name: tree
description: 以树状结构显示当前项目的目录和文件。Use when the user asks for 目录树、tree、查看目录结构、项目结构、文件夹层级。
metadata:
  opencode/slash: "true"
---

# 显示目录树

以树状结构展示当前工作区的目录与文件，帮助用户快速了解项目布局。

## 工作流程

1. **确定目标路径**：默认从用户的工作目录（`cwd`）开始。若用户指定了子目录，则切换到该目录。
2. **执行 tree 命令**：

   在 Windows 上，优先使用系统自带的 `tree.com` 命令：

   ```powershell
   # 只显示目录
   tree

   # 显示目录和文件（推荐）
   tree /F

   # 显示完整路径
   tree /F /A
   ```

   - `/F` 显示每个文件夹中的文件名。
   - `/A` 使用 ASCII 字符而非扩展字符绘制树（兼容性更好）。

   如果系统没有 `tree` 命令（极少数情况），改用 PowerShell 递归输出：

   ```powershell
   Get-ChildItem -Recurse | ForEach-Object { $_.FullName }
   ```

3. **过滤无关目录**：tree 输出中通常包含 `.git/`、`.opencode/`、`node_modules/` 等内部或依赖目录。根据 `.gitignore` 的规则，对输出进行说明或手动排除这些目录后再呈现给用户。

4. **输出格式**：将 tree 的原始输出直接作为纯文本代码块展示给用户，不要重新排版或添加额外装饰。

## 示例

用户说 "tree" 或 "看看目录结构" 时：

```powershell
tree /F /A
```

输出示例：
```
D:\1
├───cdn
│   ├───font-awesome
│   │   ├───css
│   │   └───webfonts
│   └───lucide
├───font
├───img
├───index.html
├───css.css
└───...
```

## 注意事项

- 对包含大量文件的目录（如 `node_modules/`、`img/`），可以只展示到当前层级或跳过，避免输出过长。
- 用户如果只关心顶层结构，使用不加 `/F` 的 `tree` 只展示目录。
- 始终在用户指定的工作目录下执行，不要跨盘符或跳出项目范围。
