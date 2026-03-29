# Reviewer FaHb

Thank you for taking the time to read and evaluate our paper. Here are our responses to your well-thought questions:

> **Q1**: `Can you provide a concrete example of a function class that is upper-linearizable but not (weakly) DR-submodular? If not, what is the practical benefit of the linearizability abstraction over working directly with DR-submodular functions?'

We can indeed provide a concrete example. $\alpha$-approximately convex functions, investigated by papers such as [1] are the negative of upper-linearizable functions, which demonstrates that this abstraction encompasses problems entirely outside the submodular domain. One example is the $l_2$-regularized phase retrieval problem. The standard objective for estimating an unknown signed vector from absolute values of linear measurements is $f(x) = \frac{1}{2} ||y - |\Phi x|||^2 + \frac{\lambda}{2}||x||^2$. This is a classic non-convex optimization problem in signal processing, and it is not a DR-submodular function. It has been explicitly proven that this objective is $(1+1/\gamma)$-approximately convex (where $\gamma = \lambda / ||\Phi||_{2\to 2}^2$). Consequently, its maximization counterpart is strictly upper-linearizable. Therefore, linearizability is not merely co-extensive with DR-submodularity. 

The practical benefit of this abstraction is that it acts as a universal bridge. 
Working directly with DR-submodular functions in online settings historically required designing custom variants of algorithms and proving complex regret bounds from scratch for every combination of constraint set geometry and feedback model.
The linearizability abstraction decouples the non-convexity of objective functions from the geometry of the constraint set, seamlessly reducing both to standard Online Linear Optimization (OLO). Instead of a `notational generalization', it is what allows us to swap in different base linear learners (like SO-OGA or Improved Ader) to instantly unlock new dynamic and adaptive regret bounds across different feedback settings.

[1] Sarkar, Dhruv, Samrat Mukhopadhyay, and Abhishek Sinha. "Online Learning for Approximately-Convex Functions with Long-term Adversarial Constraints." arXiv preprint arXiv:2508.16992 (2025).

> **Q2** `Of the eight entries in your results comparison table, which require proof work beyond plugging Theorem 1 into the existing OMBQ framework and its reduction lemmas? Please be specific about which results involve non-trivial new analysis vs. direct application of prior machinery.'

We appreciate the question on the clarity of our contribution. To answer directly: none of the eight entries require fundamentally new proof work beyond Theorem 1 and the BQND estimator. The eight bounds are direct corollaries of plugging our $1/e$-linearizability proof (Theorem 1) and our Jacobian-corrected gradient estimator (Algorithm 1) into the existing OMBQ framework and its reduction lemmas. However, we do not view this as a weakness of the paper, but rather as a testament to the strength of Theorem 1. The prior inability to achieve these bounds was blocked by a theoretical bottleneck: the lack of a suitable surrogate potential for the non-monotone, down-closed regime that admitted an unbiased, single-query estimator. Our non-trivial new analysis is entirely concentrated in Theorem 1 and Lemma 1. By breaking this bottleneck, the eight bounds follow naturally. We can revise the introduction and the contributions list to be more clear, highlighting Theorem 1 as the primary technical contribution and the table as the resulting applications.

> **Q3** `Can you clarify the relationship between Lemma 1 and Lemma 4.1 of Buchbinder & Feldman (2024)? To what extent is Lemma 1 a special case vs. a genuinely new result?'

To answer directly, Lemma 1 is not a ground-breaking novel inequality. Mathematically speaking, it is a specialized variant of Lemma 4.1 from Buchbinder & Feldman (2024). However, Lemma 4.1 provides a highly general, infinite-sum lower bound for time-varying paths, which is not at all directly applicable to the adversarial online  setting that we investigate.
In Lemma 1, we specialize this to the adversarial online setting by assuming a constant path ($x(\tau) = x$), which allows us to drop the infinite sum of non-negative terms. This yields the strictly weaker but much cleaner lower bound: $f(1-a \odot e^{-zx}) \ge e^{-z} f(1-a)$. We then apply the down-closed property of the constraint set to arrive at the final form.

The novelty of Lemma 1 lies not in the derivation of the inequality itself, but in the realization that this specific, weakened formulation is the exact missing structural puzzle piece required to make the integration-by-parts exact cancellation work in our surrogate potential $F(x)$ in Theorem 1. We will add a paragraph to Appendix B to explicitly describe the relationship between Lemma 1 and Lemma 4.1 to give credit.

# Reviewer CLzi

We sincerely thank the reviewer for the careful reading of our manuscript and for the constructive comments and questions. Below, we address each questions in detail.


