# Support Points
My notes for reading *Support Points*, by Simon Mak and V. Roshan Joseph. This paper proposed a method for compacting a continuous probability distribution into a set of representative points called support points.

## Energy Distance 
**Definition 1** (Energy distance). Let $F$ and $G$ be two distributions on a non-empty set $\mathcal{X}\subset\mathbb{R}^d$ with finite means, and let $\mathbf{X},\mathbf{X}'\overset{\text{i.i.d.}}{\sim} G$ and $\mathbf{Y},\mathbf{Y}'\overset{\text{i.i.d.}}{\sim} F.$ The energe distance between $F$ and $G$ is defined as
<p>$$\mathcal{E}(F,G) := 2\mathbb{E}\Vert\mathbf{X}-\mathbf{Y}\Vert_2 - \mathbb{E}\Vert\mathbf{X}-\mathbf{X}'\Vert_2 - \mathbb{E}\Vert\mathbf{Y} - \mathbf{Y}'\Vert_2.$$</p>

**Proposition 1.** The energy distance admits the following representation: denote the characteristic functions of distributions $F$ and $G$ by $\hat{f}$ and $\hat{g},$ respectively, then

$$\mathcal{E}(F,G) = \frac{\Gamma\left(\frac{d+1}{2}\right)}{\pi^{(d+1)/2}}\int_{\mathbb{R}^d}\frac{\vert\hat{f}(\omega)-\hat{g}(\omega)\vert^2}{\Vert\omega\Vert_2^{d+1}}\mathrm{d}\omega.$$

Hence we have $\mathcal{E}(F,G)\geq 0$ with equality holding if and only if $F=G.$

Now we consider the $L_2$ distance between $F$ and $G$ in the univariate case:
<p>
  $$\begin{align*}
\Vert F-G\Vert_{L_2(\mathbb{R})}^2 &= \int_\mathbb{R} \left[F(t) - G(t)\right]^2\mathrm{d}t\\
&= \int_{-\infty}^{+\infty} [F(t)(1-G(t)) + G(t)(1-F(t))\\
&\qquad\qquad - F(t)(1-F(t)) - G(t)(1-G(t))]\mathrm{d}t;\\
\end{align*}$$
</p>

Moreover, for independent $X,X^\prime\sim G,\ Y,Y^\prime\sim F,$ we have
<p>
  $$\begin{align*}
\mathbb{E}\vert X - Y\vert &= \int_{-\infty}^{+\infty}\int_{-\infty}^{+\infty}\vert x-y\vert\ \mathrm{d}G(x)\mathrm{d}F(y)\\
&=\int_{-\infty}^{+\infty}\int_{y}^{+\infty}(x-y)\mathrm{d}G(x)\mathrm{d}F(y) + \int_{-\infty}^{+\infty}\int_{-\infty}^{y}(y-x)\mathrm{d}G(x)\mathrm{d}F(y)\\
&=\int_{-\infty}^{+\infty}\int_{-\infty}^{x}x\mathrm{d}F(y)\mathrm{d}G(x) - \int_{-\infty}^{+\infty}\int_{y}^{+\infty}y\mathrm{d}G(x)\mathrm{d}F(y)\\
&\qquad + \int_{-\infty}^{+\infty}\int_{-\infty}^{y}y\mathrm{d}G(x)\mathrm{d}F(y) - \int_{-\infty}^{+\infty}\int_{x}^{+\infty}x\mathrm{d}F(y)\mathrm{d}G(x)\\
&=\int_{-\infty}^{+\infty}xF(x)\mathrm{d}G(x) - \int_{-\infty}^{+\infty}y(1-G(y))\mathrm{d}F(y)\\
&\qquad + \int_{-\infty}^{+\infty}yG(y)\mathrm{d}F(y) - \int_{-\infty}^{+\infty}x(1-F(x))\mathrm{d}G(x)\\
&= 2\int_{-\infty}^{+\infty}t\mathrm{d}\lbrace F(t)G(t)\rbrace - \int_{-\infty}^{+\infty}t\mathrm{d}F(t) - \int_{-\infty}^{+\infty}t\mathrm{d}G(t)\\
&= -\int_{-\infty}^{+\infty}t\mathrm{d}\lbrace F(t)(1-G(t)) + (1-F(t))G(t)\rbrace\\
&= \int_{-\infty}^{+\infty}\lbrace F(t)(1-G(t)) + (1-F(t))G(t)\rbrace\mathrm{d}t,
\end{align*}$$
</p>

