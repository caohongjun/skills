# hojun-skills

我的 [Claude Code](https://docs.anthropic.com/en/docs/claude-code) 自定义技能集。

## 安装

使用 `skills` CLI（基于 `npx`）一行安装：

```bash
# 安装全部技能
npx skills add caohongjun/skills -g --all

# 安装单个技能
npx skills add caohongjun/skills -g --skill hojun-book
npx skills add caohongjun/skills -g --skill hojun-app-teardown

# 查看仓库中有哪些技能
npx skills add caohongjun/skills -l
```

### 参数说明

| 参数 | 说明 |
|------|------|
| `-g` | 全局安装到 `~/.claude/skills/`（推荐）。不加则装到当前项目 `.claude/skills/` |
| `--skill <name>` | 指定安装某个技能，可重复使用 |
| `--all` | 安装仓库内全部技能 |
| `-l` | 仅列出可用技能，不安装 |

### 替代方式：git clone

```bash
git clone https://github.com/caohongjun/skills.git ~/.claude/plugins/hojun-skills
```

## 技能列表

| 技能 | 说明 |
|------|------|
| **hojun-book** | 读书技能 — 输入一本书，输出走查 + 拆书两份合并交付的 Markdown 文件 |
| **hojun-app-teardown** | 产品走查技能 — 对任意移动 App 进行深度竞品走查，输出结构化中文报告（5章必含 + 7个可选章节） |

## 技能详情

### hojun-book

读书技能 — 输入一本书，输出**走查 + 拆书**两份合并交付的 Markdown 文件。

**触发词**：`走查这本书` / `走查 + 拆 X` / `book walkthrough` / `读书笔记 + 拆书`

**输出结构**：
- **走查（4模块）**：书籍背景、速读总结、章节拆解、思考与意义
- **拆书（6段）**：真问题、隐含前提、思考工具、核心判断、保留一件、迁移外推

**输出路径**：`~/Documents/notes/{时间戳}--走查拆书-{书名}__book.md`

**使用示例**：
```
走查这本书：《国富论》
```

### hojun-app-teardown

产品走查技能 — 对任意移动 App 进行深度竞品走查，输出结构化中文报告。

**触发方式**：仅在 `hojun-app-teardown` 角色激活时工作（`/role hojun-app-teardown`）

**工作方式**：开放性研究（商店数据、网络搜索、媒体报道、用户评论挖掘）

**报告结构**：
- **核心结论速读**（前置，60秒电梯版）
- **5章必含**：基本信息、投放素材、产品内功能、用户评价、思考与总结
- **7个可选补充章节**：国际化策略、创始人媒体形象、关键人才信号、下一步预测、平台政策风险、关键事件黑天鹅、同名混淆风险

**使用示例**：
```
/role hojun-app-teardown
Glow https://apps.apple.com/us/app/glow-ai-therapy-chat/id6446215842
```

## 项目结构

```
hojun-skills/
├── skills/              # 技能定义目录
│   ├── hojun-book/      # 读书技能
│   │   ├── SKILL.md     # 技能定义文件
│   │   └── README.md    # 技能详细说明
│   └── hojun-app-teardown/  # 产品走查技能
│       ├── SKILL.md     # 技能定义文件
│       └── README.md    # 技能详细说明
├── .claude-plugin/      # Claude Code 插件配置
│   └── manifest.json    # 插件清单
├── CLAUDE.md            # 插件元信息
└── README.md            # 项目说明
```