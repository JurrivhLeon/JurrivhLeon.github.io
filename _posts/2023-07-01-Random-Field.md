# Random Field
## Definition and Properties
**Definition.** Given a probability space $(\Omega, \mathcal{F},\mathbb{P})$, a random field is a family or collection of random variables indexed by elements in a topological space $\mathcal{T}$. That is, a random field $Z(\cdot)$ is a collection

$$\left\lbrace Z(s): s\in\mathcal{T}\right\rbrace,$$

where each $Z(s)$ is a random variable.

Some examples are shown below.

Time Series: $X(t),\ t\in\mathbb{Z},$ like random walk;

Stochastic Process: $X(t),\ t\in\mathbb{R}$, like Poisson process, Brownian process, etc.;

Spatial Process: $Z(s),\ s\in\mathcal{D}\subseteq \mathbb{R}^d,\ d\geq 2$;

Spatio-temporal Process: $Z(s,t),\ s\in\mathcal{D},\ t\in\mathbb{R}$.

### Mean and Covariance
For a random field $Z(\cdot)$ defined on $\mathcal{D}\subseteq \mathbb{R}^d$, the mean function is defined as a function $\mu_Z:\mathcal{D}\to\mathbb{R}$, given by

$$\mu_Z(s) := \mathbb{E}[Z(s)];$$

the covariance function $K_Z:\mathcal{D}\times\mathcal{D}\to\mathbb{R}$ is defined as

$$K_Z(s_1,s_2) := \mathrm{Cov}\left(Z(s_1),Z(s_2)\right) = \mathbb{E}\left[\left(Z(s_1)-\mu_Z(s_1)\right)\left(Z(s_2)-\mu_Z(s_2)\right)\right].$$

By definition, the covariance function is symmetric. Furthermore, it is positive definite, i.e., $\forall n\in\mathbb{N}^*,\ \forall s_1,\cdots,s_n\in\mathcal{D}$, the Gram matrix defined by $\mathbf{K} = \lbrace K_Z(s_i,s_j)\rbrace_{i,j=1}^n$ is positive semidefinite.

### Properties of Covariance Function
Suppose that $K(\cdot,\cdot)$ is a valid covariance function defined on $\mathcal{D}\times\mathcal{D}$.

**Definition** (Stationarity). A covariance function $K$ is called stationary if $\forall s_1,s_2\in\mathcal{D}$, $K(s_1,s_2)$ only depends on $s_1-s_2$. That is, there exists some $K_1:\mathcal{D}-\mathcal{D}\to\mathbb{R}$ such that

$$K(s_1,s_2) = K_1(s_1-s_2)\ \ \forall s_1,s_2\in\mathcal{D}.$$

A stationary covariance function is invariant to translation.

**Definition** (Isotropy). A covariance function $K$ is called isotropic if $\forall s_1,s_2\in\mathcal{D}$, $K(s_1,s_2)$ only depends on $\Vert s_1-s_2\Vert$, where $\Vert\cdot\Vert$ denotes $L_2$-norm. That is, there exists some $K_2:\mathbb{R}^*\to\mathbb{R}$ such that

$$K(s_1,s_2) = K_2(\Vert s_1-s_2\Vert)\ \ \forall s_1,s_2\in\mathcal{D}.$$

**Definition** (Dot product). A covariance function $K$ has rotational invariance if $\forall s_1,s_2\in\mathcal{D}$, $K(s_1,s_2)$ only depends on their dot product $s_1\cdot s_2$. That is, there exists some $K_3:\mathbb{R}\to\mathbb{R}$ such that

$$K(s_1,s_2) = K_3(s_1\cdot s_2)\ \ \forall s_1,s_2\in\mathcal{D}.$$

