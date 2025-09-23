The attached article is a revised version of:

D. Fioravanti, D. Gregori and H.Shu, "Integrability, susy $SU(2)$ matter gauge theories and black holes" [arXiv:2208.14031 [hep-th]](https://arxiv.org/pdf/2208.14031)



# ThermodynamicBetheAnsatzSolve

Solve integral equations of Thermodynamic Bethe Ansatz type

### Documentation

#### Usage

<img width="246" height="19" alt="1fexgolzrtiy0" src="https://github.com/user-attachments/assets/f074be08-ffff-4e35-8279-04fad464e014" />

solves a TBA equation.

<img width="261" height="19" alt="1sjltph8xvsqu" src="https://github.com/user-attachments/assets/6d378e3b-4696-4a2a-8604-5c11bb98da27" />


solves a TBA and returns the value of the solution.

<img width="320" height="21" alt="0dspcpaupt2ma" src="https://github.com/user-attachments/assets/33711a2c-7b78-4479-89f8-2790e6e9fd8f" />


solves a system of TBA equations.

<img width="391" height="21" alt="0bgvr998muh8s" src="https://github.com/user-attachments/assets/a46d7930-64c2-476b-8834-ea52c128680a" />


solves a system of TBA equations and returns the specified solutions values.

#### Details & Options

The Thermodynamic Bethe Ansatz (TBA) is a type of nonlinear integral equation studied extensively, in recent decades, in various branches of theoretical physics (especially in statistical field theory, quantum integrability and gauge theories).

Mathematically it has the following general form:

<img width="424" height="39" alt="1c1h7fjcz5rhw" src="https://github.com/user-attachments/assets/1b7f294b-1846-493f-83d3-40cc90e8ce8b" />

where $y_j(x)$, for $j=1,...,n$ are the dependent variables, $f_j(x)$ the non-homogeneous (or forcing) terms, $\varphi_{jk}(x)$ the integral kernels (typically combinations of hyperbolic functions), $c_{jk}$ some constants and $\sigma_{jk}=\pm 1$. Notice that the integral terms are always convolutions.

Due to its quite involved form, the TBA cannot be solved analytically or symbolically through [DSolve](https://reference.wolfram.com/language/ref/DSolve), but requires a special numerical treatment.

In particular, `ThermodynamicBetheAnsatzSolve` implements the method of successive approximations. In details, the solution is set first equal to the forcing terms, and then iterated in the equation until convergence is reached. Moreover, the convolutions are [Fourier](https://reference.wolfram.com/language/ref/Fourier)-transformed, then evaluated efficiently as products and transformed back through [InverseFourier](https://reference.wolfram.com/language/ref/InverseFourier). To do this, the domain is discretized on a uniform finite grid and finally the solution is returned as an [InterpolatingFunction](https://reference.wolfram.com/language/ref/InterpolatingFunction) object.

The first argument of `ThermodynamicBetheAnsatzSolve` is a TBA equation or a list of them. Then the solution is returned as a list of [Rule](https://reference.wolfram.com/language/ref/Rule) expressions with an interpolating function solution for each dependent variable, in analogy with the output of [NDSolve](https://reference.wolfram.com/language/ref/NDSolve). The second argument, possibly empty, allows to return only the values of the solutions, in analogy with [NDSolveValue](https://reference.wolfram.com/language/ref/NDSolveValue).

`ThermodynamicBetheAnsatzSolve` accepts the following options:

<img width="662" height="182" alt="1osrbzzgsz983" src="https://github.com/user-attachments/assets/0a65ffd9-4993-4910-aaff-a2588489b6ee" />


In principle, the numeric grid to obtain the solution, should have infinite cutoff (or range) and resolution. It is possible to simulate this ideal behaviour by giving increasingly values to the options "GridCutoff" and "GridResolution".

The iterations of the mathematical method of successive approximations are also in principle infinite and on the computer require a stopping rule. This can be simply a maximum limit on the iterations, as set by the option [MaxIterations](https://reference.wolfram.com/language/ref/MaxIterations). Alternatively, they can be stopped even at a lower value, if the desired "StoppingAccuracy" error threshold is reached. This is defined, heuristically, as the maximum [Norm](https://reference.wolfram.com/language/ref/Norm) difference, along all the grid, between the solution at a given iteration and the solution at the previous iteration.

The option "MonitorIterations" allows to [Monitor](https://reference.wolfram.com/language/ref/Monitor) the number of numeric iterations in real time and to then [Echo](https://reference.wolfram.com/language/ref/Echo) the total number of iterations used.

By default, each numeric iteration is stored in the memory. This is not really necessary as to compute the next approximate solution only a single previous iteration is strictly required. So in some cases it may be desirable to use the option "SaveMemory" to use a special [DataStructure](https://reference.wolfram.com/language/ref/DataStructure)["LeastRecentlyUsedCache"] to maintain inside the cache only the last iteration. This is not set as the default behaviour because it is found to lead also to a slightly lower speed.

For some special TBAs, further boundary conditions, besides the forcing terms, are required, either for specifying some further parameters or to improve accuracy. These boundary conditions are mathematically derived as terms to subtract to the forcing term and to the convolution integrand. They can be specified through the option "BoundaryCondition" as a list of two elements, or as a list of lists, if the TBA has multiple equations with different boundary conditions.

An "ansatz" is roughly translated as "educated guess." Alternates to the "thermodynamic" Bethe ansatz include algebraic, analytic, coordinate, functional and nested.


#### Basic Examples

Solve a single non-linear integral equation, known as the "Sinh-Gordon" Thermodynamic Bethe Ansatz (TBA):

![15l6ro1jorw2m](img/15l6ro1jorw2m.png)

<img width="422" height="36" alt="15l6ro1jorw2m" src="https://github.com/user-attachments/assets/a24a13f5-072a-498b-a61c-e789ec69fcb9" />


```wl
ThermodynamicBetheAnsatzSolve[tbaSHG /. r -> 0.1]
```
<img width="414" height="42" alt="019oiigz8xwmn" src="https://github.com/user-attachments/assets/dde183c6-9149-4f91-8c94-f5ba14d9a882" />

![019oiigz8xwmn](img/019oiigz8xwmn.png)

Plot the solution:

```wl
In[]:= Plot[y[x] /. %, {x, -5, 5}]
```

![1g5zdjy2uq6wi](img/1g5zdjy2uq6wi.png)

<img width="130" height="81" alt="1g5zdjy2uq6wi" src="https://github.com/user-attachments/assets/aedd8cf4-e0d4-4ba5-b7f1-2a1c5cdd8318" />




#### Full documentation

Read the rest of the documentation on the [Wolfram Function Repostiroy](https://resources.wolframcloud.com/FunctionRepository/resources/ThermodynamicBetheAnsatzSolve/).
