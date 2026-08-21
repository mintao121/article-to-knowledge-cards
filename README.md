# make-knowledge-cards

把文章或笔记变成可记忆、可复习的**知识卡片**。

一个开源的 [Agent Skill](https://agentskills.io)（兼容 WorkBuddy、Claude Code、OpenAI Codex 等支持 Agent Skills 的 agent）。它把用户**粘贴的文章**或**本地 Markdown / TXT 文件**，提炼成 5–8 张知识卡片，每张只讲一个知识点。

## 功能

- 输入：直接粘贴的文本，或本地 `.md` / `.txt` 文件路径
- 输出：Markdown 知识卡片，每张包含
  - **标题** — 点题的名词短语或结论
  - **核心知识** — 一句话点明最关键的内容
  - **简明解释** — 大白话，写给第一次接触这个概念的人
  - **例子** 或 **自测问题** — 至少其一，用于理解或检验
- 质量约束：提取真正重要的知识、一张卡片只讲一个点、删除重复、不编造原文没有的信息、信息不足时不硬凑数量

## 不支持（会直接拒绝并说明原因）

- 网页抓取 / URL 抓取（请复制正文后粘贴）
- PDF（请先导出为文本 / `.md` / `.txt`）
- Anki（只产出 Markdown，不导入或同步）
- 图形界面（只通过对话输出 Markdown）

## 安装方法

本 Skill 遵循 [Agent Skills 开源规范](https://agentskills.io)，支持以下方式安装：

### 方式一：手动复制（推荐用于本地测试）

将 `skills/make-knowledge-cards/` 整个目录复制到你的 Agent Skills 加载路径下：

```bash
# WorkBuddy 用户级 Skills 路径
cp -r skills/make-knowledge-cards ~/.workbuddy/skills/make-knowledge-cards

# 或项目级 Skills 路径（仅当前项目生效）
cp -r skills/make-knowledge-cards <你的项目>/.workbuddy/skills/make-knowledge-cards
```

### 方式二：通过 Skill 市场安装（如果已发布）

在支持 Agent Skills 的 agent 中搜索 `make-knowledge-cards` 并安装。

### 方式三：作为 Git 子模块引入

```bash
git submodule add https://github.com/<你的用户名>/1st_Skill.git skills/make-knowledge-cards
```

### 安装后验证

```bash
python skills/make-knowledge-cards/quick_validate.py skills/make-knowledge-cards
# 期望输出：Skill is valid!
```

## 使用方法

1. 把文章直接粘贴到对话里，或给出本地文件路径，例如：
   - 「把这段文章做成知识卡片」
   - 「读一下 notes.md，帮我生成复习卡片」
   - 「提取这篇笔记的知识点，每张讲一个点」
2. agent 会自动加载本技能，按流程产出卡片并做质量自检。
3. 需要保存时，可让 agent 把同样内容写入 `<原名>-cards.md`。

## 卡片格式

```markdown
### 卡片 N：<标题>
- **核心知识**：<一句话点明这个知识点最关键的内容>
- **简明解释**：<用大白话解释，1-3 句>
- **例子**：<一个具体例子或类比>（可选，建议提供）
- **自测问题**：<能当场检验理解的问题>（可选，与例子二选一必填）
```

## 输入 / 输出示例

**输入（粘贴）：**

> 复利就是利滚利。你把利息继续投资，下一期的本金就变大了，长期下来差距惊人。巴菲特说复利是世界第八大奇迹。

**输出：**

```markdown
## 知识卡片（共 2 张）

### 卡片 1：复利 = 利滚利
- **核心知识**：把投资产生的利息再投资，下一期以更大的本金计息，收益会指数级增长。
- **简明解释**：普通计息只算本金的利息；复利连「利息的利息」也算，时间越长雪球越大。
- **例子**：本金 1 万、年化 10%，单利 10 年利息共 1 万；复利 10 年后约 2.59 万。
- **自测问题**：为什么同样利率，复利长期收益比单利高得多？

### 卡片 2：复利需要时间才显威力
- **核心知识**：复利的优势在「长期」才明显，短期几乎看不出差别。
- **简明解释**：雪球效应前期慢、后期快，越早开始、持有越久越受益。
- **自测问题**：如果只持有 1 年，复利和单利差别大吗？为什么长期才重要？
```

## 目录结构

```
skills/make-knowledge-cards/
├── SKILL.md          # 技能指令与元数据（必填）
├── agents/
│   └── openai.yaml   # OpenAI Agents SDK 兼容接口
├── quick_validate.py # 结构校验脚本
├── LICENSE           # MIT
└── tests/            # 不同类型文章的输入 / 输出样例（技术文、随笔、清单、短文）
```

## 校验

```bash
python skills/make-knowledge-cards/quick_validate.py skills/make-knowledge-cards
# 输出：Skill is valid!
```

## 许可

[MIT](skills/make-knowledge-cards/LICENSE)
