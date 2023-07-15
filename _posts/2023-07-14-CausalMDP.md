# Reinforcement Learning: Markov Decision Process with Prior Causal Knowledge
In tabular Markov decision processes where the state and action spaces have finite cardinalities, strategic exploration algorithms have been developed, and their regret or sample complexity can be bounded with the number of states $S$ and actions $A.$ However, both $S$ and $A$ can be very large in practice.

To circumvent the curse of dimensionality where $A$ is enormous, a causal approach can be utilized to reduce the exploration steps. Instead of treating all interventions independently (as in standard RL), we can connect the intervention set with the low dimensional key variables based on some prior causal knowledge. Moreover, state factorization can be applied to deal with large $S.$

## Preliminaries and Notations
### Causal Graph
A directed acyclic graph $\mathcal{G}$ is used to model the causal structure over a set of random variables $\mathbf{X}=\lbrace X_1,\cdots,X_n\rbrace$ in $\boldsymbol{\mathcal{X}}=\mathcal{X}_ 1\times\cdots\times\mathcal{X}_ n.$ Denote the joint distribution over $\mathbf{X}$ along graph $\mathcal{G}$ at states $s$ as $p(\cdot\vert s).$ A size $m$ intervention (action) corresponds to $\mathrm{do}(\mathbf{X}_ a=\mathbf{x}_ a)$ such that $\mathrm{dim}(\mathbf{X}_ a)=m.$ Such an intervention results a new graph $\mathcal{G}_ {\overline{\mathbf{X}_ a}}$ and induces a probability distribution $p(\cdot\vert s,\mathrm{do}(\mathbf{X}_ a=\mathbf{x}_ a))$ over $\mathbf{X}\backslash\mathbf{X}_ a.$

### Markov Decision Process (MDP)
A tabular episodic MDP is defined by a tuple $\mathcal{M}=(\mathcal{S},\mathcal{A},\mathbb{P},R,H),$ where $\mathcal{S}$ and $\mathcal{A}$ are state and action spaces with finite cardinalities $S$ and $A,$ respectively, $H$ is the planning horizon in each episode, $\mathbb{P}$ is a transition kernel such that $\mathbb{P}(\cdot\vert s,a)$ defines the distribution over next state if an action $a\in\mathcal{A}$ is taken on state $s\in\mathcal{S},$ and $R=\lbrace R_h\rbrace_{h\in[H]}$ is a group of rewards such that $R_h:\mathcal{S}\times\mathcal{A}\to [0,1]$ is a deterministic reward function over a state-action pair.

The agent interacts with the enviroment in a sequence of episodes: at the begining, an initial state $s_1\in\mathcal{S}$ is picked by an adversary. At each step $h\in[H],$ the agent observes the environment state $s_h\in\mathcal{S},$ takes an action $a_h\in\mathcal{A}$ and obtains a reward $R_h(s_h,a_h).$ The environment samples the next state $s_{h+1}\sim\mathbb{P}(\cdot\vert s_n,a_h).$ The episode ends when $s_{h+1}$ is reached.

A (deterministic) policy is defined by a mapping $\mathcal{S}\times[H]\to\mathcal{A}.$ The value function $V^{\pi}_h:\mathcal{S}\to\mathbb{R}$ is defined as the expected sum of remaining rewards starting from step $h,$ under the policy $\pi.$ i.e.,
<p>
  $$V^\pi_h(s) := \mathbb{E}_\pi\left[\left.\sum_{t=h}^H R_t(s_t,a_t)\right\vert s_h=s\right].$$
</p>

Similarly, the Q-value $Q_h^\pi:\mathcal{S}\times\mathcal{A}\to\mathbb{R}$ is defined as
<p>
  $$Q^\pi_h(s) := \mathbb{E}_\pi\left[\left.\sum_{t=h}^H R_t(s_t,a_t)\right\vert s_h=s,a_h=a\right].$$
</p>

Furthermore, a policy $\pi$ also induces the state transition kernel $\mathbb{P}^\pi$ such that $\mathbb{P}^\pi_ h(\cdot\vert s) = \mathbb{P}_ h(\cdot\vert s,\pi_ h(s)),$ and the reward function $r^\pi$ such that $r^\pi_ h: s \mapsto R(s,\pi_h(s)).$ 

