One interesting thing about DSA prep: the order you learn topics in matters far more than most people realize — and almost everyone gets it wrong on their first attempt.

When people start DSA prep, they usually follow a generic roadmap:
arrays → strings → sorting → searching → linked lists → trees → graphs → dynamic programming. 
It feels logical because it goes from "simple" to "complex."
But there's a hidden dependency that this ordering ignores: recursion.

Recursion isn't just "one more topic" in the list — it's the mental model that trees, graphs, backtracking, divide-and-conquer, and dynamic programming are all built on top of.
If you treat recursion as a checkbox topic (learn factorial, learn Fibonacci, move on), you're setting yourself up for a rough time later.
Because when you hit tree traversals or graph DFS a few weeks later, you're not just learning a new topic — you're simultaneously trying to learn the topic and relearn how recursive thinking works, under pressure, while also feeling behind. 
That combination is why so many people say "I understood arrays and sorting fine, but trees and DP just don't click" — it's rarely the trees or DP that are the real problem. 
It's an underbaked foundation in recursion.

Here's the practical difference between someone who "learned recursion" and someone who's actually internalized it:

Surface-level: can write a recursive function that works, by pattern-matching to examples they've seen.
Internalized: can take a recursive function they've never seen before, and manually trace the call stack on paper — what gets pushed, what gets returned, in what order — without running the code.

That second skill is what actually transfers to trees, backtracking, and DP later.
It's slower to build early on, which is exactly why people skip it — it feels like you're "wasting time" tracing simple examples when you could be solving more problems.
But it's one of those areas where slowing down early buys you speed later, because you stop having to re-derive the mental model every time you hit a new recursive pattern.

Most people's DSA repos end up being just a graveyard of solved problems with no story behind them — a folder of problem1.py, problem2.py, etc. 
Six months later, that repo tells you what you solved but nothing about how you think, which is actually the more useful thing to look back on.

A small habit that fixes this: for each problem, commit a tiny notes.md alongside your solution that captures:

Your first approach (even if — especially if — it was wrong)
Why it failed or was inefficient
What clicked to get you to the working solution
