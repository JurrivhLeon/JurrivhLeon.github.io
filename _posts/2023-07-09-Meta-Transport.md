# Causal Inference: Meta-Transportability
This blog continues to discuss the topic of combining causal knowledge from multiple heterogeneous domains.

## Formulation
**Definition 1** ($\mu$-transportability). Let $\mathscr{D}=\lbrace\mathcal{D}_ 1,\cdots,\mathcal{D}_ n\rbrace$ be a collection of selection diagrams relative to source domains $\Pi=\lbrace \pi_1,\cdots,\pi_n\rbrace$ and target domain $\pi^{* },$ respectively. Let $\langle p^{(i)},I^{(i)}\rangle$ be the pair of observational and interventional distributions of $\pi_i,$ and $p^{* }$ be the observational distribution of $\pi^{* }.$ The causal effect $R=p(y\vert\mathrm{do}(x))$ is said to be $\mu$-transportable from $\Pi$ to $\pi^{* }$ in $\mathscr{D}$ if $p(y\vert\mathrm{do}(x))$ is uniquely computable from $\bigcup_{i\in[n]} \langle p^{(i)},I^{(i)}\rangle\cup p^{* }$ in any model that induces $\mathscr{D}.$

The principle of decomposition can be applied to solve the $\mu$-transportability problems:

**Theorem 1.** Given a set of domains $\Pi=\lbrace \pi_1,\cdots,\pi_n\rbrace$ and target domain $\pi^{* }$ characterized by selection diagrams $\mathscr{D}=\lbrace\mathcal{D}_ 1,\cdots,\mathcal{D}_ n\rbrace$ relative to target domain $\pi^{* }.$ A relation $R(\pi^{* })$ is $\mu$-transportable from $\Pi$ to $\pi^*$ if it can be decomposed into a set of subrelations of the form:

$$R_k = p^*(V_k|\mathrm{do}(W_k), Z_k)\ \ k=1,\cdots,K$$

such that each $R_k$ is uniquely computable from some $\mathcal{D}_i\in\mathscr{D}.$

Some special cases of $\mu$-transportability are stated below.

**Definition 2** (Trivial $\mu$-transportability). A relation $R(\pi^{* })$ is said to be trivially $\mu$-transportable from $\Pi$ to $\pi^{* }$ if it is identifiable from the data available in $\pi^{* },$ i.e. the observational distribution $p^{* }$ and underlying causal DAG $\mathcal{G}.$

**Definition 3** (Direct $\mu$-transportability). A relation $R(\pi^{* })$ is said to be directly $\mu$-transportable from $\Pi$ to $\pi^{* }$ if there exists $\pi_i\in\Pi$ such that $R(\pi^{* }) = R(\pi_i).$

**Remark.** The trivial $\mu$-transportability allows one to estimate $R(\pi^{* })$ using only the observational data from domain $\pi^{* }.$ The direct $\mu$-transportability implies an identical form of causal relation in both domains hence no recalibration is required.

An immediate test for direct $\mu$-transportability follows from rule 1 of do-calculus: for some domain $\pi_i,$ if $S^{(i)}\perp Y\vert \lbrace X,Z\rbrace$ in $\mathcal{D}^{(i)}_{\overline{X}},$ where $S^{(i)}$ denotes the set of selection nodes in $\mathcal{D}^{(i)},$ then the $Z$-specific effect $R=p^{* }(y\vert\mathrm{do}(x),z)$ is directly $\mu$-transportable.

Aside from deriving $\mu$-transportability, it is also important to recognize the case in which a quantity is not $\mu$-transportable.

**Lemma 1.** Let $X,Y$ be two disjoint sets of variables in domains $\Pi$ and $\pi^{* }$ characterized by $\mathscr{D}.$ The causal effect $R=p^{* }(y|\mathrm{do}(x))$ is not $\mu$-transportable from $\Pi$ to $\pi^{* }$ if there exists two model families $\mathscr{M}_ 1$ and $\mathscr{M}_ 2$ compatible with $\mathscr{D}$ such that for all $i=1,\cdots,n,$ $W\subset V,$
<p>
  $$p^{(i)}_{\mathscr{M}_1}(V) = p^{(i)}_{\mathscr{M}_2}(V),\ p^*_{\mathscr{M}_1}(V) = p^*_{\mathscr{M}_2}(V),\ p^{(i)}_{\mathscr{M}_1}(V\backslash W|\mathrm{do}(W)) = p^{(i)}_{\mathscr{M}_2}(V\backslash W|\mathrm{do}(W)),$$
</p>

and 
<p>
  $$p^{*}_{\mathscr{M}_1}(y\vert\mathrm{do}(x)) \neq p^{*}_{\mathscr{M}_2}(y\vert\mathrm{do}(x)).$$
</p>

The proof is straightforward. Let $\mathcal{I}=\bigcup_{i\in[n]} I^{(i)}$ and $\mathcal{P}=\bigcup_{i\in[n]} p^{(i)}.$ The collection of all interventional distributions $p^{(i)}(V\backslash W\vert\mathrm{do}(W))\in I^i,\ \forall W\subseteq V.$ Then the last inequality rules out the existence of a function from $\langle\mathcal{I},\mathcal{P},p^{* }\rangle$ to $p^{* }(y\vert\mathrm{do}(x)).$

