# Example-of-Github-Repo

个人代码示例仓库，收录各类语言和框架的代码片段与实践示例。

## 目录结构

```text
.
├── .github/
│   ├── ISSUE_TEMPLATE/
│   │   ├── config.yml                # 禁用空白 Issue，配置联系链接
│   │   ├── bug_report_zh.md          # Bug 报告模板（中文）
│   │   ├── bug_report_en.md          # Bug Report Template (English)
│   │   ├── feature_request_zh.md     # 功能请求模板（中文）
│   │   └── feature_request_en.md     # Feature Request Template (English)
│   ├── workflows/
│   │   └── lint.yml                  # Markdown + YAML lint CI
│   ├── dependabot.yml                # 自动更新 Actions 依赖（每周一）
│   ├── PULL_REQUEST_TEMPLATE.md      # PR 模板（中英双语）
│   └── copilot-instructions.md       # Copilot 工作区指令
├── .env.example                      # GitHub Token 配置模板
├── .gitignore
├── .markdownlint.json                # Markdown lint 规则配置
├── CODE_OF_CONDUCT.md                # 行为准则
├── CONTRIBUTING.md                   # 贡献指南
├── LICENSE                           # GNU GPLv3
├── README.md
└── SECURITY.md                       # 安全漏洞上报流程
```

## 本地配置

1. 复制 Token 模板文件：

   ```powershell
   Copy-Item .env.example .env
   ```

2. 编辑 `.env`，填入你的 GitHub Personal Access Token：

   ```ini
   GH_TOKEN=ghp_xxxxxxxxxxxxxxxxxxxx
   ```

   > Token 申请地址：GitHub → Settings → Developer settings → Personal access tokens

3. 加载环境变量（每次新开终端执行一次）：

   ```powershell
   $content = Get-Content .env; foreach ($line in $content) { if ($line -match "^GH_TOKEN=(.+)$") { $env:GH_TOKEN = $Matches[1] } }
   ```

4. 验证配置：

   ```powershell
   gh auth status
   ```

## CI 检查说明

所有 Pull Request 合并到 `master` 前，必须通过以下两个自动检查（定义于 [.github/workflows/lint.yml](.github/workflows/lint.yml)）：

### Markdown Lint

使用 [markdownlint-cli2](https://github.com/DavidAnson/markdownlint-cli2-action) 检查所有 `*.md` 文件，规则如下：

| 规则 | 状态 | 说明 |
|------|------|------|
| 默认全部规则 | ✅ 启用 | 包含标题格式、列表缩进、空行等 |
| MD013 行长度限制 | ❌ 关闭 | 允许长行（文档场景不适合限制行长） |
| MD033 内联 HTML | ❌ 关闭 | 允许在 Markdown 中使用 HTML 标签 |
| MD041 首行必须是 H1 | ❌ 关闭 | 允许文件不以 H1 开头 |

### YAML Lint

使用 [action-yamllint](https://github.com/ibiqlik/action-yamllint) 检查所有 `.yml` / `.yaml` 文件，规则如下：

| 规则 | 配置 | 说明 |
|------|------|------|
| 基础规则 | `extends: default` | yamllint 默认规则集 |
| 行长度 | 最长 200 字符 | 放宽默认的 80 字符限制 |
| 布尔值写法 | 仅允许 `true` / `false` | 禁止 `yes` / `no` / `on` / `off` 等写法 |

### 触发时机

- **PR 创建 / 更新**：目标分支为 `master` 时自动触发
- **直接 push 到 master**（仅管理员可操作）：同样触发检查

---

## 相关链接

- [Blog](https://blog.vaciller.cn)
- [GitHub Profile](https://github.com/VaillerTeeter)
