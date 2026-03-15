# Copilot Coding Agent Instructions

## 仓库概述

这是一个**纯文档仓库**，提供面向 GitHub Copilot CLI 和 Claude Code 的浏览器自动化 Agent Skill。仓库中没有可执行代码——所有内容均为 Markdown 文档。

## 仓库结构

```
agent-browser/
├── .github/
│   ├── copilot-instructions.md   # 本文件
│   └── prompts/
│       └── create-instructions.prompt.md  # 生成/更新本文件的可复用提示
├── SKILL.md                      # Skill 主入口（frontmatter + 常见工作流）
├── README.md                     # 安装与使用说明
└── resources/
    └── REFERENCE.md              # 完整命令参考及高级用法
```

## 核心文件说明

### `SKILL.md`
- **顶部 frontmatter**（`---` 围栏）包含：`name`、`description`、`compatibility`、`allowed-tools`
- `description` 字段列举该 skill 的适用场景（中文编号列表）
- 正文包含**常见工作流**（bash 代码块）和**命令速查表**
- 每增加新的使用场景，需同步更新 `description` 字段

### `resources/REFERENCE.md`
- 完整命令参考，按功能分节（`##` 级别）
- 每节提供可直接运行的 `bash` 代码块示例
- 新工作流应在此添加对应的详细参考节

### `README.md`
- 面向最终用户的安装与使用说明
- 目录结构图应与实际文件保持同步

## 修改规范

### 添加新工作流
1. 在 `SKILL.md` 的 `## 常见工作流` 下新增 `###` 节
2. 在 `resources/REFERENCE.md` 中新增对应的 `##` 参考节（含详细示例）
3. 若场景是新类别，更新 `SKILL.md` frontmatter 的 `description` 字段

### 代码块格式
- 所有示例均使用 ```` ```bash ```` 围栏
- 复杂 JavaScript 使用 heredoc 格式：
  ```bash
  agent-browser eval --stdin <<'EVALEOF'
  // JS 代码
  EVALEOF
  ```
- 行内注释使用 `#` 说明每一步的意图

### 禁止事项
- **不要**在仓库中添加可执行脚本、package.json、Python 包等代码文件
- **不要**提交 build 产物、`node_modules`、`.env` 等非文档文件
- **不要**修改 `SKILL.md` frontmatter 中的 `name` 字段（必须保持 `agent-browser`）
- **不要**删除现有工作流节，只能新增或修正

## 语言约定

- 所有文档使用**中文**撰写
- 技术术语（命令名、参数、API 路径等）保持英文原文
- 代码块中的注释使用中文

## 测试 / 验证

此仓库无构建或测试流程。修改后的验证方式：
1. 确认所有 Markdown 文件语法正确（可用 `markdownlint` 检查，若已安装）
2. 确认 `SKILL.md` frontmatter 的 YAML 格式有效
3. 确认所有代码块中的 `agent-browser` 命令与 `resources/REFERENCE.md` 中已记录的命令一致
