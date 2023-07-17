# Reinforcement Learning: Multi-Armed Bandit Problem with Unobserved Confounders
In many reinforcement learning tasks with complex environments, active learning agents make decisions according to their observation and receive feedback that depends on the quality of their choices. If data collected from observing other agents are available, then one may exploit these data for more efficient experimentation. 

However, data from different sources are often collected under heterogenous conditions, with unobserved confounders that influence the action an agent would take as well as the outcome of
this action. Averaging out these confounders possibly introduces bias to our estimation.

The goal of our agent is to quickly learn about its environment by consolidating data collected from observing other agents and data collected through its own experience. Three types of data may be employed by an autonomous agent to inform its decision-making:
+ **Observational data** is gathered through passive examination of the actions and rewards of agents other than the actor, but for whom the actor is assumed to be exchangeable.
+ **Experimental data** is gathered through randomization (e.g., standard MAB exploration), or from a fixed, non-reactive policy.
+ **Counterfactual data** (though traditionally computed from a fully specified model or under specific empirical conditions) represents the rewards associated with actions under a particular (or “personalized”) instantiation of the unobserved confounders.

## $K$-Armed Bandits with Unobserved Confounders
**Setting.** In standard multi-armed bandit problems, an agent is faced with $K\geq 2$ arms, each associated with an unknown independent distribution of rewards. In each round the agent pulls an arm and receives a reward sampled from the corresponding distribution. The goal of the agent is to maximize the cumulative rewards over a series of rounds.

**Definition 1** (Intent). Consider a structural causal model $\mathcal{M}$ and an endogeneous variable $X\in{V}$ which is determined by the structural equation $X=f_x({Pa}_ X,{U}_ X)$ and can be intervene on. After realization ${Pa}_ X={pa}_ x,{U}_ X={u}_ x,$ the output of the structural function given the current configuration of all unobserved confounders is said to be the agent's intent, i.e. $I=f_x({pa}_ x,{u}_ x).$ 

Intent can be viewed as an agent’s chosen action before its execution, which is a proxy for any influencing unobserved confounders.

**Definition 2** ($K$-armed bandits with unobserved confounders, MABUC) A $K$-armed bandit problem ($K\in\mathbb{N},K\geq 2$) with unobserved confounders is defined as a SCM $\mathcal{M}$ with a reward distribution over $p(u)$ where, for each round $0 < t < T,\ t\in\mathbb{N}:$
1. Unobserved confounders: $U_t$ represents the unobserved variables encoding the payout rate and unobserved influences to the propensity to choose arm $x_t$ at round $t.$
2. Intent: $I_t \in \lbrace x_ 1, \cdots, x_ k\rbrace$ represents the agent’s intended arm choice at round $t$ (prior to its final choice, $X_t$) such that $I_t = f_i(pa_{x_t}, u_t).$
3. Policy: $\pi_t \in \lbrace x_ 1, \cdots, x_ k\rbrace$ denotes the agent’s decision algorithm as a function of its history and current intent, $f_\pi(h_t, i_t).$
4. Choice: $X_t \in \lbrace x_ 1, \cdots, x_ k\rbrace$ denotes the agent’s final arm choice that is “pulled” at round $t$, $x_t = f_x(\pi_t).$
5. Reward: $Y_t \in \lbrace 0, 1\rbrace$ represents a Bernoulli reward (0 for losing, 1 for winning) from choosing arm $x_t$ under unobserved confounder state $u_t$ as decided by $y_t = f_y(x_t, u_t).$

A graphical illustration of MABUC is given below.

<div align='center'>
   <img src='https://github.com/JurrivhLeon/JurrivhLeon.github.io/raw/main/figs/mabuc.png' width='350'>
</div>

At every round $t$ of MABUC, the unobserved state $u_t$ is drawn from distribution $p_(u)$, which is then considered by the strategy $\pi_t$ in concert with the game’s history $h_t$; the
strategy makes a final arm choice, which is then pulled, as represented by $x_t,$ and the reward $y_t$ is revealed.

**Definition 3** (Regret decision criterion, RDC). In a MABUC instance with arm choice $X,$ intent $I = i$, and reward $Y,$ agents should choose the action a that maximizes their intent-specific reward, or formally:

<p>$$a^* = \underset{a}{\mathrm{argmax}}[Y_a|X=i],$$</p>

