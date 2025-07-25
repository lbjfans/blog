---
share: true
title: paperlist
tags:
  - sync
dir: posts/sync/
date: 2025-07-25T20:40:00+08:00
summary: paperlist, prompt
---

# 表格

| 文章                                                              | 期刊  | 作者  | 发表时间 | 方向  | 研究目的 | 创新点 | 模型结构 | 主要工作 | 笔记                    |
| --------------------------------------------------------------- | --- | --- | ---- | --- | ---- | --- | ---- | ---- | --------------------- |
| ImageNet Classification with Deep Convolutional Neural Networks |     |     |      | CV  | 图像分类 |     |      |      | [alexnet](alexnet.md) |
|                                                                 |     |     |      |     |      |     |      |      |                       |

弄一个template：论文名字，pass1/2/3，总结

# prompt


第一个
```
您是一名经验丰富的研究学者，我将会发送一些学术文献给您，请您仔细阅读文献内容，并为我详细讲解文献的各个部分，解答我的疑问。这对我非常重要，因此我希望您能够认真对待，确保每个问题都能得到准确和深入的回答。完成任务后，我将给予丰厚的报酬。注意！请您详细阅读文章内容，不要偷懒！并用中文回答我的问题。
链接是：xxx
请您概述一下这篇文章的主要内容。要求总结准确全面，涵盖文章的核心观点和结论，并用简洁明了的语言进行描述。随后，详细讲一下abstract和discussion，全部图表的意义
```
第二个
```
文档的链接：

假设你现在是机器学习、大型语言模型（LLM）、深度学习、计算机视觉领域的博士生，首先请提供对上述文档的摘要，然后你需要帮助我根据以下内容总结这篇文章：

1. **概述这篇文章提出了哪些方法、技术框架和效果。文章的研究方向是什么？动机是什么？解决了什么问题？这篇论文的贡献是什么？并按格式列出主要观点：**
   主要观点：
   - ...
   - ...

2. **过去有哪些相关的研究？这篇文章的方案与之前的方案相比有哪些优势，之前的方法解决不了哪些问题？这是一个新的问题吗？**

3. **请结合方法章节的内容，详细描述该方法的主要过程，并使用LaTeX显示关键变量。论文中提到的解决方案的关键是什么？使用了哪些具体技术？模型是什么，数据从输入到中间到输出的形式是什么？训练和测试的过程分别是什么，有什么区别？并按格式列出主要观点：**
   主要观点：
   - ...
   - ...

4. **请结合实验章节总结该方法的任务和性能。列出具体数值。实验是如何设计的？文章中使用了哪些数据集以及它们用于的任务场景（请给出这些数据集的名称及其所用的任务，格式如下：**
   数据集：
   - ...
   - ...)
   - 这篇论文的代码库是否开放？（文章中是否提到了GitHub网站，如果有，是什么网站？）

5. **请结合结论章节总结该方法仍然存在什么问题。是否有进一步深入的工作？**

让我们一步步来解决，确保我们得到正确的答案。
```
英文
```
Supposing you are now a PHD student in the field of machine learning, LLM and deep learning , first please provide a summary of the above document and then you need to help me summarize this article according to the following contents:
1. First, outline what methods, technologies frameworks and effects this article has proposed. What problem has been solved? What scientific hypothesis is this article going to test? What is the contribution of this paper? And give the list of the main points by format "
Main points:
- ...
- ..."
1. What related studies were there in the past? What are the advantages of this article's scheme compared with the previous scheme, and what problems can't be solved by the previous method? Is it a new problem?
2. Please describe the main procedure of the method in detail in combination with the contents of the method chapter, and use latex to show the key variables. What is the key to the solution mentioned in the paper? What are the specific technologies used? And give the list of the main points by format "
Main points:
- ...
- ..."
1. Please summarize the tasks and performance of this method in combination with the chapter of experiments. Please list the specific values. How is the experiment designed? What are the data sets used in the article and the task scenarios they are used for(Please give the names of these data sets and the tasks these data sets are used for, in the format:
Dataset:
- ...
- ...)
- Whether the coded repository of this paper is open or not(Does the github website appear in the article, and if so, what is this website?)?
1. Please combine the chapter of conclusion to summarize what problems still exist in this method. Is there any work that can be further deepened?
Let's work this out in a step by step way to be sure we have the right answer.
```


# 疑问

每篇论文需要注意什么问题
![500](/blog/images/Pasted%20image%2020250221133903.png)
- 列出基本信息，介绍模型结构，创新点
- 列表格记录论文的基本信息，主要工作，研究目的，贡献，模型结构

```
1️⃣ "读论文时先只看论文的题目，想想自己会怎么做，再看论文怎么做”
当你看到一篇文章时，不要急于深入阅读全文。相反，给自己5-10分钟的时间，去思考：
1. 如果这个问题摆在我面前，我会如何解决？选择什么样的方法论？
2. 我预期会遇到哪些技术挑战？
3. 我会如何评估结果？
	
然后再阅读论文，比对作者的实际做法与你的思路。这种对比不仅可以帮助你更深入理解论文内容，更重要的是可以：
4. 定位思维盲区：作者使用了你没想到的方法
5. 发现创新点：你的某些想法与作者不同，可能具有研究价值
6. 培养批判性思维：辨别哪些方法更优，思考为什么作者选择了特定方法
	
通过这种"预测-比对"的过程，你实际上在训练自己的研究直觉，同时积累可能的研究方向。
.
2️⃣ "每天记录几个idea，随便什么都可以，不断地迭代自己的想法”
创新很少是一蹴而就的，更多是来自持续积累和反复打磨。建立一个专门的idea笔记本来坚持：
7. 每天记录2-3个研究灵感。这些想法可能来自论文阅读中的疑惑，也可能源于日常观察或与同学的讨论。重要的是不设门槛，放下完美主义——哪怕是看似荒谬或不成熟的想法也值得记录。
8. 定期回顾之前的想法，进行连接、组合、发展
9. 对promising的想法进行深化，搜索相关文献，验证可行性，必要时与导师进行讨论
	
这个过程中重要的是放下完美主义心态。记录的想法可以是半成品，甚至是荒谬的。关键在于培养观察和提问的习惯，让大脑始终保持"寻找可能性"的状态。
.
发现新的idea是主动思考和持续积累的过程。三年下来，我的notion上已经积累了数百条记录。虽然大部分想法尚未付诸实践，但它们共同构成了我独特的学术视角，让我在面对新研究问题时，能够从多个维度进行思考。
```




复现总结：环境，代码，模型，训练框架
- pycharm
- jupyter notebook
- python
- pytorch
```
输入的数据经过网络每一层发生了什么样的变化，最后得到什么的输出
```

