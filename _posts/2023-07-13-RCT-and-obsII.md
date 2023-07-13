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

Now, we use a reweighting approach to obtain the expression of $\tau$：
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


## References
+ Colnet, B., Mayer, I., Chen, G., Dieng, A., Li, R., Varoquaux, G., Vert, J.P., Josse, J. and Yang, S. (2020). Causal inference methods for combining randomized trials and observational studies: a review. *arXiv preprint arXiv:2011.08047*.
+ Kallus, N., Puli, A. M., and Shalit, U. (2018). Removing hidden confounding by experimental grounding. In *Advances in Neural Information Processing Systems*, pages 10888–10897.
+ Yang, S., Zeng, D., and Wang, X. (2020). Improved inference for heterogeneous treatment effects using real-world data subject to hidden confounding. *arXiv preprint arXiv:2007.12922*.