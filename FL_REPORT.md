# Foster-Lyapunov Proof Attempt — Collatz Conjecture
**Author: Ziad Salah**

> Honest, exhaustive attempt using Truthimatics + all session discoveries.
> Status: **FL_UNPROVABLE_VIA_CURRENT_PATH**

---

## What Foster-Lyapunov Requires

```
FL CONDITION: E[log₂(n_{t+1}) | n_t = n] < log₂(n) − c
              uniformly for all n > N₀, for some c > 0.

For a DETERMINISTIC map, E[·|n_t=n] IS the single actual drift:
  drift(n) = log₂((3n+1)/2^{v₂(3n+1)}) − log₂(n)
           = log₂(3+1/n) − v₂(3n+1)

For v₂(3n+1)=1: drift ≈ log₂(3)−1 = +0.585 > 0  (ASCENDING)
For v₂(3n+1)=2: drift ≈ log₂(3)−2 = −0.415 < 0  (DESCENDING)
=> Pointwise FL is FALSE for ~50% of odd n.
```

---

## FL Gate Results

| Gate | Score | Status | Key Finding |
|------|-------|--------|-------------|
| DriftConsistency | 0.0486 | **PROVEN** | mean_drift=-0.56628 target=-0.41504 pct_below_log23=0.0000 |
| StoppingTime | 0.5000 | **EMPIRICAL** | max_ratio=8.000  mean=0.526  K_empirical=8.0  [CANNOT PROVE — k_max unbounded] |
| CantorFiniteness | 0.8500 | **PROVEN** | k_max finite per n: PROVEN. k_max uniformly bounded: FALSE. Construction: n≡-5 m |
| OptionB | 0.0010 | **FAILS** | Option B (escape_v2≥k+2) FAILS. Violations: 165/180 tested. Escape v2 can be 1,2 |
| ErgodicityDependence | 0.4500 | **OPEN** | NN R²=-0.0001≈0: time-avg not predictable from n [EMPIRICAL]. Suggests ergodicit |

**FL TCI:** 0.8338  |  **Verdict:** `FL_UNPROVABLE_VIA_CURRENT_PATH (1 gate(s) FAIL)`

---

## What Was Established

| Claim | Status | Source |
|-------|--------|--------|
| E[drift/step] = log₂3−2 = −0.41504 (space-avg) | **PROVEN** | Theorem EXACT |
| k_max(n) is finite for each n | **PROVEN** | Theorem CANTOR |
| T(n) ≤ 8·log₂n for n=3..99,999 | **EMPIRICAL** | Sweep measurement |
| All E[v₂_next\|v₂_now=i] > log₂3 for i=1..6 | **EMPIRICAL** | Transition matrix |
| 0 trajectories with time-avg v₂ ≤ log₂3 (n=3..500001) | **EMPIRICAL** | Full sweep |

---

## Where The Proof Breaks

### Gap 1 — k_max is not uniformly bounded

```
THEOREM CANTOR proves:  k_max(n) is FINITE for each specific n.
DOES NOT PROVE:         k_max is bounded by a constant K across all n.

CONSTRUCTION: For any K, the integer n = (−5) mod 2^{3K+1} has k_max(n) = K.
Such n exist with density 1/8^K in ℕ.
=> k_max is O(log₈(n)) = O(log₂(n)/3) — unbounded globally.
```

Verification:

| K | n_example | bits | k_max verified | density |
|---|-----------|------|---------------|---------|
| 1 | 11 | 4 | 1 | 1/8^1 = 1.25e-01 |
| 2 | 123 | 7 | 2 | 1/8^2 = 1.56e-02 |
| 3 | 1019 | 10 | 3 | 1/8^3 = 1.95e-03 |
| 4 | 8187 | 13 | 4 | 1/8^4 = 2.44e-04 |
| 5 | 65531 | 16 | 5 | 1/8^5 = 3.05e-05 |
| 6 | 524283 | 19 | 6 | 1/8^6 = 3.81e-06 |
| 7 | 4194299 | 22 | 7 | 1/8^7 = 4.77e-07 |
| 8 | 33554427 | 25 | 8 | 1/8^8 = 5.96e-08 |

### Gap 2 — Option B fails

```
OPTION B HYPOTHESIS: After k rounds of alternating, escape v₂ ≥ k+2.
If true: drift at escape = log₂3 − (k+2) < 0 for k ≥ 1.
         This would cancel the ascending drift from k rounds.

RESULT: OPTION B FAILS.
Theorem RT guarantees escape v₂ ≥ 2 only — not ≥ k+2.
Empirical: escape v₂ can be 1, 2, 3 regardless of k.
```

| k | tested | violations of v₂≥k+2 | Option B holds? |
|---|--------|---------------------|-----------------|
| 1 | 30 | 22 | ✗ FAILS |
| 2 | 30 | 26 | ✗ FAILS |
| 3 | 30 | 28 | ✗ FAILS |
| 4 | 30 | 29 | ✗ FAILS |
| 5 | 30 | 30 | ✗ FAILS |
| 6 | 30 | 30 | ✗ FAILS |

### Gap 3 — Ergodicity is circular

```
The space-average drift = −0.415 is PROVEN by Theorem EXACT.
Using it for recovery steps in the FL proof requires:
  time-average drift ≈ space-average drift
  ≡ the system is ERGODIC
  ≡ the Collatz Conjecture.

The NN probe (R² = −0.0001) provides evidence of ergodicity,
but cannot constitute a mathematical proof.
```

---

## What Would Close It

```
PATH A: Prove k_max(n) ≤ C·log₈(n) + D uniformly for all n.
        Then: FL holds with T(n) = O(log n) and c from Cantor.
        But: this IS essentially the conjecture restated.

PATH B: Find a DIFFERENT Lyapunov function V(n) where E[V(n_{t+1})|n_t] < V(n_t) − c
        pointwise, without the ascent during alternating phases.
        No such V is currently known.

PATH C: Prove ergodicity by a non-circular argument.
        Tao 2019 reached logarithmic density — the closest attempt.
        No one has reached full measure / pointwise ergodicity.
```

---
*Foster-Lyapunov Engine — Author: Ziad Salah*