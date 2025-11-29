## Security Parameter
A numeric value $\lambda$ that controls the security level of a cryptographic system.

## Polynomial-Time Algorithm (PPT)
An algorithm whose running time is bounded by a polynomial in $\lambda$.

## Negligible Function
A function $f(\lambda)$ such that:
$$
\forall \text{polynomial } p(\lambda), \quad \lim_{\lambda \to \infty} p(\lambda) \cdot f(\lambda) = 0
$$

## Computational Indistinguishability
Two distributions $X$ and $Y$ are computationally indistinguishable if:
$$
\left| \Pr[\mathcal{A}(X)=1] - \Pr[\mathcal{A}(Y)=1] \right| \le \text{negl}(\lambda)
$$
for all PPT adversaries $\mathcal{A}$.

## Pseudorandom Function (PRF)

A deterministic function $F_k$ that is computationally indistinguishable from a truly random function.

## Pseudorandom Permutation (PRP)

A bijective, efficiently invertible PRF on a fixed domain.

## Advantage
The success gap of an adversary:
$$
\text{Adv}_{\mathcal{A}} = \left| \Pr[b'=b] - \tfrac12 \right|
$$