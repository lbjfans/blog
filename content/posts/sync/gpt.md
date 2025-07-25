---
share: true
title: gpt
tags:
  - sync
dir: posts/sync/
date: 2025-07-25T20:40:00+08:00
summary: gpt操作
---

```
🔍 第一步：选题与文献
🟨 利用Perplexity总结定位相关文献
Perplexity是一个智能搜索引擎，结合了大语言模型和实时网络搜索功能，支持学术论文查找和总结。这个工具可以获取相关课题的最新内容，并且提供了信息的来源链接，便于验证。prompts参考p4
	
🟨 利用Deepseek-R1的思维链进行头脑风暴
最近deepseek很出圈，可以让他根据某个课题进行brainstorm，prompt参考p5
	
📝 第二步：实验设计
🟦 使用Claude-3.5探索一些技术路线。Claude相比GPT更专注于学术和专业领域，具有较强的代码和数学能力。所以我一般上遇到技术实现上的问题，会先把这个问题抛给Claude，让他给我提供一些可行的技术方案。根据他的方案，我再做判断，对方案进行微调和适配。prompts：直接说明自己的需求即可
	
🟦 使用Cursor快速实现想法，搭建demo，调试代码。Cursor是一个专门面向开发者的代码编辑器，根据你的需求编写代码并且有非常智能的代码补全和bug修复能力。
	
✍️ 第三步：论文写作
🟧 使用GPT-4o优化论文写作。GPT的语言能力特别强大。我会先用蹩脚的英语写出一个初稿，并提供一些相关论文的上下文，然后让GPT来帮我把论文的表达变得更学术。参考prompt：“Please check the grammar and polish this academic writing”
	
🔄 完整工作流
Perplexity调研 → Deepseek R1梳理思路  → Claude编程实验  → GPT润色论文
	
💡
- 不同AI擅长不同任务，组合使用效果最佳
- AI是助手而非替代者，我们始终需要保持清醒的头脑和critical thinking的能力，保持主导权
- 重在提升科研效率，而非依赖AI
```

![](/blog/images/Pasted%20image%2020250311130348.png)

![](/blog/images/Pasted%20image%2020250311130359.png)

