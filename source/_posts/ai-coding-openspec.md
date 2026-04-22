title: OpenSpec使用
tags: [OpenSpec]
date: 2026-04-22 16:33:53
categories: AI
---

# OpenSpec是啥
规范驱动开发，让AI按照规范来生成。如果是小修改，可以不用OpenSpec，比较烧TOKEN
官网：https://openspec.dev/

2.1 安装与初始化
``` shell
# 全局安装 OpenSpec
npm install -g @fission-ai/openspec@latest
# 在项目目录下初始化
cd /path/to/your-project
openspec init
```
![openspec init](openspec-init.jpeg)

初始化时，OpenSpec 会提示你选择使用的 AI 工具（Claude Code、Cursor、Trae、Qoder 等）。选择生成后，需要回到AI编程工具里面去输入命令，比如我的claude code、codex、qwen的CLI命令行里面
<!-- more -->
选择AI工具后，会生成对应的规范命令文件
![commands skills](openspec-ai.jpeg)

commands/openspec 这个目录定义了三个不同的命令，每个命令文件中所写的提示词，都是 AI 在执行该命令时需要参考的"规范"。上述三个命令分别是：
- proposal.md：发起新变更提案
- apply.md：表示执行已批准的变更
- archive.md：归档已完成的变更

# OpenSpec 规范核心要点
无论使用哪种 AI 工具，OpenSpec 的核心工作流都是一致的。理解这套规范，你就能更好地与 AI 协作。

三阶段工作流：
```
阶段1：创建变更（Proposal）
    ↓
阶段2：实现变更（Apply）
    ↓
阶段3：归档变更（Archive）
```
何时必须创建提案？

| 场景	| 是否需要提案 |
|  ----  | ----  |
| 新增功能或能力	| ✅ 必须 |
|破坏性变更（API/Schema）|	✅ 必须 |
|架构或模式调整|	✅ 必须|
|Bug 修复（恢复既有行为）|	❌ 跳过|
|拼写、格式、注释修正	|❌ 跳过|
|非破坏性依赖升级	|❌ 跳过|
常用命令：
```
openspec list              # 列出所有变更
openspec list --specs      # 列出所有规范
openspec validate <id>     # 校验变更
openspec archive <id>      # 归档变更
```

# 实操
1. openspec init
2. claude
使用上面的3步，提案->变更->归档
/opsx:propose
/opsx:apply
/opsx:archive
![opsx](opsx.jpeg)

# 文档说明
proposal.md  需求
design.md 设计
tasks.md 任务

/opsx:apply archive时指定是哪个提案，避免把所有未归档的都执行了