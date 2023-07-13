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


## References
+ Kallus, N., Puli, A. M., and Shalit, U. (2018). Removing hidden confounding by experimental grounding. In *Advances in Neural Information Processing Systems*, pages 10888–10897.
+ Yang, S., Zeng, D., and Wang, X. (2020). Improved inference for heterogeneous treatment effects using real-world data subject to hidden confounding. arXiv preprint arXiv:2007.12922.
