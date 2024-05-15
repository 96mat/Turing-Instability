# Turing-Instability
Advection-Diffusion Eq

$$\begin{cases} u_t-\varDelta u=f(u,v)\\ 
              v_t -d\varDelta v =g(u,v)\\ 
              u(x,y,z)=u_0(x,y)\\ 
              v(x,y,z)=v_0(x,y)\\ 
              +\text{Periodic BC} 
\end{cases}$$

where the forcing terms are:

$$\begin{align}
              f(u,v)&=u-\alpha v +\gamma u v - u^3\\ 
              g(u,v)&= u-\beta v
\end{align}$$

and
$u_0(x,y),v_0(x,y)$ are random numbers. $\beta, \thinspace d, \thinspace \alpha,\thinspace \gamma$ are scalar constants
