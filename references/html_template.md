# HTML 报表模板

## 设计规范

- 色调：深蓝渐变头部 → 白色卡片内容区
- 配色：主色 `#4F6EF7`，辅助色 `#6C5CE7`、`#00B894`、`#FF7675`、`#FDCB6E`
- 字体：系统默认字体栈，中文优先 PingFang SC
- 圆角卡片风格，柔和阴影

## 完整模板

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>腾讯会议报表 — {{REPORT_DATE}}</title>
<style>
  :root {
    --primary: #4F6EF7;
    --secondary: #6C5CE7;
    --success: #00B894;
    --danger: #FF7675;
    --warning: #FDCB6E;
    --bg: #F5F6FA;
    --card-bg: #FFFFFF;
    --text: #2D3436;
    --text-secondary: #636E72;
    --border: #E8ECF1;
  }
  * { margin: 0; padding: 0; box-sizing: border-box; }
  body {
    font-family: -apple-system, BlinkMacSystemFont, "PingFang SC", "Microsoft YaHei", sans-serif;
    background: var(--bg);
    color: var(--text);
    line-height: 1.6;
  }

  /* ── Header ── */
  .header {
    background: linear-gradient(135deg, #2D3561 0%, #4F6EF7 50%, #6C5CE7 100%);
    color: #fff;
    padding: 48px 32px 40px;
    text-align: center;
    position: relative;
    overflow: hidden;
  }
  .header::after {
    content: '';
    position: absolute;
    top: -50%; left: -50%;
    width: 200%; height: 200%;
    background: radial-gradient(circle at 30% 70%, rgba(255,255,255,0.06) 0%, transparent 50%),
                radial-gradient(circle at 70% 30%, rgba(255,255,255,0.04) 0%, transparent 50%);
    pointer-events: none;
  }
  .header h1 {
    font-size: 28px;
    font-weight: 700;
    margin-bottom: 8px;
    position: relative;
    z-index: 1;
    letter-spacing: 1px;
  }
  .header .subtitle {
    font-size: 14px;
    opacity: 0.8;
    position: relative;
    z-index: 1;
  }

  /* ── Container ── */
  .container {
    max-width: 960px;
    margin: -20px auto 40px;
    padding: 0 24px;
    position: relative;
    z-index: 2;
  }

  /* ── Overview Cards ── */
  .overview-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    gap: 16px;
    margin-bottom: 24px;
  }
  .overview-card {
    background: var(--card-bg);
    border-radius: 12px;
    padding: 20px;
    text-align: center;
    box-shadow: 0 2px 12px rgba(0,0,0,0.06);
    transition: transform 0.2s;
  }
  .overview-card:hover { transform: translateY(-2px); }
  .overview-card .label {
    font-size: 13px;
    color: var(--text-secondary);
    margin-bottom: 8px;
  }
  .overview-card .value {
    font-size: 28px;
    font-weight: 700;
    color: var(--primary);
  }
  .overview-card .unit {
    font-size: 14px;
    font-weight: 400;
    color: var(--text-secondary);
  }

  /* ── Section ── */
  .section {
    background: var(--card-bg);
    border-radius: 12px;
    padding: 24px;
    margin-bottom: 20px;
    box-shadow: 0 2px 12px rgba(0,0,0,0.06);
  }
  .section-title {
    font-size: 18px;
    font-weight: 700;
    margin-bottom: 16px;
    padding-bottom: 12px;
    border-bottom: 2px solid var(--border);
    display: flex;
    align-items: center;
    gap: 8px;
  }
  .section-title .icon { font-size: 20px; }

  /* ── Tables ── */
  table {
    width: 100%;
    border-collapse: collapse;
    font-size: 14px;
  }
  th {
    text-align: left;
    padding: 10px 12px;
    background: #F8F9FC;
    color: var(--text-secondary);
    font-weight: 600;
    font-size: 13px;
    border-bottom: 2px solid var(--border);
  }
  td {
    padding: 12px;
    border-bottom: 1px solid var(--border);
  }
  tr:last-child td { border-bottom: none; }
  tr:hover td { background: #F8F9FC; }

  /* ── Badges ── */
  .badge {
    display: inline-block;
    padding: 3px 10px;
    border-radius: 20px;
    font-size: 12px;
    font-weight: 600;
  }
  .badge-success { background: #E6F9F3; color: #00B894; }
  .badge-danger { background: #FFF0F0; color: #FF7675; }
  .badge-warning { background: #FFF8E7; color: #E5A100; }
  .badge-info { background: #EEF1FF; color: #4F6EF7; }

  /* ── Heatmap ── */
  .heatmap-row {
    display: flex;
    align-items: center;
    gap: 12px;
    padding: 6px 0;
    border-bottom: 1px solid var(--border);
  }
  .heatmap-row:last-child { border-bottom: none; }
  .heatmap-date {
    width: 80px;
    font-weight: 600;
    font-size: 13px;
  }
  .heatmap-bar-wrap {
    flex: 1;
    height: 28px;
    background: #F0F2F8;
    border-radius: 6px;
    overflow: hidden;
    position: relative;
  }
  .heatmap-bar {
    height: 100%;
    border-radius: 6px;
    transition: width 0.5s ease;
    display: flex;
    align-items: center;
    padding-left: 10px;
    font-size: 12px;
    color: #fff;
    font-weight: 600;
    min-width: fit-content;
  }
  .heatmap-count {
    width: 70px;
    text-align: right;
    font-size: 13px;
    color: var(--text-secondary);
  }

  /* ── Empty State ── */
  .empty-state {
    text-align: center;
    padding: 32px;
    color: var(--text-secondary);
    font-size: 14px;
  }

  /* ── Footer ── */
  .footer {
    text-align: center;
    padding: 24px;
    color: var(--text-secondary);
    font-size: 12px;
  }
  .footer span { margin: 0 12px; }

  /* ── Highlight ── */
  .highlight-row { background: #FFFDF5; }
  .highlight-row td { font-weight: 500; }

  .subject-link {
    color: var(--primary);
    text-decoration: none;
    font-weight: 600;
  }
  .subject-link:hover { text-decoration: underline; }

  .tag-row {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    margin-top: 12px;
  }
</style>
</head>
<body>

<div class="header">
  <h1>腾讯会议数据报表</h1>
  <div class="subtitle">{{REPORT_PERIOD}}</div>
</div>

<div class="container">

  <!-- 一、总览卡片 -->
  <div class="overview-grid">
    <div class="overview-card">
      <div class="label">会议总数</div>
      <div class="value">{{TOTAL_COUNT}}<span class="unit"> 场</span></div>
    </div>
    <div class="overview-card">
      <div class="label">累计时长</div>
      <div class="value">{{TOTAL_HOURS}}<span class="unit"> 小时</span></div>
    </div>
    <div class="overview-card">
      <div class="label">日均会议</div>
      <div class="value">{{AVG_DAILY}}<span class="unit"> 场/天</span></div>
    </div>
    <div class="overview-card">
      <div class="label">录制覆盖</div>
      <div class="value">{{RECORD_RATE}}<span class="unit">%</span></div>
    </div>
    <div class="overview-card">
      <div class="label">智能纪要</div>
      <div class="value">{{SMART_MINUTES}}<span class="unit"> 场</span></div>
    </div>
    <div class="overview-card">
      <div class="label">我发起的</div>
      <div class="value" style="color: #00B894;">{{HOST_COUNT}}<span class="unit"> 场</span></div>
    </div>
    <div class="overview-card">
      <div class="label">我参与的</div>
      <div class="value" style="color: #FDCB6E;">{{ATTEND_COUNT}}<span class="unit"> 场</span></div>
    </div>
  </div>

  <!-- 二、会议类型分布 -->
  <div class="section">
    <div class="section-title"><span class="icon">📊</span>会议类型分布</div>
    <table>
      <thead><tr><th>类型</th><th>场次</th><th>占比</th><th>累计时长</th></tr></thead>
      <tbody>
        {{TYPE_ROWS}}
      </tbody>
    </table>
  </div>

  <!-- 三、每日会议热力图 -->
  <div class="section">
    <div class="section-title"><span class="icon">📅</span>每日会议分布</div>
    {{HEATMAP_ROWS}}
  </div>

  <!-- 四、会议明细 -->
  <div class="section">
    <div class="section-title"><span class="icon">📋</span>会议明细</div>
    <table>
      <thead><tr><th>#</th><th>主题</th><th>日期</th><th>时间</th><th>时长</th><th>类型</th><th>角色</th><th>会议号</th><th>录制</th><th>纪要</th></tr></thead>
      <tbody>
        {{MEETING_ROWS}}
      </tbody>
    </table>
  </div>

  <!-- 五、可复盘会议 -->
  <div class="section">
    <div class="section-title"><span class="icon">💡</span>可复盘会议</div>
    {{REVIEW_ROWS}}
  </div>

</div>

<div class="footer">
  <span>数据来源：腾讯会议 API</span>
  <span>生成时间：{{GENERATE_TIME}}</span>
  <span>追踪 ID：{{TRACE_ID}}</span>
</div>

</body>
</html>
```

## 变量替换说明

生成 HTML 时将 `{{变量}}` 替换为实际数据：

| 变量 | 说明 | 示例 |
|------|------|------|
| `{{REPORT_DATE}}` | title 中的日期 | `2026-07-21` |
| `{{REPORT_PERIOD}}` | 统计周期描述 | `2026年7月21日` 或 `2026年7月1日 — 2026年7月21日` |
| `{{TOTAL_COUNT}}` | 会议总数 | `3` |
| `{{TOTAL_HOURS}}` | 累计时长(小时，保留1位小数) | `4.0` |
| `{{AVG_DAILY}}` | 日均会议数 | `3.0` |
| `{{RECORD_RATE}}` | 录制覆盖率(整数) | `0` |
| `{{SMART_MINUTES}}` | 有智能纪要的会议数 | `0` |
| `{{HOST_COUNT}}` | 我发起的会议数 | `2` |
| `{{ATTEND_COUNT}}` | 我参与的会议数 | `1` |
| `{{TYPE_ROWS}}` | 会议类型分布表格行 | `<tr><td>普通会议</td><td>3</td><td>100%</td><td>4h 0m</td></tr>` |
| `{{HEATMAP_ROWS}}` | 热力图行（单日或每日） | 见下方说明 |
| `{{MEETING_ROWS}}` | 会议明细表格行 | 见下方说明 |
| `{{REVIEW_ROWS}}` | 可复盘会议行或空状态提示 | 见下方说明 |
| `{{GENERATE_TIME}}` | 报表生成时间 | `2026-07-22 20:57` |
| `{{TRACE_ID}}` | 追踪 ID | `77c28d1cdc...` |

## 热力图行格式

单日报表（只有一天）：
```html
<div class="heatmap-row">
  <div class="heatmap-date">07-21</div>
  <div class="heatmap-bar-wrap"><div class="heatmap-bar" style="width: 60%; background: linear-gradient(90deg, #FDCB6E, #FF7675);">3场 / 4h</div></div>
  <div class="heatmap-count">🔥 密集</div>
</div>
```

多日报表（多个热力图行，按天排列）。

颜色规则：
- 0场 → 灰色 `#B2BEC3`，宽度 2%，文字"无"
- 1-2场 → 蓝色渐变 `#4F6EF7 → #6C5CE7`，宽度按比例(最大场次为100%)
- 3-4场 → 黄橙渐变 `#FDCB6E → #FF7675`，密度标记 🔥
- 5+场 → 红色渐变 `#FF7675 → #D63031`，密度标记 💥

## 会议明细行格式

```html
<tr>
  <td>1</td>
  <td><span class="subject-link">智能体训练营DAY3</span></td>
  <td>07-21</td>
  <td>20:00 - 22:00</td>
  <td>2h</td>
  <td><span class="badge badge-info">普通会议</span></td>
  <td><span class="badge badge-warning">我参与的</span></td>
  <td>792330725</td>
  <td><span class="badge badge-danger">无录制</span></td>
  <td><span class="badge badge-danger">无</span></td>
</tr>
```

角色列：`badge-success`（我发起的）/ `badge-warning`（我参与的）

录制列：✅ → `badge-success`，❌ → `badge-danger`
纪要列：✅ → `badge-success`，❌ → `badge-danger`

## 可复盘会议格式

有可复盘会议时：
```html
<table>
  <thead><tr><th>#</th><th>主题</th><th>日期</th><th>会议号</th><th>推荐理由</th></tr></thead>
  <tbody>
    <tr class="highlight-row">
      <td>1</td><td>智能体训练营DAY3</td><td>07-21</td><td>792330725</td>
      <td><span class="badge badge-success">有智能纪要可复盘</span></td>
    </tr>
  </tbody>
</table>
```

无可复盘会议时：
```html
<div class="empty-state">此时间段内的会议暂无录制和智能纪要，建议开启云录制以便后续复盘。如需查看已有录制，可前往 <a href="https://meeting.tencent.com" target="_blank">腾讯会议官网</a>。</div>
```

## 会议时长计算

- 按 `end_time - start_time` 计算总分钟数
- 不足60分钟显示为 "Xm"（如 "45m"）
- 整小时显示为 "Xh"（如 "2h"）
- 其他显示为 "Xh Ym"（如 "1h 30m"）
