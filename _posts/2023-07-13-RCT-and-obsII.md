# Combining Experimental and Observational Data (II)
In this blog we continue to discuss combining experimental and observational data. Last blog: [Combining Experimental and Observational Data (I)](https://jurrivhleon.github.io/2023/07/12/RCT-and-obs.html).

## Observational Data with Treatment and Outcome
In observational data, the actual data generating process is unknown and there exists unmeasured confounding in general, i.e.

$$\lbrace Y(1),Y(0)\rbrace\not\perp A\ |\ X.$$

Based on observational data, we can obtain the standard causal inference estimator $\hat{\tau}^{\mathcal{O}}_m$ of ATE $\tau,$ and $\hat{\tau}^{\mathcal{O}}_m(x)$ of CATE $\tau(x),$ with some confounding bias:
<p>
  $$\lim_{m\to\infty}\hat{\tau}^\mathcal{O}_m \neq \tau,\ \text{and}\ \lim_{m\to\infty}\hat{\tau}^\mathcal{O}_m(x) \neq \tau(x).$$
</p>

### Deconfounding: Model the bias
This idea comes from Kallus et al. (2018). Define a bias function $\eta(x)\neq 0$ to model the discrepancy between the true CATE and the estimated CATE:
<p>
  $$\eta(x) := \tau(x) - \tau^\mathcal{O}(x).$$
</p>

Here $\tau^{\mathcal{O}}(x) = \mathbb{E}[Y\vert X=x,A=1] - \mathbb{E}[Y\vert X=x,A=0].$ Our estimator $\hat{\tau}^\mathcal{O}_m(x)$ is obtained by plugging-in $\tau^{\mathcal{O}}(x)$ since it is identifiable.  Suppose $\eta(x)$ can be well approximated by a function with low complexity. We use a linear model to simulate the bias:
<p>
  $$\tau(x) = \tau^\mathcal{O}(x) + \theta^\top x,\ \theta\in\mathbb{R}^p.$$
</p>

Now, we use a reweighting approach to obtain the expression of $\tau$:
<p>
  $$\tau^*_i = \left(\frac{A_i}{e(X_i)} - \frac{1-A_i}{1-e(X_i)}\right)Y_i.$$
</p>

Then we can learn $\theta$ through a least-squares approach on the RCT sample:
<p>
  $$\hat{\theta} = \underset{\theta\in\mathbb{R}^p}{\mathrm{argmin}}\sum_{i=1}^n\left(\tau_i^* - \hat{\tau}_m^\mathcal{O}(X_i) - \theta^\top X_i\right)^2$$
</p>

note that $\hat{\tau}_m^\mathcal{O}(\cdot)$ is learned on the observational data. Using the estimated $\hat{\theta},$ we can recover the causal effect:
<p>
  $$\hat{\tau}_{n,m}(x) = \hat{\tau}_m^\mathcal{O}(x) + \hat{\theta}_{n,m}^\top x.$$
</p>

Under some regularity conditions, the $\hat{\theta}_{n,m}$ estimated through least squares is consistent, and $\hat{\tau}(\cdot)$ is consistent on its target population.

### Deconfounding: Model the confounding
This idea comes from Yang et al. (2020). Different from the previous discussion, we use $S=1$ to denote the RCT sample and $S=0$ the observational data.

**Assumption 1** (Transportability and strong ignorability of trial treatment assignment). 
+ $\mathbb{E}[Y(1)-Y(0)\vert X,S=1] = \mathbb{E}[Y(1) - Y(0)\vert X,S=0] = \tau(X),$ and
+ $Y(a)\perp A\ \vert\ (X,S=1)\ \text{for}\ a\in\lbrace 0,1\rbrace,$ and $e(X,S)>0$ almost surely.

We denote the CATE $\tau(\cdot)$ and confounding function by
<p>
  $$\begin{align}
  \tau(x) &= \mathbb{E}[Y(1)-Y(0)|X=x],\\
  \lambda(x) &= \mathbb{E}[Y(0)|A=1,X=x,S=0] - \mathbb{E}[Y(0)|A=0,X=x,S=0].
  \end{align}$$
</p>

Both of them can be identified:
<p>
  $$\begin{align}
  \tau(x) &= \mathbb{E}[Y|A=1,X=x,S=1] - \mathbb{E}[Y|A=0,X=x,S=1],\\
  \lambda(x) &= \mathbb{E}[Y|A=1,X=x,S=0] - \mathbb{E}[Y|A=0,X=x,S=0] - \tau(x).
  \end{align}$$
</p>


We parameterize $\tau(\cdot)$ and $\lambda(\cdot)$ as follows:

**Assumption 2** (Parametric tructural model). The heterogeneity of treatment effect and confounding functions are
<p>$$\tau(x)=\tau_{\varphi_0}(x),\ \lambda(x)=\lambda_{\phi_0}(x),$$</p>

where $\psi_0 = (\varphi_0^\top,\phi_0^\top)^\top\in\mathbb{R}^{p_1}\times\mathbb{R}^{p_2}.$

**Remark.** A crude estimator of $\psi_0$ can be obtained by least squares, since $\tau_ {\varphi_ 0}(x)$ and $\lambda_ {\phi_ 0}(x)$ are idetified.

Note that
<p>
  $$\begin{align}
  \mathbb{E}[Y-\tau(X)A|A,X,S] &= \mathbb{E}[Y(A)-A(Y(1)-Y(0))|A,X,S] = \mathbb{E}[Y(0)|A,X,S],
  \end{align}$$
</p>

that by Assumption 1
<p>
  $$\mathbb{E}[Y(0)|A,X,S=1] - \mathbb{E}[Y(0)|X,S=1] = 0,$$
</p>

and that
<p>
  $$\begin{align}
  &\mathbb{E}[Y(0)|A,X,S=0] - \mathbb{E}[Y(0)|X,S=0] \\
  &= \mathbb{E}[Y(0)|A,X,S=0] - \mathbb{E}[Y(0)|A=1,X,S=0]\times e(X,0)\\
  &\quad - \mathbb{E}[Y(0)|A=0,X,S=0]\times(1-e(X,0))\\
  &= \left(A-e(X,0)\right)\left\lbrace\mathbb{E}[Y(0)|A=1,X,S=0]-\mathbb{E}[Y(0)|A=0,X,S=0]\right\rbrace\\
  &= \lambda_{\phi_0}(X)\left(A-e(X,0)\right).
  \end{align}$$
</p>

Then, we introduce the following variable to mimic $Y(0):$
<p>
  $$H_{\psi_0} = Y-\tau_{\varphi_0}(X)A - (1-S)\lambda_{\phi_0}(X)(A-e(X)).$$
</p>

**Proposition 1.** Under Assumptions 1 and 2, we have $\mathbb{E}[H_{\psi_0}\vert A,X,S] = \mathbb{E}[Y(0)\vert X,S].$

Furthermore, the semiparametric efficiency score of $\psi_0$ can be derived. An estimating equation using this score is applied to solve a semiparametric efficient estimator of $\psi_0.$


## References
+ Colnet, B., Mayer, I., Chen, G., Dieng, A., Li, R., Varoquaux, G., Vert, J.P., Josse, J. and Yang, S. (2020). Causal inference methods for combining randomized trials and observational studies: a review. *arXiv preprint arXiv:2011.08047*.
+ Kallus, N., Puli, A. M., and Shalit, U. (2018). Removing hidden confounding by experimental grounding. In *Advances in Neural Information Processing Systems*, pages 10888–10897.
+ Yang, S., Zeng, D., and Wang, X. (2020). Improved inference for heterogeneous treatment effects using real-world data subject to hidden confounding. *arXiv preprint arXiv:2007.12922*.
