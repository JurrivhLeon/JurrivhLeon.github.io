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

## A Koksma-Hlawka-like bound
### Conditionally positive definite kernel
First we introduce the concept of conditionally positive definite (c.p.d.) kernels.

**Definition 3** (c.p.d. kernel). A continuous function $\Phi:\mathbb{R}^d\to\mathbb{R}$ is a c.p.d. kernel of order $m$ if for all $N\in\mathbb{N},$ all pairwise distinct $\mathbf{x}_ 1,\cdots,\mathbf{x}_ N$ and all $\boldsymbol{\zeta}\in\mathbb{R}^d\backslash\lbrace 0\rbrace$ satisfying $\sum_ {j=1}^N \zeta_ jp(\mathbf{x}_ j)$ for all real-valued polynomials of degree less than $m,$ the quadratic form
<p>
  $$\begin{align*}\sum_{j=1}^N\sum_{k=1}^N \zeta_j\zeta_k\Phi(\mathbf{x}_j - \mathbf{x}_k) > 0.\end{align*}$$
</p>

**Definition 4** (Generalized Fourier transform, GFT). Suppose $\Phi:\mathbb{R}^d\to\mathbb{C}$ is continuous and slowly increasing. A measurable function $\widehat{\Phi}\in L_2^{\mathrm{loc}}(\mathbb{R}^d\backslash\lbrace 0\rbrace)$ is called the generalized Fourier transform of $f$ if $\exists m\in\frac{1}{2}\mathbb{N}_0$ such that
<p>$$\int_{\mathbb{R}^d}\Phi(\mathbf{x})\widehat{\gamma}(\mathbf{x})\mathrm{d}\mathbf{x} = \int_{\mathbb{R}^d}\widehat{\Phi}(\omega)\gamma(\omega)\mathrm{d}\omega$$</p>

is satisfied for all $\gamma\in\mathcal{S}_ {2m},$ where $\widehat{\gamma}$ denotes the standard Fourier transform of $\gamma,$ $\mathcal{S}_ {2m}=\lbrace\gamma\in\mathcal{S}:\gamma(\omega)=\mathcal{O}(\Vert\omega\Vert_2^{2m})\ \text{for}\ \Vert\omega\Vert_2\to 0\rbrace,$ and $\mathcal{S}$ denote the Schiwartz space.

With the GFT, c.p.d. kernels can be characterized:

**Theorem 3.** Suppose $\Phi: \mathbb{R}^d\to\mathbb{C}$ is continuous, slowly increasing, and possesses a GFT $\widehat{\Phi}$ of order $m/2,$ which is continuous on $\mathbb{R}^d\backslash\lbrace 0\rbrace.$ Then $\Phi$ is a c.p.d. kernel of order $m$ if and only if $\widehat{\Phi}$ is nonnegative and nonvanishing.

### Native space
Denote the space of $d$-variate polynomials of absolute degree at most $m$ by $\pi_m(\mathbb{R}^d).$ Then dimensionality of $\pi_m(\mathbb{R}^d)$ is
<p>$$\begin{align*}
  \mathrm{dim}\left(\pi_m(\mathbb{R}^d)\right) = \sum_{k=0}^m \begin{pmatrix} k+d-1 \\ d-1\end{pmatrix} = \begin{pmatrix} m+d \\ d\end{pmatrix}.
\end{align*}$$</p>

**Definition 5** (Unisolvent Set). The points $\mathbf{X}=\lbrace\mathbf{x}_1,\cdots,\mathbf{N}\rbrace\subset\mathbb{R}^d$ with $N\geq\mathrm{dim}\left(\pi_m(\mathbb{R}^d)\right)$ are called $\pi_m(\mathbb{R}^d)$-unisolvent if the zero polynomial is the only polynomial from $\pi_m(\mathbb{R}^d)$ that vanishes on all of them.

