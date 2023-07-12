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

Conditional mean outcome: for $x\in\mathbb{R}^p,\ a\in\lbrace 0,1\rbrace,$
<p>
  $$\mu_a(x)=\mathbb{E}[Y(a)|X=x],\ \mu_{a,1}(x)=\mathbb{E}[Y(a)|X=x].$$
</p>

Denote by $\alpha(x)$ the conditional odds that an individual with covariates $x$ is in the RCT or in the observational sample:
<p>
  $$\alpha(x) = \frac{\mathbb{P}(i\in\mathcal{R}\ |\ \exists i\in\mathcal{R}\cup\mathcal{O},X_i=x)}{\mathbb{P}(i\in\mathcal{O}\ |\ \exists i\in\mathcal{R}\cup\mathcal{O},X_i=x)} = \frac{\pi_\mathcal{R}(x)}{\pi_\mathcal{O}(x)} = \frac{\pi_\mathcal{R}(x)}{1-\pi_\mathcal{R}(x)}.$$
</p>

Finally, we denote the distribution of covariates in observational data as $f.$ Correspondingly, the distribution of covariates in RCT can be obtaine by $f_1(\cdot)=f(\cdot\vert S=1).$

## Observational Data with no Treatment and Outcome
We first suggest two assumptions to generalize the findings in RCT to a target population.

**Assumption 1** (Transportability of CATE). $\tau(x)=\tau_1(x)$ for all $x.$

**Assumption 2** (Positivity of trial participation) There exists some constant $c>0$ such that $\mathbb{P}(S=1|X) \geq c$ almost surely.

### Inverse probability of sampling weighting (IPSW)
Under Assumption 1 and 2, and exchangeability holds for RCT, the ATE can be identified:
<p>
  $$\begin{align}
  \tau &= \mathbb{E}\left[\left.\frac{n}{m\alpha(X)}\tau_1(X)\right|S=1\right]\\
  &= \mathbb{E}\left[\left.\frac{n}{m\alpha(X)}\left(\frac{A}{e_1(X)} - \frac{1-A}{1-e_1(X)}\right)Y\right|S=1\right].
  \end{align}$$
</p>

**Proof.** First, note that
<p>
  $$\begin{align}
  \tau_1(x) &= \mathbb{E}[Y(1)|X=x,S=1] - \mathbb{E}[Y(0)|X=x,S=1]\\
  &= \frac{\mathbb{E}[AY(1)|X=x,S=1]}{e_1(x)} - \frac{\mathbb{E}[(1-A)Y(0)|X=x,S=1]}{1-e_1(x)} \ \ \text{(Exchangeability)}\\
  &= \frac{\mathbb{E}[AY|X=x,S=1]}{e_1(x)} - \frac{\mathbb{E}[(1-A)Y|X=x,S=1]}{1-e_1(x)} \ \ \text{(Consistency)}\\
  &= \mathbb{E}\left[\left.\left(\frac{A}{e_1(X)} - \frac{1-A}{1-e_1(X)}\right)Y\right|X=x,S=1\right],
  \end{align}$$
</p>

we have
<p>
  $$\begin{align}
  \mathbb{E}\left[\left.\frac{n\tau_1(X)}{m\alpha(X)}\right|S=1\right] &= \mathbb{E}\left[\left.\mathbb{E}\left[\left.\frac{n}{m\alpha(X)}\left(\frac{A}{e_1(X)} - \frac{1-A}{1-e_1(X)}\right)Y\right|X,S=1\right]\right|S=1\right]\\
  &= \mathbb{E}\left[\left.\left(\frac{A}{e_1(X)} - \frac{1-A}{1-e_1(X)}\right)Y\right|S=1\right],
  \end{align}$$
</p>

which concludes the second equality.

Furthermore,
<p>
  $$\begin{align}
  \alpha(x) &= \frac{\mathbb{P}(i\in\mathcal{R}\ |\ \exists i\in\mathcal{R}\cup\mathcal{O},X_i=x)}{\mathbb{P}(i\in\mathcal{O}\ |\ \exists i\in\mathcal{R}\cup\mathcal{O},X_i=x)}\\
  &= \frac{\mathbb{P}(i\in\mathcal{R})}{\mathbb{P}(i\in\mathcal{O})}\times\frac{\mathbb{P}(X_i=x|i\in\mathcal{R})}{\mathbb{P}(X_i=x|i\in\mathcal{O})}\\
  &= \frac{n}{m}\frac{f_1(x)}{f(x)}.
  \end{align}$$
</p>

