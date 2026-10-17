# make-knowledge-cards

## 项目解决什么问题

阅读文章后，手动挑选重点、去除重复并整理复习材料比较费时。本项目提供一个 Codex Skill，将用户提供的正文整理为便于理解和自测的知识卡片。

它是一组供 Codex 执行的指令，不是独立运行的转换程序。

## 主要功能

- 接受粘贴文本或本地 `.md`、`.markdown`、`.txt` 文件。
- 围绕文章主旨提取重要知识，每张卡片只讲一个知识点，合并重复内容。
- 默认用中文 Markdown 输出 5～8 张；信息不足时减少数量，没有可提取知识时说明原因。
- 每张包含标题、核心知识、简明解释，以及原文例子或自测问题；没有合适例子时使用可从原文回答的问题。
- 要求内容有原文依据，保留必要条件和观点归属，不编造背景、数字或例子。

暂不支持网页抓取、PDF、Anki 导出或图形界面。仅提供网页链接时，需要先粘贴正文或提供支持的本地文件。生成结果仍需人工核对。

## 安装方法

前提：已有可使用技能的 Codex 环境。Skill 本身无需额外脚本或第三方依赖，日常使用不需要 Python。

将本项目的 `skills/make-knowledge-cards` 整个文件夹复制到 Codex 技能目录（通常为 `~/.codex/skills/`）。Windows 通常对应 `%USERPROFILE%\.codex\skills\`。如果目标中已有同名 Skill，先核对版本，避免直接覆盖。

复制后的目录结构应为：

```text
~/.codex/skills/make-knowledge-cards/
├── SKILL.md
└── agents/
    └── openai.yaml
```

## 使用方法

安装后，在新会话中输入技能名称并提供正文：

```text
请使用 $make-knowledge-cards 把下面的文章整理为知识卡片：
（粘贴文章正文）
```

请提供完整正文，避免因内容缺失影响知识点提取。

处理本地文件时，提供 Codex 能读取的文件路径；相对路径以当前工作目录为准：

```text
请使用 $make-knowledge-cards 读取 ./notes.md 并生成知识卡片。
```

默认在对话中输出。如需保存，可明确指定输出路径：

```text
请使用 $make-knowledge-cards 读取 ./notes.txt，将知识卡片保存为 ./cards.md。
```

不安装时，也可让 Codex 读取本项目的 `skills/make-knowledge-cards/SKILL.md` 并按其中规则处理正文。

## 输入输出示例

输入：

```text
主动回忆是先不看材料，尝试从记忆中提取内容。例如，合上书后写出刚学的三个要点，再对照原文检查遗漏。
```

输出：

```markdown
原文仅支持 1 个独立知识点，因此生成 1 张卡片。

### 卡片 1：主动回忆
- **核心知识**：主动回忆是先不看材料，尝试从记忆中提取内容。
- **简明解释**：先自行回想，再对照原文检查遗漏。
- **例子**：合上书后写出刚学的三个要点，再对照原文检查遗漏。
```

## 本地验证

```text
python -X utf8 <skill-creator目录>/scripts/quick_validate.py skills/make-knowledge-cards
```

官方工具只验证结构，不评判卡片内容质量。行为试跑及修订记录见 [tests/README.md](tests/README.md)。