**Definition 6** (Native space). Let $\Phi:\mathbb{R}^d\to\mathbb{R}$ be a c.p.d. kernel of order $m\geq 1$ and $\mathcal{P}=\pi_{m-1}(\mathbb{R}^d)$ be the space of polynomials with degree less than $m.$ Define the linear space
<p>$$\begin{align*}
  F_\Phi(\mathbb{R}^d) = \left\lbrace f(\cdot) = \sum_{j=1}^N\zeta_j\Phi(\mathbf{x}_j - \cdot):\begin{array}
  \ N\in\mathbb{N},\boldsymbol{\zeta}\in\mathbb{R}^N,\mathbf{x}_1,\cdots,\mathbf{x}_N\in\mathbb{R}^d,\\
  \sum_{j=1}^N\zeta_jp(\mathbf{x}_j)=0\ \forall p\in\mathcal{P}
  \end{array}\right\rbrace
\end{align*}$$</p>

equipped with the inner product
<p>$$\begin{align*}
  \left\langle \sum_{j=1}^M\zeta_j\Phi(\mathbf{x}_j - \cdot), \sum_{k=1}^N\zeta_k^\prime\Phi(\mathbf{y}_k - \cdot)\right\rangle_\mathbf{\Phi} = \sum_{j=1}^M\sum_{k=1}^N\zeta_j\zeta_k^\prime\Phi(\mathbf{x}_j - \mathbf{y}_k).
\end{align*}$$</p>

Let $\mathcal{F}_ \Phi(\mathbb{R}^d)$ be the completion of $F_ \Phi(\mathbb{R}^d).$ Let $\lbrace\boldsymbol{\psi}_ 1,\cdots,\boldsymbol{\psi}_ m\rbrace\subset\mathbb{R}^d, m=\mathrm{dim}(\mathcal{P})$ be a $\mathcal{P}$-unisolvent subset, and $\lbrace p_1,\cdots,p_m\rbrace$ be a Lagrange basis of $\mathcal{P}$ for such a subset. Furthermore, define the projective map $\Pi_ \mathcal{P}:C(\mathbb{R}^d)\to\mathcal{P}$ as $\Pi_ \mathcal{P}(f) = \sum_ {k=1}^m f(\boldsymbol{\psi}_ k)p_ k,$ and the map $\mathcal{R}:\mathcal{F}_ \Phi(\mathbb{R}^d)\to C(\mathbb{R}^d)$ as $\mathcal{R}[f] (\mathbf{x}) = f(\mathbf{x}) - \Pi_ \mathcal{P}f(\mathbf{x}).$ The native space for $\Phi$ is then defined as
<p>$$\mathcal{N}_\Phi(\mathbb{R}^d) = \mathcal{R}(\mathcal{F}_\Phi\left(\mathbb{R}^p)\right) + \mathcal{P},$$</p>

and is equipped with the semi-inner product
<p>$$\langle f,g\rangle_{\mathcal{N}_\Phi(\mathbb{R}^d)} = \left\langle \mathcal{R}^{-1}(f - \Pi_\mathcal{P}f), \mathcal{R}^{-1}(g - \Pi_\mathcal{P}g)\right\rangle_\Phi.$$</p>

**Remark.** Generally, $\Phi(x-\cdot)\in\mathcal{N}_\Phi(\mathbb{R}^d)$ does not hold. Instead, we define a new functional
<p>$$\delta_{(\mathbf{x})} = \delta_\mathbf{x} - \sum_{l=1}^mp_l(x)\delta_{\boldsymbol{\psi}_l}$$</p>

and a function
<p>$$G(\cdot,\mathbf{x}) = \Phi(\mathbf{x}-\cdot) - \sum_{l=1}^m p_l(\mathbf{x})\Phi(\boldsymbol{\psi}_l - \cdot).$$</p>

Then we have that for any $f\in\mathcal{F}_\Phi(\mathbb{R}^d),$
<p>$$\mathcal{R}[f] (\mathbf{x}) = \delta_{(\mathbf{x})}[f] = f(\mathbf{x})-\sum_{l=1}^m f(\boldsymbol{\psi}_l)p_l(\mathbf{x}) = \langle f,G(\cdot,\mathbf{x})\rangle_\Phi.$$</p>

For any $\boldsymbol{\psi}_ l,$ we have that $G(\boldsymbol{\psi}_ l) = 0,$ then $\Pi_ \mathcal{P}\mathcal{R}[f] = \sum_ {l=1}^m \mathcal{R}[f] (\boldsymbol{\psi}_ l)p_ l = 0.$ Therefore, for any $h\in\mathcal{N}_ \Phi(\mathbb{R}^d)$ such that $h=\mathcal{R}[f] + p$ with $f\in\mathcal{F}_ \Phi(\mathbb{R}^d)$ and $p\in\mathcal{P},$ we have $\Pi_ \mathcal{P}\ h = p,$ and
<p>$$f = \mathcal{R}^{-1}[h - \Pi_\mathcal{P}h].$$</p>

