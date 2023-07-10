# Causal Inference: Negative Control
## Set-up
In the following discussion, we are interested about the causal effect of an action $A$ (or treatment, in biomedical literatures) on an outcome $Y$.

Define the action space to be $\mathcal{A}$ which can be discrete or continuous. Correspoondingly, define a measure $\mu$ on $\mathcal{A}$, *e.g.* the counting measure if $\mathcal{A}$ is finite or Lebesgue measure if $\mathcal{A}$ is continuous. Let $Y(a)$ denote the real-valued counterfactual outcome that would be observed if the action was set to $a\in\mathcal{A}.$ Then the observed outcome $Y$ is consistent with the actual action $A$, i.e. $Y=Y(A).$ Furthermore, let $X\in\mathcal{X}\subseteq\mathbb{R}^d$ be a collection of observed covariates. 

### Generalized Average Causal Effect
**Definition 1** (Generalized average causal effect, GACE). Given a contrast function $\pi:\mathcal{A}\times\mathcal{X}\to\mathbb{R},$ the corresponding GACE is defined as
<p>
  $$J := \mathbb{E}\left[\int_\mathcal{A}Y(a)\pi(a|X)\mathrm{d}\mu(a)\right]. \tag{1}$$
</p>

Some illustrative examples are given below.
+ (Counterfactual mean). Let $\mathcal{A}$ be discrete and finite with counting measure $\mu,$ and define $\pi(a|x)=\mathbb{1}{\lbrace a=a_0\rbrace}.$ Then the counterfactual mean can be derived as
  <p>$$\mathbb{E}[Y(a_0)] = \sum_{a\in\mathcal{A}}\mathbb{E}\left[Y(a)\mathbb{1}{\lbrace a=a_0\rbrace}\right] =: J_{a_0} $$</p>
+ (Average treatment effect, ATE). Let $\mathcal{A}=\lbrace 0,1\rbrace.$ If we are interested in the effect of treatment $A=1$ compared with control $A=0,$ we can define $\pi(a\vert x)=2a-1,$ then we can obtain the average treatment effect $J=\mathbb{E}[Y(1) - Y(0)].$
+ (Policy evaluation). If $\pi(a\vert x)$ is a density on $\mathcal{A}$ for each $x\in\mathcal{X}$ with respect to $\mu,$ then $J$ is the average outcome when we follow a policy that assigns an action drawn from $\pi(\cdot\vert X)$ to an individual with covariate $X.$

In real scenario, the observed covariates $X$ are not sufficient to account for confounding. Hence, assume there exist additional unmeasured confounders $U\in\mathcal{U}\subseteq\mathbb{R}^{p_u},$ so that

$$Y(a)\not\perp A\ \vert\ X,\ \text{but}\ Y(a)\perp A\ \vert\ U,X\ \ \forall a\in\mathcal{A}.$$

We could easily identify $J$ by adjusting for both $X$ and $U$ if they were observed.

### Identification of GACE under NUCA
In ideal setting, no unobserved confounding assumption is admitted, i.e. both $X$ and $U$ were observed. We suggest three commonly-used estimators below.
+ (Inverse probability weighting, IPW). Define the generalized propensity score $f(a\vert u,x)$ as the conditional density of the distribution $A\ \vert\ U, X$ relative to the base measure $\mu.$ 
  <p>$$\phi_\mathrm{IPW}(y,a,u,x;f) = \frac{\pi(a|x)}{f(a|u,x)}y. $$</p>
