---
name: hojun-teardown
description: >-
  Deep-dive competitive teardown of any mobile app. Produces a structured 5-chapter Chinese
  markdown report covering team/background, ad creative strategy, product features, user
  reviews, and strategic insights — with an optional scoring table and up to 7 supplementary
  chapters. Activated ONLY when the app-teardown role is active; never keyword-triggered.
  Distinct from app-walkthrough (which does live UX emulation via Appetize.io). This skill
  does open research: store data, web search, review mining, media signals.
allowed-tools: WebFetch, WebSearch, Bash, Read, Write
model: sonnet
---

# App Teardown Skill (v1.0)

对任意移动 App 进行深度竞品走查，输出结构化中文报告。

---

## 定位 & 与 app-walkthrough 的边界

| 维度 | app-teardown（本技能）| app-walkthrough |
|------|----------------------|-----------------|
| 工作方式 | 开放性研究：商店、搜索、媒体、评论 | 云端 emulator 实时操作 |
| 主要产出 | 5 章结构化中文报告 | UX 流程截图 + 体验分析 |
| 需要 APK | 否 | 是 |
| 适合问题 | 商业模式、增长路径、用户口碑 | 界面交互、操作流、界面细节 |
| 触发方式 | 仅 app-teardown 角色激活时 | 独立技能，关键词触发 |

---

## 触发机制 — 仅在 app-teardown 角色下工作，关键词不触发

本技能**不响应**任何关键词。只有在 `app-teardown` 角色（`/role app-teardown`）激活后，
agent 才进入走查模式，并将后续用户输入解析为走查请求。

---

## 输入要求

```
<App 名称> <App Store 或 Google Play 链接> [--focus <重点章节>] [--vs <对比 App 名>]
```

- **App 名称**：用于命名输出文件夹和报告标题
- **商店链接**：支持 App Store（`apps.apple.com`）或 Google Play（`play.google.com`）
- `--focus`（可选）：指定某章节深入研究，如 `--focus 投放素材`
- `--vs`（可选）：并行拉取对比 App 数据，报告末尾加对比表

---

## 8 步工作流

### Step 1 — 解析输入

提取 App 名、商店链接类型（iOS / Android）、App ID、可选参数。

### Step 2 — 抓取商店元数据

运行 `scripts/fetch-store.js <url>`，获取：
- App 名、开发者、评分、评价数、分类、更新日期、版本、描述、截图数量

### Step 3 — 抓取评论

- iOS：运行 `scripts/fetch-reviews.js <app-id> --country us --limit 50`
- Android：WebFetch 抓取 Google Play 评论页
- 同步抓取中文区评论（`--country cn`）如链接为国区产品

### Step 4 — 基础信息网络研究

多轮 WebSearch：
- `"<App名>" founder team CEO LinkedIn`
- `"<App名>" revenue ARR funding Crunchbase`
- `"<App名>" TechCrunch Forbes Inc site:techcrunch.com OR site:inc.com`
- `"<App名>" TikTok marketing growth strategy`

### Step 5 — 投放素材研究

- WebSearch: `"<App名>" ad creative TikTok Facebook Meta`
- WebFetch: 抓取 Meta Ad Library（如可访问）
- 归纳核心 Hook 公式、渠道分布、素材方向

### Step 6 — 产品功能分析

- WebFetch 商店描述 + What's New 历史
- WebSearch: `"<App名>" feature update changelog`
- 结合 Step 2 截图数量 + 描述推断 Onboarding 结构

### Step 7 — 评论挖掘 & 情感分析

对 Step 3 数据：
- 提取 Top-3 正向卖点（高频关键词）
- 提取 Top-3 负向痛点（高频抱怨）
- 查找差评爆发时间节点（评分骤降 + 媒体报道交叉验证）

### Step 8 — 撰写报告

按 `references/article-template.md` 结构输出完整中文 markdown 报告，存入：
`~/zylos/workspace/teardowns/<App名>-teardown-<YYYY-MM-DD>.md`

---

## 报告结构

### 顶部必含板块：核心结论速读（v1.1 起）

**放在数据速览之后、第一章之前**，作为整篇报告的"60 秒电梯版"。控制 250-400 字。结构：

- **<App 名> 是什么**：1-2 句话浓缩公司 + 商业体量 + 定位 + 综合评分
- **N 个产品赌注**（3-4 条 bullet，每条带 emoji + 关键时间 + 关键数据）
- **对 GlowMate 的 N 条核心启示**（3-5 条，每条「一句结论 → 一句行动」格式）
- **主要风险**（一行列出 3 个最严重负面信号，分号分隔）

