# Causal Inference: General Transportability
This blog continues to discuss the topic of combining causal knowledge from multiple heterogeneous domains. Through this passage, we use bold letter like $\mathbf{U},\mathbf{S}$ to denote the set of variables. Without ambiguity, we use $\mathbf{V}$ to denote the set of nodes in causal DAG $\mathcal{G}.$ Moreover, $p_\mathbf{x}(\mathbf{y}\vert\mathbf{z})$ is short for post-intervention distribution $p(\mathbf{y}\vert\mathrm{do}(\mathbf{x}),\mathbf{z}).$

## Formulation
We consider the set of heterogenous domains $\Pi=\lbrace\pi_1,\cdots,\pi_n\rbrace,$ where each domain associates with a SCM compatible with a common diagram $\mathcal{G}.$ Then, we fix the domain $\pi_0$ as the domain in which we are interested in answerinf a causal query. For emphasis we write $\pi_0$ as $\pi^{* }.$

**Definition 1** (Domain discrepancy). Let $\pi$ and $\pi^{* }$ be domains compatible with a causal DAG $\mathcal{G}.$ We denote $\Delta\subseteq\mathbf{V}$ as a set of variables such that, for each $V\in\Delta,$ there might exist a discrepancy; either $f_ V\neq f^{* }_ V,$ or $p(\mathbf{U}_ V) \neq p^{* }(\mathbf{U}_ V).$

In the following discussion, we denote $\Delta_i$ as the discrepancy between domain $\pi_i$ and $\pi^*.$

**Definition 2** (Selection Diagram). Given a collection of discrepancies $\boldsymbol{\Delta}=\lbrace\Delta_1,\cdots,\Delta_n\rbrace$ with respect to graph $\mathcal{G}=\langle\mathbf{V},\mathbf{E}\rangle,$ let $\mathbf{S}=\lbrace S_V: V\in\bigcup_{i\in[n]}\Delta_i\rbrace$ be the set of selection variables. Then, the selection diagram $\mathcal{G}^\boldsymbol{\Delta}$ is defined as
<p>
  $$\mathcal{G}^\boldsymbol{\Delta}:=\langle\mathbf{V}\cup\mathbf{S},\mathbf{E}\cup\lbrace S_V\to V\rbrace_{V:S_V\in\mathbf{S}}\rangle.$$
</p>

**Remark.** We denote the domain specific selection variable set by $\mathbf{S}^{(i)}=\lbrace S_V:V\in\Delta_i\rbrace,$ and the rest by $\mathbf{S}^{(-i)}=\mathbf{S}\backslash\mathbf{S}^{(i)}.$ Selection variables work like switches selecting the domain of interest. The state space of $S_V\in\mathbf{S}$ is the index set $\lbrace 0\rbrace\cup\lbrace i:V\in\Delta_i\rbrace.$ Hence, a selection diagram can be viewed as the causal diagram for a unifying SCM representing heterogeneous SCMs where 
<p>
  $$p_\mathbf{x}(\mathbf{y}|\mathbf{w},\mathbf{S}^{(i)}=i,\mathbf{S}^{(-i)}=0) = p_\mathbf{x}^{(i)}(\mathbf{y}|\mathbf{w}).$$
</p>

Now we are ready to establish the most general transportability.

**Definition 3** (General transportability, g-transportability). Let $\mathcal{G}^\boldsymbol{\Delta}$ be a selection diagram relative to source domains $\Pi=\lbrace\pi_1,\cdots,\pi_n\rbrace$ and a target domain $\pi^{* }.$ Let $\mathscr{Z}=\lbrace\mathcal{Z}^{(i)}\rbrace_ {i=1}^n$ be a specification of available experiments, where $\mathcal{Z}^{(i)}$ is the collection of sets of variables for $\pi_i$ in which experiments on each set of $\mathbf{Z}\in\mathcal{Z}^{(i)}$ can be conducted. Given disjoint sets of variables $\mathbf{X},\mathbf{Y}$ and $\mathbf{W},$ the conditional causal effect $p_\mathbf{x}(\mathbf{y}\vert\mathbf{w})$ is said to be g-transportable with respect to $\langle\mathcal{G}^\boldsymbol{\Delta},\mathscr{Z}\rangle$ if $p_\mathbf{x}(\mathbf{y}\vert\mathbf{w})$ is uniquely computable from $\mathcal{P}^{\Pi}_ \mathscr{Z} = \lbrace p_\mathbf{z}^{(i)}\ \vert\ \mathbf{Z}\in\mathcal{Z}^{(i)},\mathcal{Z}^{(i)}\in\mathscr{Z}\rbrace.$

