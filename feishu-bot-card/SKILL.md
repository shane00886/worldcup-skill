---
name: feishu-bot-card
description: 飞书机器人卡片消息构建和发送。当需要向飞书群发送消息卡片（新闻、预测、积分榜等）时使用此技能。
agent_created: true
---

# 飞书机器人卡片消息构建

## 核心函数

### 获取 Access Token

```python
from worldcup_card import get_access_token
token = get_access_token()  # 返回 tenant_access_token
```

### 构建卡片

支持的卡片类型：

| 函数 | 用途 | 卡片类型 |
|------|------|---------|
| `build_news_card(news_data, prediction_summary)` | 早间新闻推送 | blue header |
| `build_prediction_card(predictions, team_analysis, championship_odds)` | 晚间预测推送 | purple header |
| `build_status_card(status_data)` | 实时状态更新 | green header |
| `build_reply_card(reply_text)` | 用户回复 | 自动检测类型 |

### 发送卡片

```python
from worldcup_card import send_card, build_reply_card

card = build_reply_card("查询结果文本")
result = send_card(card)
# result["data"]["message_id"] 包含消息ID
```

## 卡片组件

### 基础组件

```python
_text(tag, content, lines=None)     # 文本组件
_div(text_elems, extra=None)        # div模块
_hr()                               # 分割线
_note(content)                      # 备注
_action(btns)                       # 按钮组
_btn(text, url, type="default")     # 按钮（primary/default）
```

### 常用的卡片头部颜色

```python
{"blue": "赛程", "indigo": "积分榜", "gold": "夺冠",
 "green": "新闻", "purple": "分析", "grey": "搜索"}
```

## 权限要求

| 权限 | 用途 |
|------|------|
| `im:message:send` | 发送消息 |
| `im:message:send_as_bot` | 以机器人身份发送 |
| `im:message.group_msg` | 读取群消息（轮询模式需要） |

## 常见错误

### code=99991663 (Token过期)
重新获取 token 即可，`get_access_token()` 每次调用都获取新 token。

### code=99991672 (权限不足)
检查应用是否已开启 `im:message:send` 和 `im:message:send_as_bot` 权限。
在飞书开放平台 → 应用详情 → 权限管理 → 添加权限。
