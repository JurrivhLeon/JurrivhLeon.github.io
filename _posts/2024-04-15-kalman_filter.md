# Kalman Filtering

Throughout our discussion, we suppose all related random variables fall in a Gaussian space, which is a closed subspace of $L^2$ containing only (centered) Gaussian random variables. We use notations $\langle X,Y\rangle = \mathbb{E}[XY]$ and $\Vert X\Vert^2=\mathbb{E}[X^2]$.

## One-dimensional Case
**Setting.** We consider the following dynamics, where $X_0$, $(\xi_ n)_ {n=0}^\infty$ and $(\eta_ n)_ {n=1}^\infty$ are independent Gaussian variables:
<p>$$\begin{aligned}&\textit{State:}\quad X_0=0,\ X_{n+1}=aX_n+\xi_n,\ \ \forall n\geq 0,\ \ \textit{where}\ \ \xi_n\sim N(0,\sigma^2)\ \ i.i.d.,\\
  &\textit{Observation:}\quad Y_{n} = cX_{n} + \eta_{n},\ \ \forall n\geq 1,\ \ \textit{where}\ \ \eta_n\sim N(0,\gamma^2)\ \ i.i.d..
  \end{aligned}$$</p>
  
Our goal is to find a recursive formula for computing these conditional expectations:

<p>$$\begin{aligned}
  M_n=\mathbb{E}[X_n|Y_1,\cdots,Y_n],\ \ \forall n\geq 0,\quad \textit{and}\quad \widehat{M}_n=\mathbb{E}[X_n|Y_1,\cdots,Y_{n-1}],\ \ \forall n\geq 1.
  \end{aligned}$$</p>

**Step I.** We have $\widehat{M}_{n+1}=aM_n$ for all $n\geq 0$:
<p>$$\begin{aligned} \mathbb{E}[aX_n+\xi_n|Y_1,\cdots,Y_n] = \mathbb{E}[aX_n+\xi_n|Y_1,\cdots,Y_n] = a\mathbb{E}[X_n|Y_1,\cdots,Y_{n-1},Y_n].\end{aligned}$$</p>

The second equality holds because $\xi_n\perp (Y_1,\cdots,Y_n)$.

**Step II.** Define $Z_n:=Y_n-c\widehat{M}_n$ for all $n\geq 1$. Then for all $1\leq k\leq n-1,$ we have
<p>$$\begin{aligned}\langle Y_k,Z_n\rangle = \mathbb{E}\left[\mathbb{E}\left[Y_k(cX_n+\eta_n-c\widehat{M}_n)|Y_1,\cdots,Y_{n-1}\right]\right]=\mathbb{E}\left[Y_k(c\widehat{M}_n-c\widehat{M}_n)\right]=0.\end{aligned}$$</p>

As a result, $Z_n$ is orthogonal to the Gaussian space spanned by $\lbrace Y_1,\cdots,Y_{n-1}\rbrace,$ and we have the following decomposition:

<p>$$\mathrm{span}\lbrace Y_1,\cdots,Y_n\rbrace = \mathrm{span}\lbrace Y_1,\cdots,Y_{n-1}\rbrace \oplus \mathrm{span}\lbrace Z_n\rbrace.$$</p>

According to the properties of projection in Hilbert spaces, we have
<p>$$\begin{aligned}\mathbb{E}[X_n|Y_1,\cdots,Y_n]=\mathbb{E}[X_n|Y_1,\cdots,Y_{n-1}]+\mathbb{E}[X_n|Z_n]\quad \Rightarrow\quad M_n=\widehat{M}_n + \frac{\langle X_n,Z_n\rangle}{\Vert Z_n\Vert^2}Z_n.\end{aligned}$$</p>

**Step III.** Define the projection error $P_n=\mathbb{E}[(X_n-\widehat{M}_n)^2] = \langle X_n, X_n-\widehat{M}_n\rangle$ for all $n\geq 1$. We can express $\langle X_n,Z_n\rangle$ and $\Vert Z_n\Vert^2$ in terms of $P_n$:
<p>$$\begin{aligned}\langle X_n,Z_n\rangle=\mathbb{E}\left[X_n\left(cX_n+\eta_n - c\widehat{M}_n\right)\right] = \mathbb{E}[X_n\eta_n] + c\mathbb{E}\left[X_n\left(X_n-\widehat{M}_n\right)\right]=cP_n,\\
  \Vert Z_n\Vert^2 = \mathbb{E}\left[\left(cX_n+\eta_n - c\widehat{M}_n\right)^2\right] = c^2\mathbb{E}\left[\left(X_n - \widehat{M}_n\right)^2\right]+\mathbb{E}[\eta_n^2] = c^2P_n+\gamma^2.\end{aligned}$$</p>

