# hojun-teardown

[Claude Code](https://docs.anthropic.com/en/docs/claude-code) 产品走查技能 — 对任意移动 App 进行深度竞品走查，输出结构化中文报告。

## 定位

| 维度 | hojun-teardown（本技能）| app-walkthrough |
|------|----------------------|-----------------|
| 工作方式 | 开放性研究：商店、搜索、媒体、评论 | 云端 emulator 实时操作 |
| 主要产出 | 5 章结构化中文报告 | UX 流程截图 + 体验分析 |
| 需要 APK | 否 | 是 |
| 适合问题 | 商业模式、增长路径、用户口碑 | 界面交互、操作流、界面细节 |
| 触发方式 | 仅 hojun-teardown 角色激活时 | 独立技能，关键词触发 |

## 安装

```bash
# git clone (推荐)
git clone https://github.com/caohongjun/skills.git ~/.claude/plugins/hojun-skills

# 或单文件
mkdir -p ~/.claude/skills/hojun-teardown
cp skills/hojun-teardown/SKILL.md ~/.claude/skills/hojun-teardown/SKILL.md
```

## 使用

### 第一步：激活角色

在 Claude Code 中输入：
```
/role hojun-teardown
```

### 第二步：输入走查请求

```
<App 名称> <App Store 或 Google Play 链接> [--focus <重点章节>] [--vs <对比 App 名>]
```

**示例：**
```
Glow https://apps.apple.com/us/app/glow-ai-therapy-chat/id6446215842
```

**带参数示例：**
```
TikTok https://play.google.com/store/apps/details?id=com.zhiliaoapp.musically --focus 投放素材 --vs Instagram
```

### 参数说明

| 参数 | 说明 |
|------|------|
| `--focus` | 指定某章节深入研究，如 `--focus 投放素材` |
| `--vs` | 并行拉取对比 App 数据，报告末尾加对比表 |

## 报告结构

### 核心结论速读（前置）

60 秒电梯版，控制 250-400 字：
- **App 是什么**：公司 + 商业体量 + 定位 + 综合评分
- **N 个产品赌注**（3-4 条，带 emoji + 关键时间 + 数据）
- **核心启示**（3-5 条，「结论 → 行动」格式）
- **主要风险**（3 个最严重负面信号）

### 5 章必含

| 章节 | 核心内容 |
|------|---------|
| 1. 基本信息 | 创始团队、创业起源、融资/收购情况、上线时间地区 |
| 2. 投放素材 | 渠道占比、核心 Hook 公式、素材方向、用户洞察 |
| 3. 产品内功能 | 核心使用闭环、差异化亮点、Onboarding 与付费墙设计 |
| 4. 用户评价 | 商店评分、Top-3 正向卖点、Top-3 负向痛点、差评时间线 |
| 5. 思考与总结 | 产品/商业/增长亮点分析、对业务的启示 |

### 7 个可选补充章节

根据数据丰富程度决定是否纳入：
1. **国际化策略** — 多语言支持、出海重点市场、本地化程度
2. **创始人媒体形象** — 公开发言、播客/访谈、个人品牌建设
3. **关键人才信号** — 近期关键岗位招聘，推断产品方向
4. **下一步预测** — 基于现有信号推断产品路线图或增长动作
5. **平台政策风险** — App Store / Google Play 合规史、IAP 规则风险点
6. **关键事件黑天鹅** — 下架、泄露、媒体危机等重大事件复盘
7. **同名混淆风险** — 商标、商标争议、同名 App 竞争

### 综合评分表

报告末尾必附：
| 维度 | 评分（/10）| 一句话理由 |
|------|-----------|----------|
| UX 体验 | — | — |
| 商业模式 | — | — |
| 增长能力 | — | — |
| 团队执行力 | — | — |
| **综合分** | — | — |

## 质量标准

- **有引用才能写数据**：无法溯源的数字必须标注"推测"或"未找到公开数据"
- **引用分级**：优先 S 级，降级时在文中注明
- **不放大信号**：1 条帖子不能写成"用户普遍反映"
- **避免空洞结论**：每个亮点/痛点必须有至少 1 个具体例子

## 信源分级

| 级别 | 来源 |
|------|------|
| **S** | 官方 / 商店 / 招股书 / 官方公告 |
| **A** | TechCrunch / CNBC / Forbes / Bloomberg / Inc. |
| **B** | Sensor Tower / Latka / Crunchbase / Similarweb |
| **C** | 行业 blog / 专业分析师 newsletter / 创始人访谈 |
| **D** | Reddit / Trustpilot / Twitter 舆论 / 匿名论坛 |

## 输出

- 文件路径：`~/zylos/workspace/teardowns/<App名>-teardown-<YYYY-MM-DD>.md`
- 格式：Markdown
- 语言：中文正文，代码/变量/文件路径用英文

## Continuation Behavior

每篇报告交付后，自动发送：
```
报告已交付 ✅ 还要继续走查下一个吗？给我下一个 App 名 + 链接就行，或 `/role default` 退出。
```