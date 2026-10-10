# how to learn dynamic programming: the 4-question method, the 9 pattern groups, and a practice order that actually sticks

Most people don't fail dynamic programming because the math is hard. They fail because they skip straight to the table.

You can write a working recursive solution in five minutes and still stare blankly at the DP version, because the difficult part was never the syntax. It's deciding what to remember, and in what order. Get that decision right and the loops are almost mechanical. Get it wrong and you end up memorizing `dp[i][j]` transitions like vocabulary words, which is why DP keeps feeling random six months in.

This is a breakdown of how the topic actually fits together, the practice sequence that builds the skill instead of just exposing you to problems, and where a structured platform like AlgoMonster fits if self-directed grinding hasn't worked.

## DP is recursion plus memory, nothing more

Two properties decide whether dynamic programming applies at all:

- **Optimal substructure** — the best answer to the whole problem can be assembled from the best answers to smaller versions of it.
- **Overlapping subproblems** — those smaller versions repeat, typically an exponential number of times in the naive recursion tree.

The Fibonacci function is the standard first example for a reason. Plain recursion recomputes `fib(n-2)` inside both branches of every call, so the number of calls roughly doubles per step. Memoize the results in a dictionary and the same computation drops to linear time. Nothing changed about the logic; you just refused to solve the same subproblem twice.

There's a useful framing that shows up in CMU's algorithm lectures and in most good DP explanations: DP is "clever brute force." You try every choice at each step, then take the best outcome — and memoization collapses the exponential search into something tractable. That framing matters because it removes the pressure to be clever. You're not looking for an insight. You're looking for a complete description of the choices, then caching the results.

## Answer four questions before writing a line of code

This is the part worth drilling until it's automatic. For any DP problem, write these down in plain English:

1. **What decision am I making at each step?** Take this item or skip it. Jump one stair or two. Cut the string here or there.
2. **What state uniquely defines the remaining subproblem?** An index alone, an index plus a remaining capacity, a pair of indices, a start and end bound.
3. **How does a state transition?** Usually `max` or `min` or a sum over the options you identified in question 1.
4. **What is the base case, and which state holds the final answer?** Empty string, zero capacity, `dp[n]`, `dp[0][m]`.

House Robber is a clean test case. At house `i` you decide: rob it and take `nums[i] + best(i-2)`, or skip it and take `best(i-1)`. Two choices, one state (the index), one transition (max of the two), base case at `i = 0`.

Coin Change is where question 2 starts doing real work. The state is the amount remaining, and the transition tries every coin. If you can't describe the state in a sentence without saying "and also," you haven't found the state yet — you've found two.

## Write top-down first, then convert

The fastest route to a correct DP solution is usually to write the brute-force recursion, memoize it, and only then rewrite it bottom-up.

**Top-down with memoization.** Same recursive function, plus a cache check at the top and a cache write before returning. It mirrors how you think, and it's easy to debug because the call structure is visible. In Python, that's a dictionary or `functools.lru_cache`.

**Bottom-up with tabulation.** You figure out the dependency order and fill an array or table iteratively. Usually faster in practice because there's no recursion overhead, but you have to know which entry to compute first — which is exactly why doing it top-down first helps.

A rule of thumb that most people converge on: if you can't write the brute-force recursion, you don't understand the problem well enough to write the DP version. Reviews of AlgoMonster and the beginner roadmaps on Educative both name "skipping the recursive brute-force step" as one of the most common ways beginners stall out.

Once the bottom-up version works, space optimization is the last step. Fibonacci only needs the previous two values, so `O(n)` space collapses to `O(1)`. Many 2D tables only need the previous row. This is polish, not the main event — don't optimize before the recurrence is correct.

## The pattern map: nine groups cover most interview DP

DP problems are not thousands of unrelated puzzles. They cluster, and the cluster is defined by what the state looks like. AlgoMonster's own DP problem list organizes them into nine groups, which is as clean a taxonomy as you'll find anywhere:

| Group | What the state looks like | Problems in the list |
| --- | --- | --- |
| Warmup | Direct index into a 1D array | 3 |
| Constant transition | Prefix of the array, `O(1)` work per state | 5 |
| Grid | Same dimensions as the grid; subgrids | 6 |
| Dual-sequence | `dp[i][j]` over prefixes of two sequences | 6 |
| Interval | Every subarray / substring `[i, j]` | 6 |
| Non-constant transition | Prefix state, but each transition scans `j < i` | 5 |
| Knapsack | Items plus capacity | 9 |
| Topological order (advanced) | DP over subgraphs feeding into each node | 3 |
| Tree DP (advanced) | DP over every subtree | 2 |

That's 45 problems plus a miscellaneous set, and it's a much better study unit than "do 200 random DP questions." The groups exist so that after problem three or four in each one, you stop solving individual problems and start recognizing a shape. When you see two sequences in the prompt, you should already suspect the dual-sequence table. When the answer involves splitting an array into nested decisions, interval DP. That recognition is the transferable skill; the code is downstream of it.

## A practice order that builds instead of exposing

Ordering matters more than volume. This sequence works because each problem introduces exactly one new idea:

1. **Climbing Stairs** — the recurrence itself, no state beyond the index.
2. **House Robber** — a choice between two options, `max` transition.
3. **Coin Change** — unbounded reuse, minimum over a variable number of choices.
4. **Word Break** — boolean DP where the transition looks backwards over a set of words.
5. **Decode Ways** — counting rather than optimizing, plus edge cases on leading zeros.
6. **Unique Paths** — the first grid table; state is two coordinates.
7. **Longest Common Subsequence** — the dual-sequence template.
8. **Edit Distance** — same shape as LCS, three transitions instead of two.
9. **Longest Palindromic Substring or Subsequence** — interval DP, expanding bounds.
10. **Partition Equal Subset Sum** — 0/1 knapsack in disguise.
11. **Target Sum** — knapsack counting instead of feasibility.
12. **Burst Balloons** — interval DP where the order of operations is the insight.
13. **House Robber III** — moving DP onto a tree.
14. **A graph DP problem of your choice** — topological ordering of states.

Two to three of these a week, with a redo of the older ones a few days later, beats twenty problems in a weekend. Retention is the bottleneck, not exposure. If you can explain the recurrence in words to someone else, you're done with that problem. If you can't, more reps won't help.

## Five mistakes that keep DP feeling random

- **Jumping to the table.** If the recursion isn't written down, the table is guesswork.
- **Memorizing transitions.** `dp[i] = dp[i-1] + dp[i-2]` is a fact about one problem, not a skill.
- **Vague state definitions.** "The best answer so far" is not a state. "The best answer using the first `i` items with capacity `B`" is.
- **Wrong base case, correct everything else.** Most failing DP submissions are off by one at `dp[0]`, not conceptually broken.
- **Not writing the state down.** A surprising number of stuck moments disappear when you spend 60 seconds writing the four questions above on paper before touching the keyboard.

## Where structured practice changes the math

The honest version: DP is learnable from free resources. LeetCode editorials, the AlgoMonster DP list, and a free animated DP course by an ex-Google engineer (Sheldon, who teaches on the platform) cover the ground. What free resources don't give you is sequencing and a way to find the pattern when the problem statement doesn't announce it.

That's the specific gap AlgoMonster targets. It's a pattern-based course built by ex-Google engineers, and unlike a raw problem bank it teaches the pattern first, then maps problems onto it. Three features are relevant if DP is your weak spot:

- **A decision flowchart** that turns problem descriptions into candidate algorithms through a series of questions. The stated purpose is a reference tool, not something to memorize — and their own guidance is explicit that memorization is the wrong use of it.
- **Code templates** for each pattern, so the implementation is not a separate risk from the reasoning.
- **An inline IDE with an AI assistant** for explaining content and debugging submissions, plus Speedrun mode for rapid review of patterns you've already learned.

The current pricing page also advertises 500+ lessons across 6 courses, 1,000+ illustrations, and 3,000+ editorials, plus member access to a Foundation course and a Build an LLM course at no extra cost.