likewise,

$$\begin{align*}
\mathbb{E}\vert X - X'\vert &= 2\int_{-\infty}^{+\infty} G(t)(1-G(t))\mathrm{d}t,\\
\mathbb{E}\vert Y - Y'\vert &= 2\int_{-\infty}^{+\infty} F(t)(1-F(t))\mathrm{d}t.
\end{align*}$$

Therefore,

$$\begin{align*}
\mathcal{E}(F,G) = 2\Vert F-G\Vert_{L_2(\mathbb{R})}^2.
\end{align*}$$

## Support Points

When $G=F_ n$ is the empirical distribution function (e.d.f.) for $\lbrace\mathbf{x}_ i\rbrace_ {i=1}^n\subseteq\mathcal{X},$ this energy distance becomes
<p>$$\mathcal{E}(F,F_n) := \frac{2}{n}\sum_{i=1}^n\mathbb{E}\Vert\mathbf{x}_i-\mathbf{Y}\Vert_2 - \frac{1}{n^2}\sum_{1=1}^n\sum_{j=1}^n\mathbb{E}\Vert\mathbf{x}_i-\mathbf{x}_j\Vert_2 - \mathbb{E}\Vert\mathbf{Y} - \mathbf{Y}'\Vert_2.$$</p>

We assume that $F$ is a continuous distribution with finite mean. The energe distance $\mathcal{E}(F,F_ n)$ measures the goodness-of-fit. Then, support points are defined as the points set with best goodness-of-fit under the energe distance:

**Definition 2** (Support points). Let $\mathbf{Y}\sim F.$ For a fixed point set size $n\in\mathbb{N},$ the support points of $F$ are defined as
<p>$$\begin{align*}
  \lbrace\xi_i\rbrace_{i=1}^n &\in \underset{\mathbf{x}_1,\cdots,\mathbf{x}_n\in\mathcal{X}}{\mathrm{argmin}} \mathcal{E}(F,F_n)\\
  &= \underset{\mathbf{x}_1,\cdots,\mathbf{x}_n\in\mathcal{X}}{\mathrm{argmin}} \left\lbrace\frac{2}{n}\sum_{i=1}^n\mathbb{E}\Vert\mathbf{x}_i-\mathbf{Y}\Vert_2 - \frac{1}{n^2}\sum_{1=1}^n\sum_{j=1}^n\mathbb{E}\Vert\mathbf{x}_i-\mathbf{x}_j\Vert_2\right\rbrace.
  \end{align*}$$</p>

Also, in the univariate setting of $d=1$, the support points of $F$ are equal to the point set with minimum $L_2$-discrepancy, since $\mathcal{E}(F,F_n)=2\Vert F-F_n\Vert_{L_2(\mathbb{R})}$. But in multivariate setting of $d>1$, the equivalence does not generally hold.

**Theorem 1** (Distributional convergence). Let $\mathbf{X}\sim F$ and $\mathbf{X}_ n\sim F_ n,$ where $F_ n$ is the e.d.f. of the support points. Then $\mathbf{X}_n\overset{d}{\to}\mathbf{X}.$

This theorem states that support points are representative of the target dstribution $F$ as the number of points $n$ increases. Furthermore, the consistency of support points can be established:

**Corollary 1** (Consistency). Let $\mathbf{X}\sim F$ and $\mathbf{X}_ n\sim F_ n,$ where $F_ n$ is the e.d.f. of the support points.
+ If $g:\mathcal{X}\to\mathbb{R}$ is continuous, then $g(\mathbf{X}_ n)\overset{d}{\to}g(\mathbf{X}).$
+ If $g:\mathcal{X}\to\mathbb{R}$ is continuous and bounded, then $\lim_ {n\to\infty}\mathbb{E}[g(\mathbf{X}_ n)] = \mathbb{E}[g(\mathbf{X})].$

This directly follows from the continuous mapping theorem and Portmanteau theorem.

### A Koksma-Hlawka-like bound
First we introduce the concept of conditionally positive definite (c.p.d.) kernels.

**Definition 3** (c.p.d. kernel). A continuous function $\Phi:\mathbb{R}^d\to\mathbb{R}$ is a c.p.d. kernel of order $m$ if for all $N\in\mathbb{N},$ all pairwise distinct $\mathbf{x}_ 1,\cdots,\mathbf{x}_ N$ and all $\boldsymbol{\zeta}\in\mathbb{R}^d\backslash\lbrace 0\rbrace$ satisfying $\sum_ {j=1}^N \zeta_ jp(\mathbf{x}_ j)$ for all real-valued polynomials of degree less than $m,$ the quadratic form
<p>
  $$\begin{align*}\sum_{j=1}^N\sum_{k=1}^N \zeta_j\zeta_k\Phi(\mathbf{x}_j - \mathbf{x}_k) > 0.\end{align*}$$
