---
share: true
title: lora
tags:
  - sync
dir: posts/sync/
date: 2025-07-25T20:40:00+08:00
summary: lora操作
---

# diffusion

[作业十：Stable Diffusion Fine-tuning_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1BJ4m1e7g8?spm_id_from=333.788.videopod.episodes&vd_source=773a63398bea4e166f99c44cae6bee92&p=41)

[用 LoRA 微调 Stable Diffusion：拆开炼丹炉，动手实现你的第一次 AI 绘画_lora微调stable diffusion-CSDN博客](https://blog.csdn.net/weixin_42426841/article/details/142670977)

## peft

[14b. 尝试使用 LoRA 微调 Stable Diffusion 模型 - 精简版](https://www.kaggle.com/code/aidemos/14b-lora-stable-diffusion)

代码的问题
- wandb
- logger
- 没有记录best
如何改
- 自定义Loss：l2 + cliploss
	- 只用MSE，图片是否和文本匹配：有的图片生成两个人
	- snr 加权mse loss是什么
- 只用text数据


