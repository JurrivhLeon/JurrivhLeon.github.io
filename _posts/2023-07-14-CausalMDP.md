# Reinforcement Learning: Markov Decision Process with Prior Causal Knowledge
In tabular Markov decision processes where the state and action spaces have finite cardinalities, strategic exploration algorithms have been developed, and their regret or sample complexity can be bounded with the number of states $S$ and actions $A.$ However, both $S$ and $A$ can be very large in practice.

To circumvent the curse of dimensionality where $A$ is enormous, a causal approach can be utilized to reduce the exploration steps. Instead of treating all interventions independently (as in standard RL), we can connect the intervention set with the low dimensional key variables based on some prior causal knowledge. Moreover, state factorization can be applied to deal with large $S.$

## Preliminaries and Notations
### Causal Graph
A directed acyclic graph $\mathcal{G}$ is used to model the causal structure over a set of random variables $\mathbf{X}=\lbrace X_1,\cdots,X_n\rbrace$ in $\boldsymbol{\mathcal{X}}=\mathcal{X}_ 1\times\cdots\times\mathcal{X}_ n.$ Denote the joint distribution over $\mathbf{X}$ along graph $\mathcal{G}$ at states $s$ as $p(\cdot\vert s).$ A size $m$ intervention (action) corresponds to $\mathrm{do}(\mathbf{X}_ a=\mathbf{x}_ a)$ such that $\mathrm{dim}(\mathbf{X}_ a)=m.$ Such an intervention results a new graph $\mathcal{G}_ {\overline{\mathbf{X}_ a}}$ and induces a probability distribution $p(\cdot\vert s,\mathrm{do}(\mathbf{X}_ a=\mathbf{x}_ a))$ over $\mathbf{X}\backslash\mathbf{X}_ a.$

### Markov Decision Process (MDP)
A tabular episodic MDP is defined by a tuple $\mathcal{M}=(\mathcal{S},\mathcal{A},\mathbb{P},R,H),$ where $\mathcal{S}$ and $\mathcal{A}$ are state and action spaces with finite cardinalities $S$ and $A,$ respectively, $H$ is the planning horizon in each episode, $\mathbb{P}$ is a transition kernel such that $\mathbb{P}(\cdot\vert s,a)$ defines the distribution over next state if an action $a\in\mathcal{A}$ is taken on state $s\in\mathcal{S},$ and $R:\mathcal{S}\times\mathcal{A}\to [0,1]$ is a deterministic reward function over a state-action pair.

The agent interacts with the enviroment in a sequence of episodes: at the begining, an initial state $s_1\in\mathcal{S}$ is picked by an adversary. At each step $h\in[H],$ the agent observes the environment state $s_h\in\mathcal{S},$ takes an action $a_h\in\mathcal{A}$ and obtains a reward $R(s_h,a_h).$ The environment samples the next state $s_{h+1}\sim\mathbb{P}(\cdot\vert s_n,a_h).$ The episode ends when $s_{h+1}$ is reached.

A (deterministic) policy is defined by a mapping $\mathcal{S}\times[H]\to\mathcal{A}.$ The value function $V^{\pi}_h:\mathcal{S}\to\mathbb{R}$ is defined as the expected sum of remaining rewards starting from step $h,$ under the policy $\pi.$ i.e.,
<p>
  $$V^\pi_h(s) := \mathbb{E}_\pi\left[\left.\sum_{t=h}^H R(s_t,a_t)\right\vert s_h=s\right].$$
</p>

Similarly, the Q-value $Q_h^\pi:\mathcal{S}\times\mathcal{A}\to\mathbb{R}$ is defined as
<p>
  $$Q^\pi_h(s) := \mathbb{E}_\pi\left[\left.\sum_{t=h}^H R(s_t,a_t)\right\vert s_h=s,a_h=a\right].$$
</p>

Furthermore, a policy $\pi$ also induces the state transition kernel $\mathbb{P}^\pi$ such that $\mathbb{P}^\pi_ h(\cdot\vert s) = \mathbb{P}_ h(\cdot\vert s,\pi_ h(s)),$ and the reward function $r^\pi$ such that $r^\pi_ h: s \mapsto R(s,\pi_h(s)).$ 

