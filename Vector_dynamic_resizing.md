# How `vector` Manages to "Grow" Without Breaking Everything

## The Puzzle

Arrays are **fixed-size contiguous blocks** — that's exactly *why* indexing is instant (`O(1)`). But a `vector` grows dynamically with `push_back()`. So how does it stay just as fast as an array, while also being resizable?

These seem like contradictory properties. The answer is a genuinely clever trick.

## The Secret

A `vector` never actually resizes in place. It secretly rebuilds itself into a bigger house.

Internally, a `vector` is just three things sitting on the stack:

```cpp
int *data;      // pointer to the actual array, sitting on the heap
int size;       // how many elements you've actually added
int capacity;   // how many elements the current heap block can hold
```

As long as there's still room (`size < capacity`), `push_back()` is trivial — just drop the new value into the next open slot. Fast, `O(1)`, no surprises.

## What Happens When It's Full

When the vector is completely full and you push one more element, this happens behind the scenes:

1. It asks the heap for a **brand new, bigger block of memory** (usually **double** the old capacity).
2. It **copies every single existing element** from the old block into the new one.
3. It **deletes** the old block entirely.
4. *Then* it finally adds your new element.

```cpp
vector<int> v;    // capacity 0
v.push_back(1);   // capacity -> 1
v.push_back(2);   // full! capacity doubles to 2, copies old data, then adds
v.push_back(3);   // full! capacity doubles to 4, copies old data, then adds
v.push_back(4);   // still room -> fast
v.push_back(5);   // full! capacity doubles to 8, copies, then adds
```

So a `vector` never "expands" its existing memory — it can't, since neighboring heap memory is unpredictable and might already be used by something else. Instead, it quietly **moves house** into a bigger space every time it outgrows the current one, dragging every element along with it.

## Why Double, Not +1?

If it grew by exactly 1 slot every time you called `push_back()`, then *every single call* would trigger a full copy of all existing elements — turning what should be a simple, fast operation into something painfully slow, especially for large vectors.

By doubling instead:

- The expensive "move house" operation happens **less and less frequently** as the vector gets bigger, even though each individual move gets more expensive.
- If you add up the total cost of *all* the copying across a long sequence of `push_back()` calls, it averages out to **constant time per call** — even though a handful of individual calls are secretly doing a lot of hidden work.

This is called **amortized O(1)** — not because every single `push_back()` is fast, but because the *expensive ones are so rare* that the average cost per operation stays constant.

> Analogy: a slot machine that pays out big once every hundred pulls still has a steady "average payout per pull" — you just don't see the size of any individual pull from the outside.

## Why This Matters

This is a perfect example of a recurring theme in DSA:

> The real cost of an operation isn't always visible from a single call; you often have to think about total cost across many calls to understand true efficiency.

This exact kind of amortized-cost reasoning shows up again and again in interviews and algorithm analysis. Understanding *why* `vector` is fast — rather than just trusting that it is — builds the instinct needed for analyzing your own algorithms later.

## Practical Tip

If you already know roughly how many elements you'll need, pre-allocate to avoid unnecessary resizing:

