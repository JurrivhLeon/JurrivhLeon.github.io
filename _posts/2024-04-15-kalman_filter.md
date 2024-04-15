# Kalman Filtering
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
<p>$$\begin{aligned} P_{n+1} = \sigma^2 + a^2\frac{\gamma^2P_n}{c^2P_n+\gamma^2},\quad\forall n\geq 1.\end{aligned}$$</p>