For every $V:\mathbf{S}\to\mathbb{R},$ define
<p>
  $$\begin{align}
  (\mathbb{P}_hV)(s,a) := \mathbb{E}_{s'\sim\mathbb{P}_h(\cdot|s,a)}[V(s')] = \sum_{s'\in\mathcal{S}}\mathbb{P}_h(s'\vert s,a)V(s'),\\
  (\mathbb{P}^\pi_hV)(s) := \mathbb{E}_{s'\sim\mathbb{P}^\pi_h(\cdot|s)}[V(s')] = \sum_{s'\in\mathcal{S}}\mathbb{P}^\pi_h(s'\vert s)V(s').
  \end{align}$$
</p>

## Causal MDP
In causal MDPs, the actions are composed by interventions. We denote the reward and state variables by $\mathbf{R}$ and $\mathbf{S},$ respectively. Suppose that the agent is able to intervene on variables $\mathbf{X}^I,$ but not able to intervene on the parent variables of $\mathbf{R}$ (denoted as $\mathbf{Z}^\mathbf{R}:=Pa(\mathbf{R})_ \mathcal{G}$) and $\mathbf{S}$ (denoted as $\mathbf{Z}^\mathbf{S}:=Pa(\mathbf{S})_ \mathcal{G}$). The action space is defined as

$$\mathbf{A} = \lbrace\mathbf{do}(\mathbf{X}_a=\mathbf{x}_a)\vert \mathbf{X}_a\subseteq\mathbf{X}^I,\mathbf{x}_a\in\mathcal{X}_a\rbrace.$$

At each state $s\in\mathcal{S},$ two causal graphs are defined: the reward graph $\mathcal{G}^R(s)$ and the state transition graph $\mathcal{G}^S(s),$ which contain variables $\mathbf{X}^R = \mathbf{X}^I\cup\mathbf{Z}^\mathbf{R}\cup\mathbf{R}$ and $\mathbf{X}^S = \mathbf{X}^I\cup\mathbf{Z}^\mathbf{S}\cup\mathbf{S}.$ The variation of $s$ does not impact the identity of variables, while it possibly changes their underlying distributions.

In a causal MDP, a learner is given the intervention set $a\in\mathcal{A},$ the identity of parent variables $\mathbf{Z}=\mathbf{Z}^\mathbf{R}\cup\mathbf{Z}^\mathbf{S}$ and their conditional distributions of $\mathbf{Z}$ given any state action pair $(s,a)\in\mathcal{S}\times\mathcal{A},$ denoted as $p(\mathbf{z}|s,a).$

Denote the domain set for $\mathbf{Z}$ as $\mathcal{Z},$ and its cardinality as $Z=\vert\mathcal{Z}\vert.$ At each step $h\in[H],$ the learner observes the state $s_n\in\mathcal{S}$ and the realizations $\mathbf{z}_h$ of $\mathbf{Z}.$ The causal identity implies that
<p>
  $$\begin{align}
  \mathbb{P}(s'|s,a) &= \sum_{\mathbf{z}\in\mathcal{Z}}\mathbb{P}(s|s',\mathbf{z})p(\mathbf{z}|s,a),\\
  R(s,a) &= \sum_{\mathbf{z}\in\mathcal{Z}}R(s,\mathbf{z})p(\mathbf{z}|s,a),
  \end{align}$$
</p>

where $R(s,\mathbf{z}):=\mathbb{E}[\mathbf{R}\vert s,\mathbf{z}]$ denotes the expected reward given a state and parent pair. Then, we can define the $q$-value function which is the expected sum of remaining rewards received under policy $\pi,$ given the state and parents:
<p>
  $$q_h^\pi(s,\mathbf{z}) := R(s,\mathbf{z}) + \mathbb{E}_\pi\left[\left.\sum_{t=h+1}^H R(s_t,a_t)\right| s_h=s,\mathbf{z}_h = \mathbf{z}\right].$$
</p>

By definition we have
<p>
  $$Q_h^\pi(s,a) = \sum_{\mathbf{z}\in\mathcal{Z}}p(\mathbf{z}|s,a)q^\pi_h(s,\mathbf{z}).$$
</p>

Therefore, a causal MDP is defined by a tuple $\mathcal{M}_C = (\mathcal{S},\mathcal{A},\mathbb{P},R,H,\mathcal{G}^R,\mathcal{G}^S),$ which can be viewed as a MDP equipped with dynamic causal graphs $\mathcal{G}^R$ and $\mathcal{G}^S.$ For simplicity, the transition kernel $\mathbb{P}$ does not vary with step $h.$  Moreover, assume the following regularity condition holds.

**Assumption 1** (Causal MDP regularity). For causal MDPs, $\mathcal{S},\mathcal{A},\mathcal{Z}$ are finite sets with cardinalities $S,A,Z,$ respectively. The immediate rewards $R(s,\mathbf{z})=\mathbb{E}[\mathbf{R}\vert s,\mathbf{z}] \in [0,1]$ are known for $s\in\mathcal{S},\mathbf{z}\in\mathcal{Z}.$

## Causal UCB-VI
**Goal.** Denote the number of episodes by $K,$ staring state and policy by $s_1^k$ and $\pi^k$ for each episode. The performance of learner over $T=KH$ steps can be measured by the total expected regret:
<p>
  $$\mathrm{Regret}(K) = \sum_{k=1}^K\left(V_1^*(s_1^k) - V_1^{\pi_k}(s_1^k)\right),$$
</p>

where the optimal policy $\pi^{* } := \underset{\pi:\ \mathcal{S}\times[H]\to\mathcal{A}}{\mathrm{argmax}}V^\pi_1(s_ 1^k)$ maximize the value function, and $V^{* }_ 1(s_ 1^k) = V^{\pi^{* }}_ 1(s_ 1^k) = {\max}_ {\pi:\ \mathcal{S}\times[H]\to\mathcal{A}}V^\pi_1(s_1^k).$

The goal of our learner is to follow a sequence of policies $\pi_1,\cdots,\pi_K$ that minimizes $\mathrm{Regret}(K).$

### Algorithm
Lu et al. (2022) has proposed an efficient algorithm for causal MDPs named causal UCB-VI (C-UCB-VI), which generalizes upper confidence bound value iteration (UCB-VI) algorithm to its causal counterpart.

C-UCB-VI maintains a dataset $\mathcal{H}$ of historical tuples $(s_ h^k,a_ h^k,\mathbf{z}_ h^k, s_ {h+1}^k)$ indexed by episode and step.



## References
+ Lu Y., Meisami A. and Tewari A., 2022. Efficient Reinforcement Learning with Prior Causal Knowledge. *Proceedings of the First Conference on Causal Learning and Reasoning*, in *Proceedings of Machine Learning Research* 177:526-541.
