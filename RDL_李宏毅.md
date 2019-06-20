---
title: RDL_李宏毅
mathjax: true
---
# Deep Reinforcement Learning
* https://www.bilibili.com/video/av48285039/?p=81

## Outline:
* Policy_based
  * Actor
* Value_based
  * Critic

## 1. Policy_based Approach——learn an actor
1. Action= $ \pi(Observation) $
2. 3 steps
   * 1) Neural Network as Actor
   * 2) goodness of function
   * 3) pick the best function
3. example as a video game
   * Input: the observation of machine(as a vector or matrix)
   * Output: actions' score(probability) vector
   * ![Pic](/pic/DRL_structure.JPG "DRL_structure")
4. Goodness of Actor
   * actor $\pi_\theta(s)$ with network parameter $\theta$
   * total reward:$R_\theta=\sum_t^Tr_t$
   * $R_\theta$ has randomness, so we need to maximize the **expected value $\overline{R}_\theta$**
5. How to calculate the **expectation**?
   * An episode is a trajectory $\tau$
   * $\tau=\{s_1,a_1,r_1,s_2,a_2,r_2,...s_T,a_T,r_T \}$
   * $R(\tau)=\sum_{n=1}^Nr_n$
   * if you use an actor,each $\tau$ has a probability to be sampled. the probability depends on the actor parameter $\theta$
   * $\overline{R}_\theta=\sum_{\tau}R(\tau)P(\tau|\theta)$:sum over all possible trajectory.
   * Use $\pi_\theta$ to play the game N times, obtain{$\tau^1,\tau^2,...,\tau^N$},**sample** $\tau$ from P($\tau|\theta$) N times  
   * **_IMPORTANT EQUATION NO. 1:_**
   * $$\overline{R}_\theta=\sum_{\tau}R(\tau)P(\tau|\theta)\approx \frac{1}{N}\sum_{n=1}^{N}R(\tau^n)$$
   * **Sample**,对所有trajectory（路径）的期望reward（求和求平均）
6. choose best actor : Gradient Ascent
- 6.1 Problem Statement:
  $$ \theta^\star=argmax_\theta \overline{R}_\theta $$
  $$
  \overline{R}_\theta=\sum_{\tau}R(\tau)P(\tau|\theta)
  $$
- 6.2 Gradient ascent:
  * start with $\theta^0$
  * $\theta^1 \leftarrow \theta^0 + \eta\nabla\overline{R}_{\theta^0}$
  * $\theta^2 \leftarrow \theta^1 + \eta\nabla\overline{R}_{\theta^1}$
  * 。。。
  * $\theta=\{w_1,w_2,w_3,...\}$
  * $\nabla\overline{R}_{\theta}=\left[\begin{matrix}
   \partial\overline{R}_{\theta}/\partial w_1 \\
   \partial\overline{R}_{\theta}/\partial w_2\\
   ...\\
   \partial\overline{R}_{\theta}/\partial w_n
   ...
  \end{matrix} 
\right]$
  *  **_IMPORTANT EQUATION NO. 2_** (come from  **_IMPORTANT EQUATION NO. 1_**):
  *  ![Pic](/pic/Gradient_Ascent.JPG "DRL_structure")
  *  推导得：$\nabla\overline{R}_{\theta}\approx\frac{1}{N}\sum_{n=1}^{N}R(\tau^n)\nabla logP(\tau|\theta )$
  *  问题化为计算$\nabla logP(\tau|\theta )$
  #### 计算 $\nabla logP(\tau|\theta )$
  * $\tau=\{s_1,a_1,r_1,s_2,a_2,r_2,...s_T,a_T,r_T \}$
  * ![Pic_dlog](/pic/dlog.JPG "dlog")
  * 公式含义（游戏为例）：$\theta$下采取$\tau$路径的概率 = 第一画面为s1的概率*（s1,$\theta$）下采取a1动作的概率*(s1,a1)下转移到s2动作并获得r1奖励的概率*。。。
  * 取log，乘变加，对$\theta$微分，忽略无关部分
  * ![Pic_dlog2](/pic/dlog2.JPG "dlog")
  * so，结合之前，可得到：![Pic_dlog2](/pic/dlog3.JPG "dlog")
  * 注意：公式中，p概率是在时间t下的状态s和动作a，但是reward R是全过程的值；理解为有些动作不会产生reward，但是会对全局reward产生影响
  * 注意2：为什么取log？![Pic_dlog2](/pic/dlog4.JPG "dlog")
    * 即是dp/p，相当于做了normalization，去除概率大小本身的影响，只考虑变化