**Remark.** For any $1\leq i\leq n,$ $\emptyset\in\mathcal{Z}^{(i)},$ hence $\mathcal{Z}^{(i)}$ contains not only the post-interventional distributions but also the observational distribution $p^{(i)}$ on $\pi_i$.

### Characterizing g-transportability
**Lemma 1.** A causal effect $p^{* }_ \mathbf{x}(\mathbf{y}\vert\mathbf{w})$ is g-transportable with respect to $\langle\mathcal{G},\mathscr{Z}\rangle,$ if the expression $p_ \mathbf{x}(\mathbf{y}\vert\mathbf{w},\mathbf{S})$ is reducible, using the rules of do-calculus, to an expression in which every term of the form $p_\mathbf{z}(\mathbf{b}\vert \mathbf{c},\mathbf{S}')$ satisfies $\mathbf{Z}\in\mathcal{Z}^{(i)}$ for some domains $\pi_i\in\Pi,$ and
<p>
  $$(\mathbf{S}\backslash\mathbf{S}')\perp\mathbf{B}|\mathbf{C}\ \text{in}\ {\mathcal{G}^\boldsymbol{\Delta}\backslash\mathbf{Z}},\ \mathbf{S}^{(i)}\cap\mathbf{S}'= \emptyset.$$
</p>

**Proof.** The condition implies that $p_\mathbf{x}(\mathbf{y}\vert\mathbf{w},\mathbf{S}=0)$ can be written as an expression of $p_\mathbf{z}(\mathbf{b}\vert\mathbf{c},\mathbf{S}'=0).$ Furthermore, for any $\pi_i$ such that $\mathbf{Z}\in\mathcal{Z}^{(i)},$ we have $\mathbf{S}^{(i)}\subseteq\mathbf{S}\backslash\mathbf{S'}.$ By rule 1 of do-calculus,
<p>
  $$p_\mathbf{z}(\mathbf{b}\vert\mathbf{c},\mathbf{S}=0) = p_\mathbf{z}(\mathbf{b}\vert\mathbf{c},\mathbf{S}^{(i)}=i,\mathbf{S}^{(-i)}=0)=p^{(i)}_\mathbf{z}(\mathbf{b}\vert\mathbf{c}).$$
</p>

Since $p_\mathbf{z}^{(i)}\in\mathcal{P}^{\Pi}_ \mathscr{Z},$ the expression is uniquely computable.

**Lemma 2.** A causal effect $p^{* }_ \mathbf{x}(\mathbf{y}\vert\mathbf{w})$ is not g-transportable with respect to $\langle\mathcal{G},\mathscr{Z}\rangle,$ if there exist two SCMs compatible with $\mathcal{G}^\boldsymbol{\Delta}$ where both agree on $\mathcal{P}^\Pi_\mathscr{Z}$ while disagreeing on $p^{* }_ \mathbf{x}(\mathbf{y}\vert\mathbf{w}).$

This is a g-version of Lemma 1 in [Meta-transportability](https://jurrivhleon.github.io/2023/07/09/Meta-Transport.html).

## Graphical Criterion
Now we present a graphical criterion which can tell whether a conditional causal effect is not g-transportable.
### Unconditional case
Recall the concepts of C-component, C-forest and hedge introduced by Shipster and Pearl, 2006. We consider a graph $\mathcal{G}:$
+ C-component: A subgraph of $\mathcal{G}$ whose bidirected edges from a spanning tree over all its vertices. A graph ${G}$ can be decomposed into a set $\mathcal{C}(\mathcal{G})$ of maximal C-components.
+ $\mathbf{R}$-rooted C-forest: A C-component whose root set is $\mathbf{R}$ and all observable vertices have at most one child.
+ Hedge: A pair of C-forests $\langle\mathcal{F},\mathcal{F}'\rangle$ with an inclusive relationship $\mathcal{F}'\subseteq\mathcal{F}$ and sharing the same root is called a hedge. A pair of $\mathbf{R}$-rooted $C$-forests $\langle\mathcal{F},\mathcal{F}'\rangle$ form a hedge for $p_\mathbf{x}(\mathbf{y})$ if
  <p>
    $$\mathcal{F}'\subset\mathcal{F},\mathcal{F}\cap\mathbf{X}\neq\emptyset,\mathcal{F}'\cap\mathbf{X}=\emptyset,\mathbf{R}\subseteq An(\mathbf{Y})_{\mathcal{G}\backslash{\mathbf{X}}}.$$
  </p>

**Definition 4** (Hedgelet decomposition). The hedgelet decomposition $\mathbb{H}(\langle\mathcal{F},\mathcal{F}'\rangle)$ is the collection of hedgelets $\lbrace\mathcal{F}(\mathbf{T}):\mathbf{T}\in\mathcal{C}(\mathcal{F}\backslash\mathcal{F}')\rbrace,$ where each hedgelet $\mathcal{F}(\mathbf{T})$ is a subgraph of $\mathcal{F}$ made of (i) $\mathcal{F}[\mathbf{V}(\mathcal{F}')\cup\mathbf{T}]$ and (ii) $\mathcal{F}[De(\mathbf{T})_\mathcal{F}]$ without bidirected edges.

**Definition 5** (S-thicket). Given $\langle\mathcal{G}^\boldsymbol{\Delta},\mathscr{Z}\rangle,$ an s-thicket $\mathcal{T}$ is a minimal non-empty $\mathbf{R}$-rooted C-forest of $\mathcal{G}$ such that for each $\mathcal{Z}\in\mathcal{Z}^{(i)}\in\mathscr{Z},$ either (a) $\Delta_i\cap\mathbf{R}\neq\emptyset,$ (b) $\mathbf{Z}\cap\mathbf{R}\neq\emptyset,$ or (c) there exists $\mathcal{F}\subseteq\mathcal{T}\backslash\mathbf{Z}$ where $\langle\mathcal{F},\mathcal{T}[\mathbf{R}]\rangle$ is a hedge. If $\mathbf{R}\subseteq An(\mathbf{Y})_ {\mathcal{G}\backslash\mathbf{X}}$ and every hedgelet of the hedges intersects with $\mathbf{X},$ then we say an s-thicket $\mathcal{T}$ is formed for $p^{* }_ \mathbf{x}(\mathbf{y})$ in $\mathcal{G}^\boldsymbol{\Delta}$ with respect to $\mathscr{Z}.$ Furthermore, 

**Remark.** Intuitively, if an s-thicket formed for $p^{* }_ \mathbf{x}(\mathbf{y})$ is encountered, g-transporting $p^{* }_ {\mathbf{x}'}(\mathbf{r}),$ where $\mathbf{X}'=\mathbf{X}\cap\mathcal{T}$  is prevented, because every existing experimental distribution either (a) exhibits discrepancies over $\mathbf{R}$ and (b) is based on an intervention on the variables we wish to measure, or (c) is not sufficient to pinpoint $p^{* }_ {\mathbf{x}'}(\mathbf{r}).$ Moreover, $p^{* }_ \mathbf{x}(\mathbf{y})$ is not g-transportable since the negative result for $p^{* }_ {\mathbf{x}'}(\mathbf{r})$ can be mapped to that for $p^{* }_ {\mathbf{x}'}(\mathbf{y}')$ where $\mathbf{Y}'\subseteq\mathbf{Y}$ and $\mathbf{R}\subseteq An(\mathbf{Y}')_{\mathcal{G}\backslash\mathbf{X}'}.$

**Lemma 3.** A causal effect $p^{* }_\mathbf{x}(\mathbf{y})$ is not g-transportable with respect to $\langle\mathcal{G}^\boldsymbol{\Delta},\mathscr{Z}\rangle$ if there exists an s-thicket $\mathcal{T}$ formed for it.

Therefore, the non-existence of an s-thicket is a necessary condition for the g-transportability of an unconditional causal effect. It is proven to be a sufficient condtion as well in further works.

### Conditional case
**Assumption 1** (Conditional minimality). There is no $W\in\mathbf{W}$ such that
<p>
  $$p_\mathbf{x}^{* }(\mathbf{y}|\mathbf{w}) = p_{(\mathbf{x},w)}^{* }(\mathbf{y}|\mathbf{w}\backslash\lbrace w\rbrace).$$
</p>

**Remark.** By rule 2 of do-calculus, the conditional minimality assumption can be graphically translated to the existence of an active backdoor path from each $W\in\mathbf{W}$ to $\mathbf{Y}$  in $\mathcal{G}_{\overline{\mathbf{X}}}$ given $\mathbf{X}$ and $\mathbf{W}\backslash\lbrace W\rbrace.$

**Theorem 1.** Let every $W\in\mathbf{W}$ have an active backdoor path to $\mathbf{Y}$ in $\mathcal{G}\backslash\mathbf{X}$ given $\mathbf{W}\backslash\lbrace W\rbrace.$ A query $p^{* }_ \mathbf{x}(\mathbf{y}\vert\mathbf{w})$ is not g-transportable if $p^{* }_ \mathbf{x}(\mathbf{w})$ is not g-transportable with respect to $\langle\mathcal{G}^{\boldsymbol{\Delta}},\mathscr{Z}\rangle.$

With the theorem above, the necessary and sufficient condition for the g-transportability of an conditional causal effect is derived below:

**Theorem 2.** Let every $W\in\mathbf{W}$ have an active backdoor path to $\mathbf{Y}$ in $\mathcal{G}\backslash\mathbf{X}$ given $\mathbf{W}\backslash\lbrace W\rbrace.$ A query $p^{* }_ \mathbf{x}(\mathbf{y}\vert\mathbf{w})$ is g-transportable if and only if $p^{* }_ \mathbf{x}(\mathbf{y},\mathbf{w})$ is g-transportable with respect to $\langle\mathcal{G}^{\boldsymbol{\Delta}},\mathscr{Z}\rangle.$

**Proof.** The sufficiency holds true since
<p>
  $$p^*_\mathbf{x}(\mathbf{y}|\mathbf{w}) = \frac{p^*_\mathbf{x}(\mathbf{y},\mathbf{w})}{\sum_{\mathbf{y}'}p^*_\mathbf{x}(\mathbf{y}',\mathbf{w})}.\tag{1}$$
</p>

For the necessity, note that 
<p>
  $$p^*_\mathbf{x}(\mathbf{y},\mathbf{w}) = p^*_\mathbf{x}(\mathbf{y}|\mathbf{w})p^*_\mathbf{x}(\mathbf{w}).$$
</p>

Then if $p^{* }_ \mathbf{x}(\mathbf{y},\mathbf{w})$ is not g-transportable, at least one of $p^{* }_ \mathbf{x}(\mathbf{w})$ and $p^{* }_ \mathbf{x}(\mathbf{y}\vert\mathbf{w}).$ By theorem 1, the non-g-transportability of $p^{* }_ \mathbf{x}(\mathbf{y}\vert\mathbf{w})$ always holds.

## Sound and Complete Algorithm
Lee, Correa and Bareinboim (2020) have proposed an algorithm **gTR** for solving any g-transportability instance. The algorithm **gTR** first converts the input causal query to a conditionally minimal expression, then solve an unconditional causal effect via function **gTRu** and compute the conditional one through equation (1) in Theorem 2. Function **gTRu** breaks down the query to subqueries and solve them. It is shown that whenever **gTRu** fails to g-transport a given query, an s-thicket for the query is encountered.

The algorithm µsID is proven to be sound and complete. Furthermore, the necessary and sufficient condition for the non-g-transportability of an unconditional causal effect can be derived:

**Corollary 1.** A causal effect $p^{* }_\mathbf{x}(\mathbf{y})$ is not g-transportable with respect to $\langle\mathcal{G}^\boldsymbol{\Delta},\mathscr{Z}\rangle$ if and only if there exists an s-thicket $\mathcal{T}$ formed for it.

## References

+ Shpitser, I., and J. Pearl, 2006. Identification of joint interventional distributions in recursive semi-Markovian causal models. In *Proceedings of The Twenty-First National Conference on Artificial Intelligence*, 1219–1226. AAAI Press.
+ Lee, S., J. D. Correa and E. Bareinboim, 2020. General Transportability – Synthesizing Observations and Experiments from Heterogeneous Domains. In *Proceedings of the AAAI Conference on Artificial Intelligence, 34(06)*, 10210-10217.
