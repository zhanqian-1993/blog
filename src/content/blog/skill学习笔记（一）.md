---
title: 'Skill 学习笔记（一）'
description: '关于 AI Agent 和 Skill 的学习总结'
pubDate: '2026-02-09'
tags: ['AI', 'Skill', 'Agent']
---

## 参考文档

- [你可能不再需要 workflow，大部分场景 skills 足矣——五步框架把 Workflow 变成可进化的 Skill | BestBlogs.dev](https://www.bestblogs.dev/article/57d3eeab)
- [认知重建：Speckit 用了三个月，我放弃了——走出工具很强但用不好的困境 | BestBlogs.dev](https://www.bestblogs.dev/article/0f05763c)
- [用第一性原理拆解 Agentic Coding：从理论到实操 | BestBlogs.dev](https://www.bestblogs.dev/article/a3d04bcd)

## 一些自我总结

理想状态的 agent 工作流程：plan → work → review → compound → plan

借助于 Kiro 的 SPEC 模式，我们可以精细化地将每个大需求拆解成一个个具像化的 user story。Kiro 在给你拆解过程中，也是带了 superpower 一些能力，对你的需求边界、逻辑等进行丰富。但是 SPEC 有个很大缺陷，包括目前的 Vibe Coding，它始终是在做 0 到 1 的编码，没有"compound"，导致我们每次构建 plan 都要将历史的架构背景、业务背景、流程的坑等等事情，重新复述一遍给 Agent，不然就会在重复的地方跌倒。

Kiro 在 skill 出来之前，其实已经预见了这种场景，它的 "Agent STEERING" 模块可以解决部分上述的问题，STEERING 模块可以事先定义这个项目的编码规范、背景、甚至可以指定不同目录下执行不同代码规范和逻辑。但尽管如此，基于 skill 的编码能力仍优于 Kiro，skill 最优秀的设计点在于 "渐进式披露"的机制。模型的回答是基于自回归+注意力机制来实现的，模型是基于前面的 token，逐个输出下一个可能性最大的 token，而不是所有都想好了再说。一旦上下文达到几十 K，几百 K token，会越来越"笨"（记忆差、推理能力差）。Kiro 的 STEERING 就是一次性全部加载到上下文的，这是不如的 skill 的点之一。

目前最重要的是实现"compound"，理想的实现方式：外挂知识（固化）+ 适时适量地加载上下文 + 外挂知识自我迭代进化。这 3 个步骤 skill 理论上都能实现，skill 目录层级的管理方式也特别好维护。

编码之外的作用，当前 [skill market](https://skillsmp.com/) 封装好的 skill 特别多，按需安装，必备之一 skill-creator。

## DONE & TODO

skill 的使用我自己分为 4 种。1. 可抽象成 SOP 的日常杂事（非编码）2. 顶尖技能的工具箱，如源码级排查 bug、调研需求。 3. 一个有记忆的编码伙伴 4. 个性化合作伙伴/指导老师，和你一同脑暴、规划生活或者工作。

### Done

1. 日报、周报生成（done）。每周五要花费 1h 左右处理周报和日报的格式，查询需求信息挂靠工时，skill 自动收集 ipaas 上需求信息、整理成周报、日报两种格式，自己微调一下即可。
2. 平台发布
3. sso-auth

### Doing

小采 DA 工程编码复利（compound）。目前仍在建设中，复利效果很大程度依赖你维护的文档，所以这是一项长期完善、更新迭代的事情。

![Skill Learning](/skill-learning.png)

### TODO

1. oncall skill 化。目前小采 DA 的一些 oncall 处理起来比较耗时。比如排查云/岛端的小采 DA 的查询不到数据，1. 发布平台日志查看 2. 连接数满了/数仓表今日没跑批完成/查询引擎有问题/... 3. 登陆 starrocks cli 查看是否是当前用户查询人过多 4. granfana 是不是 starrocks 资源满了 5. 查看表的调度时间，pt 是否对，是否被清理 pt 了。这里面性价比最低的耗时：1. 登陆不同的平台页面 2. 去不同的地方拿账号密码 。相比于 RAG 只能提供"查询"能力，skill 更像是个能提供完整 CRUD 的服务工具。
2. compound 的自我进化。创建更新 skill 或者自动迭代 skill，在每次交互后获取到的新知识更新到对应的 skill 中。
3. coworker 目前没条件使用

## 缺陷

我使用的是 claude code + 智谱大模型，周报 skill 打磨了多次，流程才相对比较稳定。有些 skill 用户已经提出可以替代 workflow 了，但我目前使用情况下来，还不行，大概率是我的模型不行？有些用户反馈，相比 Claude Sonnet 4.5，GPT 会更适合 skill 的代码 script。
