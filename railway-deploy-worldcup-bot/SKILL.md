---
name: railway-deploy-worldcup-bot
description: 部署世界杯机器人项目到 Railway Cloud。当用户完成代码修改后要求部署或更新线上服务时使用此技能。
agent_created: true
---

# Railway 部署世界杯机器人

## 前置检查

1. 确保 `worldcup_config.py` 中的飞书凭据（`FS_APP_ID`, `FS_APP_SECRET`）是最新正确的
2. 确认 Railway CLI 已安装且可用

## 部署步骤

### 1. 清除代理环境变量

Railway CLI 会被本地的 HTTP 代理干扰，必须在部署前清除：

```bash
unset NODE_OPTIONS HTTP_PROXY HTTPS_PROXY http_proxy https_proxy
```

### 2. 执行部署

```bash
cd /Users/shane/WorkBuddy/worldcup-bot
railway up
```

### 3. 验证部署

等待部署完成（约 60 秒），然后验证：

```bash
curl -s https://worldcup-bot-production-6e70.up.railway.app/health
```
预期返回: `{"status":"ok","time":"..."}`

### 4. 验证飞书功能

```bash
cd /Users/shane/WorkBuddy/worldcup-bot && unset http_proxy https_proxy HTTP_PROXY HTTPS_PROXY && python3 -c "
from worldcup_card import get_access_token, send_card, build_reply_card
token = get_access_token()
print(f'Token OK: {token[:15]}...')
card = build_reply_card('部署验证 - 服务正常运行')
result = send_card(card)
print(f'发送成功: {result.get(\"data\",{}).get(\"message_id\",\"\")[:20]}...')
"
```

### 5. 检查服务状态

```bash
railway status
```
确认输出中包含 `status: ● Online`。

## 常见问题

### 部署后群聊无响应
- 检查 `worldcup_config.py` 中的 `FS_APP_ID` 和 `FS_APP_SECRET` 是否正确
- 确认没有忘记运行 `railway up` 部署最新代码
- 检查飞书开放平台的事件回调 URL 是否指向 Railway 服务地址

### 部署失败
- 确保已清除代理环境变量（第 1 步）
- 检查 Dockerfile 和 requirements.txt 是否有语法错误
- 查看 Railway 构建日志

### 飞书 Token 获取失败
- 确认 App ID 和 Secret 与飞书开放平台一致
- 检查应用是否已开启所需权限（`im:message:send`, `im:message.group_msg` 等）

## 项目信息

- 项目路径: `/Users/shane/WorkBuddy/worldcup-bot/`
- Railway 项目名: exquisite-playfulness
- 服务域名: https://worldcup-bot-production-6e70.up.railway.app
- 健康检查端点: /health
- Webhook 端点: /webhook/feishu