(This expression requires that $\mathcal{R}$ is injective. To show this, it suffices to show that $\mathcal{R}[f] = 0$ if and only if $f=0.$)

Moreover, since $\lbrace p_1,\cdots,p_m\rbrace$ is a Lagrange basis of $\mathcal{P},$ we have
<p>$$\delta_{(\mathbf{x})} [p] = 0\ \ \forall p\in\mathcal{P},$$</p>

hence $G(\cdot,\mathbf{x})\in\mathcal{F}_ \Phi(\mathbb{R}^d),$ and $G(\cdot,\mathbf{x}) = \mathcal{R}^{-1}[G(\cdot,\mathbf{x}) - \Pi_ \mathcal{P}G(\cdot,\mathbf{x})].$

**Theorem 4.** With the conclusions above, $h\in\mathcal{N}_ \Phi(\mathbb{R}^d)$ admits the following decomposition:
<p>$$\begin{align*}
  h(\mathbf{x}) &= \mathcal{R}^{-1}[h-\Pi_\mathcal{P}h](\mathbf{x}) + \Pi_\mathcal{P}h(\mathbf{x})\\
  &= \left\langle\mathcal{R}^{-1}[h-\Pi_\mathcal{P}h], G(\cdot,x)\right\rangle_\Phi + \Pi_\mathcal{P}h(\mathbf{x})\\
  &= \left\langle\mathcal{R}^{-1}[h-\Pi_\mathcal{P}h], \mathcal{R}^{-1}[G(\cdot,\mathbf{x}) - \Pi_ \mathcal{P}G(\cdot,\mathbf{x})]\right\rangle_\Phi + \Pi_\mathcal{P}h(\mathbf{x})\\
  &= \left\langle h, G(\cdot,\mathbf{x})\right\rangle_{\mathcal{N}_\Phi(\mathbb{R}^d)} + \Pi_\mathcal{P}h(\mathbf{x}).
\end{align*}$$</p>

Now suppose for any $\mathbf{x}\in\mathbb{R}^d,$
<p>$$k(\cdot,\mathbf{x}) := \mathcal{R}[G(\cdot,\mathbf{x})] + \sum_{l=1}^m p_l(\cdot)p_l(\mathbf{x}) \in \mathcal{R}\left(\mathcal{F}_\Phi(\mathbb{R}^d)\right) + \mathcal{P},$$</p>

then we have
<p>$$\mathcal{R}^{-1}[k(\cdot,\mathbf{x}) - \Pi_\mathcal{P}k(\cdot,\mathbf{x})] = \mathcal{R}^{-1}\left[\mathcal{R}[G(\cdot,\mathbf{x})]\right] = G(\cdot,\mathbf{x}).$$</p>

Then for any $h\in\mathcal{N}_ \Phi(\mathbb{R}^d),$ we have the reproducing property:
<p>$$\begin{align*}
  h(\mathbf{x}) &= \left\langle\mathcal{R}^{-1}[h - \Pi_\mathcal{P}h], G(\cdot,x)\right\rangle_\Phi + \Pi_\mathcal{P}h(\mathbf{x})\\
  &= \left\langle\mathcal{R}^{-1}[h - \Pi_\mathcal{P}h], \mathcal{R}^{-1}[k(\cdot,\mathbf{x}) - \Pi_\mathcal{P}k(\cdot,\mathbf{x})]\right\rangle_\Phi + \Pi_\mathcal{P}h(\mathbf{x})\\
  &= \left\langle h,k(\cdot,\mathbf{x})\right\rangle_{\mathcal{N}_\Phi(\mathbb{R}^d)} + \sum_{l=1}^m h(\boldsymbol{\psi}_l)k(\boldsymbol{\psi}_l,\mathbf{x}).
\end{align*}$$</p>

Thus we can transform a native space into a RKHS.

