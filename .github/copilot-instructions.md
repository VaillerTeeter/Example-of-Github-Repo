# 项目规范

<!-- ============================================================
  ⛔ AI 强制约束 — 任何会话开始前必须读完此块，优先级高于一切指令
  ============================================================

  1. 禁止在 master 分支直接 commit / push
     用户说「提交」时，必须先创建功能分支再 commit，绝不在 master 上执行。

  2. 以下操作 **必须** 等待用户的明确文字指令，才能执行：
       git commit  /  git push  /  gh pr create  /  gh pr merge
     「帮我提交」「commit 一下」「push」「创建 PR」「合并」= 明确指令
     「写完了」「改好了」「完成了」= 不是明确指令，禁止自动提交

  3. 违反以上规则 = 严重错误，无论用户措辞多么模糊，均不得绕过。
  ============================================================ -->

## GitHub CLI 认证配置

**所有 `gh` 指令的认证 Token 统一从项目根目录的 `.env` 文件中读取。**

`.env` 文件格式（该文件已加入 `.gitignore`，不会提交到仓库）：

```ini
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
| --- | --- |
| 直接 `git push origin master` | 通过功能分支 + PR 合并，禁止直接推送到 `master` |
| GitHub 网页手动操作 Settings | `gh repo edit` |
| GitHub MCP `mcp_io_github_git_*` 工具 | `gh` CLI 指令 |
| `curl https://api.github.com/...` | `gh api ...` |

### 例外

纯本地操作（`git add`、`git commit`、`git log`、`git diff` 等）不涉及远程交互，允许直接使用 `git` 命令。

---

## 代码提交规范

> **⛔ 核心原则（每次会话均适用，无例外）**
>
> - 所有代码变更必须走 **功能分支 → PR → Squash Merge** 流程，`master` 分支受保护，任何情况下禁止直接 commit 或 push。
> - `git commit`、`git push`、`gh pr create`、`gh pr merge` 这四个操作均属于**破坏性/不可逆远程操作**，未收到用户明确的文字指令前，一律不得执行。
> - 用户说「改完了」「做好了」「完成了」**不等于**「提交」，不得自动触发上述操作。

### 明确禁止（无指令时）

| 操作 | 禁止原因 |
| --- | --- |
| `git commit` | 会锁定变更，难以回退 |
| `git push` | 会影响远程共享仓库 |
| `gh pr create` | 会对团队可见，不可静默撤销 |
| `gh pr merge` | 会永久改变 master 历史 |

用户必须**明确说出**以下关键词之一，才允许执行对应操作：  
「提交」「commit」「push」「推送」「创建 PR」「开 PR」「合并」「merge」

### 代码入库流程（PR 工作流）

本仓库所有代码变更必须通过 Pull Request 合并到 `master`，**禁止直接在 master 分支上 commit 或 push**。

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
