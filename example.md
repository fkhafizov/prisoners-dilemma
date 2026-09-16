2026.09.16

```
The 100 Prisoners Riddle

The setup: 100 death row prisoners, numbered 1 through 100, are offered a chance to survive.
The room: A room contains 100 closed drawers, also numbered 1 through 100. Inside each drawer is one slip of paper bearing a prisoner’s number.
The slips are placed randomly, and each prisoner’s number appears exactly once.
The challenge: The prisoners enter the room one at a time. Each prisoner may open up to 50 drawers and must find the slip bearing their own number.
The rules: Each prisoner must leave everything exactly as they found it, closing the drawers before leaving.
They cannot move the slips, leave messages, or communicate with the other prisoners after their turn.
The slips remain in the same drawers throughout the challenge.
The stakes: If every prisoner finds their own number, all 100 go free. If even one prisoner fails, all 100 are executed.
The planning: Before anyone enters the room, the prisoners may discuss the challenge and agree on a strategy.

What strategy gives the prisoners the best chance of all surviving—and how good are their chances?
```

## The graph model

Let $\pi$ be the permutation defined by

$$
\pi(i) = \text{the number on the slip inside drawer } i, \qquad i \in \{1, \dots, 100\}.
$$

Build a directed graph $G_\pi$ with vertex set $\{1, \dots, 100\}$ and an edge $i \to \pi(i)$ for each $i$. Each vertex has exactly one outgoing edge (each drawer holds one slip) and exactly one incoming edge (each number appears once). So $G_\pi$ is a disjoint union of directed cycles, which is exactly the cycle decomposition of $\pi$. The random placement of slips is a uniformly random permutation in $S_{100}$, so $G_\pi$ is a random disjoint union of cycles.

## The strategy: follow your own cycle

Prisoner $k$:

1. Opens drawer $k$ and reads the slip, say $j_1 = \pi(k)$.
2. Opens drawer $j_1$ and reads $j_2 = \pi(j_1)$.
3. Continues opening drawer $j_t$ to read $j_{t+1} = \pi(j_t)$, until finding $k$ or using up 50 drawers.

In graph terms, prisoner $k$ walks along the edges of $G_\pi$ starting from vertex $k$. Because $k$ lies on a cycle, the walk must eventually return to $k$. Slip $k$ sits in the drawer that is the predecessor of $k$ on that cycle, $\pi^{-1}(k)$. If the cycle has length $L$, prisoner $k$ finds his slip on exactly the $L$-th drawer.

**Analogy.** It works like a scavenger hunt in which each clue names the next location. The loop that starts at your own drawer is guaranteed to lead back to you. Starting at your own number is what puts you on the right loop. The only question is whether the loop is short enough.

**Small example** ($n = 8$, 4 openings allowed). Take $\pi = (1\,5\,3)(2\,8\,4\,7\,6)$.

- Prisoners $1, 5, 3$ lie on a 3-cycle, so each succeeds in 3 steps.
- Prisoner $4$ opens drawers $4, 7, 6, 2$ and reads slips $7, 6, 2, 8$. His slip is in drawer $8$, which would be the 5th opening, so he fails.
- Everyone on the 5-cycle fails.

**Consequence:**

$$
\text{all prisoners succeed} \iff G_\pi \text{ has no cycle of length } > 50.
$$

The prisoners' fates are no longer independent. Either everyone wins, or at least 51 prisoners fail together. That strong correlation is the whole trick.

## Computing the probability

Let $n = 100$ and $L > n/2 = 50$. The number of permutations of $[n]$ containing a cycle of length exactly $L$ is found in three steps:

- choose the $L$ elements on the cycle: $\binom{n}{L}$ ways;
- arrange them into a cycle: $(L-1)!$ ways;
- permute the remaining elements: $(n-L)!$ ways.

$$
\binom{n}{L}(L-1)!\,(n-L)! = \frac{n!}{L}.
$$