> 这是为了让读者（曹洪军及团队、潜在的飞书/公众号读者）在 60 秒内 get 到本文核心论断，再决定是否往下细看。

### 5 章必含底线（每篇报告必须包含）

| # | 章节 | 核心内容 |
|---|------|---------|
| 1 | **基本信息** | 创始团队、创业起源、融资/收购情况、上线时间地区 |
| 2 | **投放素材** | 渠道占比、核心 Hook 公式、素材方向、用户洞察 |
| 3 | **产品内功能** | 核心使用闭环、差异化亮点、Onboarding 与付费墙设计 |
| 4 | **用户评价** | 商店评分、Top-3 正向卖点（含引言）、Top-3 负向痛点、差评时间线 |
| 5 | **思考与总结** | 产品/商业/增长亮点分析、对 GlowMate 的启示（如适用）|

> **注**：核心结论速读本质是第 5 章的浓缩前置，写法上必须与第 5 章保持一致 — 不要在速读里出现第 5 章里没有的论断。

### 7 个可选补充章节

根据研究所得数据丰富程度决定是否纳入（至少有 2 个实质性数据点才写该章）：

1. **国际化策略** — 多语言支持、出海重点市场、本地化程度
2. **创始人媒体形象** — 创始人公开发言、播客/访谈、个人品牌建设
3. **关键人才信号** — 近期关键岗位招聘（LinkedIn/Indeed），推断产品方向
4. **下一步预测** — 基于现有信号推断产品路线图或下一个增长动作
5. **平台政策风险** — App Store / Google Play 合规史、IAP 规则风险点
6. **关键事件黑天鹅** — 下架、泄露、媒体危机等重大事件复盘
7. **同名混淆风险** — 商标、商标争议、同名 App 竞争

### 综合评分表

每篇报告末尾必须附综合评分表（4 个维度，参见 `references/scoring-rubric.md`）：

| 维度 | 评分（/10）| 一句话理由 |
|------|-----------|----------|
| UX 体验 | — | — |
| 商业模式 | — | — |
| 增长能力 | — | — |
| 团队执行力 | — | — |
| **综合分** | — | — |

### 引用来源

所有引用按 `①②③...` 编号，集中列于报告末尾，格式：
`① 媒体名 — [标题](URL)`

### 免责声明（每篇必有，固定文字）

> **免责声明：** 本报告基于公开数据整理，不构成投资建议；数据截至报告日，可能随版本/政策变化失效。
> ARR、下载量、收购价等财务数据系第三方估算或媒体报道，实际数字以官方披露为准。

---

## 质量标准

- **有引用才能写数据**：无法溯源的数字必须标注"推测"或"未找到公开数据"
- **引用分级**：优先 S 级，降级时在文中注明（参见 `references/source-priority.md`）
- **不放大信号**：1 条 Reddit 帖子不能写成"用户普遍反映"
- **避免空洞结论**：每个亮点/痛点必须有至少 1 个具体例子（数字、引言或事件）
- **中文正文，代码/变量/文件路径用英文**

---

## 信源 S-D 五级

详见 `references/source-priority.md`。简要：
- **S**：官方 / 商店 / 招股书 / 官方公告
- **A**：TechCrunch / CNBC / Forbes / Bloomberg / Inc.
- **B**：Sensor Tower / Latka / Crunchbase / Similarweb / Superwall Case Study
- **C**：行业 blog / 专业分析师 newsletter / 创始人访谈（非主流媒体）
- **D**：Reddit / Trustpilot / Twitter 舆论 / 匿名论坛

---

## 错误处理

| 错误情况 | 处理方式 |
|---------|---------|
| 商店链接 404 / 无法访问 | 报告链接失效，建议用户核实，仍可用 WebSearch 补全 |
| fetch-store.js 返回 error 字段 | 记录错误，跳过商店解析，完全依赖 Web 研究 |
| fetch-reviews.js 返回 403 | 切换 country 参数重试；完全失败则标注"评论数据不可用，以媒体二手报道为替代" |
| 找不到创始人信息 | 在基本信息章注明"公开资料暂无创始团队信息" |
| --vs App 数据不足 | 对比表改为单边已知列，未知列标"数据不足" |
| 研究后信息量不足以写某可选章 | 直接跳过该章，不写"暂无数据"占位 |

---

## Continuation Behavior

每篇报告交付后，agent 必须自动发送：

> 报告已交付 ✅ 还要继续走查下一个吗？给我下一个 App 名 + 链接就行，或 `/role default` 退出。

不需要等用户主动追问。
