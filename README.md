# hojun-skills

我的 [Claude Code](https://docs.anthropic.com/en/docs/claude-code) 自定义技能集。

## 技能列表

| 技能名 | 描述 | 触发词 |
|--------|------|--------|
| **hojun-book** | 读书技能 — 输入一本书，输出走查 + 拆书两份合并交付的 Markdown 文件 | `走查这本书` / `走查 + 拆 X` / `book walkthrough` / `读书笔记 + 拆书` |

## 安装

使用 `skills` CLI（基于 `npx`）一行安装：

```bash
# 安装全部技能（全局, org-mode 格式）
npx skills add caohongjun/skills -g --all

# 安装全部技能 (Markdown 格式, 适用于 Obsidian / VSCode / Notion 等)
npx skills add caohongjun/skills#md -g --all

# 安装单个技能
npx skills add caohongjun/skills -g --skill hojun-book

# 安装单个技能 (Markdown 格式)
npx skills add caohongjun/skills#md -g --skill hojun-book

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
| `#md` | 使用 Markdown 格式输出（默认是 org-mode） |

## 项目结构

```
hojun-skills/
├── skills/              # 技能定义目录
│   └── hojun-book/      # 单个技能
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

## 致谢

拆书部分思路参考自 [lijigang/ljg-book](https://github.com/lijigang/ljg-skills/blob/master/skills/ljg-book/SKILL.md)。