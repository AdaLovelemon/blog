---
title: Week 1
published: 2025-03-11
tags: [Study, Blogs, Coding]
category: Study Logs
draft: true
image: cover.jpg
---

# 📚课堂收获
### TD迭代公式

$$
\large
v_{t+1}(s) = \begin{cases}
v_t(s) - \alpha_t(s) [v_t(s) - (r_{t+1} + \gamma v_t(s_{t+1}))], \quad \text{if} \ s = s_t \\
v_t(s), \quad \text{if} \ s \neq s_t
\end{cases}
$$
需要注意的是，这才是真正的迭代公式。每一轮，每个状态的价值函数都会更新，但是只有$t$时刻选到的状态$s_t$的价值函数才有真正更新。注意，
$$
\alpha_t(s) > 0 \quad \text{if} \ s = s_t \quad \text{else} \quad \alpha_t(s) = 0 
$$
证明算法收敛性的一个通法是将$t$时刻的各个价值函数与最终收敛的价值函数做差，证明差随$t$收敛至0即可。

#### 引理：Dvoretzky's convergence theorem
Consider a stochastic process
$$
\large
\begin{align}
\Delta_{t+1} = (1 - \alpha_t)\Delta_t + \beta_t \eta_t
\end{align}
$$
where $\{\alpha_t\}_{t=1}^\infty$, $\{\beta_t\}_{t=1}^\infty$ and $\{\eta_t\}_{t=1}^\infty$ are stochastic sequences, and $\alpha_t > 0, \beta_t > 0$ for all $t$. Then $\Delta_t$ converges to zero almost surely if the following conditions are satisfied:
$$
\large
\begin{align}
&(i). \ \sum_{t=1}^\infty \alpha_t = \infty, \sum_{t=1}^\infty \alpha_t^2 < \infty, and \ \sum_{t=1}^\infty \beta_t^2 < \infty \\
&(ii). \ \mathbb{E}[\eta_t|\mathcal{H}_t] = 0, and \ \mathbb{E}[\eta^2_t|\mathcal{H}_t] \le C \ almost \ surely\\
&where \ \mathcal{H}_t = \{\Delta_t, \Delta_{t-1}, \dots, \eta_{t-1}, \dots, \alpha_{t-1}, \dots,\beta_{t-1}, \dots \}
\end{align}
$$
#### 收敛性证明
$$
\large 
\Delta_t(s) := v_{t+1}(s) - v_\pi(s)
$$
有
$$
\large
\Delta_{t+1}(s) = (1 - \alpha_t(s))\Delta_t(s) + \alpha_t(s) \eta_t(s)
$$
其中
$$
\large
\eta_t(s) = \begin{cases}
r_{t+1} + \gamma v_t(s_{t+1}) - v_\pi(s_t), \quad if \ s = s_t\\
0, \quad else
\end{cases}
$$
$s \ne s_t$条件下的$\eta_t(s)$是在这里定义的，因为实际上，$\alpha_t(s) = 0$,这个值只要是常数就行。
由于Markovian性质，$\eta_t(s)$与历史信息无关(注意是历史信息，t+1以上的不是)只要$s$确定。
对于$s \ne s_t$, 
$$
|\mathbb{E}[\eta_t(s)]| = 0 \le \gamma ||\Delta_t||_\infty
$$
对于$s = s_t$,
$$
\large
\mathbb{E}[\eta_t(s_t)] = \mathbb{E}[r_{t+1} + \gamma v_t(s_{t+1}) | s_t] - v_\pi(s_t)
$$
由于
$$
\large
v_\pi(s_t) = \mathbb{E}[r_{t+1} + \gamma v_\pi(s_{t+1}) | s_t]
$$
因此
$$
\large
\mathbb{E}[\eta_t(s_t)] = \gamma\mathbb{E}[v_{t}(s_{t+1}) - v_\pi(s_{t+1})| s_t] \le \gamma \max_{s'\in S}|v_t(s') - v_\pi(s)| = \gamma ||\Delta_t(s)||_\infty
$$
满足Dvoretzky's theorem。
从而得到收敛。

****
# 📜论文阅读



****
# 💻代码积累
### 三维空间李群李代数
将旋转矩阵转换为旋转向量基于Lie群和Lie代数之间的对应关系，核心是指数映射和对数映射。

#### 李群$SO(3)$与李代数$so(3)$
旋转矩阵属于$SO(3)$, 李代数$so(3)$由所有反对称矩阵构成，
$$
so(3) = \{ \omega \in \mathbb{R}^3 | w^\top = -\omega\} 
$$
每个反对称矩阵对应一个三维向量。

#### 李代数到李群：指数映射
$$
R = \exp(\omega) = I + \sin\theta \ [u]_\times + (1 - \cos\theta)[u]_\times^2 
$$
#### 李群到李代数: 对数映射
$$
tr(R) = R_{11} + R_{22} + R_{33} = I + 2\cos \theta
$$
$$
\theta = \arccos(\frac{tr(R) - 1}{2})
$$
$$旋转轴\quad
Ru = u
$$
$$
u = \frac{1}{2\cos\theta}\begin{bmatrix} R_{32} - R_{23} \\ R_{13} - R_{31} \\ R_{21} - R_{12}\end{bmatrix}
$$
因为
$$
R - R^\top = 2\sin\theta [u]_\times
$$
$$旋转向量 \quad
v = \theta u
$$
当$\theta \rightarrow 0$ 和 $\theta \rightarrow \pi$ 时，存在特殊情况

****
# ✨心得体会



****
# 🧭目标规划

