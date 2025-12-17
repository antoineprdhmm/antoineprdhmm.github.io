+++
title = "Advent of Code 2025 Day 12 Explained - Christmas Tree Farm"
description = "Detailled explaination of how I solved day 12 for AoC 2025, Christmas Tree Farm, with a link to my Rust implementation."
date = 2025-12-17
+++

You can find my [Rust implementation on Github](https://github.com/antoineprdhmm/advent_of_code/blob/master/src/y2025/day12/mod.rs).

--- 

At first, this looked like an impossible problem to solve with a generic algorithm.

If you had to do it in real life, you would probably just try things. There’s no obvious “smart” way to do it, I think.

So I started by looking at the input and noticed that the shapes themselves are pretty simple.

For a 2D shape, using rotations and flips, you can obtain **8 distinct shapes**
(the [symmetry group of the square](https://en.wikipedia.org/wiki/Group_(mathematics)#Second_example:_a_symmetry_group)).

In the input, there are only **6 shapes**, each defined on a **3×3** grid.

At first, I thought there might be some trick involving merging shapes together.

My initial idea was to generate all 48 shapes (6 × 8) and then try to merge them to find pairs or combinations that leave no empty space.

But then I realized: just because we can merge shapes without leaving empty space doesn’t mean it’s a valid solution. You might fill a huge contiguous area and leave no room for the remaining pieces. On the other hand, leaving small gaps everywhere might actually be fine.

Also:

- where you place the first present has a huge impact
- where you place the second one around it too
- and so on…

I considered working on an infinite grid instead of a fixed one, only checking whether the final width + height fits inside the allowed area.

Maybe some recursive approach could work? But the number of possibilities is just enormous. It felt completely infeasible.

And I really didn’t want to manipulate all those shapes 😅

So I decided to look at the other part of the input: **the regions**.

There are **1000 regions** in total, which is a lot.

The challenge is really about fitting a lot of presents into these regions. Otherwise, if the space were always large enough, the problem would be trivial.

So I started by eliminating the impossible cases.

If the total number of cells occupied by all the presents (ignoring empty cells) is greater than the available space in a region, then there’s no way it can work. No matter how good the algorithm is.

To my surprise, **only 467 regions** passed this simple check.

Among these 467 regions, I tried something very simple.

What if we don’t try to merge the presents at all?
What if we just place the 9×9 presents side by side, without any fancy arrangement?

And surprisingly: it works for all 467 of them.

Which means we actually don’t need to do anything smart here:

- For **533 / 1000**, there are simply more present cells than available area, so no solution is possible

- For **467 / 1000**, there is enough space that we can place the presents side by side without any special arrangement
