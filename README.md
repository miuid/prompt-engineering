# Prompts

This is a project the collect useful and smart LLM prompts

# References

## Claude Best Practices

- https://www.anthropic.com/engineering/claude-code-best-practices

## Awesome Copilot

- https://github.com/miuid/awesome-copilot

## Claude Agents

- https://github.com/miuid/claude-agents
- https://github.com/miuid/agents

## Command Suite

- https://github.com/miuid/Claude-Command-Suite

## Kingkongshot

- https://github.com/kingkongshot/prompts/tree/main

## 引导词仓库 (Prompts Collection)

一个精心组织的引导词集合，帮助提升AI协作效率和质量。

### 📋 快速导航

#### 🛠️ 开发工具类
- **[Claude](./prompts/claude/)** - Claude AI 开发规范、Agent配置与自定义命令
- **[Kiro](./prompts/kiro/)** - Kiro 三阶段工作流规范（需求→设计→实施）

#### 🎨 设计可视化类
- **[Obsidian Canvas](./prompts/visualization/obsidian-canvas/)** - 使用Obsidian Canvas绘制功能框架图

### 📂 目录结构

```
prompts/
├── claude/              # Claude AI 相关
│   ├── CLAUDE.local.md  # 本地开发规范
│   ├── agents/          # Agent 配置
│   └── commands/        # 自定义命令
├── kiro/                # Kiro 工作流
│   ├── spec.md          # 英文规范
│   └── spec_zh.md       # 中文规范
└── visualization/       # 可视化工具
    └── obsidian-canvas/ # Obsidian Canvas图表
```

### 🌟 精选推荐

#### 开发规范
- [Claude本地开发规范](./prompts/claude/CLAUDE.local.md) - Linus Torvalds风格的基础引导词
- [Kiro工作流](./prompts/kiro/spec_zh.md) - 结构化的需求→设计→实施流程

#### Claude Code Agent 配置
- [Memory Network Builder](./prompts/claude/agents/memory-network-builder.md) - 配合 Obsidian构建知识网络的Agent
- [Library Usage Researcher](./prompts/claude/agents/library-usage-researcher.md) - 搜索真实代码和官方文档，提供基础说明和巧妙用法的Agent

#### 自定义命令
- [Commit-As-Prompt](./prompts/claude/commands/commit-as-prompt.md) - 将Git提交转化为结构化Prompt

#### 可视化工具
- [功能框架图绘制](./prompts/visualization/obsidian-canvas/使用%20Obsidian%20Canvas%20绘制功能框架图.md) - 编写 obsidian canvas 图透视项目情况
