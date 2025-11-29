## Theorem: Negligible Function Definition
A function $f(\lambda)$ is negligible if:
$$
\forall p(\lambda) \in \text{poly}, \quad \lim_{\lambda \to \infty} p(\lambda)\cdot f(\lambda) = 0
$$

---

## Theorem: PRF Security (Game Definition)
Let $b \in \{0,1\}$ be random.

- If $b=0$, oracle is truly random
- If $b=1$, oracle is $F_k$

Adversary outputs $b'$.  
PRF security means:
$$
\left| \Pr[b'=b] - \tfrac12 \right| \le \text{negl}(\lambda)
$$

---

## Theorem: PRP–PRF Switching Lemma
If a permutation is chosen uniformly at random, it is computationally indistinguishable from a random function with small loss in advantage.

This allows us to analyze block ciphers as PRFs.

---

## Proof Idea: Why PRPs Look Random
Even though PRPs are deterministic:
- The key is unknown
- The mapping is unpredictable
- Any pattern would allow distinguishing