So $\Pr[\text{a cycle of length } L] = \dfrac{1}{L}$.

A permutation of $[n]$ can contain at most one cycle longer than $n/2$, so these events are disjoint for different $L$. Therefore

$$
\Pr[\text{failure}] = \sum_{L=51}^{100} \frac{1}{L} = H_{100} - H_{50} \approx 0.688172,
$$

$$
\Pr[\text{all survive}] = 1 - \left(H_{100} - H_{50}\right) \approx 0.311828 \approx 31.18\%.
$$

For $n = 2m$ prisoners each opening $m$ drawers, the success probability decreases toward a limit:

$$
1 - \left(H_{2m} - H_m\right) \;\longrightarrow\; 1 - \ln 2 \approx 0.30685 \quad (m \to \infty).
$$

The success chance never falls below about 30.7%, no matter how many prisoners there are.

**Comparison with random guessing.** If each prisoner opens 50 drawers at random, their outcomes are independent and

$$
\Pr[\text{all survive}] = \left(\tfrac{1}{2}\right)^{100} \approx 7.9 \times 10^{-31}.
$$

## Optimality

This cycle-following strategy is optimal; Curtin and Warshauer (2006) proved that no strategy does better. The proof compares the game to a relaxed version in which drawers stay open between prisoners and the next unfound prisoner continues the search.

- In the relaxed game, every strategy wins with the same probability, namely $1 - \sum_{L=51}^{100} 1/L$.
- Any strategy for the original game wins no more often than a corresponding strategy in the relaxed game.

## Adversarial warden

Suppose the warden knows the strategy and deliberately arranges a single 100-cycle. The prisoners can defend against this by secretly agreeing on a random relabeling $\sigma \in S_{100}$ of the drawers, and treating drawer $\sigma(i)$ as "drawer $i$." The graph they walk is then a uniformly random conjugate of $\pi$, which restores the 31.18% success probability.


-----

# Q
for Computing the probability take n=8 and show all options for L>4 and all permutations and cycles.

# A

## Setup for $n = 8$

There are 8 prisoners and 8 drawers, and each prisoner may open $n/2 = 4$ drawers. The slips form a uniformly random permutation $\pi \in S_8$, so there are $8! = 40{,}320$ equally likely arrangements.

Everyone goes free exactly when $\pi$ has no cycle of length $L > 4$. That leaves $L \in \{5, 6, 7, 8\}$ as the failure cases.

These four failure cases cannot overlap. Two cycles of length at least 5 would need at least $10 > 8$ elements, so a permutation has at most one long cycle.

## Counting permutations with a cycle of length $L > 4$

$$
\#\{\pi : \pi \text{ has an } L\text{-cycle}\} = \binom{8}{L}\,(L-1)!\,(8-L)! = \frac{8!}{L}
$$

| $L$ | $\binom{8}{L}$ | $(L-1)!$ | $(8-L)!$ | count $= 8!/L$ | probability $= 1/L$ |
|---|---|---|---|---|---|
| 5 | 56 | 24 | 6 | 8,064 | $1/5 = 0.200000$ |
| 6 | 28 | 120 | 2 | 6,720 | $1/6 \approx 0.166667$ |
| 7 | 8 | 720 | 1 | 5,760 | $1/7 \approx 0.142857$ |
| 8 | 1 | 5,040 | 1 | 5,040 | $1/8 = 0.125000$ |
| **Total** | | | | **25,584** | $533/840 \approx 0.634524$ |

$$
\Pr[\text{failure}] = H_8 - H_4 = \tfrac15 + \tfrac16 + \tfrac17 + \tfrac18 = \frac{533}{840},
\qquad
\Pr[\text{all free}] = \frac{307}{840} \approx 36.55\%.
$$

Check: $40{,}320 - 25{,}584 = 14{,}736$, and $14{,}736 / 40{,}320 = 307/840$.