For every $V:\mathbf{S}\to\mathbb{R},$ define
<p>
  $$\begin{align}
  (\mathbb{P}_hV)(s,a) &:= \mathbb{E}_{s'\sim\mathbb{P}_h(\cdot|s,a)}[V(s')] = \sum_{s'\in\mathcal{S}}\mathbb{P}_h(s'\vert s,a)V(s'),\\
  (\mathbb{P}^\pi_hV)(s) &:= \mathbb{E}_{s'\sim\mathbb{P}^\pi_h(\cdot|s)}[V(s')] = \sum_{s'\in\mathcal{S}}\mathbb{P}^\pi_h(s'\vert s)V(s').
  \end{align}$$
</p>

## Causal MDP
In causal MDPs, the actions are composed by interventions. We denote the reward and state variables by $\mathbf{R}$ and $\mathbf{S},$ respectively. Suppose that the agent is able to intervene on variables $\mathbf{X}^I,$ but not able to intervene on the parent variables of $\mathbf{R}$ (denoted as $\mathbf{Z}^\mathbf{R}:=Pa(\mathbf{R})_ \mathcal{G}$) and $\mathbf{S}$ (denoted as $\mathbf{Z}^\mathbf{S}:=Pa(\mathbf{S})_ \mathcal{G}$). The action space is defined as

<p>
  $$\mathcal{A} = \left\lbrace\mathbf{do}(\mathbf{X}_a=\mathbf{x}_a)\vert \mathbf{X}_a\subseteq\mathbf{X}^I,\mathbf{x}_a\in\bigotimes_{i:X_i\in\mathbf{X}_a}\mathcal{X}_i\right\rbrace.$$
</p>

At each state $s\in\mathcal{S},$ two causal graphs are defined: the reward graph $\mathcal{G}^R(s)$ and the state transition graph $\mathcal{G}^S(s),$ which contain variables $\mathbf{X}^R = \mathbf{X}^I\cup\mathbf{Z}^\mathbf{R}\cup\mathbf{R}$ and $\mathbf{X}^S = \mathbf{X}^I\cup\mathbf{Z}^\mathbf{S}\cup\mathbf{S}.$ The variation of $s$ does not impact the identity of variables, while it possibly changes their underlying distributions.

In a causal MDP, a learner is given the intervention set $a\in\mathcal{A},$ the identity of parent variables $\mathbf{Z}=\mathbf{Z}^\mathbf{R}\cup\mathbf{Z}^\mathbf{S}$ and their conditional distributions of $\mathbf{Z}$ given any state action pair $(s,a)\in\mathcal{S}\times\mathcal{A},$ denoted as $p(\mathbf{z}\vert s,a).$

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

Therefore, a causal MDP is defined by a tuple $\mathcal{M}_C = (\mathcal{S},\mathcal{A},\mathbb{P},R,H,\mathcal{G}^R,\mathcal{G}^S),$ which can be viewed as a MDP equipped with dynamic causal graphs $\mathcal{G}^R$ and $\mathcal{G}^S.$ For simplicity, the transition kernel $\mathbb{P}$ and rewards $R$ does not vary with step $h.$  Moreover, assume the following regularity condition holds.

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

C-UCB-VI maintains a dataset $\mathcal{H}$ of historical tuples $(s_ h^k,a_ h^k,\mathbf{z}_ h^k, s_ {h+1}^k)$ indexed by episode and step to estimate the transition kernel. After each episode, the transition kernel $\mathbb{P}$ is estiamted as follows:
<p>
  $$\begin{align}
  N^k(s,\mathbf{z},y) &= \sum_{(s',\mathbf{z}',y')\in\mathcal{H}}\mathbb{1}(s'=s,\mathbf{z}'=\mathbf{z},y'=y)\ \forall (s,\mathbf{z},y)\in\mathcal{S}\times\mathcal{Z}\times\mathcal{S},\\
  N^k(s,\mathbf{z}) &= \sum_{(s',\mathbf{z}')\in\mathcal{H}}\mathbb{1}(s'=s,\mathbf{z}'=\mathbf{z})\ \forall (s,\mathbf{z})\in\mathcal{S}\times\mathcal{Z},\\
  \hat{\mathbb{P}}^k(y|s,\mathbf{z}) &= \frac{N^k(s,\mathbf{z},y)}{N^k(s,\mathbf{z})}\ \forall (s,\mathbf{z},y)\in\mathcal{S}\times\mathcal{Z}\times\mathcal{S}\ \ \text{s.t.}\ N^k(s,\mathbf{z})>0.
  \end{align}$$
</p>

**Value Iteration.** Like in UCB-VI, we first propose a bonus to construct an upper confidence bound. We choose some $\delta >0.$ Then for all $(s,\mathbf{z})$ such that $N^k(s,\mathbf{z})>0,$ define the bonus
<p>
  $$b_h^k = 7HL\sqrt{\frac{S}{N^k(s,\mathbf{z})}},\ \text{where}\ L=\log(5SHKZT/\delta).$$
</p>

Based on the estimated kernel $\hat{\mathbb{P}}^k$ and bonus $b^k,$ the learner runs value iteration in each episode $k.$ Initialize $V_ {H+1}^k(s) = 0\ \forall s\in\mathcal{S},$ the following steps are executed for $h=H,H-1,\cdots,1.$
+ $\forall (s,\mathbf{z})\in\mathcal{S}\times\mathcal{Z},\ q_h^k(s,\mathbf{z}) = \min\lbrace R(s,\mathbf{z}) + \hat{\mathbb{P}}^k V_ {h+1}^k(s,\mathbf{z}) + b_ h^k(s,\mathbf{z}), H\rbrace$
   $\text{if}\ N(s,\mathbf{z}) > 0,\ \text{else}\ H.$
+ $\forall (s,a)\in\mathcal{S}\times\mathcal{A},\ Q_ h^k(s,a) = \sum_{\mathbf{z}\in\mathcal{Z}} p(\mathbf{z}\vert s,a)q_ h^k(s,\mathbf{z}).$
+ $\forall s\in\mathcal{S},\ V_ h^k(s) = \max_{a\in\mathcal{A}} Q_ h^k(s,a).$

**Running Episodes.** Since transition kernel $P$ and Q-value function are estimated, an immediate choice of policy is the one that maximizes the estimated value. Initialize the dataset $\mathcal{H}=\emptyset$ and $Q^1_ h(s,a)=H$ for all $h,s,a,$ the learner execute the following steps sequentially in each episode $k=1,\cdots,K$:
1. run value iteration to get $Q_ h^k(s,a)$ for all $(h,s,a)\in[H]\times\mathcal{S}\times\mathcal{A}.$
2. for step $h=1,\cdots,H,$ take the action $a_{k,h} = \mathrm{argmax}_ {a\in\mathcal{A}}Q_ h^{k}(s,a),$ and update $\mathcal{H}\gets\mathcal{H}\cup\lbrace(s_ h^k,a_ h^k,\mathbf{z}_ h^k,s_ {h+1}^k)\rbrace.$

Note that in the first episode, the learner always adopts a uniform policy $\pi^1_h=\mathrm{Unif}(\mathcal{A}).$

### Analysis
**Lemma 1.** (Optimism) With probability at least $1-\delta,$ we have
<p>
  $$V_h^k(s) \geq V_h^*(s)\ \forall h\in[H],k\in[K],s\in\mathcal{S}.$$
</p>

This lemma states that the optimal $V^{* }$ is upper bounded by the estimated value function $V^k$ with high probability.

**Theorem 1.** (Regret bound for C-UCB-VI). With probability at least $1-\delta,$ the regret of C-UCB-VI is bounded by:
<p>
  $$\mathrm{Regret}(K) = \tilde{O}\left(HS\sqrt{ZT}\right),$$
</p>

Where $\tilde{O}(\cdot)$ absorbs all logarithmic terms.

**Remark.** Compared with standard UCB-VI, the regret bound of C-UCB-VI does not depend on the cardinality $A$ of action space $\mathcal{A}.$  In practical applications, $Z$ is usually less than $A,$ hence the regret bound is improved via a causal approach. 

## Causal Factored MDP (CF-MDP)
**Definition 1** (Scope operation). For any subset $I$ of indices $\lbrace 1,\cdots,m\rbrace,$ define the scope set $\mathcal{S}[I] := \bigotimes_ {i\in I}\mathcal{S}_ i.$ For any $s\in\mathcal{S},$ define the scope variable $s[I]\in\mathcal{S}[I]$ to be the value of the variables $s_i\in\mathcal{S}_i$ with indices $i\in I.$ For singleton sets, write $s[\lbrace i\rbrace] = s[i]$ for simplicity.

Let $\mathcal{P}_{\mathcal{X},\mathcal{Y}}$ be the set of funnctionals mapping elements of a finite set $\mathcal{X}$ to probability mass functions over a finite set $\mathcal{Y}.$

**Definition 2** (Factored state transition) The transition function class $\mathcal{P}$ is factored over $\mathcal{S}\times\mathcal{Z}$ and $\mathcal{S} = \bigotimes_ {i\in[m]}\mathcal{S}_ i$ with scopes $I_ 1,\cdots,I_ m$ if and only if, $\forall \mathbb{P}\in\mathcal{P}, s,s'\in\mathcal{S},\mathbf{z}\in\mathcal{Z},$ there exists some $\lbrace \mathbb{P}_ i\in\mathcal{P}_ {\mathcal{S}[I_i]\times\mathcal{Z},\mathcal{S}_i}\rbrace$ such that
<p>
  $$\mathbb{P}(s'|s,\mathbf{z}) = \prod_{i=1}^m\mathbb{P}_ {[i]}(s'[i]|s[I_ i],\mathbf{z}).$$
</p>

**Definition 3** (Factored deterministic reward functions). The reward function $R$ is factored over $\mathcal{S}\times\mathcal{Z}$ with scopes $J_ 1,\cdots,J_ m,$ i.e. $\forall s\in\mathcal{S},\mathbf{z}\in\mathcal{Z}$ we have
<p>
  $$R(s,\mathbf{z}) = \sum_{i=1}^m R_i(s[J_i],\mathbf{z}).$$
</p>

Then, a causal factored MDP can be defined as a causal MDP with factored rewards and factored transitions:
<p>
  $$\mathcal{M}_{CF} = \left(\lbrace\mathcal{S}_i\rbrace_{i=1}^m, \mathcal{A},\lbrace I_i\rbrace_{i=1}^m, \lbrace\mathbb{P}_i\rbrace_{i=1}^m, \lbrace J_i\rbrace_{i=1}^m, \lbrace R_i\rbrace_{i=1}^m, \lbrace J_i\rbrace_{i=1}^m, H, \mathcal{G}^\mathbf{R}, \mathcal{G}^\mathbf{S}\right).$$
</p>

### Causal Factored UCB-VI (CF-UCB-VI)
With slight modification, we can extend the C-UCB-VI algorithm to CF-MDPs. Using factored state transition, we have
<p>
  $$\hat{\mathbb{P}}^k(s'|s,\mathbf{z}) = \prod_{i=1}^m\hat{\mathbb{P}}^k_{[i]}(s'[i]|s[I_i],\mathbf{z}) = \prod_{i=1}^m\frac{N^k_{[i]}(s[I_i],\mathbf{z},s'[i])}{N^k_{[i]}(s[I_i],\mathbf{z})}.$$
</p>

The remaining part is analogous to C-UCB-VI. Further analysis finds a regret bound for CF-UCB-VI as follows.

**Theorem 2.** (Regret bound for CF-UCB-VI). With probability at least $1-\delta,$ the regret of CF-UCB-VI is bounded by:
<p>
  $$\mathrm{Regret}(K) = \tilde{O}\left(H\sum_{i=1}^m\sqrt{S[I_i]S[i]ZT}\right),$$
</p>

Since $S = \prod_{i\in[m]} S[i],$ CF-UCB-VI indeed reduces the cost of learning compared with C-UCB-VI.

## Causal Linear MDP
**Definition 4** (Causal linear MDP). A causal linear MDP is a causal MDP equipped with a feature map $\phi:\mathbf{S}\times\mathbf{Z}\to\mathbb{R}^d,$ where there exists $d$ unknown measures $\mu = (\mu^{(1)},\cdots,\mu^{(d)})^\top$ over $\mathcal{S}$ and an unknown vector $\omega\in\mathbb{R}^d,$ such that for any $(s,\mathbf{z})\in\mathcal{S}\times\mathcal{Z},$ we have
<p>
  $$\mathbb{P}(s'|s,\mathbf{z}) = \langle \phi(s,\mathbf{z}), \mu(s')\rangle,\ R(s,\mathbf{z}) = \langle \phi(s,\mathbf{z}), \omega\rangle.$$
</p>

Furthermore, assume norm bounds:
+ $\sup_{(s,\mathbf{z})\in\mathcal{S}\times\mathcal{Z}}\Vert \phi(s,\mathbf{z})\Vert_2 \leq 1;$
+ $\left\Vert \int_\mathcal{S}f\mathrm{d}\mu(s)\right\Vert_2 \leq \sqrt{d}\ \ \forall \Vert f\Vert_\infty \geq 1;$
+ $\Vert \omega\Vert_2 \leq W.$

For every $(s,a)\in\mathcal{S}\times\mathcal{A},$ it holds
<p>
  $$\begin{align}
  \mathbb{P}(s'|s,a) &= \left\langle\sum_{\mathbf{z}\in\mathcal{Z}}p(\mathbf{z}|s,a)\phi(s,\mathbf{z}),\mu(s')\right\rangle,\\
  R(s,a) &= \left\langle\sum_{\mathbf{z}\in\mathcal{Z}}p(\mathbf{z}|s,a)\phi(s,\mathbf{z}),\omega\right\rangle.
  \end{align}$$
</p>

By defining a state-action map $\psi(s,a)=\sum_{\mathbf{z}\in\mathcal{Z}}p(\mathbf{z}|s,a)\phi(s,\mathbf{z}),$ one can treat causal linear MDPs as a special case of standard linear MDPs, and a regret bound of $\tilde{O}\left(\sqrt{d^3H^3T}\right)$ is reachable using UCB-LSVI.

## References
+ Lu Y., Meisami A. and Tewari A., 2022. Efficient Reinforcement Learning with Prior Causal Knowledge. *Proceedings of the First Conference on Causal Learning and Reasoning*, in *Proceedings of Machine Learning Research* 177:526-541.