then the first equality holds using the idea of importance sampling:
<p>
  $$\begin{align}
  \tau = \int \tau(x)f(x)\mathrm{d}x &= \int \tau(x)\frac{f(x)}{f_1(x)}f_1(x)\mathrm{d}x\\
  &= \int \tau_1(x)\frac{n}{m\alpha(x)}f_1(x)\mathrm{d}x = \mathbb{E}\left[\left.\frac{n}{m\alpha(X)}\tau_1(X)\right|S=1\right].
  \end{align}$$
</p>

Replace the expectation operator with an empirical one, we obtain the IPSW estimator:

**Definition 1** (IPSW estimator) Let $\alpha_{n,m}$ be an estimate of the odds of the indicatrix of being in the RCT, the IPSW estimator is given by
<p>
  $$\hat{\tau}_{\mathrm{IPSW},n,m} = \frac{1}{n}\sum_{i=1}^n\frac{nY_i}{m\hat{\alpha}_{n,m}(X_i)}\left(\frac{A_i}{e_1(X_i)} - \frac{1-A_i}{1-e_1(X_i)}\right).$$
</p>

It is shown that, under some regularity conditions, the IPSW estimator is consistent.

**Assumption 3** (Consistency of conditional odds $\alpha$) Denoting by $\frac{n}{m\hat{\alpha}_{n,m}(x)}$ the estimated weights, the following conditions hold:
+ $\sup_{x\in\mathcal{X}}\left\vert \frac{n}{m\hat{\alpha}_ {n,m}(x)} - \frac{f_ X(x)}{f_ {X|S=1}(x)}\right\vert = \epsilon_{n,m} \overset{\mathrm{a.s.}}{\to} 0$ whem $n,m\to\infty.$
+ For all $n,m$ large enough $\mathbb{E}[\epsilon^2_{n,m}]$ exists and $\mathbb{E}[\epsilon^2_{n,m}]\overset{\mathrm{a.s.}}{\to} 0$ whem $n,m\to\infty.$
+ $Y$ is square integrable.

**Theorem 1** (Consistency of IPSW estimator) Suppose consistency and exchangeability hold for RCT. Under assumptions 1, 2 and 3, $\hat{\tau}_ {\mathrm{IPSW},n,m}$ converges to $\tau$ in $L^1$ norm:

$$\hat{\tau}_{\mathrm{IPSW},n,m}\overset{L_1}{\to}\tau,\ n,m\to\infty.$$

### Plug-in g-formula
If exchangeability holds for RCT and Assumption 1,2 holds, then ATE can be identified with another approach:
$$\tau = \mathbb{E}[\mu_{1,1}(X) - \mu_{0,1}(X)] = \mathbb{E}[\tau_1(X)].$$

Then, we can fit an estimator $\hat{\mu}_ {a,1}(\cdot)$ of $\mu_ {a,1}(\cdot)$ for $a=0,1,$ using the RCT data. Note that a correctly specified regression model consistently estimates the mean outcome:
<p>
  $$\begin{align}
  \mathbb{E}[Y(a)|X=x] &= \mathbb{E}[Y(a)|X=x,S=1] \ \ \text{(Mean exchangeability)}\\
  &= \mathbb{E}[Y(a)|A=a, X=x,S=1]\ \ \text{(Exchangeability)}\\
  &=  \mathbb{E}[Y|A=a, X=x,S=1].\ \ \text{(Consistency)}
  \end{align}$$
</p>

Note that the assumption of mean exchangeability is stronger than Assumption 1. With an observational distribution of $X,$ plug-in the estimated (conditional) mean outcome to g-formula, we obtain

<p>
  $$\mathbb{E}[Y(a)] = \int\mathbb{E}[Y|A=a,X=x,S=1]f(x)\mathrm{d}x.$$
</p>

Take the empirical version, we obtain the plug-in g-formula estimator:

**Definition 2** (Plug-in g-formula estimator / Outcome model-based estimator) Let $\hat{\mu}_ {a,1, n}$ be an estimator of $\mu_ {a,1}$ fitted using RCT data. The plug-in g-formula estimator is then defined as:
<p>
  $$\hat{\tau}_{\mathrm{G},m,n} = \frac{1}{m}\sum_{i=n+1}^{n+m} \left(\hat{\mu}_ {1,1,n}(X_i) - \hat{\mu}_ {0,1,n}(X_i)\right).$$
</p>

Analogous to the IPSW estimator, the plug-in g-formula estimator $\hat{\tau}_ {\mathrm{G},m,n}$ converges to $\tau$ in $L^1$ norm as $n,m\to\infty,$ provided some regularity conditions about the consistency and boundedness of $\hat{\mu}_ {a,n}.$

### Calibration weighting


### Doubly-robust estimators
