<!-- solve-problem:e5fdedfd-986a-4deb-9279-bd1a9fd6710c -->
# Simple stochastic games in polynomial time

A [Tangle Solve problem](https://solve.tangle.tools/problems/e5fdedfd-986a-4deb-9279-bd1a9fd6710c).

# Research Brief: Polynomial-Time Algorithms for Simple Stochastic Games

## What is the problem?

A **simple stochastic game (SSG)** is a two-player zero-sum game on a directed graph. Vertices have three types: MIN vertices (player 1 chooses an outgoing edge), MAX vertices (player 2 chooses an outgoing edge), and AVG vertices (a successor is chosen uniformly at random among outgoing edges). Each vertex has outdegree ≥ 1 (typically ≥ 2 for nontriviality). Two special sink vertices, 0-sink and 1-sink, have values 0 and 1. Play starts at a designated vertex and moves forever or until a sink is reached; player MAX wants to maximize the probability of reaching the 1-sink, player MIN wants to minimize it.

The **value** v(v) of each vertex is the unique fixed point of the natural functional equations (uniqueness holds under the standard "stopping game" assumption — every play reaches a sink with probability 1; this can be enforced in polynomial time). The problem:

1. **Value problem:** compute v(s) for the start vertex s exactly (as a rational) or to within a given additive error ε, in time polynomial in the graph size and log(1/ε).
2. **Threshold problem:** given rational q, decide whether v(s) > q.

The open question: is either problem solvable in **polynomial time**? Decision versions lie in NP ∩ coNP (Condon 1992); a P algorithm (or equivalently, for the threshold problem, an NP = P-style collapse of this class to P) is the goal.

## Why does it matter?

A polynomial-time algorithm would resolve one of the few natural problems known to be in NP ∩ coNP but not known to be in P — the canonical example of "LP-like" structure without a known P algorithm. SSGs subsume and generalize several solved problems (parity games, mean-payoff games, stochastic satisfiability), so progress here would likely ripple across quantitative verification and synthesis of probabilistic systems. Practically, SSGs model controller synthesis under probabilistic uncertainty (robotics, operations research, Markov decision processes with an adversary), where currently only subexponential or pseudo-polynomial methods are available.

## What's already known?

- **Condon (1992):** defined SSGs, proved the decision problem lies in NP ∩ coNP, and showed it is at least as hard as (and equivalent to, via reductions) the general problem in a precise sense; also showed the value vector is a unique fixed point of the Bellman operator.
- **Subexponential algorithms:** Ludwig (1995) gave a 2^{O(√n)} randomized algorithm via a random-facet-style pivot rule; Halman (2007) derandomized this to deterministic subexponential time. This mirrors the situation for parity and mean-payoff games — all three are subexponential via the same machinery.
- **Strategy complexity:** optimal (ε-optimal) deterministic, memoryless strategies exist for both players, proven via fixpoint/Knaster–Tarski arguments; this underlies the NP ∩ coNP membership.
- **Approximation:** values can be approximated to ε in time polynomial in n and 1/ε (but not poly in log(1/ε)) — e.g., via value iteration with the stopping-games reduction (Gill, Mayr, et al.; also Condon's iterative approximation). Since values may be doubly exponentially small/large in bit length, approximate computation alone does not settle the threshold problem.
- **Approaches tried and failed:** strategy improvement (counterexample families by Fearnley; also Friedmann's examples for parity-game strategy improvement), quadratic/linear programming formulations (LP formulations exist but may be exponentially large or nonunique — see the work on LP formulations for parity/SSG by Friedmann, Lange, Somla; also residual/sink-free formulations by Björklund, Sandberg, Vorobyov), and combinatorial classification attempts (Andersson, Miltersen have partial results; Gimbert and Horn showed SSGs with few random vertices are easy).
- **Restrictions solved in P:** games with no random vertices (mean-payoff/reachability games: P by reduction to LP — Zwick–Paterson for quantitative reachability), games with ≤ k random vertices (Fearnley), certain network-like structures.

## What would count as solving it?

Any one of:
1. **Positive (P membership):** a deterministic algorithm that, given an SSG with n vertices and rational threshold q, decides whether v(s) > q in time poly(n, |q|) — with correctness proof. (Then also exact value computation in poly time by bit-extraction/binary search with polynomial bit-length bounds, which needs care.)
2. **Negative (hardness):** a proof that the SSG threshold problem is NP-hard (or coNP-hard), which would imply NP = coNP and is considered extremely unlikely — but a relativized/oracle separation showing there is no "natural" algorithm class containment would also be interesting (Condon showed SSG is not P-complete under certain assumptions unless NP = coNP... (verify)).
3. **Partial but major:** a strongly polynomial algorithm for the associated problem class (e.g., solving all stopping SSGs via a compact polynomial-size exact LP), or a proof that no polynomial-size exact LP formulation exists (in the spirit of LP-extension-complexity lower bounds).

## How to start

1. **Read Condon (1992)** for the formal model, fixpoint characterization, and NP ∩ coNP proof — Sections 2–4 contain the core hardness-of-the-general-case and approximation results.
2. **Read Halman (2007)** and Ludwig (1995) to internalize the subexponential technique (random facet pivot rules on acyclic unique-sink orientations). The natural modern attack: understand why the LP polytope for SSG resists interior-point/pivoting methods, and whether new LP machinery (e.g., operator scaling, geometric rescaling for linear complementarity problems) applies.
3. **Study the LP-complexity angle:** Friedmann–Lange–Somla's LP formulations of parity games, and recent work on whether such games admit polynomial LP formulations — this connects to open problems in LP complexity and the linear complementarity problem (P-matrix LCP, also in PPAD ∩ PLS-like classes and NP ∩ coNP).
4. **Main obstacle:** no known technique converts NP ∩ coNP membership into P; the Bellman operator's fixpoint has exponentially steep behavior (values with many bits), so both value iteration and naive LP formulations fail. Consider restricting to stopping games first — the reduction from general to stopping games (Condon) may itself lose structure worth re-examining.

## Key references

- A. Condon, "The complexity of stochastic games," *Information and Computation*, 96(2):203–224, 1992. Defines SSGs, proves NP ∩ coNP membership and equivalence results among SSG problem variants. https://doi.org/10.1016/0890-5401(92)90048-K
- W. Ludwig, "A subexponential randomized algorithm for the simple stochastic game problem," *Information and Computation*, 117(1):151–155, 1995. Gives the 2^{O(√n)} randomized algorithm via random pivot rules. https://doi.org/10.1006/inco.1995.1040
- N. Halman, "Simple stochastic games, parity games, mean payoff games and discounted payoff games are all LP-type problems," *Algorithmica*, 49(1):37–50, 2007. Unifies these games as LP-type and derandomizes the subexponential algorithm. https://doi.org/10.1007/s00453-007-0175-3
- O. Friedmann, M. Lange, S. Somla, "The complexity of strategy iteration for parity games," or the LP-formulation paper (Friedmann's thesis, 2009) — proves exponential lower bounds for strategy improvement and studies LP formulations of positionally determined games. (verify exact citation)

## Warnings

- **Stopping assumption matters:** uniqueness of the value fixpoint holds only for stopping games; non-stopping games can have multiple fixpoints, and the standard reduction to stopping games perturbs values — check that any claimed P algorithm handles this (Condon's original proof of a subtlety here is frequently rediscovered).
- **Exact vs. approximate:** "compute the value" can mean exact rational (values may have exponentially many bits) vs. ε-approximation; only the exact/poly-log-ε version captures the open problem. Ambiguity in the original informal statement should be resolved toward the threshold decision problem.
- **Known dead ends:** plain value iteration needs 1/ε iterations; naive LP formulations have either exponential size or non-unique optima; strategy improvement has exponential counterexamples.
- **Do not conflate** with discounted stochastic games (Shapley 1953), which are solvable in pseudo-polynomial/exponential time and have a different status; SSGs are the reachability-special case. Mean-payoff and parity games reduce to SSG-like classes, but a P algorithm for SSGs is not implied by parity-game results and vice versa (verify direction of reductions you use).

---
*`ALGO-000173` · unverified*

## Acceptance guidance

Any one of:
1. **Positive (P membership):** a deterministic algorithm that, given an SSG with n vertices and rational threshold q, decides whether v(s) > q in time poly(n, |q|) — with correctness proof. (Then also exact value computation in poly time by bit-extraction/binary search with polynomial bit-length bounds, which needs care.)
2. **Negative (hardness):** a proof that the SSG threshold problem is NP-hard (or coNP-hard), which would imply NP = coNP and is considered extremely unlikely — but a relativized/oracle separation showing there is no "natural" algorithm class containment would also be interesting (Condon showed SSG is not P-complete under certain assumptions unless NP = coNP... (verify)).
3. **Partial but major:** a strongly polynomial algorithm for the associated problem class (e.g., solving all stopping SSGs via a compact polynomial-size exact LP), or a proof that no polynomial-size exact LP formulation exists (in the spirit of LP-extension-complexity lower bounds).

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/e5fdedfd-986a-4deb-9279-bd1a9fd6710c?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000173.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000173)