What it doesn't have is worth knowing before you pay. There are no video walkthroughs — the content is text and diagrams. Independent reviews consistently flag the smaller problem bank (roughly 231 mapped problems against LeetCode's thousands), introductory-only system design coverage, and the absence of live mock interviews. The same reviews frame the curriculum as built for something like 4–8 weeks at one to two focused hours a day. If your problem is that you've done 200 LeetCode problems and still can't pick an approach quickly, that's the profile it's designed for. If your problem is volume or you learn better from video, it's the wrong purchase.

## AlgoMonster plans and pricing

Here's what the official pricing page currently shows:

| Plan | What you get | Price | Billing | Get it |
| --- | --- | --- | --- | --- |
| Free | Limited access to a subset of lessons and problems; enough to test the teaching format | $0 | — | [Start with the free tier](https://algo.monster/subscribe?fpr=li69) |
| Pro – Yearly | All patterns, lessons and problems, unlimited AI assistant, unlimited Speedrun, 500+ lessons across 6 courses, bonus Foundation and Build an LLM courses, member discount on live bootcamps | $69.99/year, listed as $5.84/month billed annually | 12 months, cancel or upgrade anytime | [See the yearly plan](https://algo.monster/subscribe?fpr=li69) |
| Pro – Monthly (short term) | One month of full access; the pricing page lists it at roughly $0.64/day | ≈$0.64/day as listed | 1 month | [Check the monthly option](https://algo.monster/subscribe?fpr=li69) |
| Pro – Lifetime | Everything in the yearly plan plus lifetime updates, one payment | $89 | Billed once, yours forever | [Get lifetime access](https://algo.monster/subscribe?fpr=li69) |

A few things worth flagging. There is no separate per-plan URL — plan selection happens on the subscription page, which is why every link above lands there. AlgoMonster runs frequent sales, and third-party sources quote a wide spread of historical prices, so treat anything you read outside the checkout page with suspicion and look at the number on the page itself before you decide. Lifetime at $89 against yearly at $69.99 is an unusual gap; if you're going to prep for interviews more than once, the lifetime option is the one that stops making you think about it.

For the actual DP curriculum — the nine groups listed earlier — you can browse the problem list directly: 👉 [See the Monster Dynamic Programming problem list](https://algo.monster/problems/dp-list?fpr=li69).

## A concrete six-week DP plan

If your interview is roughly six weeks out and DP is the weak spot:

- **Week 1:** Recursion drills. Climbing Stairs, House Robber, and the same problems again with memoization bolted on. Goal: the mechanical feel of cache-check-then-return.
- **Week 2:** 1D DP with transitions that look backwards. Coin Change, Word Break, Decode Ways. Start asking the four questions out loud for each one.
- **Weeks 3–4:** Grid and dual-sequence. Unique Paths, LCS, Edit Distance. These are the two templates that show up most often in real interview loops.
- **Week 5:** Interval DP and knapsack. Partition Equal Subset Sum, Target Sum, then one interval problem you find genuinely unpleasant.
- **Week 6:** Review only. Redo the first seven problems from scratch, no notes. Then move to timed practice so the recognition happens under pressure, which is a different skill from recognizing the pattern at your own desk.

Notice what's missing: hundreds of problems. The pattern-based approach is the whole argument — a small set of shapes, understood deeply, transfers to problems you have never seen. The failure mode is the opposite: recognizing ten patterns shallowly and then freezing when an interviewer adds a constraint you didn't practice.

The other thing worth doing in week 6 is being honest about what DP prep doesn't fix. It won't help you if you can't yet write fluent code in one language, and it won't help if the round you're actually failing is system design or behavioral. Those need a different plan.

The short version of all this: stop collecting DP solutions, start writing down states and transitions in plain English, and work through the groups in order. That's the whole method. Everything else — memos, tables, space optimization, flowcharts, Speedrun — is machinery built on top of two sentences you should be able to say about any DP problem you meet.
