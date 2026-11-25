# project-memory

让 AI agent 读写**项目长期共享记忆**的 skill：跨会话、跨工具留住架构决策、实现约束、
踩过的坑与被否决的方案，而不是每次都从零推导。

## 两种后端

| 后端 | 位置 | 何时使用 |
|------|------|----------|
| FNS | Fast Note Sync MCP 知识库 | 会话里能拿到 FNS 的 note 工具时 |
| 文件 | 项目根目录 `docs/memory/` | 没有配 FNS 时（默认、零依赖） |

一个会话只选一种后端，绝不双写。文件后端布局：

```
docs/memory/
├── MEMORY.md        # 索引：每个主题文件一行
├── architecture.md
└── payments-decisions.md
```

`MEMORY.md` 每条只留一行 `- [标题](file.md) — 一句话钩子`，正文全部放进对应主题文件，
避免索引膨胀到每个会话都吃满上下文。

## 安装

```bash
npx skills add 8liang/project-memory -g -y
```

手动安装：

```bash
git clone https://github.com/8liang/project-memory.git ~/Workspace/project-memory
ln -s ~/Workspace/project-memory "$HOME/.agents/skills/project-memory"
```

## 使用

在项目里提出需要沿用历史决策的任务即可，skill 会在动手前先读记忆、在发现持久知识时写回。
也可以直接说「读一下项目记忆」或「把这条决定记到项目记忆里」。
