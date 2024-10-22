---
title: "Problems"
description: "A page of my unsolved research problems."
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  tex2jax: {
    inlineMath: [['$','$']],
    displayMath: [['$$','$$']]
  },
    "HTML-CSS": {
    scale: 80
  }
});
</script>

<script type="text/javascript"
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

<style>
    main{
        margin: 0;
        max-width: 100%;
    }

    .article-meta{
        display: none;
        border: none;
    }
</style>

This page is a collection of some of the unsolved problems during my research. Most of them are in a theoretical flavor but not necessarily a well-defined mathematical problem. Please feel free to conctact me if you have any ideas or thoughts about any of those problems.

## Problems

    
1. __The Role of Symmetric Transformation__

    _Related Paper: [Transformers from an Optimization Perspective](https://arxiv.org/abs/2205.13891)._

    In [this paper](https://arxiv.org/abs/2205.13891) we tried to explain an iterative process (e.g. forward process of an RNN) as the optimization process of an underlying optimization problem. For example, let $W \in \mathbb R^{n\times n}$ be a PSD matrix with a spectral radius $< 1$ and $\sigma = \max(\cdot, 0)$ be the ReLU activation, then the update \begin{align}x^{(t+1)} = \sigma\left(Wx^{(t)}\right)\end{align}
    can be viewed as a projected-gradient step of the following optimization objective \begin{align}
    x^* = \arg\min_{x \in \mathbb R_+^n} \frac 12\left\\|(I-W)x\right\\|^2
    \end{align} 
    with step size $ = 1$.

    Above we assumed $W$ is PSD, and it turns out that if $W$ is not PSD but only symmetric, we can still do the same thing by adopting some tricks. However, if $W$ is an asymmetric matrix (still, with spectral radius $< 1$), then it will be very hard to explain the update as an optimization step, despite that we know the process will still converge.

    This raises to two questions. The first question is technical: if there is a way to find an optimization obejctive, such that the above process with asymetric $W$ can be explained as an optimization step, or if there is a way to prove that this is not possible? The second question is rather philosophical: it seems there is an essential distinction between symmetric and asymmetric matrices, in terms of the process derived by them, and is there anything deep underlying property (other than commenly known ones like diagonalizability) that explains this distinction?  

    Notice that, this question has a trivial solution: the distance from current point to the convergent point (if the process converges). However, this solution is non-interpatable since we usually can not predict which point the model converges to. So one additional constraint should be that the energy function must be with an explicit explaination or at least easier to compute than the convergent point (I still have not figured out what should be the most suitable constraint, and that is another problem...).  

2. __The Stability of Matrix Spectral Approximation under Squaring__

    _Related Paper: [HERTA: A High-Efficiency and Rigorous Training Algorithm for Unfolded Graph Neural Networks](https://arxiv.org/abs/2403.18142), see Appendix D.2._

    For two PD matrices $A,B$, we say their **(spectral) approximation rate** is $$\phi(A,B) = \max\left\\{ \left\\|B^{-1/2}AB^{-1/2}\right\\|, \left\\|A^{-1/2}BA^{-1/2}\right\\| \right\\},$$
    and if $\phi(A,B) \leq 1+\epsilon$ we say $A$ is an $\epsilon$-approximator of $B$, denoted as $A \approx_{\epsilon} B$.

    it's not hard to prove that, in the worst case, if $A \approx_{\epsilon} B$ where $\epsilon < \frac 12$, then $A^2 \approx_{\epsilon O(\kappa(A))} B^2$, where $\kappa(A)$ is the condition number of $A$. This means, even if two matrices are very good spectral approximator of each other, the spectral approximation rate of their squared version will be much larger when they are ill-conditioned. 

    However, if we conduct experiments to verify this bound, we will not see the case: in most cases if two PD matrices are good spectral approximatiors of each other, then their square will also be very good spectral approximators. It turns out that only under a very rather extreme conidtion the upper bound will be tight. So the question is: under what condition this bound of the approximation rate of squared matrices will be tight?

    One hypothesis is that the tightness of this bound will be related to the matchness of the eigenspace of $A$ and $B$. In Appendix D.2 of [this paper](https://arxiv.org/abs/2403.18142), we studied a very special case: when $A$ has only one small eigenvalue and all of its other eigenvalues are large and equal (and so does $B$ because $A \approx B$). In this case we showed that the bound $\phi(A^2,B^2) \leq 1+\epsilon O(\kappa(A))$ will be tight only when the eigenvectors corresponding to the smallest eigenvalue of $A$ and $B$ are very aligned (i.e. the angle between them is small) but not the same.

    I restate the question here to conclude. The technical part is: can we find a condition, such that under this condition $A \approx_{\epsilon} B$ implies $A^2 \approx_{\epsilon\tilde O(1)} B^2$? The more foundamental part is: what is the essential property that controls the spectral approximation rate of $A^2$ and $B^2$, given $A \approx_{\epsilon} B$ for a small $\epsilon$?
