# agent-browser 完整命令参考

> 快速上手请看 [SKILL.md](../SKILL.md)

## 导航

```bash
agent-browser open <url>      # 打开 URL（别名: goto, navigate）
                              # 支持: https://, http://, file://, about:, data://
                              # 不带协议时自动补全 https://
agent-browser back            # 后退
agent-browser forward         # 前进
agent-browser reload          # 刷新
agent-browser close           # 关闭浏览器（别名: quit, exit）
agent-browser connect 9222    # 通过 CDP 端口连接浏览器
```

## 快照（页面分析）

```bash
agent-browser snapshot            # 完整无障碍树
agent-browser snapshot -i         # 仅可交互元素（推荐）
agent-browser snapshot -c         # 紧凑输出
agent-browser snapshot -d 3       # 限制深度为 3
agent-browser snapshot -s "#main" # 限定 CSS 选择器范围
agent-browser snapshot --json     # JSON 格式输出
```

## 交互（使用快照返回的 @refs）

```bash
agent-browser click @e1           # 点击
agent-browser dblclick @e1        # 双击
agent-browser focus @e1           # 聚焦
agent-browser fill @e2 "text"     # 清空并输入
agent-browser type @e2 "text"     # 追加输入
agent-browser press Enter         # 按键（别名: key）
agent-browser press Control+a     # 组合键
agent-browser keydown Shift       # 按下并保持
agent-browser keyup Shift         # 释放按键
agent-browser hover @e1           # 悬停
agent-browser check @e1           # 勾选复选框
agent-browser uncheck @e1         # 取消勾选
agent-browser select @e1 "值"     # 下拉框选项
agent-browser select @e1 "a" "b"  # 多选
agent-browser scroll down 500     # 滚动（默认向下 300px）
agent-browser scrollintoview @e1  # 滚动到元素可见（别名: scrollinto）
agent-browser drag @e1 @e2        # 拖拽
agent-browser upload @e1 file.pdf # 上传文件
```

## 获取信息

```bash
agent-browser get text @e1        # 获取元素文本
agent-browser get html @e1        # 获取 innerHTML
agent-browser get value @e1       # 获取输入值
agent-browser get attr @e1 href   # 获取属性
agent-browser get title           # 获取页面标题
agent-browser get url             # 获取当前 URL
agent-browser get count ".item"   # 统计匹配元素数量
agent-browser get box @e1         # 获取边界盒
agent-browser get styles @e1      # 获取计算样式（字体、颜色、背景等）
```

## 状态检查

```bash
agent-browser is visible @e1      # 是否可见
agent-browser is enabled @e1      # 是否启用
agent-browser is checked @e1      # 是否已勾选
```

## 截图与 PDF

```bash
agent-browser screenshot          # 截图保存到临时目录
agent-browser screenshot path.png # 保存到指定路径
agent-browser screenshot --full   # 完整页面截图
agent-browser pdf output.pdf      # 保存为 PDF
```

## 等待

```bash
agent-browser wait @e1                     # 等待元素出现
agent-browser wait 2000                    # 等待毫秒
agent-browser wait --text "成功"           # 等待文本出现（或 -t）
agent-browser wait --url "**/dashboard"    # 等待 URL 匹配（或 -u）
agent-browser wait --load networkidle      # 等待网络空闲（或 -l）
agent-browser wait --fn "window.ready"     # 等待 JS 条件（或 -f）
```

## 录制视频

```bash
agent-browser record start ./demo.webm    # 开始录制
agent-browser record stop                 # 停止并保存
agent-browser record restart ./take2.webm # 停止当前 + 开始新录制
```

## 鼠标控制

```bash
agent-browser mouse move 100 200      # 移动鼠标
agent-browser mouse down left         # 按下按钮
agent-browser mouse up left           # 释放按钮
agent-browser mouse wheel 100         # 滚轮
```

## 语义定位器（@refs 不可用时的备选）

```bash
agent-browser find role button click --name "提交"
agent-browser find text "登录" click
agent-browser find text "登录" click --exact      # 精确匹配
agent-browser find label "邮箱" fill "user@test.com"
agent-browser find placeholder "搜索" type "关键词"
agent-browser find alt "Logo" click
agent-browser find testid "submit-btn" click
agent-browser find first ".item" click
agent-browser find last ".item" click
agent-browser find nth 2 "a" hover
```

## 浏览器设置

```bash
agent-browser set viewport 1920 1080          # 设置视口大小
agent-browser set device "iPhone 14"          # 设备模拟
agent-browser set geo 37.7749 -122.4194       # 设置地理位置
agent-browser set offline on                  # 离线模式
agent-browser set headers '{"X-Key":"v"}'     # 自定义 HTTP 头
agent-browser set credentials user pass       # HTTP Basic 认证
agent-browser set media dark                  # 深色模式
```

