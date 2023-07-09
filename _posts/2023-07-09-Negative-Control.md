# Causal Inference: Negative Control
## Set-up
In the following discussion, we are interested about the causal effect of an action $A$ (or treatment, in biomedical literatures) on an outcome $Y$.

Define the action space to be $\mathcal{A}$ which can be discrete or continuous. Correspoondingly, define a measure $\mu$ on $\mathcal{A}$, *e.g.* the counting measure if $\mathcal{A}$ is finite or Lebesgue measure if $\mathcal{A}$ is continuous. Let $Y(a)$ denote the real-valued counterfactual outcome that would be observed if the action was set to $a\in\mathcal{A}.$ Then the observed outcome $Y$ is consistent with the actual action $A$, i.e. $Y=Y(A).$ Furthermore, let $X\in\mathcal{X}\subseteq\mathbb{R}^d$ be a collection of observed covariates. 

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

$$Y(a)\not\perp A\ \vert\ X,\ \text{but}\ Y(a)\perp A\ \vert\ X,U\ \ \forall a\in\mathcal{A}.$$

We could easily identify $J$ by adjusting for both $X$ and $U$ if they were observed. However, since $U$ are unobserved, we may resort to some alternative methods with extra data or stronger assumptions.

## Negative Control
The negative control framework is proposed to handle the problem of unmeasured confounding. Two additional types of observed variables are introduced in this framework: negative control actions $Z\in\mathcal{Z}\subseteq \mathbb{R}^{p_z}$ and negative control outcomes $W\in\mathcal{W}\subseteq\mathbb{R}^{p_u}.$

The idea of negative control is borrowed from biomedical experiments. The negative actions $Z$ do not directly affect the outcome $Y$, and neither the negative actions $Z$ nor the primary action $A$ can affect the negative outcomes $W.$ Meanwhile, both $Z$ and $W$ are relevant to the unmeasured confounders, hence they can be viewed as proxies for $U$ to some degree. A typical causal DAG for this setting is shown below.

<div align=center>
   <img src='https://github.com/JurrivhLeon/JurrivhLeon.github.io/raw/main/figs/negcontrol.png' width='320'/> 
</div>

