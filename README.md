# agent-browser skill

一个用于 GitHub Copilot CLI / Claude Code 的浏览器自动化 Agent Skill，底层驱动 [`agent-browser`](https://www.npmjs.com/package/agent-browser) CLI 完成任意网页交互任务。

## 功能

- 打开网页、导航 URL
- 点击按钮、填写表单、提交数据
- 截图或保存 PDF
- 抓取/提取网页内容
- 网页自动化测试
- 登录网站并保存会话状态
- 并行多会话、连接已有 Chrome

## 前提条件

安装 `agent-browser` CLI：

```bash
npm install -g agent-browser
```

## 安装 Skill

将此仓库克隆到 Copilot CLI 或 Claude Code 的 skills 目录，**目录名必须为 `agent-browser`**（与 SKILL.md 中的 `name` 字段保持一致）：

```bash
# GitHub Copilot CLI（个人 skill）
git clone https://github.com/smellgamed3/agent-browser ~/.copilot/skills/agent-browser

# Claude Code（个人 skill）
git clone https://github.com/smellgamed3/agent-browser ~/.claude/skills/agent-browser

# 项目级 skill（放到仓库内）
git clone https://github.com/smellgamed3/agent-browser .github/skills/agent-browser
# 或
git clone https://github.com/smellgamed3/agent-browser .claude/skills/agent-browser
```

## 使用方式

安装后，Copilot / Claude 会在你描述浏览器相关任务时自动加载此 skill。也可以在提示词中直接指定：

```
使用 /agent-browser skill 登录 https://example.com 并截图
```

在 Copilot CLI 中管理 skill：

```bash
/skills list          # 查看已加载的 skills
/skills info          # 查看 skill 详情及路径
/skills reload        # 运行时重新加载 skills
```

## 目录结构

```
agent-browser/
├── SKILL.md              # Skill 主入口（frontmatter + 核心工作流说明）
├── README.md             # 本文件
└── resources/
    └── REFERENCE.md      # 完整命令参考及高级用法
```

## 核心工作流

```bash
agent-browser open https://example.com
agent-browser snapshot -i          # 获取可交互元素引用 @e1, @e2...
agent-browser fill @e1 "内容"
agent-browser click @e2
agent-browser wait --load networkidle
agent-browser snapshot -i          # 导航后必须重新快照
```

> 详细命令请参阅 [resources/REFERENCE.md](resources/REFERENCE.md)

## 规范参考

本 skill 遵循 [Agent Skills 规范](https://agentskills.io/specification)，兼容 GitHub Copilot CLI 和 Claude Code。
