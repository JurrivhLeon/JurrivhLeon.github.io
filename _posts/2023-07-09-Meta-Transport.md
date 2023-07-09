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

The proof is straightforward. Let $\mathcal{I}=\bigcup_{i\in[n]} I^{(i)}$ and $\mathcal{P}=\bigcup_{i\in[n]} p^{(i)}.$ The collection of all interventional distributions $p^{(i)}(V\backslash W\vert\mathrm{do}(W))\in I^i,\ \forall W\subseteq V.$ Then the last inequality rules out the existence of a function from $\langle\mathcal{I},\mathcal{P},p^{* }\rangle$ to $p^{* }(y|\mathrm{do}(x)).$

## Characterizing $µ$-Transportable Relations
Tian and Pearl (2002) introduced the concept of confounded components (or C-components, for short) to represent the clusters of variables connected through bidirected arcs, which stand for unobserved confoundings. 

