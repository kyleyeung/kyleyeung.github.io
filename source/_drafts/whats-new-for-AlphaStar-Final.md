---
title: AlphaStar是如何上天梯宗师的
date: 2019-11-06 14:32:48
catetories:
- Reinforcement Learning
tags:
- StarCraft 2
- DeepMind

---

DeepMind于2019年10月30日在Nature上刊登了他们关于AlphaStar项目的最新进展：*Grandmaster Level in Starcraft II using multi-agent reinforcement learning* ([原文链接](https://www.nature.com/articles/s41586-019-1724-z.pdf ))，同时也在官方Blog上刊登了[同名文章](https://deepmind.com/blog/article/AlphaStar-Grandmaster-level-in-StarCraft-II-using-multi-agent-reinforcement-learning)。

监督学习：优化网络输出与人类样本的KL散度

强化学习：人类数据用来采样统计$z$ , agent 经验收集之后，通过 TD($\lambda$) ,V-trace,UPGO结合KL loss 更新supervised agent

To integrate spatial and non-spatial information, we introduce scatter connections.

To manage the structured, combinatorial action space, the agent uses an auto-regressive policy7,10,11 and recurrent pointer network12.

7. StarCraft II: a new challenge for reinforcement learning.

10. Recurrent neural network based language model
11. Discrete sequential prediction of continuous actions for deep RL
12. Pointer networks



AlphaStar’s reinforcement learning algorithm is based on a policy gradient algorithm similar to advantage actor–critic13. Updates were applied asynchronously14 on replayed experiences15.

