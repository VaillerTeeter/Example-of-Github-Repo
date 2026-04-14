# 项目规范

## GitHub CLI 认证配置

**所有 `gh` 指令的认证 Token 统一从项目根目录的 `.env` 文件中读取。**

`.env` 文件格式（该文件已加入 `.gitignore`，不会提交到仓库）：
```
GH_TOKEN=your_github_personal_access_token_here
```

在执行任何 `gh` 指令前，需确保环境变量已加载：
```powershell
# Windows PowerShell — 每次新开终端时执行一次
$content = Get-Content .env; foreach ($line in $content) { if ($line -match "^GH_TOKEN=(.+)$") { $env:GH_TOKEN = $Matches[1] } }

# 验证 Token 已生效
gh auth status
```

> `.env.example` 是提交到仓库的模板文件，不含真实 Token，可参考格式进行配置。

---

## Git 操作规范

**所有 git 相关操作必须优先使用 `gh` CLI 指令，而不是裸 `git` 命令或 GitHub MCP 工具。**

### 常用 gh 指令速查

#### 仓库管理
```powershell
# 查看仓库信息
gh repo view

# 修改仓库描述、Topics、主页
gh repo edit --description "描述" --add-topic "tag" --homepage "https://..."

# 修改仓库可见性
gh repo edit --visibility public   # 或 private
```

#### 分支操作
```powershell
# 列出所有分支
gh api repos/{owner}/{repo}/branches

# 创建分支保护规则
gh api repos/{owner}/{repo}/branches/{branch}/protection \
  --method PUT --input protection.json
```

#### Issue 管理
```powershell
gh issue list
gh issue create --title "标题" --body "内容"
gh issue close <number>
```

#### Pull Request
```powershell
gh pr list
gh pr create --title "标题" --body "内容" --base master
gh pr merge <number> --squash
```

#### 发布版本
```powershell
gh release create v1.0.0 --title "Release v1.0.0" --notes "变更说明"
gh release list
```

#### 工作流 / Actions
```powershell
gh workflow list
gh run list
gh run view <run-id>
```

### 禁止直接使用的替代方式

| 避免使用 | 改用 |
|----------|------|
| 直接 `git push origin master` | 通过功能分支 + PR 合并，禁止直接推送到 `master` |
| GitHub 网页手动操作 Settings | `gh repo edit` |
| GitHub MCP `mcp_io_github_git_*` 工具 | `gh` CLI 指令 |
| `curl https://api.github.com/...` | `gh api ...` |

### 例外

纯本地操作（`git add`、`git commit`、`git log`、`git diff` 等）不涉及远程交互，允许直接使用 `git` 命令。

---

## 代码提交规范

### 严格禁止

**未经用户明确指示，禁止执行以下操作：**

- `git commit` — 提交代码
- `git push` — 推送到远程
- `gh pr create` — 创建 Pull Request
- `gh pr merge` — 合并 Pull Request

只有用户明确说「提交」「commit」「push」「创建 PR」「合并」等指令时，才可执行上述操作。

### 代码入库流程（PR 工作流）

本仓库所有代码变更必须通过 Pull Request 合并到 `master`，禁止直接 push 到 `master`。

标准流程如下：

```powershell
# 1. 从 master 创建功能分支
git checkout master
git checkout -b feat/your-feature-name

# 2. 完成代码修改后，暂存并提交（需用户明确指示）
git add .
git commit -m "feat: 描述本次变更"

# 3. 推送分支到远程（需用户明确指示）
git push origin feat/your-feature-name

# 4. 创建 PR（需用户明确指示）
gh pr create --title "标题" --body "内容" --base master

# 5. 合并 PR（需用户明确指示，使用 squash）
gh pr merge <number> --squash --delete-branch
```

> 合并策略统一使用 **Squash Merge**，保持 master 线性历史。
