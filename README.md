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
npx skills add caohongjun/skills -g --skill hojun-ai-health-compainon

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
| **hojun-ai-health-compainon** | AI 健康助手技能 — 提供饮食记录、健康咨询、AI宠物健康情感互动、健康管理建议等服务 |

## 项目结构

```
hojun-skills/
├── skills/              # 技能定义目录
│   ├── hojun-book/      # 读书技能
│   │   └── SKILL.md     # 技能定义文件
│   ├── hojun-app-teardown/  # 产品走查技能
│   │   └── SKILL.md     # 技能定义文件
│   └── hojun-ai-health-compainon/  # AI 健康助手技能
│       └── SKILL.md     # 技能定义文件
├── .claude-plugin/      # Claude Code 插件配置
│   └── manifest.json    # 插件清单
├── CLAUDE.md            # 插件元信息
└── README.md            # 项目说明
```

## 输出配置

### 输出格式
所有技能均输出 **Markdown (.md)** 文件，方便阅读和二次编辑。

### 输出路径

| 技能 | 输出路径 |
|------|---------|
| hojun-book | `~/Documents/notes/{时间戳}--走查拆书-{书名}__book.md` |
| hojun-app-teardown | `~/Documents/notes/{时间戳}--App走查-{App名}__app.md` |
| hojun-ai-health-compainon | 非文件输出，而是AI PET对话，支持饮食记录，健康互动 |

### 文件命名规则

- **时间戳格式**：`YYYYMMDDTHHMMSS`（如 `20240115T143022`）
- **文件后缀**：`__book.md`（书籍走查）、`__app.md`（App 走查）、`__health.md`（健康助手）
- **统一目录**：所有输出文件均保存至 `~/Documents/notes/`，便于集中管理和检索