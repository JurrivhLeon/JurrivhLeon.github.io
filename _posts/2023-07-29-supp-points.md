# Support Points
My notes for reading *Support Points*, by Simon Mak and V. Roshan Joseph. This paper proposed a method for compacting a continuous probability distribution into a set of representative points called support points.

## Energy Distance and Support Points
**Definition 1** (Energy distance). Let $F$ and $G$ be two distributions on a non-empty set $\mathcal{X}\subset\mathbb{R}^p$ with finite means, and let $\mathbf{X},\mathbf{X}'\overset{\text{i.i.d.}}{\sim} G$ and $\mathbf{Y},\mathbf{Y}'\overset{\text{i.i.d.}}{\sim} F.$ The energe distance between $F$ and $G$ is defined as
<p>$$\mathcal{E}(F,G) := 2\mathbb{E}\Vert\mathbf{X}-\mathbf{Y}\Vert_2 - \mathbb{E}\Vert\mathbf{X}-\mathbf{X}'\Vert_2 - \mathbb{E}\Vert\mathbf{Y} - \mathbf{Y}'\Vert_2.$$</p>

**Proposition 1.** The energy distance admits the following representation: denote the characteristic functions of distributions $F$ and $G$ by $\hat{f}$ and $\hat{g},$ respectively, then

$$\mathcal{E}(F,G) = \frac{\Gamma\left(\frac{d+1}{2}\right)}{\pi^{(d+1)/2}}\int_{\mathbb{R}^d}\frac{\vert\hat{f}(\omega)-\hat{g}(\omega)\vert^2}{\Vert\omega\Vert_2^{d+1}}\mathrm{d}\omega.$$

Hence we have $\mathcal{E}(F,G)\geq 0$ with equality holding if and only if $F=G.$

Now we consider the $L_2$ distance between $F$ and $G$ in the univariate case:

$$\begin{align*}
\Vert F-G\Vert_{L_2(\mathbb{R})}^2 &= \int_\mathbb{R} \left[F(t) - G(t)\right]^2\mathrm{d}t\\
&= \int_{-\infty}^{+\infty} [F(t)(1-G(t)) + G(t)(1-F(t))\\
&\qquad\qquad - F(t)(1-F(t)) - G(t)(1-G(t))]\mathrm{d}t;\\
\end{align*}$$

Moreover, for independent $X,X^\prime\sim G,\ Y,Y^\prime\sim F,$ we have

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

likewise,

$$\begin{align*}
\mathbb{E}\vert X - X'\vert &= 2\int_{-\infty}^{+\infty} G(t)(1-G(t))\mathrm{d}t,\\
\mathbb{E}\vert Y - Y'\vert &= 2\int_{-\infty}^{+\infty} F(t)(1-F(t))\mathrm{d}t.
\end{align*}$$

Therefore,

$$\begin{align*}
\mathcal{E}(F,G) = 2\Vert F-G\Vert_{L_2(\mathbb{R})}^2.
\end{align*}$$


When $G=F_ n$ is the empirical distribution function (e.d.f.) for $\lbrace\mathbf{x}_ i\rbrace_ {i=1}^n\subseteq\mathcal{X},$ this energy distance becomes
<p>$$\mathcal{E}(F,F_n) := \frac{2}{n}\sum_{i=1}^n\mathbb{E}\Vert\mathbf{x}_i-\mathbf{Y}\Vert_2 - \frac{1}{n^2}\sum_{1=1}^n\sum_{j=1}^n\mathbb{E}\Vert\mathbf{x}_i-\mathbf{x}_j\Vert_2 - \mathbb{E}\Vert\mathbf{Y} - \mathbf{Y}'\Vert_2.$$</p>

We assume that $F$ is a continuous distribution with finite mean. The energe distance $\mathcal{E}(F,F_ n)$ measures the goodness-of-fit. Then, support points are defined as the points set with best goodness-of-fit under the energe distance:

**Definition 2** (Support points). Let $\mathbf{Y}\sim F.$ For a fixed point set size $n\in\mathbb{N},$ the support points of $F$ are defined as
<p>$$\begin{align*}
  \lbrace\xi_i\rbrace_{i=1}^n &\in \underset{\mathbf{x}_1,\cdots,\mathbf{x}_n\in\mathcal{X}}{\mathrm{argmin}} \mathcal{E}(F,F_n)\\
  &= \underset{\mathbf{x}_1,\cdots,\mathbf{x}_n\in\mathcal{X}}{\mathrm{argmin}} \left\lbrace\frac{2}{n}\sum_{i=1}^n\mathbb{E}\Vert\mathbf{x}_i-\mathbf{Y}\Vert_2 - \frac{1}{n^2}\sum_{1=1}^n\sum_{j=1}^n\mathbb{E}\Vert\mathbf{x}_i-\mathbf{x}_j\Vert_2\right\rbrace.
  \end{align*}$$</p>
