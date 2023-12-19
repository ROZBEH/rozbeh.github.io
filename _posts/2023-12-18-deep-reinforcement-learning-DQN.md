---
layout: post
title:  "Deep Reinforcement Learning: DQN"
date:   2023-12-18 22:36:56 -0800
excerpt: "Deep Reinforcement Learning: DQN"
---

In this post I am going to talk about Deap Q Learning (DQN). The main idea is based 
  on two papers that were published in 2013 and 2015 by Google Deepmind. 
  The <a href="https://arxiv.org/pdf/1312.5602.pdf">firt one</a> is a NIPS paper, 
  lets call it DQN1 and the <a href="https://storage.googleapis.com/deepmind-media/dqn/DQNNaturePaper.pdf">second paper</a> 
  is a nature paper and we call it DQN2 in this post. 

DQN1: Playing Atari with Deep Reinforcement Learning
DQN2: Human-level control through deep reinforcement learning

In order to make it easier for the general reader to follow, this post contains sub-posts that will dive deeper into the subject. This post is also accompanied by the code snippet of the tensorflow implementation of each part.



The main idea of these two papers is to be able to beat the computer at Atari games with no minimum supervision.

The papers combine ideas from reinforcement learning (Q-learning) and deep learning (CNN) in order to achieve the performance.
I also wrote short post about Q-learning. Follow <a href="Qlearning.html">this</a> link if you want to learn more about it. In summary, Q-learning is a dynamic programming approach for updating our estimation about agent's future success.

One thing that comes very handy in reinforcement learning is the Bellman Equation. Bellman equation is a way of discounted future reward.


$$R_t = r_t + \gamma (r_{t+1} + \gamma ( r_{t+2} + ...)) = r_t + \gamma R_{t+1}$$

Since we use \(Q\) in order to express expected future reward, we will have:

$$Q(s_t, a) = \max_{\pi}  R_{t+1}$$

$\pi(s) = \argmax_{a} Q(s, a)$ is the policy that leads to the best reward $Q(s,a)$.

For simple transition ${<}s_t,a,r,s_{t+1}{>}$, the bellman equation would be:

$$Q(s_t,a) = r + \gamma \max_{a'} Q(s_{t+1},a')$$

In the [DQN1 paper](https://arxiv.org/pdf/1312.5602.pdf) that was published in 2013, a convolutional neural network (CNN) is used as a function to estimate $Q$. The actual reward $r$ at each time step is being returned by the simulation environment, which the authors call an emulator.

The overall workflow of the DQN consists of two major parts. Here we will cover each part in detail:

test
