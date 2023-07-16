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
2. Intent: $I_t \in \lbrace i_1, \cdots, i_ k\rbrace$ represents the agent’s intended arm choice at round $t$ (prior to its final choice, $X_t$) such that $I_t = f_i(pa_{x_t}, u_t).$
3. Policy: $\pi_t \in \lbrace x_ 1, \cdots, x_ k\rbrace$ denotes the agent’s decision algorithm as a function of its history and current intent, $f_\pi(h_t, i_t).$
4. Choice: $X_t \in \lbrace x_ 1, \cdots, x_ k\rbrace$ denotes the agent’s final arm choice that is “pulled” at round $t$, $x_t = f_x(\pi_t).$
5. Reward: $Y_t \in \lbrace 0, 1\rbrace$ represents the Bernoulli reward (0 for losing, 1 for winning) from choosing arm $x_t$ under unobserved confounder state $u_t$ as decided by $y_t = f_y(x_t, u_t).$
