# hojun-skills

我的 [Claude Code](https://docs.anthropic.com/en/docs/claude-code) 自定义技能集。

## 技能列表

| 技能名 | 描述 | 触发方式 |
|--------|------|----------|
| **hojun-book** | 读书技能 — 输入一本书，输出走查 + 拆书两份合并交付的 Markdown 文件 | `走查这本书` / `走查 + 拆 X` / `book walkthrough` / `读书笔记 + 拆书` |
| **app-teardown** | 产品走查技能 — 对任意移动 App 进行深度竞品走查，输出结构化中文报告（5章必含 + 7个可选章节） | 仅在 `app-teardown` 角色激活时工作（`/role app-teardown`） |

## 安装

使用 `skills` CLI（基于 `npx`）一行安装：

```bash
# 安装全部技能
npx skills add caohongjun/skills -g --all

# 安装单个技能
npx skills add caohongjun/skills -g --skill hojun-book
npx skills add caohongjun/skills -g --skill app-teardown

# 查看仓库中有哪些技能
npx skills add caohongjun/skills -l
```

## 参数说明：

| 参数 | 说明 |
|------|------|
| `-g` | 全局安装（推荐） |
| `--all` | 安装全部技能 |
| `--skill <name>` | 指定安装单个技能 |
| `-l` | 列出仓库中的所有技能 |

## 项目结构

```
hojun-skills/
├── skills/              # 技能定义目录
│   ├── hojun-book/      # 读书技能
│   │   ├── SKILL.md     # 技能定义文件
│   │   └── README.md    # 技能详细说明
│   └── appteardown/     # 产品走查技能
│       └── SKILL.md     # 技能定义文件
├── .claude-plugin/      # Claude Code 插件配置
│   └── manifest.json    # 插件清单
├── CLAUDE.md            # 插件元信息
└── README.md            # 项目说明
```

## 技能详情

### hojun-book

读书技能 — 输入一本书，输出**走查 + 拆书**两份合并交付的 Markdown 文件。

- **走查铺面** — 让你"知道这本书在讲什么"（背景、速读、章节、意义）
- **拆书钻骨** — 让你"看穿作者怎么写这本书"（真问题、前提、工具、判断、内核、外推）

输出文件路径：`~/Documents/notes/{时间戳}--走查拆书-{书名}__book.md`

**使用示例：**

在 Claude Code 中输入：
```
走查这本书：《国富论》
```

### app-teardown

产品走查技能 — 对任意移动 App 进行深度竞品走查，输出结构化中文报告。

**工作方式**：开放性研究（商店数据、网络搜索、媒体报道、用户评论挖掘）

**报告结构**：
- **5章必含**：基本信息、投放素材、产品内功能、用户评价、思考与总结
- **7个可选补充章节**：国际化策略、创始人媒体形象、关键人才信号、下一步预测、平台政策风险、关键事件黑天鹅、同名混淆风险

**触发方式**：仅在 `app-teardown` 角色激活时工作

**使用示例：**

在 Claude Code 中：
```
/role app-teardown
```

然后输入：
```
Glow https://apps.apple.com/us/app/glow-ai-therapy-chat/id6446215842
```