7. Tips
  * 1. Add a baseline: 防止没sample到的action的概率减少
  ![Pic_dlog2](/pic/dlog5.JPG "dlog")
  * 2. Assigne Suitable Credit 加weight
  ![Pic_dlog2](/pic/Tip2.JPG "dlog")
  * 只取动作之后的reward，并且按时间远近做discount，$\gamma$, 即遗忘因子
  * Advantage Function: $A^{\theta}\left(s_{t}, a_{t}\right)$=$R\left(\tau^{n}\right)$

## 2. Proximal Policy Optimization (PPO)
https://www.bilibili.com/video/av24724071/?p=2
1. On-plicy -> off-policy
2. PPO:
  * (1) $J_{P P O}^{\theta^{\prime}}(\theta)=J^{\theta^{\prime}}(\theta)-\beta K L\left(\theta, \theta^{\prime}\right)$
  * (2) $J^{\theta^{\prime}}(\theta)=E_{\left(s_{t}, a_{t}\right) \sim \pi_{\theta^{\prime}}}\left[\frac{p_{\theta}\left(a_{t} | s_{t}\right)}{p_{\theta^{\prime}}\left(a_{t} | s_{t}\right)} A^{\theta^{\prime}}\left(s_{t}, a_{t}\right)\right]$
  * (3) $\nabla f(x)=f(x) \nabla \log f(x)$
3. 解释：(1)为PPO,在J上加入两个分布的KL散度，使两个分布接近；(2) 为off-policy公式，通过$\theta^{\prime}$来得到$\theta$; (3)为f和df的公式
4. 注意：$\beta K L\left(\theta, \theta^{\prime}\right)$ 不是参数距离，而是action的距离。是output的action distribution的距离。
5. PPO2: 
$$
J_{P P O 2}^{\theta^{k}}(\theta) \approx \sum_{\left(s_{t}, a_{t}\right)} \min \left(\frac{p_{\theta}\left(a_{t} | s_{t}\right)}{p_{\theta^{k}}\left(a_{t} | s_{t}\right)} A^{\theta^{k}}\left(s_{t}, a_{t}\right)\right.,
\operatorname{clip}\left(\frac{p_{\theta}\left(a_{t} | s_{t}\right)}{p_{\theta^{k}}\left(a_{t} | s_{t}\right)}, 1-\varepsilon, 1+\varepsilon\right) A^{\theta^{k}}\left(s_{t}, a_{t}\right) )$$

## 3. Q-Learning
1. 训练critic
2. how to estimate V(s)
   * $V^\pi(s)$ 是s状态之后按照$\pi$策略所有reward的期望
   * Monte-Carlo (MC) based approach：
     *  看很多state，玩到游戏结束，获得在这些state之后的V，然后成为 _回归问题_
   * Temporal-difference(TD) approach
     * 前后两个s，a 的 V 做差得到 $r_t$
3. Another Critic: **Q function**
   * State-action value function $Q^\pi(s,a)$
   * Q function强制使用a at state s when using actor $\pi$；对比：V function只考虑state s and use action a in actor $\pi$
   * 再解释一遍：V function，计算在状态s以后，使用$\pi$策略的reward； Q function，计算在状态s和使用a动作（这一次不一定按照$\pi$策略）后，后面全部采用$\pi$策略得到的reward
   * learn Q，然后找new pi
   * 一定可以找到比$\pi$好的$\pi'$
  $$ \pi^{\prime}(s)=\arg \max _{a} Q^{\pi}(s, a) $$
   * 解释：在$\pi$策略下改变a，穷举所有a使得Q最大，则新的a就是$\pi'$

 ![Q_learning](/pic/Q_learning.JPG "Q_learning")