## Characterizing $µ$-Transportable Relations
Tian and Pearl (2002) introduced the concept of confounded components (or C-components, for short) to represent the clusters of variables connected through bidirected arcs, which stand for unobserved confoundings. An illustrative example is given below.

<div align=center>
   <img src='https://github.com/JurrivhLeon/JurrivhLeon.github.io/raw/main/figs/causalDAGa.png' width='648.9'/> 
</div>

The left causal DAG includes unobserved variables $U_0,U_1,U_2$ as its nodes. We modified it as the right causal DAG so that it contains only observed variables as its nodes. In this DAG, the $C$-components are $\lbrace W_0,X,Y,Z\rbrace$ and $\lbrace W_1\rbrace.$ Note that any causal DAG $\mathcal{G}$ can be uniquely partitioned to an irreducible set $\mathcal{C}(\mathcal{G})$ of C-components.

**Definition 4** (Selection confounded component, sC-component). Let $\mathcal{D}$ be a selection diagram with variable set $V$ and the set of selection nodes $S.$ If a subset of the bidirected arcs in $\mathcal{D}$ forms a spanning tree over $V$, then $\mathcal{D}$ is a sC-component.

The existence of sC-components does not prevent $\mu$-transportability. Recall the example given in the previous blog:

<div align=center>
   <img src='https://github.com/JurrivhLeon/JurrivhLeon.github.io/raw/main/figs/causalDAG9.png' width='648.9'/> 
</div>

both graphs are sC-components, but $p^{* }(y\vert\mathrm{do}(x))$ is $\mu$-transportable.

**Definition 5** (sC-tree). Let $\mathcal{D}$ be a selection diagram such that, all observable nodes have at most one child, and there is a node $Y$, which is a descendent of all nodes, and there is a selection node pointing to $Y$. Then $\mathcal{D}$ is called a $Y$-rooted sC-tree.

**Definition 6** (sC-forest). Let $\mathcal{D}$ be a selection diagram, where $Y$ is the maximal root set. Then $\mathcal{D}$ is a $Y$-rooted sC-forest if $\mathcal{D}$ is a sC-component, all observable nodes have at most one child, and there is a selection node pointing to some node of $\mathcal{D}$ (Not necessarily in $Y$).

**Definition 7** ($\mu s$-hedge). Let $X,Y$ be two disjoint sets of variables in domains $\Pi$ and $\pi^{* }$ characterized by $\mathscr{D}.$ Let $F,F'$ be $R$-rooted sC-forests such that
<p>
$$F\cap X\neq \emptyset,\ F'\cap X=\emptyset,\ F'\subseteq F,\ R\subset An(Y)_{\mathcal{G}_{\overline{X}}}$$ 
</p>

in every $\mathcal{D}_i\in\mathscr{D}.$ Then $F$ and $F'$ form a $\mu s$-hedge for $p^{* }(y\vert\mathrm{do}(x))$ in $\mathscr{D}.$

An oversimplified sample is presented below.

<div align=center>
   <img src='https://github.com/JurrivhLeon/JurrivhLeon.github.io/raw/main/figs/causalDAGb.png' width='648.9'/> 
</div>

In this example, $Y$-rooted sC-forests $F'=\lbrace Z,Y\rbrace$ and $F=\lbrace X,Z,Y\rbrace$ forms a $\mu s$-hedge for $p^{* }(y\vert\mathrm{do}(x)).$

A formal relationship between $\mu s$-hedges and $\mu$-transportability is stated below.

**Theorem 2.** Assume there exists a pair of sC-forests $F,F'$ that form a $\mu s$-hedge for $p^{* }(y\vert\mathrm{do}(x))$ in $\mathscr{D}.$ Then $p^{* }(y\vert\mathrm{do}(x))$ is not transportable from $\Pi$ to $\pi^{* }.$

## Complete Algorithm for Deciding $\mu$-Transportability of Joint Effects
Bareinboim and Pearl (2013b) has constructed an algorithm named **µsID** to solve the general $\mu$-identifiability problem. The algorithm **µsID** simplifies and decomposes the inputted selection diagrams $\mathcal{D}$ (which is also a causal diagram over $\pi^*$ when we disregard the $S$-nodes), partitioning the original problem into smaller blocks until either the entire expression is $\mu$-transportable, or it runs into the problematic $\mu s$-hedge structure.

The algorithm **µsID** is proven to be sound and complete. Furthermore, one of its corollary is given below.

**Corollary 1.** $p^{* }(y\vert\mathrm{do}(x))$ is $\mu$-transportable from $\Pi$ to $\pi^{* }$ in $\mathscr{D}$ if and only if there exists no $\mu s$-hedge for $p^{* }(y'\vert\mathrm{do}(x'))$ in $\mathscr{D}$ for any $X'\subseteq X$ and $Y'\subseteq Y.$


## References
+ Bareinboim, E. and J. Pearl (2013a). A general algorithm for deciding transportability of experimental results. *Journal of Causal Inference 1*, 107–34.
+ Bareinboim, E. and J. Pearl (2013b). Meta-transportability of causal effects: A formal approach. In *Proceedings of the 16th International Conference on Artificial Intelligence
  Statistics (AISTATS 2013)*.