Plugging-in to the result we obtain in Steps I and II, we have
<p>$$\begin{aligned} M_n=\widehat{M}_n + \frac{cP_n}{c^2P_n+\gamma^2}Z_n,\ \ \forall n\geq 1,\quad\Rightarrow\quad \widehat{M}_{n+1}=a\left(\widehat{M}_n + \frac{cP_n}{c^2P_n+\gamma^2}Z_n\right).\end{aligned}$$</p>

**Step IV.** Now it remains to find the recursive formula for computing $P_n$. Clearly, we have $\widehat{M}_n=\mathbb{E}[X_1]=0$, and $P_1=\sigma^2$. For the case $n>1$, note that $X_n-M_n\perp M_n-\widehat{M}_n$. Then the projection theorem implies
<p>$$\begin{aligned} P_n &=\Vert X_n-\widehat{M}_n\Vert^2 = \Vert X_n-M_n\Vert^2 + \Vert M_n-\widehat{M}_n\Vert^2\\
  &= \frac{1}{a^2}\left\Vert X_{n+1}-\xi_n - \widehat{M}_{n+1}\right\Vert^2 + \frac{\langle X_n,Z_n\rangle^2}{\Vert Z_n\Vert^2}\\
  &=\frac{1}{a^2}\left(\left\Vert X_{n+1} - \widehat{M}_{n+1}\right\Vert^2 - \left\Vert\xi_n\right\Vert^2\right) + \frac{\langle X_n,Z_n\rangle^2}{\Vert Z_n\Vert^2}\\
  &= \frac{1}{a^2}\left(P_{n+1} - \sigma^2\right) + \frac{c^2P_n^2}{c^2P_n+\gamma^2}.\end{aligned}$$</p>

Hence we have
<p>$$\begin{aligned} P_{n+1} = \sigma^2 + a^2\frac{1}{(c^2/\gamma^2)+P_n^{-1}},\quad\forall n\geq 1.\end{aligned}$$</p>

Up to now, we have determined a recursive formula for computing $P_n$, $M_n$ and $\widehat{M}_n$.

## Multi-dimensional Case
**Setting.** We consider the following dynamics, where $x_0$, $(\xi_ n)_ {n=0}^\infty$ and $(\eta_ n)_ {n=1}^\infty$ are independent Gaussian vectors:
<p>$$\begin{aligned}&\textit{State:}\quad x_0\sim N(m_0,C_0),\ x_{n+1}=\Phi x_n+\xi_n,\ \ \forall n\geq 0,\ \ \textit{where}\ \ \xi_n\sim N(0,\Sigma)\ \ i.i.d.,\\
  &\textit{Observation:}\quad y_n = Hx_n + \eta_{n},\ \ \forall n\geq 1,\ \ \textit{where}\ \ \eta_n\sim N(0,\Gamma)\ \ i.i.d..
  \end{aligned}$$</p>
  
Here $(x_n)_ {n=0}^\infty$ are $p$-dimensional variables, $(y_n)_ {n=1}^\infty$ are $q$-dimensional variables, $\Phi\in\mathbb{R}^{p\times p}$, and $H\in\mathbb{R}^{q\times p}$. Without loss of generality, we suppose $m_0=\mathbf{0}_p$. Again, our goal is to find a recursive formula for computing these conditional expectations:

<p>$$\begin{aligned}
  m_n=\mathbb{E}[x_n|y_1,\cdots,y_n],\ \ \forall n\geq 0,\quad \textit{and}\quad \widehat{m}_n=\mathbb{E}[x_n|y_1,\cdots,y_{n-1}],\ \ \forall n\geq 1.
  \end{aligned}$$</p>

**Step I.** We have $\widehat{m}_{n+1}=\Phi m_n$ for all $n\geq 0$:
<p>$$\begin{aligned} \mathbb{E}[\Phi x_n+\xi_n|Y_1,\cdots,Y_n] = \mathbb{E}[\Phi x_n+\xi_n|y_1,\cdots,y_n] = \Phi\mathbb{E}[x_n|y_1,\cdots,y_n].\end{aligned}$$</p>

The second equality holds because $\xi_n\perp (y_1,\cdots,y_n)$.

**Step II.** Define $z_n:=y_n-H\widehat{m}_n$ for all $n\geq 1$. Then for all $1\leq k\leq n-1,$ we have
<p>$$\begin{aligned} \mathbb{E}[y_kz_n^\top] = \mathbb{E}\left[\mathbb{E}\left[y_k(\Phi x_n+\eta_n-\Phi\widehat{m}_n)^\top|y_1,\cdots,y_{n-1}\right]\right]=\Phi\mathbb{E}\left[y_k(\widehat{m}_n-\widehat{m}_n)\right]=0.\end{aligned}$$</p>

As a result, the space spanned by components of $z_n$ is orthogonal to the Gaussian space spanned by $\lbrace y_1,\cdots,y_{n-1}\rbrace,$ giving rise the following decomposition:

<p>$$\begin{aligned}&\mathrm{span}\lbrace y_1,\cdots,y_n\rbrace = \mathrm{span}\lbrace y_1,\cdots,y_{n-1}\rbrace \oplus \mathrm{span}\lbrace z_n\rbrace\\
  &\Rightarrow\quad \mathbb{E}[x_n|y_1,\cdots,y_n]=\mathbb{E}[x_n|y_1,\cdots,y_{n-1}]+\mathbb{E}[x_n|z_n].\end{aligned}$$</p>

Now we determine the conditional expectation $\mathbb{E}[x_n\vert z_n]=A^*z_n$, where:
<p>$$\begin{aligned} A^* &= \underset{A\in\mathbb{R}^{p\times q}}{\mathrm{argmin}}\ \mathbb{E}\left[\Vert x_n-Az_n\Vert^2\right]\\
  &=\underset{A\in\mathbb{R}^{p\times q}}{\mathrm{argmin}}\ \mathrm{tr}\left(A\,\mathbb{E}[z_nz_n^\top] A^\top - 2A\,\mathbb{E}[z_nx_n^\top]\right).\end{aligned}$$</p>

The maximand is a convex function of $A$, and we can let its derivative vanish to obtain the maximizer $A^*$. Thus we have
<p>$$ m_n = \widehat{m}_n + \mathbb{E}[x_nz_n^\top]\left(\mathbb{E}[z_n z_n^\top]\right)^{-1} z_n.$$</p>

**Step III.** We define the prediction error $P_n=\mathbb{E}[(x_n-\widehat{m}_n)(x_n-\widehat{m}_n)^\top]\in\mathbb{R}^{p\times p}$. Analogous to the Step III in one-dimensional case, we have
<p>$$\begin{aligned} \mathbb{E}[x_nz_n^\top]=P_nH^\top,\quad \mathbb{E}[z_nz_n^\top]= \Gamma + HP_nH^\top.\end{aligned}$$</p>

This gives the formulas to compute $m_n$ and $\widehat{m}_n$ respectively:
<p>$$\begin{aligned} m_n &= \widehat{m}_n + P_nH^\top\left(\Gamma + HP_nH^\top\right)^{-1}(y_n-Hm_n),\\
  &= \widehat{m}_n + (P_n^{-1}+H^\top\Gamma^{-1}H)^{-1}H^\top\Gamma^{-1}(y_n-Hm_n);\\
  \widehat{m}_{n+1}&=\Phi m_n.\end{aligned}$$</p>

where the last equality follows from Sherman-Morrison-Woodbury formula.

**Step IV.** It remains to find the recursion formula for $P_n$. For $n=1$, we have $P_1=\mathbb{E}[(\Phi x_0+\xi_0)(\Phi x_0+\xi_0)^\top]= \Sigma+\Phi C_0\Phi^\top$. Furthermore,
<p>$$\begin{aligned}P_{n+1}&=\mathbb{E}\left[\left(\Phi x_n + \xi_n - \Phi m_n\right)\left(\Phi x_n + \xi_n - \Phi m_n\right)^\top\right]\\
  &=\mathbb{E}[\xi_n\xi_n^\top]+\Phi\,\mathbb{E}\left[\left(x_n - m_n\right)\left(x_n - m_n\right)^\top\right]\Phi^\top\\
  &=\Sigma + \Phi^\top\,\mathbb{E}\left[\left(x_n - \widehat{m}_n\right)\left(x_n - \widehat{m}_n\right)^\top-\left(m_n - \widehat{m}_n\right)\left(m_n - \widehat{m}_n\right)^\top\right]\Phi\\
  &=\Sigma + \Phi^\top\left(P_n-\mathbb{E}[x_nz_n^\top]\left(\mathbb{E}[z_nz_n^\top]\right)^{-1}\mathbb{E}[z_nx_n^\top]\right)\Phi\\
  &=\Sigma + \Phi^\top\left(P_n-P_nH^\top(\Gamma+HP_nH^\top)^{-1}HP_n\right)\Phi\\
  &=\Sigma + \Phi^\top\left(P_n^{-1}+H^\top\Gamma^{-1}H\right)^{-1}\Phi.
  \end{aligned},$$</p>

where the last equality follows from Sherman-Morrison-Woodbury formula. To summarize, we have
<p>$$\begin{aligned}
  &P_1 = \Sigma + \Phi^\top C_0\Phi,\quad P_{n+1}=\Sigma + \Phi^\top\left(P_n^{-1}+H^\top\Gamma^{-1}H\right)^{-1}\Phi,\\
  &\widehat{m}_1 = 0,\quad m_n = \widehat{m}_n + P_nH^\top\left(\Gamma + HP_nH^\top\right)^{-1}(y_n-Hm_n),\quad \widehat{m}_{n+1}=\Phi m_n. \end{aligned}$$</p>

### Posterior Distribution

### A Sequential Optimization Viewpoint

