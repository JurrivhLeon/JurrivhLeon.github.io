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
+ observation of arm choices and payouts from other players, in forms of $\mathbf{E}[Y\vert X]$;
+ the randomized experimental results from the state investigator, in forms of $\mathbb{E}[Y\vert \mathbf{do}(X)]$;
+ the knowledge to employ intent in its decision-making for choosing arms by maximizing the counterfactual RDC $\mathbb{E}[Y_a\vert X=i]$ where $i$ encodes information about unobserved confounders since $i=f_i(pa_{x},u)$.

We expand the expected counterfactual $\mathbb{E}[Y_x]$ as follows:
<p>
  $$\mathbb{E}[Y_x] = \sum_{i=1}^K \mathbb{E}[Y_x|X=x_i]\mathbb{P}(X=x_i).$$
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
Since the ETT can be estimated empirically through randomization within intent conditions, a counterfactual reward-history table can be recorded as follows.

|  | $I=x_1$ | $I=x_2$ | $\cdots$ | $I=x_K$ |
| :-: | :-: | :-: | :-: | :-: |
| $X=x_1$ | $\mathbb{E}[Y_{x_1}\vert x_1]$ | $\mathbb{E}[Y_{x_1}\vert x_2]$ | $\cdots$ | $\mathbb{E}[Y_{x_1}\vert x_K]$ |
| $X=x_2$ | $\mathbb{E}[Y_{x_2}\vert x_1]$ | $\mathbb{E}[Y_{x_2}\vert x_2]$ | $\cdots$ | $\mathbb{E}[Y_{x_2}\vert x_K]$ |
| $\vdots$ | $\vdots$ | $\vdots$ | $\ddots$ | $\vdots$ |
| $X=x_K$ | $\mathbb{E}[Y_{x_K}\vert x_1]$ | $\mathbb{E}[Y_{x_K}\vert x_2]$ | $\cdots$ | $\mathbb{E}[Y_{x_K}\vert x_K]$ |

The diagonal cells can be filled since $\mathbb{E}[Y_x\vert I=x]=\mathbb{E}[Y\vert I=x]$ by the axiom of consistency, and the non-diagonal cells, though not initially known, can be learned by our agent. With finite-sample concerns, different learning strategies are proposed to exploit the datasets’ relationship while managing the uncertainty implicit in a MAB learning scenario.

#### Strategy 1. Cross-Intent Learning
Consider the expansion of counterfactual quantity $\mathbb{E}[Y_x],$ a single cell in this system can be solved；
<p>
   $$\mathbb{E}_{\text{XInt}}[Y_{x_r} | x_w] = \frac{\mathbb{E}[Y_{x_r}] - \sum_{i=1,i\neq w}^K\mathbb{E}[Y_{x_r}|x_i]\mathbb{P}(x_i)}{\mathbb{P}(x_w)}$$
</p>