+ (Regression adjustment). To account for confoundings, covariates $X$ and $U$ can be included in a regression model. We define the regression function as $k_0$ and assume that it is correct, i.e. $k_0(a,u,x)=\mathbb{E}[Y\vert A=a,U=u,X=x].$ The regression-based estimator is obtained by taking the expectation of
  <p>$$\phi_\mathrm{REG}(y,a,u,x;k_0) = \int k_0(a',u,x)\pi(a'|x)\mathrm{d}\mu(a').$$</p>
+ (Doubly robust estimation) Based on the generalized propensity score $f$ and regression function $k_0,$ the dobly robust estimator is obtained by taking the expectation of
  <p>$$\phi_\mathrm{DR}(y,a,u,x;k_0,f) = \frac{\pi(a|x)}{f(a|u,x)}\left(y - k_0(a,u,x)\right) + \int k_0(a',u,x)\pi(a'|x)\mathrm{d}\mu(a'). $$</p>

**Lemma 1.** If $Y(a)\perp A\ \vert\ U,X$ and $\vert\pi(A\vert X)/f(A\vert X,U)\vert < \infty,$ then
<p>$$J = \mathbb{E}\phi_\mathrm{IPW}(Y,A,U,X;f) = \mathbb{E}\phi_\mathrm{REG}(Y,A,U,X;k_0) = \mathbb{E}\phi_\mathrm{DR}(Y,A,U,X;k_0,f).$$</p>

However, since $U$ are unobserved, we may resort to some alternative methods with extra data or stronger assumptions.

## Negative Control
### Framework
The negative control framework is proposed to handle the problem of unmeasured confounding. Two additional types of observed variables are introduced in this framework: negative control actions $Z\in\mathcal{Z}\subseteq \mathbb{R}^{p_z}$ and negative control outcomes $W\in\mathcal{W}\subseteq\mathbb{R}^{p_u}.$

The idea of negative control is borrowed from biomedical experiments. The negative actions $Z$ do not directly affect the outcome $Y$, and neither the negative actions $Z$ nor the primary action $A$ can affect the negative outcomes $W.$ Meanwhile, both $Z$ and $W$ are relevant to the unmeasured confounders, hence they can be viewed as proxies for $U$ to some degree. A typical causal DAG for this setting is shown below.

<div align=center>
   <img src='https://github.com/JurrivhLeon/JurrivhLeon.github.io/raw/main/figs/negcontrol.png' width='320'/> 
</div>

Let $W(a,z)$ and $Y(a,z)$ denote the corresponding counterfactuals with respect to $(a,z)\in\mathcal{A}\times\mathcal{Z},$ then we can formalize the negative control assumptions as follows.

**Assumption 1** (Negative controls).
1. Consistency: $Y=Y(A,Z),\ W=W(A,Z)$;
2. Negative control actions: $Y(a,z)=Y(a),\ \forall a\in\mathcal{A}$;
3. Negative control outcomes: $W(a,z)=W,\ \forall a\in\mathcal{A},z\in\mathcal{Z}$;
4. Latent unconfoundedness: $(A,Z)\perp(Y(a),W)\ \vert\ U,X,\ \forall a\in\mathcal{A}$;
5. Overlap: $\left\vert\frac{\pi(a\vert x)}{f(a\vert x,u)}\right\vert < \infty, \forall a\in\mathcal{A},x\in\mathcal{X},u\in\mathcal{U}$.

For brevity, we use $O:=(Z,X,W,A,Y)$ to denote the observable variables, since $U$ is unobserved. In general, our observations contains $n$ independent and identically distributed realizations of $O.$

### Bridge Functions
We inherit the notations from the NUCA setting, i.e. $f(a\vert u,x)$ is the generalized propensity score and $k_0(a,u,x)=\mathbb{E}[Y\vert A=a,U=u,X=x]$ the regression function. When $U$ is unobserved, neither $f$ nor $k_0$ are estimable. In the negative control framework, two types of bridge functions are proposed for compensation.

**Definition 2** (Bridge functions). An outcome bridge function is $h_0 ∈\in L_2(W, A, X)$ with
<p>$$\mathbb{E}[h_0(W,A,X)|A,U,X] = k_0(A,U,X).$$</p>

An action bridge function is $q_0$ with $\pi q_0\in L_2(Z,A,X)$ and
<p>$$\mathbb{E}[\pi(A|X)q_0(Z,A,X)|A,U,X] = \frac{\pi(A|X)}{f(A|U,X)}.$$</p>

These bridge functions are not necessarily unique, and we define the classes of bridge functions as follows:
<p>
  $$\begin{align}
  \mathcal{H}_0 &= \lbrace h\in L_2(W,A,X):\ \mathbb{E}[Y - h(W,A,X)|A,U,X] = 0\rbrace, \\
  \mathcal{Q}_0 &= \left\lbrace q:\ \pi q\in L_2(Z,A,X),\ \mathbb{E}\left[\pi(A|X)(q(Z,A,X) - \frac{1}{f(A|U,X)})|A,U,X\right]\right\rbrace.
  \end{align}$$
</p>

### Identification of GACE
With correctly specified bridge functions, the identification of GACE becomes possible. We define an integral operator $\mathcal{T}:L_2(W,A,X)\to L_2(W,X)$ as

$$(\mathcal{T}h)(w,x) = \int h(w,a,x)\pi(a|x)\mathrm{d}\mu(a).$$

Then analogous to the NUCA case, we define the corresponding estimators below:
<p>
  $$\begin{align}
  \tilde{\phi}_\mathrm{IPW}(O;q_0) &= \pi(A|X)q_0(Z,A,X)Y,\\
  \tilde{\phi}_\mathrm{REG}(O;h_0) &= (\mathcal{T}h_0)(W,X),\\
  \tilde{\phi}_\mathrm{DR}(O;h_0,q_0) &=  \pi(A|X)q_0(Z,A,X)(Y - h_0(W,A,X)) + (\mathcal{T}h_0)(W,X).
  \end{align}$$
</p>

**Lemma 2.** Suppose that Assumption 1 holds. For any $h_0\in\mathcal{H}_0$ and $q_0\in\mathcal{Q}_0,$
<p>
  $$J = \mathbb{E}\tilde{\phi}_\mathrm{IPW}(O;q_0) = \mathbb{E}\tilde{\phi}_\mathrm{REG}(O;h_0) = \mathbb{E}\tilde{\phi}_\mathrm{DR}(O;h_0,q_0).$$
</p>

**Proof.** By assumption 1, we have the latent unconfoundedness $Z \perp(Y,W)\ \vert\ A,U,X.$

For the IPW estimator,
<p>
  $$\begin{align}
  \mathbb{E}[\tilde{\phi}_\mathrm{IPW}(O;q_0)\,|\, A,U,X] &= \mathbb{E}[\pi(A|X)q_0(Z,A,X)\,|\, A,U,X]\,\mathbb{E}[Y\,|\, A,U,X]\\
  &= \frac{\pi(A|X)}{f(A|U,X)}\mathbb{E}[Y\,|\, A,U,X],\\
  \mathbb{E}[\tilde{\phi}_\mathrm{IPW}(O;q_0)\,|\,U,X] &= \int \frac{\pi(a|X)}{f(a|U,X)}\mathbb{E}\left[Y(a)\,|\, U,X\right]f(a|U,X)\mathrm{d}\mu(a)\\
  &= \int \pi(a|X)\,\mathbb{E}\left[Y(a)\,|\, U,X\right]\mathrm{d}\mu(a)\\
  &= \mathbb{E}\left[\int Y(a)\pi(a|X)\mathrm{d}\mu(a)\,|\, U,X\right],\\
  \mathbb{E}[\tilde{\phi}_\mathrm{IPW}(O;q_0)] &= \mathbb{E}\left[\mathbb{E}[\tilde{\phi}_\mathrm{IPW}(O;q_0)\,|\,U,X]\right]\\
  &= \mathbb{E}\left[\mathbb{E}\left[\int Y(a)\pi(a|X)\mathrm{d}\mu(a)\,|\, U,X\right]\right]\\
  &= \mathbb{E}\left[\int Y(a)\pi(a|X)\mathrm{d}\mu(a)\right] = J.
  \end{align}$$
</p>

For the regression-based estimator,
<p>
  $$\begin{align}
  \mathbb{E}[\tilde{\phi}_\mathrm{REG}(O;h_0)\,|\, U,X] &= \mathbb{E}\left[\left.\int h(W,a,X)\pi(a|X)\mathrm{d}\mu(a)\right| U,X\right]\\
  &= \int k_0(a,U,X)\pi(a|X)\mathrm{d}\mu(a)\\
  &= \mathbb{E}\left[\left.\int Y(a)\pi(a|X)\mathrm{d}\mu(a)\right|U,X\right],\\
  \mathbb{E}[\tilde{\phi}_\mathrm{REG}(O;h_0)] &= \mathbb{E}\left[\mathbb{E}[\tilde{\phi}_\mathrm{REG}(O;h_0)\,|\, U,X]\right]\\
  &= \mathbb{E}\left[\mathbb{E}\left[\left.\int Y(a)\pi(a|X)\mathrm{d}\mu(a)\right|U,X\right]\right]\\
  &= \mathbb{E}\left[\int Y(a)\pi(a|X)\mathrm{d}\mu(a)\right] = J.
  \end{align}$$
</p>

For the doubly robust estimator, note that $Z\perp W\ \vert\ A,U,X,$ we have
<p>
  $$\begin{align}
  \mathbb{E}[\tilde{\phi}_\mathrm{IPW}(O;q_0)\,|\, A,U,X] &= \frac{\pi(A|X)}{f(A,U,X)}\mathbb{E}[Y\,|\, A,U,X]\\
  &= \mathbb{E}[\pi(A|X)q_0(Z,A,X)\,|\,A,U,X]\\
  &\quad\; \times\mathbb{E}[h_0(W,A,X)\,|\,A,U,X]\\
  &= \mathbb{E}[\pi(A|X)q_0(Z,A,X)h_0(W,A,X)\,|\,A,U,X],
  \end{align}$$
</p>

then
<p>
  $$\mathbb{E}[\tilde{\phi}_\mathrm{IPW}(O;q_0)] - \mathbb{E}[\pi(A|X)q_0(Z,A,X)h_0(W,A,X)] = 0,$$
</p>
<p>
  $$\mathbb{E}[\tilde{\phi}_\mathrm{DR}(O;h_0,q_0)] = \mathbb{E}[\tilde{\phi}_\mathrm{REG}(O;h_0)].$$
</p>

Therefore,
<p>
  $$J = \mathbb{E}\tilde{\phi}_\mathrm{IPW}(O;q_0) = \mathbb{E}\tilde{\phi}_\mathrm{REG}(O;h_0) = \mathbb{E}\tilde{\phi}_\mathrm{DR}(O;h_0,q_0).$$
</p>
