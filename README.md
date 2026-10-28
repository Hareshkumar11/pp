# Reviewer PdiG (<1500)

We thank the reviewer for the helpful feedback. We will clarify all these revised version.

**W1:** Building on [1,2], we provide a unified delayed $\alpha$-regret transfer for upper-linearizable objectives and dynamic restricted-feedback extensions. The SFTT transfer in [1] does not cover dynamic regret. Our Theorem 4.1 and Appendix H handle moving comparators through an $O(LP_T)$ within-block variation cost and induced block delays. For more details please check Weakness 1 of Reviewer L6XS.

**W2–W3:** We assume nonempty compact convex $\mathcal K$, class-specific upper-linearization, Lipschitz rewards, and reward obliviousness for randomized extensions (to be added to Corollary H.3).

- SFTT uses at most $K$ non-adaptive queries per block, with $K$ independent of $T$, $L>K$, and compatibility/boundedness in Eq. (75). Each feasible query point is played at its exploration round.
- STB requires bounded rewards and Eqs. (13)–(14); non-direct classes also require compatible surrogates and action/query maps into $\mathcal K_\rho$.
- FOTZO requires bounded values for one-point estimates; two-point estimates use Lipschitzness and two values of the same $f_t$.

STB/FOTZO require compatible stochastic base learners. For query centers $z\in\mathcal K_\rho$ and $v\in\mathbb S_0$, Eq. (11) ensures $z\pm\rho v\in\mathcal K$. STB observes only the played reward; FOTZO permits separate queries. Thus, feasibility and feedback restrictions are respected.





# Reviewer RfxV (<1400)

We thank the reviewer for helpful suggestions.

**Response to weakness 1:** Writing such details will improve the clarity of the results and the applicability of the algorithms. We will add a remark with the following sufficient conditions for the dynamic-regret bounds in Tables 2–4 to be $o(T)$, with other parameters fixed. Require the first two conditions for in-order arrivals and all three jointly for general arrivals.

| Feedback | $P_T$ | $d_{\mathrm{tot}}(1+P_T)$ | $d_{\max}P_T$ |
| --- | --- | --- | --- |
| First-order full-information | $o(T)$ | $o(T^2)$ | $o(T)$ |
| Semi-bandit: direct | $o(T)$ | $o(T^2)$ | $o(T)$ |
| Semi-bandit: non-direct | $o(T^{2/3})$ | $o(T^2)$ | $o(T)$ |
| Bandit: direct | $o(T^{1/2})$ | $o(T^{3/2})$ | $o(T^{1/2})$ |
| Bandit: non-direct | $o(T^{2/5})$ | $o(T^{8/5})$ | $o(T^{3/5})$ |
| Zeroth-order full-information: one-point | $o(T^{1/2})$ | $o(T^{3/2})$ | $o(T^{1/2})$ |
| Zeroth-order full-information: two-point | $o(T)$ | $o(T^2)$ | $o(T)$ |

**Response to weakness 2:** Expectation and maximization cannot generally be interchanged. For dynamic regret, the comparator sequence is fixed, so no interchange is needed. For static regret under oblivious rewards, the maximizing comparator is independent of the learner's randomness, allowing the fixed-comparator analysis to apply. We will make these justifications explicit.



# Reviewer L6XS (=1500)


Thank you for your feedback on our manuscript.

**Weakness 1:** Algorithm 1 builds on SOLID's scheduling and Pedramfar and Aggarwal's (2024) upper-linearization. Our contribution is a unified delayed $\alpha$-regret transfer for structured non-concave objectives and dynamic restricted-feedback extensions.

The main technical step is delayed, dynamic $\mathtt{SFTT}$. Their Theorem 7 treats constant comparators and does not transfer dynamic regret see the discussion before Corollary 8. Our extension must control within-block comparator variation and a different delay process: each block's feedback becomes available only after all randomly assigned query observations arrive.

Theorem 4.1 and Appendix H bound the cost of comparator changes within blocks by $O(LP_T)$ and their variation across blocks by $P_T$. Lemma H.2 shows that the expected total delay seen by the learner operating on blocks is at most $Q+K d_{\mathrm{tot}}/L^2$, provided delays are independent of query randomization. These bounds give the dynamic-regret guarantee in Corollary H.4 and are also used in the non-direct bandit analysis in Appendix K. We will clarify in the introduction how these arguments extend prior work.

**Weakness 2:** The paper focuses on proving worst-case static and dynamic $\alpha$-regret guarantees for a unified delayed reduction; we leave experimental evaluation to future work. We will define the rate symbols and expressions from Tables 2–4 in the main text and shorten the captions.



# Reviewer sLoD (=1500)

We thank the reviewer for the positive assessment and helpful feedback.

**Weakness 1:** Please see our response to Weakness 1 of Reviewer PD1o for a possible adaptive extensions.

**Weakness 2:** Please see our response to Weakness 3 of Reviewer PD1o for the clarification in revisions.

**Weakness 3:** We already defined that notation in Section 4, before the Theorem 4.1.

**Weakness 4:** We will correct these in the revised manuscript.

**Weakness 5:** Assumption 2.5 bounds base regret but does not bound $\Delta_{\mathcal A}$. Therefore Theorem 3.2 retains $\Delta_{\mathcal A}$ explicitly. For particular base learner OGA, Appendix E proves $\Delta_{OGA}\le\eta Gd_{\mathrm{tot}}$, yielding Theorem 3.4. We will clarify this distinction after Theorem 3.2.

**Weakness 6:** We will define the notation at the start of Section 5. The inner-product identity holds because the relative gradient is the orthogonal projection of the ambient gradient onto $\mathcal L_0$ and $u-z\in\mathcal L_0$. We will state this justification explicitly.

**Weakness 7:** We will add this assumption in Corollary H.3 and clarify its scope in the relevant results.

**Specific Point 2:** We thank the reviewer for pointing this. The cited phase-retrieval result concerns approximate convexity for cost minimization; negating the cost does not yield the $\alpha_P<1$ upper-linearization certificate claimed in our Appendix B. We will therefore remove FC 7 from the application tables and the corresponding claims.




# Reviewer PD1o (<1500)

We thank the reviewer for constructive suggestions.

**Weakness 1:**
We agree that our optimized dynamic-regret bounds require knowledge of $P_T$, and certain static and dynamic bounds also require $d_{\mathrm{tot}}$. We will state these requirements explicitly in the theorem and tables. Removing knowledge of $P_T$ requires additional analysis of prediction drift or delayed expert aggregation. A delay-aware doubling scheme may address unknown $d_{\mathrm{tot}}$. We will discuss these adaptive extensions in the revised manuscript.

**Weakness 2:**  We agree that upper-linearizability absorbs the non-identity map in the initial reduction. Our reduction preserves the distinction between internal iterates and played actions, while the main analysis handles stale and out-of-order feedback. We will revise the motivation to clarify this role.

**Weakness 3:** We thank the reviewer. Table 4 is restricted to FC 1–4; the required smoothing and surrogate conditions have not been established for the remaining classes. Appendix M's current specialization directly supports FC 2 but does not justify the auxiliary surrogates required for FC 1, FC 3, and FC 4.

The revised construction of Pedramfar and Aggarwal (arXiv v4, 13 September 2026) combines smoothing with rescaling and provides a route to addressing this gap. We will revise Appendix M to explicitly construct the required surrogate estimators from original-reward queries and verify its feasibility  under precise assumptions.


