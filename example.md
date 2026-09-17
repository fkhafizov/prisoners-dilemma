2026.09.16


* https://claude.ai/chat/0275da06-a176-4e5e-a1f2-5391e36790cd
  
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



============

# Q

 Describe relevant graph and how it is related to $S_n$

# A

The vertices are the $n$ drawers (equivalently the prisoners), not the elements of $S_n$. Each permutation gives its own small graph.

For each $\pi \in S_n$ define

$$
G_\pi = (V, E_\pi), \qquad V = [n] = \{1, \dots, n\}, \qquad E_\pi = \{\, i \to \pi(i) : i \in [n] \,\}.
$$

- **Vertices.** There are $n$ of them: drawer $i$, which is also prisoner $i$.
- **Edges.** There are $n$ directed arcs, one leaving each drawer and pointing to the number on its slip.
- **Structure.** Every vertex has in-degree 1 and out-degree 1 because $\pi$ is a bijection. So $G_\pi$ is a disjoint union of directed cycles, and these are exactly the cycles of $\pi$.
  - A fixed point $\pi(i) = i$ is a loop.
  - A 2-cycle is a pair of opposite arcs.
- **Role of $S_n$.** It is the set of all possible graphs, one per arrangement of slips. The warden's shuffle picks one $G_\pi$ uniformly at random from $n!$ possibilities.

In the app, each row of the list is one permutation, so each row is one graph $G_\pi$ on $n$ vertices. The SVG panel draws that single graph. The list itself has no edges between rows; it is just an enumeration of the sample space.

The prisoners win iff the longest cycle in $G_\pi$ has length at most $n/2$. So the probability question is

$$
\Pr_{\pi \sim \mathrm{Unif}(S_n)}\bigl[\text{every cycle of } G_\pi \text{ has length} \le n/2\bigr].
$$

A graph with vertex set $S_n$ would be a different object, such as a Cayley graph $\mathrm{Cay}(S_n, T)$ for a generating set $T$. That graph plays no role in this riddle.







# Q

is there a holography or a similar concept allowing to solve problem using holographic duality : node <-> string.

# A

There's no literal holographic (AdS/CFT-style) duality that solves this riddle. There is, however, an exact node ↔ string correspondence that does solve it. It turns the cycle graph $G_\pi$ into a binary string with independent bits, and the $1/L$ law then follows from a one-line telescoping sum. Separately, physics has a real dictionary in which permutation cycles are strings.

## 1. The exact duality: Foata's transformation and the Feller coupling

**Step A: graph → word (Foata).** Write each cycle of $\pi$ with its largest element first. Order the cycles by increasing largest element. Then erase the parentheses to get a word $w$.

This is a bijection $S_n \to S_n$. The cycles of $\pi$ correspond exactly to the blocks of $w$ that each start at a left-to-right maximum (a record).

For the running example:

$$
\pi = (1\,5\,3)(2\,8\,4\,7\,6) \;\mapsto\; (5\,3\,1)(8\,4\,7\,6\,2) \;\mapsto\; w = 5\,3\,1\,8\,4\,7\,6\,2 .
$$

The records of $w$ are $5$ at position 1 and $8$ at position 4. Cutting $w$ before each record recovers the two cycles, of lengths 3 and 5.

**Step B: word → binary string.** Define

$$
\xi_k = \mathbf 1\{\text{position } k \text{ of } w \text{ is a record}\}, \qquad k = 1, \dots, n,
$$

and append a sentinel $\xi_{n+1} = 1$. In the example,

$$
\xi = 1\,0\,0\,1\,0\,0\,0\,0 \mid 1 .
$$

**Dictionary.** Cycle lengths of $\pi$ are exactly the gaps between consecutive 1's in $\xi$.

Since $w$ is uniform on $S_n$, Rényi's record theorem says

$$
\xi_1, \dots, \xi_n \text{ are independent}, \qquad \xi_k \sim \mathrm{Bernoulli}(1/k).
$$

This is the Feller coupling (Arratia–Barbour–Tavaré). The whole random cycle graph is encoded in a 1-D string of independent bits.

**Solving the riddle on the string side.** A gap of length $L$ starting at position $i$ requires $\xi_i = 1$, then $L - 1$ zeros, then a 1 at position $i + L$. The middle factor telescopes:

$$
\prod_{k=i+1}^{i+L-1}\Bigl(1 - \tfrac1k\Bigr) = \frac{i}{i+L-1}.
$$

There are two cases:

$$
\Pr[\text{gap } (i, i+L)] =
\begin{cases}
\dfrac{1}{i}\cdot\dfrac{i}{i+L-1}\cdot\dfrac{1}{i+L} = \dfrac{1}{i+L-1} - \dfrac{1}{i+L}, & i + L \le n,\\[3mm]
\dfrac{1}{i}\cdot\dfrac{i}{n}\cdot 1 = \dfrac{1}{n}, & i + L = n + 1.
\end{cases}
$$

Summing over $i$, the first case telescopes to $\tfrac1L - \tfrac1n$, and the second adds $\tfrac1n$:

$$
\mathbb E[\#\text{gaps of length } L] = \Bigl(\frac1L - \frac1n\Bigr) + \frac1n = \frac1L .
$$

For $L > n/2$ there can be at most one such gap. So this expectation is a probability, and

$$
\Pr[\text{prisoners fail}] = \sum_{L > n/2} \frac1L = H_n - H_{n/2}.
$$

## 2. The algebraic version: the exponential formula

Generating functions give the same duality. A permutation is a set of cycles, and $\exp$ converts connected structures (cycles) into sets of them (permutations). Let $p_n$ be the number of permutations of $[n]$ with all cycles of length at most $m$. Then

$$
\sum_{n \ge 0} p_n \frac{x^n}{n!} = \exp\Bigl(\sum_{k=1}^{m} \frac{x^k}{k}\Bigr).
$$

With $m = n/2$, the coefficient extraction gives the survival probability $p_n / n!$. Without the length cap, the sum is $\exp(-\ln(1-x)) = 1/(1-x)$, which just counts all $n!$ permutations.

## 3. Physics: cycles as long strings

In the symmetric orbifold CFT $\mathrm{Sym}^N(M)$, twisted sectors are labeled by conjugacy classes of $S_N$. A cycle of length $L$ is a single long string wound $L$ times. This is the DMVV picture (Dijkgraaf–Moore–Verlinde–Verlinde, 1997), used in AdS$_3$/CFT$_2$.

In that language, the prisoners win iff no string is longer than $N/2$. The combinatorial engine behind DMVV is again the exponential formula from §2. So the analogy is faithful, but it doesn't give a new route to the answer.

## 4. A name clash to avoid

Valiant's holographic algorithms (Holant problems, matchgates) are a genuine counting technique based on basis changes. They aren't needed here, because this count is already elementary.

For computation and intuition, §1 is the useful duality. It replaces a random graph on $n$ nodes with a string of $n$ independent coins, and turns the event "no long cycle" into "no long run of zeros between consecutive 1's."