> **Q1**: `Can you explain what you consider to be novel in this work and what is based on prior work?'

Appreciate the opportunity to clarify the exact boundaries of our contribution.

Based on Prior Work: We explicitly rely on the OMBQ meta-algorithm, the feedback reduction lemmas established by Pedramfar & Aggarwal (2024) and others. We also make use of on the hihgly general inequality from Buchbinder & Feldman (2024), which we derive a specialized variant in Lemma 1.

Novel Contributions: Our core novelty is twofold: (1) Theorem 1, which establishes that non-monotone DR-submodular functions over down-closed sets are upper-linearizable with $1/e$-approximation, improving upon the previous $1/4$-approximation results in this regime and openning doorways to utilize the OMBQ framework. (2) The BQND estimator (Algorithm 1), which leverages the exponential reparameterization to construct an unbiased, single-query linear surrogate.

While the algorithms in Section 5 utilize prior meta-frameworks, they could not exist without Theorem 1 and BQND.

> **Q2**: `Is there any intuition behind the definitions of $h(x)$ and $F(x)$? Similar forms appear in prior works, but it is not clear what the intuition is, apart from the fact that the analysis works.'

The mathematical elegance of the cancellation can obscure the intuition some time. The reparameterization $h(x) = 1 - e^{-x}$ represents the continuous, deterministic analog of independent randomized rounding (often seen in the multilinear extension of submodular set functions). In a continuous greedy process, a particle's trajectory naturally follows this exponential curve $1 - e^{-zt}$ to ensure it stays within the bounds of the hypercube while accumulating value. The surrogate potential $F(x)$ acts as the integrating factor for this trajectory. The specific density $p(z) \propto e^{z-1}$ is intentionally designed to heavily weight the early stages of the continuous greedy path and exponentially decay the later stages. In the non-monotone setting, ascending the gradient too aggressively later in the trajectory actively destroys value. The density $p(z)$ perfectly offsets this non-monotone penalty, allowing integration by parts to exactly cancel the negative gradient components, leaving only the certified $1/e$ lower bound.

> **Q3**: `Can you specify whether there exist any known lower bounds for each of the settings you consider? Maybe you can add those bounds Table 1.'

Yes, the regret rates we achieve are optimal with respect to $T$. Because our framework successfully reduces the problem to Online Linear Optimization (OLO), we inherit the information-theoretic lower bounds of standard online convex optimization. For first-order full information, the $\mathcal{O}(T^{1/2})$ static and adaptive regret bounds match the $\Omega(T^{1/2})$ lower bound for adversarial OCO (hazan, 2016). For dynamic regret, the $\tilde{\mathcal{O}}(\sqrt{T(1+P_T)})$ bound matches the established lower bounds for tracking moving targets in non-stationary environments.Similarly, the $\mathcal{O}(T^{2/3})$ rate for semi-bandit and $\mathcal{O}(T^{4/5})$ for bandit feedback match the known projection-free lower bounds in those respective limited-information regimes. We will gladly add a column or a footnote to Table 1 detailing these fundamental lower bounds to highlight the tightness of our regret rates.


[1] Hazan, Elad. "Introduction to online convex optimization." Foundations and Trends in Optimization 2, no. 3-4 (2016): 157-325.


> **Q4**: `My understanding is the factor $1/e$ is not the theoretically optimal factor. Is this correct? Is there a concrete obstacle to improving this factor?'

The absolute NP-hard upper bound for maximizing non-monotone submodular functions is $0.478$ (Gharan & Vondrák, 2011; Qi, 2024). Recently, offline algorithms have achieved a $0.401$ approximation (Buchbinder & Feldman, 2024). However, in the online setting, $1/e \approx 0.367$ remains the state-of-the-art optimal approximation ratio (Thang & Srivastav, 2021; Zhang et al., 2023). The concrete obstacle to breaking the $1/e$ barrier online is the adversarial nature of the problem combined with the down-closed constraint. To achieve $0.401$, offline algorithms require looking at the entire function landscape to make non-greedy, globally aware corrective steps. In the online setting, the adversary can force the algorithm to commit to a seemingly optimal path early on, revealing the non-monotone penalty only after the algorithm has consumed its constraint budget. Without the ability to "time-travel" or query future functions, $1/e$ is the current limit for online methods for this regime.

