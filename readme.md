# Motivation

Inspired by [Poisson Flow Generative Models](https://arxiv.org/abs/2209.11178), we propose a new "energy-based" model. PFGM proposes to use Poisson Flow. Unlike score, where its computation is quadratic which forces us to use sliced score matching, the electric fields can be computed easily. 

Instead of learning (direction of) electric fields, we propose to learn potential. Since we can derive electric field by computing gradient, 

Pros : 
1. The batched estimator of PFGM is biased, while we don't need to use biased estimator

Cons :
1. The computation cost is roughly doubled due to backward pass.

# Details
## Learning Target
Directly learning $V$ can be problematic due to vanishing signal at $\infty$. We propose to solve this problem by learning $$r^{d+1}(V-V_\text{guassian disk})$$. 

### Multipole expansion motivation
We know the first term in the multipole expansion - total charge is $1$. Therefore we need to learn from the second term - dipole moment. We multiply by $r^{d+1}$ to make it easier. Also, we replace $V_\text{point charge}$ with $V_\text{guassian disk}$ to improve convergence near $z=0$ and to match data closer.

This can be further improved by enforcing better regularity. One idea is to use VAE as preprocessor as in [LSGM](https://arxiv.org/abs/2106.05931), making the data much more similar to gaussian distribution. 

## Network Structure
Surprisingly, or unsurprisingly, gradient signal from a NN isn't good enough in some cases. Therefore, we propose to utilize lessons from Implicit Neural Representations. 

# Appendix : Difference from Energy Models

Even though it learns energy, our model has shares almost no similarity with energy models. Instead, it is much more simliar to score-based models. 

## Learning
As there's one auxillary dimension in PFGM and our proposal, we essential learn smoothed version of energy at the same time, similar to score-based models.

## Sampling 
Similar to, and even simpler than, to score based models, we sample just by following steepest gradient, It is guaranteed to converge to original distribution in the ideal case, due to the auxillary dimension. (See PFGM paper)

# To explore
1. Actually testing it
2. Replacing $U(1)$ with other groups - this simply started as a joke, but [tangent bundle looks possible](https://arxiv.org/abs/2112.07068)