**Theorem 5** (From native space to RKHS). The native space $\mathcal{N}_ \Phi(\mathbb{R}^d)$ for a c.p.d. kernel $\Phi$ carries the inner product
<p>$$\langle f,g\rangle = \langle f,g\rangle_{\mathcal{N}_\Phi(\mathbb{R}^d)} + \sum_{l=1}^m f(\boldsymbol{\psi}_l)g(\boldsymbol{\psi}_l).$$</p>

With this inner product, $\mathcal{N}_\Phi(\mathbb{R}^d)$ becomes a RKHS with reproducing kernel:
<p>$$\begin{align*}
  k(\mathbf{x},\mathbf{y}) &= \Phi(\mathbf{x}-\mathbf{y}) - \sum_{l=1}^m p_l(\mathbf{x})\Phi(\boldsymbol{\psi}_l - \mathbf{y}) - \sum_{l=1}^m p_l(\mathbf{y})\Phi(\mathbf{x} - \boldsymbol{\psi}_l)\\
  &\qquad + \sum_{l=1}^m\sum_{l'=1}^m p_l(\mathbf{x})p_{l'}(\mathbf{y})\Phi(\boldsymbol{\psi}_l - \boldsymbol{\psi}_{l'}) + \sum_{l=1}^m p_l(\mathbf{x})p_l(\mathbf{y}).
\end{align*}$$</p>

###  A Koksma-Hlawka-like bound

**Theorem 6** (Koksma-Hlawka). Let $\lbrace\mathbf{x}_ i\rbrace_ {i=1}^n\subseteq\mathcal{X}\subseteq\mathbb{R}^d$ be a point set with e.d.f. $F_ n,$ and let $\Phi(\mathbf{x}) = -\Vert\mathbf{x}\Vert_ 2.$ Then $\Phi$ is a c.p.d. kernel of order 1. Moreover:
(a) The native space $\mathcal{N}_ \Phi(\mathbb{R}^d)$ can be explicitly written as
<p>$$\mathcal{N}_\Phi(\mathbb{R}^d) = \left\lbrace
  f\in C(\mathbb{R}^d):\begin{eqnarray}
  &\text{(G1)}&\ \exists m\in\mathbb{N}_0\ \text{s.t.}\ f(\mathbf{x})=\mathcal{O}(\Vert\mathbf{x}\Vert_2^m)\ \text{for}\ \Vert\mathbf{x}\Vert_2\to\infty\\
  &\text{(G2)}&\ f\ \text{has a GFT}\ \widehat{f}\ \text{of order}\ 1/2\\
  &\text{(G3)}&\ \int\Vert\omega\Vert_2^{p+1}\vert\widehat{f}(\omega)\vert^2\mathrm{d}\omega < \infty
  \end{eqnarray}\right\rbrace,$$</p>

with semi-inner product given by
<p>$$\langle f,g\rangle_{\mathcal{N}_\Phi(\mathbb{R}^d)} = \left\lbrace\Gamma\left(\frac{d+1}{2}\right)2^d\pi^{\frac{d-1}{2}}\right\rbrace^{-1}\int\widehat{f}(\omega)\overline{\widehat{g}(\omega)}\Vert\omega\Vert_2^{d+1}\mathrm{d}\omega.$$</p>

(b) Consider the function space $\mathcal{G}_ d=\mathcal{N}_ \Phi(\mathbb{R}^d)$ equipped with inner product
<p>$$\langle f,g\rangle_{\mathcal{G}_d} = \langle f,g\rangle_{\mathcal{N}_\Phi(\mathbb{R}^d)} + f(\boldsymbol{\psi})g(\boldsymbol{\psi})$$</p>

for a fixed choice of $\boldsymbol{\psi}\in\mathcal{X}.$ Then $(\mathcal{G}_ d,\langle\cdot,\cdot\rangle_ {\mathcal{G}_ d})$ is a RKHS, and for any integrand $g\in\mathcal{G}_ d,$ the integration error is bounded by
<p>$$ I(g;F,F_n) := \left\vert\int g(\mathbf{x})\mathrm{d}[F-F_n](\mathbf{x})\right\vert \leq \Vert g\Vert_{\mathcal{G}_d}\sqrt{\mathcal{E}(F,F_n)}.$$</p>

**Remark.** The conditionally positive-definiteness of $\Phi(\mathbf{x}) = -\Vert\mathbf{x}\Vert_2$ is characterized by its GFT of order 1/2:
<p>$$\widehat{\Phi}(\mathbf{x}) = \frac{2^{d/2}\Gamma\left(\frac{d+1}{2}\right)}{\sqrt{\pi}}\Vert\omega\Vert_2^{-(d+1)},\ \omega\in\mathbb{R}^d\backslash\lbrace 0\rbrace.$$</p>

Then part (a) can be derived from the GFT. For part (b), note the kernel $\Phi(\cdot)$ is c.p.d. of order 1, the $\mathcal{P}=\pi_0(\mathbb{R}^d)=\lbrace f(\mathbf{x})\equiv C\ \text{for some}\ C\in\mathbb{R}\rbrace.$ Hence any choice of $\boldsymbol{\psi}\in\mathcal{X}$ is $\mathcal{P}$-unisolvent, with the Lagrange basis $p(\cdot)\equiv 1.$ By Theorem 5, the native space $\mathcal{N}_ \Phi(\mathbb{R}^d)$ can be transformed into a RKHS $\mathcal{G}_ d$ by equipping a new inner product
<p>$$\langle f,g\rangle_{\mathcal{G}_d} = \langle f,g\rangle_{\mathcal{N}_\Phi(\mathbb{R}^d)} + f(\boldsymbol{\psi})g(\boldsymbol{\psi}),$$</p>

with the reproducing kernel being
<p>$$\tilde{k}(\mathbf{x},\mathbf{y}) = \Phi(\mathbf{x}-\mathbf{y}) - \Phi(\boldsymbol{\psi} - \mathbf{y}) - \Phi(\mathbf{x}-\boldsymbol{\psi}) + 1.$$</p>

Now we bound the integration error:
<p>$$\begin{align*}
  I(g;F,F_n) &= \left\vert\int g(\mathbf{x})\mathrm{d}[F-F_n](\mathbf{x})\right\vert\\
  &= \left\vert\int \langle g(\cdot),\tilde{k}(\cdot,\mathbf{x})\rangle_{\mathcal{G}_d}\mathrm{d}[F-F_n](\mathbf{x})\right\vert\\
  &= \left\vert\left\langle g(\cdot),\int\tilde{k}(\cdot,\mathbf{x})\mathrm{d}[F-F_n](\mathbf{x})\right\rangle_{\mathcal{G}_d}\right\vert\\
  &= \left\Vert g(\cdot)\right\Vert_{\mathcal{G}_d}\left\Vert\int\tilde{k}(\cdot,\mathbf{x})\mathrm{d}[F-F_n](\mathbf{x})\right\Vert_{\mathcal{G}_d},
\end{align*}$$</p>

and
<p>$$\begin{align*}
  \left\Vert\int\tilde{k}(\cdot,\mathbf{x})\mathrm{d}[F-F_n](\mathbf{x})\right\Vert_{\mathcal{G}_d} &= \sqrt{\left\langle\int\tilde{k}(\cdot,\mathbf{x})\mathrm{d}[F-F_n](\mathbf{x}),\int\tilde{k}(\cdot,\mathbf{y})\mathrm{d}[F-F_n](\mathbf{y})\right\rangle_{\mathcal{G}_d}}\\
  &= \sqrt{\int\int\tilde{k}(\mathbf{x},\mathbf{y})\mathrm{d}[F-F_n](\mathbf{x})\mathrm{d}[F-F_n](\mathbf{y})}\\
  &= \sqrt{\int\int\Phi(\mathbf{x}-\mathbf{y})\mathrm{d}[F-F_n](\mathbf{x})\mathrm{d}[F-F_n](\mathbf{y})}\\
  &= \sqrt{\mathcal{E}(F,F_n)},
\end{align*}$$</p>

where the third equality follows from 
<p>$$\int\Phi(\mathbf{x}-\boldsymbol{\psi})\mathrm{d}[F-F_n] (\mathbf{x}) = \int\Phi(\mathbf{x}-\boldsymbol{\psi})\mathrm{d}[F-F_n] (\mathbf{x}) = \int\Phi(\mathbf{x}-\boldsymbol{\psi})\mathrm{d}[F-F_n] (\mathbf{x}) = 0.$$</p>

Then we connect the integration $I(g;F,F_ n)$ with the energy distance $\mathcal{E}(F,F_ n)$ for all intergrands $g$ in the function space $\mathcal{G}_ d.$


