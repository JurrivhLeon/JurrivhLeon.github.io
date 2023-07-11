# Causal Inference: General Transportability
This blog continues to discuss the topic of combining causal knowledge from multiple heterogeneous domains. Through this passage, we use bold letter like $\mathbf{U},\mathbf{S}$ to denote the set of variables. Without ambiguity, we use $\mathbf{V}$ to denote the set of nodes in causal DAG $\mathcal{G}.$ Moreover, $p_\mathbf{x}(\mathbf{y}\vert\mathbf{z})$ is short for post-intervention distribution $p(\mathbf{y}\vert\mathrm{do}(\mathbf{x}),\mathbf{z}).$

## Formulation
We consider the set of heterogenous domains $\Pi=\lbrace\pi_1,\cdots,\pi_n\rbrace,$ where each domain associates with a SCM compatible with a common diagram $\mathcal{G}.$ Then, we fix the domain $\pi_0$ as the domain in which we are interested in answerinf a causal query. For emphasis we write $\pi_0$ as $\pi^{* }.$

**Definition 1** (Domain discrepancy). Let $\pi$ and $\pi$ be domains compatible with a causal DAG $\mathcal{G}.$ We denote $\Delta\subseteq\mathbf{V}$ as a set of variables such that, for each $V\in\Delta,$ there might exist a discrepancy; either $f_ V\neq f^{* }_ V,$ or $p(\mathbf{U}_ V) \neq p^{* }(\mathbf{U}_ V).$

In the following discussion, we denote $\Delta_i$ as the discrepancy between domain $\pi_i$ and $\pi^*.$

**Definition 2** (Selection Diagram). Given a collection of discrepancies $\boldsymbol{\Delta}=\lbrace\Delta_1,\cdots,\Delta_n\rbrace$ with respect to graph $\mathcal{G}=\langle\mathbf{V},\mathbf{E}\rangle,$ let $\mathcal{S}=\lbrace S_V: V\in\bigcup_{i\in[n]}\Delta_i\rbrace$ be the set of selection variables. Then, the selection diagram $\mathcal{G}^\boldsymbol{\Delta}$ is defined as
<p>
  $$\mathcal{G}^\boldsymbol{\Delta}:=\langle\mathbf{V}\cup\mathbf{S},\mathbf{E}\cup\lbrace S_V\to V\rbrace_{V:S_V\in\mathbf{S}}\rangle.$$
</p>

**Remark.** We denote the domain specific selection variable set by $\mathbf{S}^{(i)}=\lbrace S_V:V\in\Delta_i\rbrace,$ and the rest by $\mathbf{S}^{(-i)}=\mathbf{S}\backslash\mathbf{S}^{(i)}.$ Selection variables work like switches selecting the domain of interest. The state space of $S_V\in\mathbf{S}$ is the index set $\lbrace 0\rbrace\cup\lbrace i:V\in\Delta_i\rbrace.$ Hence, a selection diagram can be viewed as the causal diagram for a unifying SCM representing heterogeneous SCMs where 
<p>
  $$p_\mathbf{x}(\mathbf{y}|\mathbf{w},\mathbf{S}^{(i)}=i,\mathbf{S}^{(-i)}=0) = p_\mathbf{x}^{(i)}(\mathbf{y}|\mathbf{w}).$$
</p>

Now we are ready to establish the most general transportability.

**Definition 3** (General transportability, g-transportability). Let $\mathcal{G}^\boldsymbol{\Delta}$ be a selection diagram relative to source domains $\Pi=\lbrace\pi_1,\cdots,\pi_n\rbrace$ and a target domain $\pi^{* }.$ Let $\mathscr{Z}=\lbrace\mathcal{Z}^{(i)}\rbrace_ {i=1}^n$ be a specification of available experiments, where $\mathcal{Z}^{(i)}$ is the collection of sets of variables for $\pi_i$ in which experiments on each set of $\mathbf{Z}\in\mathcal{Z}^{(i)}$ can be conducted. Given disjoint sets of variables $\mathbf{X},\mathbf{Y}$ and $\mathbf{W},$ the conditional causal effect $p_\mathbf{x}(\mathbf{y}\vert\mathbf{w})$ is said to be g-transportable with respect to $\langle\mathcal{G},\mathscr{Z}\rangle$ if $p_\mathbf{x}(\mathbf{y}\vert\mathbf{w})$ is uniquely computable from $\mathcal{P}^{\Pi}_ \mathscr{Z} = \lbrace p_\mathbf{z}^{(i)}\ \vert\ \mathbf{Z}\in\mathcal{Z}^{(i)},\mathcal{Z}^{(i)}\in\mathscr{Z}\rbrace.$

**Remark.** For any $1\leq i\leq n,$ $\emptyset\in\mathcal{Z}^{(i)},$ hence $\mathcal{Z}^{(i)}$ contains not only the post-interventional distributions but also the observational distribution $p^{(i)}$ on $\pi_i$.

**Lemma 1.** A causal effect $p_\mathbf{x}(\mathbf{y}\vert\mathbf{w})$ is g-transportable with respect to $\langle\mathcal{G},\mathscr{Z}\rangle,$ if the expression $p_\mathbf{x}(\mathbf{y}\vert\mathbf{w},\mathbf{S})$ is reducible, using the rules of do-calculus, to an expression in which every term of the form $p_\mathbf{z}(\mathbf{b}\vert \mathbf{c},\mathbf{S}')$ satisfies $\mathbf{Z}\in\mathcal{Z}^{(i)}$ for some domains $\pi_i\in\Pi,$ and
<p>
  $$(\mathbf{S}\backslash\mathbf{S}')\perp\mathbf{B}|\mathbf{C}\ \text{in}\ {\mathcal{G}^\boldsymbol{\Delta}\backslash\mathbf{Z}},\ \mathbf{S}^{(i)}\cap\mathbf{S}'= \emptyset.$$
</p>

**Proof.**
