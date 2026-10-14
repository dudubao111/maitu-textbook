# 码途 · 中小学信息科技竞赛数字教材

面向中小学信息科技竞赛的 **Python 算法与程序设计** 数字教材原型平台。
基于 MkDocs + Material for MkDocs 构建，纯静态、无后端。

**在线访问：https://dudubao111.github.io/maitu-textbook/**

> 仓库：https://github.com/dudubao111/maitu-textbook （公开）
> 本地预览：`http://127.0.0.1:8000`

## 教材结构

- 首页：教材简介、适用学段（初中为主，小学高年级可跟读前 3 章）、学习目标、学习路径
- 关于本项目：项目缘起、教材定位、编写原则、版权与使用声明、技术实现
- 第 1 章 走进信息科技竞赛
- 第 2 章 计算思维与问题分解
- 第 3 章 变量、数据类型与输入输出
- 第 4 章 流程控制
- 第 5 章 列表与字符串处理
- 第 6 章 函数与模块化
- 第 7 章 基础算法入门
- 第 8 章 竞赛实战与提交规范
- 附录：术语表 / 常见问题 / 练习参考答案 / 拓展阅读

每章统一结构：**学习目标 → 知识讲解 → 案例分析 → 代码实践 → 本章小结 → 练习与参考**。

## 本地运行

```bash
# 1. 创建虚拟环境（Python 3.13）
python -m venv .venv

# 2. 安装依赖（国内建议加镜像源）
.venv/Scripts/python.exe -m pip install -r requirements.txt -i https://mirrors.aliyun.com/pypi/simple

# 3. 本地预览
.venv/Scripts/python.exe -m mkdocs serve
# 浏览器打开 http://127.0.0.1:8000

# 4. 构建静态站点
.venv/Scripts/python.exe -m mkdocs build
```

## 部署到 GitHub Pages

本项目已部署上线，地址 <https://dudubao111.github.io/maitu-textbook/>。

部署链路：推送 `main` → GitHub Actions 构建 → `mkdocs gh-deploy` 推送到 `gh-pages` 分支
→ GitHub Pages 以「传统分支」模式发布（`build_type: legacy`，源分支 `gh-pages`）。

### 更新线上内容

```bash
# 1. 本地改完内容后提交
git add -A
git commit -m "docs: 更新说明"

# 2. 推送，Actions 会自动构建并发布（约 1-2 分钟生效）
git push origin main
```

### 首次部署（换账号/换仓库时）

```bash
# 安装 gh CLI 后登录（浏览器设备流授权）
gh auth login --hostname github.com --git-protocol https --web
gh auth setup-git

# 创建公开仓库并推送
gh repo create maitu-textbook --public --source=. --remote=origin
git branch -M main
git push -u origin main

# 开启 Pages：源分支选 gh-pages（与 mkdocs gh-deploy 的输出分支一致）
gh api -X PUT repos/<用户名>/maitu-textbook/pages --input - <<'JSON'
{"build_type":"legacy","source":{"branch":"gh-pages","path":"/"}}
JSON

# 手动触发一次构建
gh workflow run deploy.yml
```

> **踩坑记录**：Pages 源若设为「GitHub Actions」模式（`build_type: workflow`），
> 而工作流用的是 `mkdocs gh-deploy` 推分支，两者不匹配——内容已在 gh-pages 分支上，
> 站点却返回 404「Site not found」。Actions 模式只认工作流通过
> `actions/upload-pages-artifact` + `actions/deploy-pages` 上传的部署产物。
> 本项目保持「传统分支」模式即可，与 `mkdocs gh-deploy` 天然对齐。

## 目录说明

```
maitu-textbook/
├── mkdocs.yml          # 站点与导航配置
├── requirements.txt    # 依赖
├── docs/               # 教材内容（Markdown）
│   ├── index.md        # 首页
│   ├── assets/         # 站点素材（logo 等）
│   ├── stylesheets/    # 自定义样式
│   ├── 01-intro/ ... 08-practice/
│   └── appendix/
├── 实验报告/            # 配套实验报告
└── .github/workflows/  # 自动部署
```

## 许可

教材内容为原创，仅用于教学与课程实验用途。