## Cookie 与存储

```bash
agent-browser cookies                     # 获取所有 Cookie
agent-browser cookies set name value      # 设置 Cookie
agent-browser cookies clear               # 清除 Cookie
agent-browser storage local               # 获取所有 localStorage
agent-browser storage local set k v       # 设置 localStorage 值
agent-browser storage local clear         # 清除 localStorage
```

## 网络拦截

```bash
agent-browser network route <url>              # 拦截请求
agent-browser network route <url> --abort      # 阻断请求
agent-browser network route <url> --body '{}'  # Mock 响应
agent-browser network unroute [url]            # 移除路由
agent-browser network requests                 # 查看请求记录
agent-browser network requests --filter api    # 过滤请求
```

## 多标签页与窗口

```bash
agent-browser tab                 # 列出所有标签页
agent-browser tab new [url]       # 新建标签页
agent-browser tab 2               # 切换到指定标签页
agent-browser tab close           # 关闭当前标签页
agent-browser window new          # 新建窗口
```

## Frame（iframe）

```bash
agent-browser frame "#iframe"     # 切换到 iframe
agent-browser frame main          # 返回主框架
```

## 对话框

```bash
agent-browser dialog accept [text]  # 确认对话框
agent-browser dialog dismiss        # 取消对话框
```

## JavaScript 执行

```bash
# 简单表达式直接执行
agent-browser eval 'document.title'

# 复杂 JS 用 heredoc（避免 shell 转义问题，推荐）
agent-browser eval --stdin <<'EVALEOF'
JSON.stringify(
  Array.from(document.querySelectorAll("a"))
    .map(a => ({ text: a.textContent, href: a.href }))
)
EVALEOF

# base64 编码（适合程序生成的脚本）
agent-browser eval -b "$(echo -n 'document.querySelectorAll("img").length' | base64)"
```

**规则：**
- 单行、无嵌套引号 → `eval 'expression'` 可用
- 含嵌套引号、箭头函数、模板字面量或多行 → 用 `eval --stdin <<'EVALEOF'`
- 程序生成的脚本 → 用 `eval -b` 加 base64

## 会话状态管理

```bash
agent-browser state save auth.json    # 保存 Cookie、存储、认证状态
agent-browser state load auth.json    # 恢复已保存状态
```

## 全局选项

```bash
agent-browser --session <name> ...    # 独立浏览器会话
agent-browser --json ...              # JSON 格式输出
agent-browser --headed ...            # 显示浏览器窗口（非无头模式）
agent-browser --full ...              # 完整页面截图（-f）
agent-browser --cdp <port> ...        # 通过 CDP 端口连接
agent-browser --auto-connect ...      # 自动发现本机运行中的 Chrome
agent-browser -p <provider> ...       # 云端浏览器提供商
agent-browser --proxy <url> ...       # 代理服务器
agent-browser --headers <json> ...    # HTTP 头（限该 origin）
agent-browser --profile <path> ...    # 持久化 Profile 目录
agent-browser --ignore-https-errors   # 忽略 SSL 证书错误
agent-browser --allow-file-access     # 允许访问本地 file:// 文件
agent-browser --version               # 显示版本（-V）
agent-browser <command> --help        # 显示命令详细帮助
```

## 调试

```bash
agent-browser --headed open example.com   # 显示浏览器窗口
agent-browser --cdp 9222 snapshot         # 通过 CDP 端口连接
agent-browser console                     # 查看控制台消息
agent-browser errors                      # 查看页面错误
agent-browser highlight @e1               # 高亮元素
agent-browser trace start                 # 开始录制 trace
agent-browser trace stop trace.zip        # 停止并保存 trace
```

## 删除 GitHub SSH/Deploy Key

> 适用场景：GitHub 仓库 Settings → Deploy keys 页面中的"Delete"按钮无法点击或删除失效。

### 基本删除流程（浏览器操作）