where $Y_a$ is the counterfactual had $X$ been $a.$

## Data Fusion Strategy
Suppose that the agent in MABUC possesses:
+ observation of arm choices and payouts from other players, in forms of $\mathbb{E}[Y\vert X]$;
+ the randomized experimental results from the state investigator, in forms of $\mathbb{E}[Y\vert \mathbf{do}(X)]$;
+ the knowledge to employ intent in its decision-making for choosing arms by maximizing the counterfactual RDC $\mathbb{E}[Y_a\vert X=i]$ where $i$ encodes information about unobserved confounders since $i=f_i(pa_{x},u)$.

We expand the expected counterfactual $\mathbb{E}[Y_x]$ as follows:
<p>
  $$\mathbb{E}[Y_x] = \sum_{i=1}^K \mathbb{E}[Y_x|X=x_i]\mathbb{P}(X=x_i). \tag{1}$$
</p>

The LHS $\mathbb{E}[Y_x]$ can be estimated from the experimental dataset since $\mathbb{E}[Y_x]=\mathbb{E}[Y\vert\mathrm{do}(X=x)].$ On the RHS, the counterfactual term $\mathbb{E}[Y_x\vert X=x_i],$ referred to as the effect of treatment on the treated (ETT), is the expectation of intent-specified counterfactual, which is to be maximized by RDC. The following theorem states that RDC indeed measures the counterfactual quantity of ETT.

**Theorem 1.** The counterfactual ETT is empirically estimable for arbitrary action-choice dimension when agents condition on their intent $I = i$ and estimate the response $Y$ to their final action choice $X = a.$

