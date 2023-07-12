# Causal Inference: Combining Experimental and Observational Data
In modern medical research, randomized controlled trials (RCTs) are widely applied for assessing the causal effect of an intervention or a treatment on an outcome of interest. Experimental data collected through RCTs are regarded to be reliable since confounding is eliminated via meticulous design and randomization. However, due to the cost of RCTs and some ethical factors, the size of experimental data are often limited. In contrast, observational data are easily accessible, but their internal validity is not guaranteed due to the existence of confounding bias.

In this blog we will discuss combining experimental and observational data. Colnet et al. (2023) has summarized the advantage of combining experimental and observational data as follows.
+ accounting for the lack of representativeness of RCT, as observational data can constitute an external representative sample of a target population of interest;
+ making observational evidence more credible using RCT to ground observational analysis, such as detecting a confounding bias;
+ improving statistical efficiency, for example to better estimate heterogeneous treatment effects as RCTs are often under-powered in such settings.

## Set-up
### Potential outcome framework
Each individual in the RCT or observational population is described by a random tuple $(X, Y(0), Y(1), A, S),$ where
+ $X$ is a $p$-dimensional vector of covariates,
+ $A$ is a dichotomous treatment assignment,
+ $Y(a)$ is the potential outcome (also referred to as counterfactual outcome, which would be observed had the subject been given treatment assignment $A=a,a\in\lbrace 0,1\rbrace$), and
+ $S$ is an indicative dichotomous variable for trial eligibility and willingness to participate. For RCT samples, $S=1$ and for observational samples, $S$ is unknown.

Suppose we have an RCT sample of size $n$ identically distributed according to $(X,Y(0),Y(1),A,S)\ \vert\ S=1,$ and an observational sample of size $m$ identically distributed according to $(X,Y(0),Y(1),A,S).$ Denote $\mathcal{R}=\lbrace 1,\cdots,n\rbrace$ and $\mathcal{O}=\lbrace n+1,\cdots,n+m\rbrace$ as the index sets of samples from RCT and observational study, respectively.

For the RCT data, we observe $\lbrace(X_i,A_i,Y_i,S_i=1)\rbrace_{i=1}^n,$ where $Y_i=A_iY_i(1) + (1-A_i)Y_i(0)$ according to the SUTVA and consistency assumption. Moreover, the randomization implies
<p>
  $$\lbrace Y(1),Y(0)\rbrace\perp A\ |\ X,S=1.$$
</p>

For the observational data, we consider two settings:
+ we only observe the covariates $X_i$, and
+ we also observe the treat and outcome $(X_i,A_i,Y_i)$.

### Non-nested design
In our discussion, the sampling mechanisms of the RCT and observational samples are assumed to be independent.

### Notations
Define the avergae treatment effect (ATE) in observational and RCT population:
<p>
  $$\tau = \mathbb{E}[Y(1)-Y(0)],\ \tau_1=\mathbb{E}[Y(1)-Y(0)|S=1].$$
</p>

With effect modification, define the conditional average treatment effect (CATE): for $x\in\mathbb{R}^p$,
<p>
  $$\tau(x) = \mathbb{E}[Y(1)-Y(0)|X=x],\ \tau_1(x)=\mathbb{E}[Y(1)-Y(0)|X=x,S=1].$$
</p>

Propensity score: for $x\in\mathbb{R}^p$,
<p>
  $$e(x) = \mathbb{P}(A=1|X=x),\ e_1(x) = \mathbb{P}(A=1|X=x,S=1).$$
</p>

Conditional mean outcome: for $x\in\mathbb{R}^p$,\ a\in\lbrace 0,1\rbrace,$
<p>
  $$\mu_a(x)=\mathbb{E}[Y(a)|X=x],\ \mu_{a,1}(x)=\mathbb{E}[Y(a)|X=x].$$
</p>

Denote by $\alpha(x)$ the conditional odds that an individual with covariates $x$ is in the RCT or in the observational sample:
<p>
  $$\alpha(x) = \frac{\mathbb{P}(i\in\mathcal{R}\ |\ \exists i\in\mathcal{R}\cup\mathcal{O},X_i=x)}{\mathbb{P}(i\in\mathcal{O}\ |\ \exists i\in\mathcal{R}\cup\mathcal{O},X_i=x)} = \frac{\pi_\mathcal{R}(x)}{\pi_\mathcal{O}(x)} = \frac{\pi_\mathcal{R}(x)}{1-\pi_\mathcal{R}(x)}.$$
</p>

Finally, we denote the distribution of covariates in observational data as $f.$ Correspondingly, the distribution of covariates in RCT can be obtaine by $f(\cdot\vert S=1).$