```bash
# 1. 加载已登录状态并打开 Deploy Keys 页面
agent-browser state load auth.json
agent-browser open https://github.com/OWNER/REPO/settings/keys
agent-browser wait --load networkidle
agent-browser snapshot -i

# 2. 获取页面所有 key 的指纹和删除链接（用于定位目标）
# 注意：以下选择器基于当前 GitHub DOM 结构，如 GitHub 更新 UI 需重新检查并调整
agent-browser eval --stdin <<'EVALEOF'
JSON.stringify(
  Array.from(document.querySelectorAll('.key-list-item, [data-target="deploy-key"]'))
    .map((el, i) => ({
      index: i,
      fingerprint: el.querySelector('.fingerprint, code, [data-fingerprint]')?.textContent?.trim(),
      deleteHref: el.querySelector('a[data-confirm], button[data-confirm], a[href*="delete"]')?.getAttribute('href')
    }))
)
EVALEOF

# 3. 点击目标 key 对应的 Delete 按钮，再确认
agent-browser find text "Delete" click
agent-browser wait --load networkidle
agent-browser snapshot -i
# 若弹出 JS confirm 对话框：
agent-browser dialog accept
# 若为页面内确认按钮（modal）：
agent-browser find role button click --name "Delete"
agent-browser wait --load networkidle
```

### 直接导航到删除 URL（跳过 UI 按钮）

```bash
# 先获取 key ID 列表
agent-browser eval --stdin <<'EVALEOF'
JSON.stringify(
  Array.from(document.querySelectorAll('form[action*="/keys/"]'))
    .map(f => ({ action: f.action, method: f.querySelector('[name="_method"]')?.value }))
)
EVALEOF

# 用目标 key 的删除 URL 直接提交（POST + _method=DELETE）
agent-browser eval --stdin <<'EVALEOF'
const form = document.querySelector('form[action="/OWNER/REPO/settings/keys/KEY_ID"]');
if (form) form.submit();
EVALEOF
agent-browser wait --load networkidle
agent-browser snapshot -i
```

### 通过 GitHub CLI 强制删除（UI 彻底失效时的回退方案）

```bash
# 列出所有 deploy key 及其 ID
gh api /repos/OWNER/REPO/keys \
  --jq '.[] | "\(.id)\t\(.title)\t\(.created_at)"'

# 若需按指纹匹配，计算每个 key 的 SHA256 指纹（ssh-keygen 方式）
gh api /repos/OWNER/REPO/keys | jq -r '.[] | "\(.id) \(.title) \(.key)"' | \
  while IFS=' ' read -r id title key; do
    fp=$(echo "$key" | ssh-keygen -lf /dev/stdin 2>/dev/null | awk '{print $2}')
    echo "id=$id  title=$title  fingerprint=$fp"
  done

# 按 ID 删除指定 key（ID 由上一步获取）
gh api -X DELETE /repos/OWNER/REPO/keys/{KEY_ID}

# 批量删除所有 deploy key（谨慎使用）
gh api /repos/OWNER/REPO/keys --jq '.[].id' | \
  xargs -I{} gh api -X DELETE /repos/OWNER/REPO/keys/{}

# 验证删除结果
gh api /repos/OWNER/REPO/keys | jq 'length'
```

### 通过 GitHub API（curl）删除用户 SSH Key

```bash
# 列出用户 SSH key
gh api /user/keys --jq '.[] | "\(.id)\t\(.title)\t\(.created_at)"'

# 删除指定用户 SSH key
gh api -X DELETE /user/keys/{KEY_ID}
```

### 常见错误排查

| 错误 | 原因 | 解决方案 |
|------|------|----------|
| `404 Not Found` | key 已被删除 | 无需处理，确认已消失即可 |
| `403 Forbidden` | Token 权限不足 | 确保 PAT 包含 `repo` scope（deploy key 操作需要此权限） |
| UI 按钮无反应 | 页面 JS 未加载 | `agent-browser wait --load networkidle` 后重试，或用 CLI 回退方案 |
| 删除后 key 重现 | CI/CD 自动重建 | 检查 Actions/外部系统是否在重新注册 key |

## 环境变量

```bash
AGENT_BROWSER_SESSION="mysession"            # 默认会话名
AGENT_BROWSER_EXECUTABLE_PATH="/path/chrome" # 自定义浏览器路径
AGENT_BROWSER_PROVIDER="browserbase"         # 云端浏览器提供商
AGENT_BROWSER_AUTO_CONNECT="true"            # 自动连接本机 Chrome
AGENT_BROWSER_STREAM_PORT="9223"             # WebSocket 流端口
```

## iOS 模拟器（需 Xcode + Appium）

```bash
agent-browser -p ios device list                          # 列出可用模拟器
agent-browser -p ios open https://example.com             # 默认 iPhone
agent-browser -p ios --device "iPhone 16 Pro" open <url>  # 指定设备
agent-browser -p ios snapshot -i                          # 同桌面端工作流
agent-browser -p ios tap @e1                              # 触摸（click 别名）
agent-browser -p ios swipe up                             # 手势滑动
agent-browser -p ios screenshot mobile.png
agent-browser -p ios close                                # 关闭模拟器
```