## 4. Tips
### 4.1. Double DQN
1. Q value is usually over estimated
2. Double DQN: two functions Q and Q'
3. 用真正 Q 选(提案) action，用target Q 执行计算Q value
### 4.2. Dueling DQN
1. 改network 架构
2. 不直接输出Q value，而是：1）V(s);2) A(s,a);求和
### 4.3. Prioritized Reply
### 4.4. Multi-step
1. Balance between MC and TD
2. N个step以后的reward（既不是只有一个，也不是后面全部）
### 4.5. Noisy Net
1. Noise on Action(Epsilon Greedy)
2. Noise on Parameters: 参数加高斯noise，在episode开始添加noise，然后固定
### 4.6. Distributional Q-function
1. model distribution
2. 原始：期望值
3. 改进：把每个action拆分成多个bin,然后预测每个bin的几率。
### 4.7. Rainbow : 以上所有方法都用上

## 小总结：
* Policy based, 拟合（learn）神经网络参数，即state-actor函数，目标是最大化reward，梯度优化reward函数。拟合直接得到actor。
* value based， 就是learn（神经网络拟合）V/Q函数(即本次state之后的reward期望)，目标是最大化每一步action的reward(即两个连续state下Q函数的差)，拟合得到对action的评分。


## 5. Actor-Critic （Asynchronous Advantage Actor-Critic (A3C)）
### 5.1 A2C: Advantage Actor-Critic
1. advantage function:
   $$ \nabla \overline{R}_{\theta} \approx \frac{1}{N} \sum_{n=1}^{N} \sum_{t=1}^{T_{n}}\left(r_{t}^{n}+V^{\pi}\left(s_{t+1}^{n}\right)-V^{\pi}\left(s_{t}^{n}\right)\right) \nabla \log p_{\theta}\left(a_{t}^{n} | s_{t}^{n}\right)$$
2. 综合Policy 和 value方式，用V函数表示Policy gradient函数。
3. 这样就会出现两个Network:一个actor，一个V function. 前面几层可以共用，如CNN
   ![这是A2C](/pic/A2C.JPG "这是A2C")
### 5.2 A3C：非同步
多个CPU同时学习，然后异步上传参数更新，覆盖原有参数
![这是A3C](/pic/A3C.JPG "这是A3C")

### 5.3. Pathwise Derivative Policy Gradient
* ![Pathwise](/pic/Pathwise.JPG "Pathwise")
* 说明：加一个network生成actor，输入给critic，使得Q增加。训练时fix Q，调节Actor
* ![Pathwise2](/pic/Pathwise2.JPG "Pathwise2")

## 6. Sparse Reward
1. reward shaping, need domain knowledge
2. Curiosity: ICM(intrinsic curiosity module )
3. Curriculum learning
   * 给机器学习做规划，从简单到复杂
   * Reverse Curriculum Generation:
     * given a goal state s_g
     * sample some states close to s_g
     * start from s_1, reward R(s)
     * delete s whose reward is too large or too small (too easy or diffiicult at this moment)
     * sample s_2 from s_1, start from s_2
4. Hierachical Reinforcement learning

## 7. Imitation Learning
1. 不知道怎么制定reward，但是可以收集好的操作资料（expert），比如用真人之间的谈话来训练机器
2. Behavior Cloning.
   * supervised learning
   * 问题：observation limited
   * 解决：Dataset Aggregation. 
     * 采用特定$\pi$,但是收集期间expert的建议
     * 用各种情况下expert的建议来学习
   * 问题2：复制无用行为
3. Inverse Reinforcement Learning
   * 有expert的action, 反推Reward Function
   * 用Reward Function重新找optimal actor