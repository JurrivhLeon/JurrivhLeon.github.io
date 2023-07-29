# Support Points
My notes for reading *Support Points*, by Simon Mak and V. Roshan Joseph. This paper proposed a method for compacting a continuous probability distribution into a set of representative points called support points.

## Energy Distance and Support Points
**Definition 1** (Energy distance). Let $F$ and $G$ be two distributions on a non-empty set $\mathcal{X}\subset\mathbb{R}^p$ with finite means, and let $\mathbf{X},\mathbf{X}'\overset{\text{i.i.d.}}{\sim} G$ and $\mathbf{Y},\mathbf{Y}'\overset{\text{i.i.d.}}{\sim} F.$ The energe distance between $F$ and $G$ is defined as
<p>$$E(F,G) := 2\mathbb{E}\Vert\mathbf{X}-\mathbf{Y}\Vert_2 - \mathbb{E}\Vert\mathbf{X}-\mathbf{X}'\Vert_2 - \mathbb{E}\Vert\mathbf{Y} - \mathbf{Y}'\Vert_2.$$</p>

When $G=F_ n$ is the empirical distribution function (e.d.f.) for $\lbrace\mathbf{x}_ i\rbrace_ {i=1}^n\subseteq\mathcal{X},$ this energy distance becomes
<p>$$E(F,F)n) := \frac{2}{n}\sum_{i=1}^n\mathbb{E}\Vert\mathbf{x}_i-\mathbf{Y}\Vert_2 - \frac{1}{n^2}\sum_{1=1}^n\sum_{j=1}^n\mathbb{E}\Vert\mathbf{x}_i-\mathbf{x}_j\Vert_2 - \mathbb{E}\Vert\mathbf{Y} - \mathbf{Y}'\Vert_2.$$</p>

We assume that $F$ is a continuous distribution with finite mean. The energe distance $E(F,F_ n)$ measures the goodness-of-fit. Then, support points are defined as the points set with best goodness-of-fit under the energe distance:

**Definition 2** (Support points). Let $\mathbf{Y}\sim F.$ For a fixed point set size $n\in\mathbb{N},$ the support points of $F$ are defined as
<p>$$\begin{align*}
  \lbrace\xi_i\rbrace_{i=1}^n &\in \underset{\mathbf{x}_1,\cdots,\mathbf{x}_n\in\mathcal{X}}{\mathrm{argmin}} E(F,F_n)\\
  &= \underset{\mathbf{x}_1,\cdots,\mathbf{x}_n\in\mathcal{X}}{\mathrm{argmin}} \left\lbrace\frac{2}{n}\sum_{i=1}^n\mathbb{E}\Vert\mathbf{x}_i-\mathbf{Y}\Vert_2 - \frac{1}{n^2}\sum_{1=1}^n\sum_{j=1}^n\mathbb{E}\Vert\mathbf{x}_i-\mathbf{x}_j\Vert_2\right\rbrace.
  \end{align*}$$</p>
