# Turing Instability
la repo è costituita da due script principalmente:
+ Il codice ```MixedWF_CH.ipynb``` e ```MixedWF_CH_v1.ipynb``` in cui viene approcciato il problema non lineare $(1)$, con risultati non adatti
+ Il codice  ```TN.ipynb``` in cui viene approcciato un problema simile ma omogeneo $(2)$, i risultati sembrano ok
+ Plot delle concentrazioni $u,v$ in ```FreeFEM```
## MixedWF_CH.ipynb, MixedWF_CH_v1.ipynb
Advection-Diffusion Eq

$$\begin{cases} u_t-\varDelta u=f(u,v)\\ 
              v_t -d\varDelta v =g(u,v) \qquad(1)\\ 
              u(x,y,z)=u_0(x,y)\\ 
              v(x,y,z)=v_0(x,y)\\ 
              + \text{Neuman BC}\\
              +\text{Periodic BC} 
\end{cases}$$

dove i termini non lineari sono:

$$\begin{align}
              f(u,v)&=u-\alpha v +\gamma u v - u^3\\ 
              g(u,v)&= u-\beta v
\end{align}$$

and
$u_0(x,y),v_0(x,y)$ sono numeri randomici. $\beta, \thinspace d, \thinspace \alpha,\thinspace \gamma$ sono costanti, il risultato:

<p align="center" width="100%">
    <img width="33%" src="media/image_.png">
</p>

[Ulteriormente](media/Current_Solution.mp4)

Idea per l'implementazione delle funzioni ```ff, gg```  in modo <i>simbolico</i> da poter usare nella formulazione debole:

```markdown
#Define the forcing terms
u_n1 = variable(u_n1)
u_n2 = variable(u_n2)
ff   = u_n1-alpha*u_n2+gamma*u_n1*u_n2-u_n1**3
gg   = u_n1-beta*u_n2
```
in alternativa all'interno della formulazione debole, si accedono alle componenti di 
```
u_n=Function(V)
u_n1, u_n2 =split(u_n)
``` 
usando ```u_n[0], u_n[1]``` e scrivendo direttamente $f(u,v),\thinspace g(u,v)$ nella formulazione debole:
```
F =  (u_1*v_1/dt)*dx \
    + (u_2*v_2/dt)*dx \
    + inner(grad(u_1), grad(v_1))*dx \
    + d*inner(grad(u_2), grad(v_2))*dx \
    -(dot(u_n1,v_1)/dt + dot(u_n2,v_2)/dt)*dx \
    -(dot(u_n[0]-alpha*u_n[1]+gamma*u_n[0]*u_n[1]-u_n[0]**3,v_1) + dot(u_n[0]-beta*u_n[1],v_2))*dx 
```
## TN.ipynb
$$\begin{cases}
\frac{\partial X}{\partial t}=a(X-h)+b(Y-k)+\mu \nabla^2 X \qquad (2)\\\\
\frac{\partial X}{\partial t}=c(X-h)+d(Y-k)+\nu \nabla^2 Y\\
+\text{Periodic BC}
\end{cases}$$

la cui Forma debole ([pagina 9](https://www.diva-portal.org/smash/get/diva2:1780187/FULLTEXT01.pdf))
```math
\begin{align}
\int_{\Omega} (X^{n+1}-X^n)v+\mu \Delta t \nabla X^{n+1}\nabla v- \Delta t(aX^{n+1}+bY^{n+1})v\thinspace d\Omega &=0\\
\int_{\Omega} (Y^{n+1}-Y^n)v+\nu \Delta t \nabla Y^{n+1}\nabla v- \Delta t(cX^{n+1}+dY^{n+1})v\thinspace d\Omega &=0
\end{align}
```

la soluzione che ho ottenuto:
<p align="center" width="100%">
    <img width="33%" src="media/TN.png">
</p>

[Ulteriormente](media/TN.mp4)

## FreeFEM
plot ottenuto attraverso il codice FreeFEM

<p align="center" width="100%">
    <img width="33%" src="media/FreeFEM.png">
</p>

[Ulteriormente](media/FreeFEM_solu.mp4)
