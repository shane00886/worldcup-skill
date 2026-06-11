---
name: worldcup-bot-security
description: 世界杯机器人安全配置和隔离指南。处理与凭据管理、环境隔离、入侵防护相关的任务时使用此技能。
agent_created: true
---

# 世界杯机器人安全隔离指南

## 核心原则

**源码中不包含任何 Token/Secret。** 所有敏感信息仅通过环境变量传入。

## 凭据管理

### 敏感配置清单

| 环境变量 | 用途 | 获取方式 |
|---------|------|---------|
| `WC_API_TOKEN` | Worldcup26.ir API Token | https://worldcup26.ir 免费注册 |
| `FS_APP_ID` | 飞书应用 App ID | 飞书开放平台 → 应用详情 |
| `FS_APP_SECRET` | 飞书应用 Secret | 飞书开放平台 → 应用详情 |
| `FS_CHAT_ID` | 推送目标群聊 ID | 飞书 API 查询 |
| `DASHBOARD_URL` | 仪表盘链接（可选） | 自行部署 |
| `FEISHU_BASE` | 飞书 API 基础地址（可选） | 国内飞书: open.feishu.cn, 海外: open.larksuite.com |

### 本地开发

1. 复制 `.env.example` 为 `.env`
2. 填入真实值
3. `.env` 已被 `.gitignore` 排除，不会提交
4. `.env.example` 中包含占位符，不包含真实凭据

### 云端部署

Railway 环境变量管理：
```
Railway Dashboard → Project → Variables → New Variable
```

### 安全检查清单

- [ ] `worldcup_config.py` 中没有硬编码的 Token/Secret
- [ ] `.env` 在 `.gitignore` 中
- [ ] GitHub 仓库中没有 `.env` 文件
- [ ] Railway 环境变量已正确设置
- [ ] 旧 Git 提交中不包含凭据（如有，需用 `git filter-branch` 清理）

## 架构隔离

### 部署隔离

```
用户/黑客 ──→ Railway Cloud (Docker) ──→ 飞书 API
                 │
                 └── 无法访问本地电脑
```

- 服务运行在 Railway 容器中，与本地网络完全隔离
- 容器内不包含本地文件系统访问权限
- Webhook 仅接收飞书平台的事件回调请求

### 输入验证

- 飞书 Webhook 接收消息时验证 event token
- 所有用户输入经过博彩关键词过滤
- API 请求超时设置（10-15 秒）

## 应急响应

### 怀疑凭据泄露
1. 立即在飞书开放平台重置 App Secret
2. 更新 Railway 环境变量
3. 更新本地 `.env` 文件
4. 如旧 Git 提交中包含凭据 → 使用 `git filter-branch` 清理

### 服务异常
1. 检查 Railway 日志
2. 验证健康检查 `/health`
3. 检查飞书 Token 是否过期

## 参考资料

- `.env.example` — 环境变量配置模板
- `.gitignore` — 排除敏感文件
- `worldcup_config.py` — 配置加载逻辑
