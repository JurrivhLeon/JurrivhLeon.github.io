# Kalman Filtering
## One-dimensional Case
**Setting.** We consider the following dynamics:
<p>$$\begin{aligned}\textit{State:}\quad &X_0\sim N(0,\nu^2),\ X_{n+1}=aX_n+\xi_n,\ \ \forall n\geq 0,\ \ \textit{where}\ \ \xi_n\sim N(0,\sigma^2)\ \ i.i.d.,\\
  \textit{Observation:}\quad &Y_{n} = cX_{n} + \eta_{n},\ \ \forall n\geq 1,\ \ \textit{where}\ \ \eta_n\sim N(0,\gamma^2)\ \ i.i.d..
  \end{aligned}$$</p>
  
Our goal is to find a recursive formula for computing these conditional expectations:

<p>$$\begin{aligned}
  M_n=\mathbb{E}[X_n|Y_1,\cdots,Y_n],\ \ \forall n\geq 0,\quad \textit{and}\quad \widehat{M}_n=\mathbb{E}[X_n|Y_1,\cdots,Y_{n-1}],\ \ \forall n\geq 1.
  \end{aligned}$$</p>

(i) We have $\widehat{M}_{n+1}=aM_n$ for all $n\geq 0$:
<p>$$\begin{aligned} \mathbb{E}[aX_n+\xi_n|Y_1,\cdots,Y_{n-1},Y_n] = \mathbb{E}[aX_n+\xi_n|Y_1,\cdots,Y_{n-1},Y_n] = a\mathbb{E}[X_n|Y_1,\cdots,Y_{n-1},Y_n].\end{aligned}$$</p>
The second equality holds because $\xi_n\perp (Y_1,\cdots,Y_n)$.

(ii) 