</p>

Denote the space of $d$-variate polynomials of absolute degree at most $m$ by $\pi_m(\mathbb{R}^d).$ Then dimensionality of $\pi_m(\mathbb{R}^d)$ is
<p>$$\begin{align*}
  \mathrm{dim}\left(\pi_m(\mathbb{R}^d)\right) = \sum_{k=0}^m \begin{pmatrix} k+d-1 \\ d-1\end{pmatrix} = \begin{pmatrix} m+d \\ d\end{pmatrix}.
\end{align*}$$</p>

**Definition 4** (Unisolvent Set). The points $\mathbf{X}=\lbrace\mathbf{x}_1,\cdots,\mathbf{N}\rbrace\subset\mathbb{R}^d$ with $N\geq\mathrm{dim}\left(\pi_m(\mathbb{R}^d)\right)$ are called $\pi_m(\mathbb{R}^d)$-unisolvent if the zero polynomial is the only polynomial from $\pi_m(\mathbb{R}^d)$ that vanishes on all of them.

**Definition 5** (Native space). Let $\Phi:\mathbb{R}^d\to\mathbb{R}$ be a c.p.d. kernel of order $m\geq 1$ and $\mathcal{P}=\pi_{m-1}(\mathbb{R}^d)$ be the space of polynomials with degree less than $m.$ Define the linear space
<p>$$\begin{align*}
  \mathcal{F}_\Phi(\mathbb{R}^d) = \left\lbrace f(\cdot) = \sum_{j=1}^N\zeta_j\Phi(\mathbf{x}_j - \cdot):\begin{array}
  \ N\in\mathbb{N},\boldsymbol{\zeta}\in\mathbb{R}^N,\mathbf{x}_1,\cdots,\mathbf{x}_N\in\mathbb{R}^d,\\
  \sum_{j=1}^N\zeta_jp(\mathbf{x}_j)=0\ \forall p\in\mathcal{P}
  \end{array}\right\rbrace
\end{align*}$$</p>

equipped with the inner product
<p>$$\begin{align*}
  \left\langle \sum_{j=1}^M\zeta_j\Phi(\mathbf{x}_j - \cdot), \sum_{k=1}^N\zeta_k^\prime\Phi(\mathbf{y}_k - \cdot)\right\rangle_\mathbf{\Phi} = \sum_{j=1}^M\sum_{k=1}^N\zeta_j\zeta_k^\prime\Phi(\mathbf{x}_j - \mathbf{y}_k).
\end{align*}$$</p>

Let $\lbrace\boldsymbol{\psi}_ 1,\cdots,\boldsymbol{\psi}_ m\rbrace\subset\mathbb{R}^d, m=\mathrm{dim}(\mathcal{P})$ be a $\mathcal{P}$-unisolvent subset, and $\lbrace{p_1,\cdots,p_m}$ be a Lagrange basis of $\mathcal{P}$ for such a subset. Furthermore, define the projective map $\Pi_ \mathcal{P}:C(\mathbb{R}^p)\to\mathcal{P}$ as $\Pi_ \mathcal{P}(f) = \sum_ {k=1}^m f(\boldsymbol{\psi}_ k)p_ k,$ and the map $\mathcal{R}:\mathcal{F}_ \Phi(\mathbb{R}^d)\to C(\mathbb{R}^d)$ as $\mathcal{R}[f] (\mathbf{x}) = f(\mathbf{x}) - \Pi_ \mathcal{P}f(\mathbf{x}).$ The native space for $\Phi$ is then defined as
<p>$$\mathcal{N}_\Phi(\mathbb{R}^d) = \mathcal{R}(\mathcal{F}_\Phi\left(\mathbb{R}^p)\right) + \mathcal{P},$$</p>

and is equipped with the semi-inner product
<p>$$\langle f,g\rangle_{\mathcal{N}_\Phi(\mathbb{R}^d)} = \left\langle \mathcal{R}^{-1}(f - \Pi_\mathcal{P}f), \mathcal{R}^{-1}(g - \Pi_\mathcal{P}g)\right\rangle_\Phi.$$</p>