> **Q5** `It seems that once the problem is reduced to online linear/convex optimization, one may use standard algorithms from the OCO literature to extend the results to adaptive and dynamic regret. Do prior works reduce the problem to OCO? If so, is there a particular reason they cannot be directly extended to handle adaptive and dynamic regret?'

This question actually speaks to the core motivation of our paper. Most prior works achieving $1/e$ (e.g., Thang & Srivastav, 2021; Zhang et al., 2023) do not reduce the problem to standard OCO. Instead, they use bespoke algorithms, typically custom variants of the continuous greedy algorithm tailored specifically to the DR-submodular geometry. Because their update rules are heavily coupled with the non-convexity of the objective, you cannot simply "plug in" a standard adaptive or dynamic OCO algorithm. Pedramfar & Aggarwal (2024) introduced the linearizability framework specifically to enable this OCO reduction. However, their generic formulation only yielded a suboptimal $1/4$-approximation for the non-monotone, down-closed case. Our work bridges this exact gap. By proving that this specific class is $1/e$-linearizable (Theorem 1), we are the first to successfully preserve the optimal (in the sense for online setting) $1/e$ approximation ratio while executing a clean reduction to OCO. This is why we are the first to unlock the adaptive and dynamic regret literature for this specific domain.








# Reviewer iHce





We thank the reviewer for the careful reading, positive assessment, and helpful suggestions.
Below, we address the reviewer's questions.

> **Q1**: `Can you elaborate a little further on whether the semi-bandit, bandit and zeroth-order regret rates are tight?'



Yes, our regret exponents are tight with respect to $T$. 
For first-order full-information feedback, the $\mathcal{O}(T^{1/2})$ static and adaptive regret bounds match the optimal $\Omega(T^{1/2})$ lower bound known for adversarial OCO (hazan, 2016). 
Likewise, our dynamic regret bound $\tilde{\mathcal{O}}\!\left(\sqrt{T(1+P_T)}\right)$ matches the standard non-stationary benchmark up to logarithmic factors. For semi-bandit, zeroth-order, and bandit feedback, the correct interpretation is that our bounds match the best-known single-query projection-free exponents under the standard feedback-conversion framework, namely $\mathcal{O}(T^{2/3})$, $\mathcal{O}(T^{3/4})$, and $\mathcal{O}(T^{4/5})$, respectively. 
We will clarify this point in the revision, for example by adding a short remark or an extra comparison column in Table~1.


> **Q2**: `If so, is there a known hard instance? I was thinking of the pricing lower bound by Kleinberg and Leighton (FOCS 2003), but I am not entirely sure if this applies.'




For first-order full-information feedback, our $\mathcal{O}(T^{1/2})$ regret rate is tight with respect to $T$, since it matches the standard $\Omega(T^{1/2})$ lower bound from adversarial online convex optimization. For the limited-feedback settings considered in this paper semi-bandit, zeroth-order full-information, and bandit feedback. Our bounds should instead be viewed as the best known rates for this exact DR-submodular setting. To the best of our knowledge, there is no known matching minimax lower bound, nor a corresponding lower-bound construction or hard instance, for setting we consider. Therefore, we do not claim that these exponents are provably unimprovable; rather, our contribution is to improve the state of the art for this problem class.


Regarding the paper of Kleinberg and Leighton (FOCS 2003), we believe it is suggestive but not directly applicable here. Their model is an online posted-price auction problem: at each round the learner posts a scalar price, observes only a binary purchase/no-purchase outcome, and regret is measured relative to the best fixed price in hindsight. In the worst-case valuation model, they prove an $\mathcal{O}\!\left(n^{2/3} (\log n)^{1/3}\right)$ upper bound and an $\Omega(n^{2/3})$-type lower bound (equivalently, they show that no strategy can achieve $o(n^{2/3})$ regret). However, this is a one-dimensional pricing/partial-monitoring problem, not an oracle-based online DR-submodular maximization problem over a down-closed convex set. Therefore, while it provides useful intuition that limited one-point feedback can fundamentally worsen the regret rate, it does not directly yield a lower bound for our semi-bandit, zeroth-order, or bandit DR-submodular setting.



> **Q3** `Regarding non-monotonicity in the analysis, is it true that if the functions were monotone, then one would get a better $\alpha$ in the $\alpha$-regret, maybe $(1-\frac{1}{e})$?'

Yes. If the functions were monotone, then one would obtain a strictly better approximation factor in the $\alpha$-regret benchmark with $\alpha=1-\frac{1}{e}$ (Pedramfar & Aggarwal, 2024). In particular, prior linearizable results for monotone DR-submodular functions over convex sets containing the origin yield $\alpha = 1-\frac{1}{e}$, which is strictly better than the $1/e$ factor in our non-monotone setting. Our paper focuses on the non-monotone regime precisely because this stronger monotonicity-based benchmark is no longer available there; the main novelty is to establish $1/e$-linearizability, together with single-query regret guarantees, for the harder non-monotone down-closed setting.




[1] Pedramfar, Mohammad, and Vaneet Aggarwal. "From linear to linearizable optimization: A novel framework with applications to stationary and non-stationary dr-submodular optimization." Advances in Neural Information Processing Systems 37 (2024): 37626-37664.
