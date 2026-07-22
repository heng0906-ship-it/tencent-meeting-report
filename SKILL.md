---
name: tencent-meeting-report
description: 腾讯会议数据报表。根据用户指定的时间范围（如"本周""本月""7月""2026-06-01到2026-07-21"），自动拉取腾讯会议历史会议数据并生成精美的 HTML 网页报表，涵盖会议总览、类型分布、每日热力图、会议明细及智能纪要摘要。当用户提到腾讯会议报表、会议统计、会议复盘、这个月开了多少会、会议数据分析时使用。
---

# 腾讯会议数据报表

## 概述

通过腾讯会议 MCP 工具拉取指定时间范围的会议数据，生成精美的 HTML 网页报表并自动在浏览器中打开。

## 前置依赖

- 已安装 `tencent-meeting-skill`，位于 `~/.claude/skills/tencent-meeting-skill/`
- 环境变量 `TENCENT_MEETING_TOKEN` 已设置
- 调用脚本：`python3 ~/.claude/skills/tencent-meeting-skill/scripts/tencent_meeting.py tools/call '<json>'`

## 工作流

### 第一步：解析时间范围

用户输入时间范围，首先获取 TENCENT_MEETING_TOKEN 环境变量，然后调用 `convert_timestamp` 获取当前时间并推算日期范围：

```
python3 ~/.claude/skills/tencent-meeting-skill/scripts/tencent_meeting.py tools/call '{"name":"convert_timestamp","arguments":{"_client_info":{"os":"darwin","agent":"claude_code","model":"deepseek-v4-pro"}}}'
```

**时间推算规则：**
- "本周" → time_week_str 至今
- "本月" → 当月1日至今
- "上月" → 上个月1日至最后一天
- "7月" → 当年7月1日至31日
- "2026-06-01到2026-07-21" → 直接使用
- 默认不传时间时，按最近7天处理

### 第二步：拉取会议列表

调用 `get_user_ended_meetings` 拉取已结束会议（时间范围不超过31天则一次拉取；超过则按31天分段拉取，每段一个调用）：

```
python3 ~/.claude/skills/tencent-meeting-skill/scripts/tencent_meeting.py tools/call '{"name":"get_user_ended_meetings","arguments":{"start_time":"<ISO开始时间>","end_time":"<ISO结束时间>","page_size":30,"_client_info":{"os":"darwin","agent":"claude_code","model":"deepseek-v4-pro"}}}'
```

同时拉取进行中/未开始会议：

```
python3 ~/.claude/skills/tencent-meeting-skill/scripts/tencent_meeting.py tools/call '{"name":"get_user_meetings","arguments":{"page_size":30,"_client_info":{"os":"darwin","agent":"claude_code","model":"deepseek-v4-pro"}}}'
```

翻页：若 `has_more` 为 true，用 `next_page_token` 继续拉取。

### 第三步：查询录制覆盖

对每场会议调用 `get_records_list` 检查录制状态，同时尝试获取智能纪要：

```
python3 ~/.claude/skills/tencent-meeting-skill/scripts/tencent_meeting.py tools/call '{"name":"get_records_list","arguments":{"meeting_id":"<meeting_id>","page_size":10,"_client_info":{"os":"darwin","agent":"claude_code","model":"deepseek-v4-pro"}}}'
```

### 第四步：生成 HTML 报表

**必须生成 HTML 文件**，读取 `references/html_template.md` 获取完整 HTML 模板和样式规范。将数据填入模板，保存为 `.html` 文件，然后用 `open` 命令在浏览器中打开。

保存路径：`/tmp/tencent-meeting-report-<日期>.html`

```bash
open /tmp/tencent-meeting-report-<日期>.html
```

## HTML 设计要求

- **不要用 Markdown 输出**，必须生成 HTML 文件
- 阅读 `references/html_template.md` 获取完整的 HTML/CSS 模板
- 将会议数据填入模板，保持设计一致
- 单日报表显示"每日热力图"部分的当天详情
- 多日报表显示完整热力图
- 所有数据直接写在 HTML 中，不需要额外 JS 请求

## 注意事项

- 所有脚本调用前必须 `export TENCENT_MEETING_TOKEN`
- 时长超过 31 天的范围需分段拉取
- 时间展示使用北京时间 `YYYY-MM-DD HH:MM`
- 追踪 ID 在报表末尾展示
- 生成后自动 `open` 打开浏览器