**Proof.** Expand the ETT:
<p>
  $$\begin{align}
  \mathbb{E}[Y_a|X=i] &= \sum_{i'\in\mathcal{X}}\mathbb{E}[Y_a|X=i,I=i']\mathbb{P}(I=i'|X=i)\\
  &= \sum_{i'\in\mathcal{X}}\mathbb{E}[Y_a|I=i']\mathbb{P}(I=i'|X=i)\\
  &= \sum_{i'\in\mathcal{X}}\mathbb{E}[Y_a|I_a=i']\mathbb{P}(I=i'|X=i)\\
  &= \sum_{i'\in\mathcal{X}}\mathbb{E}[Y|\mathrm{do}(A=a),I=i']\mathbb{P}(I=i'|X=i)\\
  &= \mathbb{E}[Y|\mathrm{do}(A=a),I=i].
  \end{align}$$
</p>

The second equality follows from $\lbrace Y_x\rbrace_{x\in\mathcal{X}}\perp X\ \vert\ I.$ The third equality, where $I_a$ is the counterfactual version of $I$ intervening $X=a,$ follows from $I\vert X$ in $\mathcal{G}_{\overline{X}}.$ The last equality holds by the fact that an agent’s final arm choice will always coincide with their intent, i.e. $\mathbb{P}(I=i'\vert X=i)=\mathbb{1}(i'=i).$

### Strategies for Online Agents
Since the ETT can be estimated empirically through randomization within intent conditions, a counterfactual dataset can be recorded as follows.

|  | $i=x_1$ | $i=x_2$ | $\cdots$ | $i=x_K$ |
| :-: | :-: | :-: | :-: | :-: |
| $X=x_1$ | $\mathbb{E}[Y_{x_1}\vert x_1]$ | $\mathbb{E}[Y_{x_1}\vert x_2]$ | $\cdots$ | $\mathbb{E}[Y_{x_1}\vert x_K]$ |
| $X=x_2$ | $\mathbb{E}[Y_{x_2}\vert x_1]$ | $\mathbb{E}[Y_{x_2}\vert x_2]$ | $\cdots$ | $\mathbb{E}[Y_{x_2}\vert x_K]$ |
| $\vdots$ | $\vdots$ | $\vdots$ | $\ddots$ | $\vdots$ |
| $X=x_K$ | $\mathbb{E}[Y_{x_K}\vert x_1]$ | $\mathbb{E}[Y_{x_K}\vert x_2]$ | $\cdots$ | $\mathbb{E}[Y_{x_K}\vert x_K]$ |

The diagonal cells can be filled since $\mathbb{E}[Y_x\vert X=x]=\mathbb{E}[Y\vert X=x]$ by the axiom of consistency, and the non-diagonal cells, though not initially known, can be learned by our agent. With finite-sample concerns, different learning strategies are proposed to exploit the datasets’ relationship while managing the uncertainty implicit in a MAB learning scenario.

#### Strategy 1. Cross-Intent Learning
Consider the expansion of counterfactual quantity $\mathbb{E}[Y_x],$ a single cell in this system can be solve as:
<p>
   $$\mathbb{E}_{\text{XInt}}[Y_{x_r} | x_w] = \frac{\mathbb{E}[Y_{x_r}] - \sum_{i=1,i\neq w}^K\mathbb{E}[Y_{x_r}|x_i]\mathbb{P}(x_i)}{\mathbb{P}(x_w)}. \tag{2}$$
</p>

This form provides a systematic way of learning about arm payouts across intent conditions, which is desirable because an arm pulled under one intent condition provides knowledge about the payouts of that arm under other intent conditions.

#### Strategy 2. Cross-Arm Learning
Consider a single-cell quary $\mathbb{E}[Y_{x_r}\vert x_w].$ For any three arms $x_r, x_s, x_w$ such that $r\notin\lbrace s,w\rbrace,$ it holds
<p>
   $$\mathbb{E}[Y_{x_r}] = \sum_{i=1}^K \mathbb{E}[Y_{x_r}|x_i]\mathbb{P}(x_i),\ \ \mathbb{E}[Y_{x_s}] = \sum_{i=1}^K \mathbb{E}[Y_{x_s}|x_i]\mathbb{P}(x_i)$$
</p>

Then the query intent $\mathbb{P}(x_w)$ can be expressed as
<p>
   $$\begin{align}
   \mathbb{P}(x_w) &= \frac{\mathbb{E}[Y_{x_r}] - \sum_{i=1,i\neq w}^K\mathbb{E}[Y_{x_r}|x_i]\mathbb{P}(x_i)}{\mathbb{E}[Y_{x_r}|x_w]}\\
   &= \frac{\mathbb{E}[Y_{x_s}] - \sum_{i=1,i\neq w}^K\mathbb{E}[Y_{x_s}|x_i]\mathbb{P}(x_i)}{\mathbb{E}[Y_{x_s}|x_w]},
   \end{align}$$
</p>

and we can solve our query in terms of the paired arm $x_s$:
<p>
   $$\mathbb{E}_{\text{XArm}}[Y_{x_r} | x_w] = \frac{\left\lbrace\mathbb{E}[Y_{x_r}] - \sum_{i=1,i\neq w}^K\mathbb{E}[Y_{x_r}|x_i]\mathbb{P}(x_i)\right\rbrace\mathbb{E}[Y_{x_s}|x_w]}{\mathbb{E}[Y_{x_s}] - \sum_{i=1,i\neq w}^K\mathbb{E}[Y_{x_s}|x_i]\mathbb{P}(x_i)}. \tag{3}$$
</p>

This formula allows our agent to estimate $\mathbb{E}[Y_{x_r}\vert x_w]$ from samples in which any arm $x_s \neq x_r$ was pulled under the same intent $x_w.$ It can be viewed as information about $Y_{x_r}\vert x_w$ flowing from arm $x_s \neq x_r$ to $x_r$ (under intent $x_w$).

A more robust estimate of the query can be obtained via inverse-variance-weighted average. Consider a function $h_ \text{XArm}$ such that $\mathbb{E}_ {\text{XArm}}[Y_ {x_ r} \vert x_ w]=h_ \text{XArm}(x_ r,x_ w,x_ s)$ and $h_ \text{XArm}$ performs the empirical evaluation of the RHS of the equation above. Moreover, let $\sigma^2_ {x,i} = \widehat{\mathrm{Var}}(Y_ x\vert i)$ indicate the empirical payout variance for each arm-intent condition. Then our estimator is
<p>
   $$\mathbb{E}_{\text{XArm}}[Y_{x_r} | x_w] = \frac{\sum_{i=1,i\neq r}^K h_\text{XArm}(x_r,x_w,x_i) / \sigma^2_{x,i}}{\sum_{i=1,i\neq r}^K 1 / \sigma^2_{x,i}}. \tag{4}$$
</p>

#### Strategy 3. The Combined Approach
Until now, three methods are proposed to estimate an intent-specific counterfactual reward $\mathbb{E}[Y_{x_r}\vert x_s]:$
+ $\hat{\mathbb{E}}[Y_ {x_ r}\vert x_ s]$ from conditionally randomized experiment.
+ $\mathbb{E}_ {\text{XInt}}[Y_ {x_ r} \vert x_ w]$ from cross-intent learning.
+ $\mathbb{E}_ {\text{XArm}}[Y_ {x_ r} \vert x_ w]$ from cross-arm learning.

While the variance of $\hat{\mathbb{E}}[Y_ {x_ r}\vert x_ s]$ can be estimated directly as the conditional sample variance $\sigma_ {x_ r,x_ s}^2=\widehat{\mathrm{Var}}(Y_ {x_ r}\vert {x_ s}),$ the cross-intent and cross-arm estimates, as combinations of sample payout estimates, have roughly estimated variances:
<p>
   $$\begin{align}
   \sigma_\text{XInt}^2 &= \frac{1}{2K-1}\left[\left(\sum_{i=1,i\neq w}^K\sigma_{x_r,x_i}^2 \right) + \left(\sum_{i=1,i\neq w}^K\sigma_{x_s,x_i}^2\right) + \sigma_{x_s,x_w}^2\right],\\
   \sigma_\text{XArm}^2 &= \frac{1}{K-1}\sum_{i=1,i\neq w}^K\sigma_{x_r,x_i}^2.
   \end{align}$$
</p>

Then, an inverse-variance weighting scheme can be employed to leverage the three estimators:
<p>
   $$\begin{align}
   &\alpha = \hat{\mathbb{E}}[Y_{x_r}\vert x_s] / \sigma_{x_r,x_s}^2 + \mathbb{E}_{\text{XInt}}[Y_{x_r} \vert x_w] / \sigma_\text{XInt}^2 + \mathbb{E}_{\text{XArm}}[Y_{x_r} \vert x_w] / \sigma_\text{XArm}^2,\\
   &\beta = 1 / \sigma_{x_r,x_s}^2 + 1 / \sigma_\text{XInt}^2 + 1 / \sigma_\text{XArm}^2,\\
   &\mathbb{E}_\text{combo}[Y_{x_r}\vert x_s] = \alpha / \beta.
   \end{align}$$
</p>

### Overview
The data fusion process can be summerized in the figure below. I borrowed it from Forney et al. (2017).

<div align='center'>
   <img src='https://github.com/JurrivhLeon/JurrivhLeon.github.io/raw/main/figs/fusion.png' width='450'>
</div>

Here is an illustration.
1. Assume that our agent has collected large samples of experimental and observational data from its environment.
2. Unobserved confounders (UCs) in the environment are realized by the agent, though their labels and values are unknown.
3. From these UCs and any other observed features in the environment, the agent’s heuristics suggest an action to take, i.e., its intent. With its intent known, the agent combines the data in its history (in this work, by the prescription of Strategy 3 above) to better inform its decision-making.
4. Based on its intent and combined history, the agent commits to a final action choice.
5. The action’s response in the environment (i.e., its reward) is observed, and the collected data point is added to the agent’s counterfactual dataset.

## Thompson Sampling with RDC
To address the MABUC problem using data fusion strategy, Forney et al. proposed an implementation of RDC using Thompson Sampling as its basis. In each round $t,$ the agent performs as follows:
+ Observe the intent $i_t$ from the realization of unobserved confounders $u_t$ in the current round;
+ For each arm $x_ r,\ r=1,\cdots,K,$ sample $\hat{\mathbb{E}}[Y_ {x_ r}\vert i_ t]$ from distribution $\mathrm{Beta}(s_ {x_ r,i_ t},f_ {x_ r,i_ t})$ in which $s_ {x_ r,i_ t}$ is the number of successes ($Y=1$) and $f_ {x_ r,i_ t}$ the number of failures ($Y=0$).
+ Compute the $i_t$-specific score for each arm using the combined datasets via Strategy 3.
+ According to RDC, choose the arm $x_a$ with the highest $i_t$-specific score.
+ Observe the result and update $\hat{\mathbb{E}}[Y_ {x_ a}\vert i_ t].$

## References
+ Forney, A., Pearl, J. and Bareinboim, E., 2017. Counterfactual Data-Fusion for Online Reinforcement Learners. *Proceedings of the 34th International Conference on Machine Learning*, in *Proceedings of Machine Learning Research* 70:1156-1164.


