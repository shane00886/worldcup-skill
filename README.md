# ⚽ Worldcup Bot Skills

世界杯机器人的 WorkBuddy 技能集合。这些技能记录了本项目开发过程中积累的可复用工作流。

## 📦 技能列表

| 技能 | 描述 | 适用场景 |
|------|------|---------|
| [railway-deploy-worldcup-bot](railway-deploy-worldcup-bot/) | 部署世界杯机器人到 Railway Cloud | 修改代码后需要更新线上服务 |
| [feishu-bot-card](feishu-bot-card/) | 飞书机器人卡片消息构建 | 需要发送飞书卡片消息时 |
| [feishu-app-publish](feishu-app-publish/) | 飞书应用上架发布 | 需要提交飞书应用审核时 |

## 🛠️ 安装方式

将需要的技能目录复制到 WorkBuddy 技能目录：

```bash
# 用户级技能
cp -r railway-deploy-worldcup-bot ~/.workbuddy/skills/
cp -r feishu-bot-card ~/.workbuddy/skills/
cp -r feishu-app-publish ~/.workbuddy/skills/
```

或单个安装：

```bash
cp -r railway-deploy-worldcup-bot ~/.workbuddy/skills/
```

## 📋 技能开发说明

### 技能结构

每个技能包含：

```
skill-name/
├── SKILL.md          # 技能描述和工作流程（必需）
├── scripts/          # 可执行代码（可选）
├── references/       # 参考文档（可选）
└── assets/           # 资源文件（可选）
```

### SKILL.md 格式

```yaml
---
name: skill-name
description: 简短描述何时使用此技能
agent_created: true
---
```

## 📄 License

MIT
