# 码途 · 中小学信息科技竞赛数字教材

面向中小学信息科技竞赛的 **Python 算法与程序设计** 数字教材原型平台。
基于 MkDocs + Material for MkDocs 构建，纯静态、无后端。

## 教材结构

- 首页：教材简介、适用学段、学习目标、学习路径
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

```bash
git init
git add .
git commit -m "feat: 初始化码途数字教材"
git branch -M main
git remote add origin https://github.com/<你的用户名>/maitu-textbook.git
git push -u origin main

# 使用 mkdocs-material 内置命令发布到 gh-pages 分支
.venv/Scripts/python.exe -m mkdocs gh-deploy
```

或者使用 GitHub Actions（见 `.github/workflows/deploy.yml`），推送 main 分支后自动构建发布。

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
