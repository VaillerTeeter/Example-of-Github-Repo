# Example-of-Github-Repo

个人代码示例仓库，收录各类语言和框架的代码片段与实践示例。

## 目录结构

```
.
├── .github/
│   ├── ISSUE_TEMPLATE/
│   │   ├── config.yml                # 禁用空白 Issue，配置联系链接
│   │   ├── bug_report_zh.md          # Bug 报告模板（中文）
│   │   ├── bug_report_en.md          # Bug Report Template (English)
│   │   ├── feature_request_zh.md     # 功能请求模板（中文）
│   │   └── feature_request_en.md     # Feature Request Template (English)
│   ├── PULL_REQUEST_TEMPLATE.md      # PR 模板（中英双语）
│   └── copilot-instructions.md       # Copilot 工作区指令
├── .env.example                      # GitHub Token 配置模板
├── .gitignore
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
   ```
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

## 相关链接

- [Blog](https://blog.vaciller.cn)
- [GitHub Profile](https://github.com/VaillerTeeter)
