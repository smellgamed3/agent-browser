---
name: agent-browser
description: 浏览器自动化工具。当用户需要：(1) 打开网页/导航 URL (2) 点击按钮、填写表单、提交数据 (3) 截图或保存 PDF (4) 抓取/提取网页内容 (5) 网页自动化测试 (6) 登录网站并保存会话 (7) 任何需要与网页交互的任务。底层使用 agent-browser CLI。
allowed-tools: Bash(agent-browser:*)
---

# 浏览器自动化 (agent-browser)

**一句话**：通过 `agent-browser` 命令驱动浏览器完成任何网页交互任务。

## 核心工作流

每次操作遵循：**导航 → 快照获取元素引用 → 交互 → 再次快照确认**

```bash
agent-browser open https://example.com
agent-browser snapshot -i          # 获取可交互元素引用 @e1, @e2...
agent-browser fill @e1 "内容"
agent-browser click @e2
agent-browser wait --load networkidle
agent-browser snapshot -i          # 导航后必须重新快照
```

## 常见工作流

### 表单填写与提交

```bash
agent-browser open https://example.com/form
agent-browser snapshot -i
# 输出: @e1 [input "Email"], @e2 [input "Password"], @e3 [button] "登录"
agent-browser fill @e1 "user@example.com"
agent-browser fill @e2 "password123"
agent-browser click @e3
agent-browser wait --load networkidle
agent-browser snapshot -i          # 确认登录结果
```

### 网页内容提取

```bash
agent-browser open https://example.com/list
agent-browser snapshot -i
agent-browser get text @e5         # 获取指定元素文本
agent-browser get text body > page.txt   # 获取全页文本
agent-browser screenshot --full    # 截取完整页面截图
```

### 登录状态保存与复用

```bash
# 第一次登录后保存状态
agent-browser open https://app.example.com/login
agent-browser snapshot -i
agent-browser fill @e1 "$USERNAME"
agent-browser fill @e2 "$PASSWORD"
agent-browser click @e3
agent-browser wait --url "**/dashboard"
agent-browser state save auth.json

# 后续直接复用
agent-browser state load auth.json
agent-browser open https://app.example.com/dashboard
```

### 并行多会话

```bash
agent-browser --session site1 open https://site-a.com
agent-browser --session site2 open https://site-b.com
agent-browser --session site1 snapshot -i
agent-browser --session site2 snapshot -i
```

### 连接已有 Chrome

```bash
# 自动发现本机运行中的 Chrome（需开启远程调试）
agent-browser --auto-connect snapshot
agent-browser --auto-connect open https://example.com

# 指定 CDP 端口
agent-browser --cdp 9222 snapshot
```

## 命令速查

| 命令 | 说明 |
|------|------|
| `open <url>` | 打开 URL（别名: goto, navigate） |
| `snapshot -i` | 获取可交互元素引用（推荐） |
| `snapshot -i -C` | 同上，含 cursor:pointer 元素 |
| `click @e1` | 点击元素 |
| `fill @e1 "text"` | 清空并输入文本 |
| `type @e1 "text"` | 在已有内容后追加输入 |
| `select @e1 "选项"` | 下拉框选项 |
| `check @e1` | 勾选复选框 |
| `press Enter` | 按键（Tab, Escape, Control+a 等） |
| `scroll down 500` | 滚动页面 |
| `get text @e1` | 获取元素文本 |
| `get url` | 获取当前 URL |
| `get title` | 获取页面标题 |
| `wait @e1` | 等待元素出现 |
| `wait --load networkidle` | 等待网络空闲 |
| `wait --url "**/page"` | 等待 URL 匹配 |
| `wait 2000` | 等待毫秒 |
| `screenshot` | 截图（临时目录） |
| `screenshot --full` | 截取完整页面 |
| `pdf output.pdf` | 保存为 PDF |
| `state save auth.json` | 保存会话状态 |
| `state load auth.json` | 加载会话状态 |
| `close` | 关闭浏览器 |

## 重要提示

**元素引用生命周期**：`@e1` 等引用在页面变化后即失效，点击链接/提交表单后**必须重新执行 `snapshot -i`**。

**语义定位器**（refs 不可用时的备选方案）：
```bash
agent-browser find text "登录" click
agent-browser find label "邮箱" fill "user@test.com"
agent-browser find role button click --name "提交"
agent-browser find placeholder "搜索" type "关键词"
```

**eval 执行 JavaScript**：
```bash
# 简单表达式
agent-browser eval 'document.title'

# 复杂 JS 用 heredoc（避免引号转义问题）
agent-browser eval --stdin <<'EVALEOF'
JSON.stringify(Array.from(document.querySelectorAll("a")).map(a => a.href))
EVALEOF
```

## 更多参考

- [REFERENCE.md](resources/REFERENCE.md) - 完整命令参考及高级用法