## Each $L$ split by what the other $8-L$ elements do

The factor $(8-L)!$ counts every possible permutation of the remaining elements. Grouping those by cycle type gives:

| $L$ | Cycle type $\lambda$ | $L$-cycles | Ways for the rest | Count |
|---|---|---|---|---|
| 5 | $(5,3)$ | $56 \cdot 24 = 1344$ | 2 | 2,688 |
| 5 | $(5,2,1)$ | 1344 | 3 | 4,032 |
| 5 | $(5,1,1,1)$ | 1344 | 1 | 1,344 |
| 6 | $(6,2)$ | $28 \cdot 120 = 3360$ | 1 | 3,360 |
| 6 | $(6,1,1)$ | 3360 | 1 | 3,360 |
| 7 | $(7,1)$ | $8 \cdot 720 = 5760$ | 1 | 5,760 |
| 8 | $(8)$ | 5040 | 1 | 5,040 |

## All 22 cycle types of $S_8$

The number of permutations of cycle type $\lambda$ is $8!/z_\lambda$, where

$$
z_\lambda = \prod_i i^{m_i}\, m_i!
$$

and $m_i$ is the number of $i$-cycles in $\lambda$.

| $\lambda$ | $z_\lambda$ | Count | Outcome |
|---|---|---|---|
| $(8)$ | 8 | 5,040 | 8 fail |
| $(7,1)$ | 7 | 5,760 | 7 fail |
| $(6,2)$ | 12 | 3,360 | 6 fail |
| $(6,1,1)$ | 12 | 3,360 | 6 fail |
| $(5,3)$ | 15 | 2,688 | 5 fail |
| $(5,2,1)$ | 10 | 4,032 | 5 fail |
| $(5,1^3)$ | 30 | 1,344 | 5 fail |
| $(4,4)$ | 32 | 1,260 | free |
| $(4,3,1)$ | 12 | 3,360 | free |
| $(4,2,2)$ | 32 | 1,260 | free |
| $(4,2,1,1)$ | 16 | 2,520 | free |
| $(4,1^4)$ | 96 | 420 | free |
| $(3,3,2)$ | 36 | 1,120 | free |
| $(3,3,1,1)$ | 36 | 1,120 | free |
| $(3,2,2,1)$ | 24 | 1,680 | free |
| $(3,2,1^3)$ | 36 | 1,120 | free |
| $(3,1^5)$ | 360 | 112 | free |
| $(2^4)$ | 384 | 105 | free |
| $(2^3,1^2)$ | 96 | 420 | free |
| $(2^2,1^4)$ | 192 | 210 | free |
| $(2,1^6)$ | 1440 | 28 | free |
| $(1^8)$ | 40320 | 1 | free |

I checked every count above by brute-force enumeration of all of $S_8$ in Python.

## Every permutation and its cycles

Listing all 40,320 permutations doesn't fit in a chat reply, so I built an explorer that does it. It enumerates all of $S_8$ in lexicographic order and shows each one in one-line and cycle notation, with any cycle longer than 4 highlighted.

- **Summary bar.** The page opens with a bar showing all 40,320 permutations, split into the 22 cycle types. Clicking a block, or any row of the two tables, lists exactly those permutations.
- **Filters.** You can show all permutations, only the survivable ones, a specific $L \in \{5,6,7,8\}$, or any one cycle type.
- **Search.** You can type a permutation in one-line notation (`58173264`) or cycle notation (`(1 5 3)(2 8 4 7 6)`).
- **Selected permutation.** It draws $G_\pi$ as disjoint directed cycles, colored green or red. You can pick a prisoner and step through their drawer openings; the path lights up on the graph and the opening count is compared against the limit of 4.

It opens on the earlier example $\pi = (1\,5\,3)(2\,8\,4\,7\,6)$, which is one of the 8,064 permutations with a 5-cycle.



