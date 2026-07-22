# 腾讯会议数据报表 V3.0

[![SkillHub](https://img.shields.io/badge/SkillHub-install-blue)](https://skillhub.cn)

根据指定时间范围，自动拉取腾讯会议历史数据，生成精美的 HTML 网页报表。

## V3.0 新特性

**智能纪要嵌入**：有录制且有智能纪要的会议，可直接在报表中点击「查看纪要」展开完整内容，无需跳转外部页面。

| 功能 | V2.0 | V3.0 |
|------|------|------|
| 会议总览卡片 | ✅ | ✅ |
| 类型分布 | ✅ | ✅ |
| 每日热力图 | ✅ | ✅ |
| 会议明细 | ✅ | ✅ |
| 可复盘会议推荐 | ✅ | ✅ |
| 智能纪要内嵌阅读 | ❌ | ✅ 点击展开/收起 |
| 角色标识（发起/参与） | ✅ | ✅ |

## 效果预览

| 总览卡片 | 会议明细 |
|----------|----------|
| 会议总数、累计时长、日均会议、录制覆盖率、智能纪要 | 每场会议的主题、时间、时长、录制/纪要状态 |

| 类型分布 | 每日热力图 | 可复盘会议 |
|----------|------------|------------|
| 普通会议/周期性/个人会议室占比 | 按天展示，emoji 密度标记 | 有智能纪要的会议高亮推荐，点击展开全文 |

## 安装

```bash
# 1. 确保已安装 tencent-meeting-skill
skillhub install tencent-meeting-skill --dir ~/.claude/skills/

# 2. 安装本报表 skill
skillhub install tencent-meeting-report --dir ~/.claude/skills/

# 3. 配置腾讯会议 Token
export TENCENT_MEETING_TOKEN=<你的token>
```

## 使用

```
# 单日报表
生成本周腾讯会议报表

# 月度报表
帮我统计 7 月的会

# 自定义范围
生成 2026-06-01 到 2026-07-21 的会议报表
```

## 依赖

- `tencent-meeting-skill` v1.0.11+
- `TENCENT_MEETING_TOKEN` 环境变量（从 https://meeting.tencent.com/ai-skill 获取）

## 许可证

MIT
