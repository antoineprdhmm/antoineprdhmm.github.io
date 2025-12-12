+++
title = "Advent of Code 2025 Day 11 Part 2 Explained - Reactor"
description = "Detailled explaination of how I solved the part 2 of day 11 for AoC 2025, Reactor, with a link to my Rust implementation."
date = 2025-12-12
+++

You can find my [Rust implementation on Github](https://github.com/antoineprdhmm/advent_of_code/blob/master/src/y2025/day11/mod.rs).


## Part 1 — 😌

No difficulties here. I solved it with a simple BFS that generates all possible paths.

## Part 2 — 🥵

This one was a real pain… **until** I found a super simple solution 😅.

Usually I’m comfortable with graph problems, but the `dac` and `fft` devices bothered me. I also got stubborn about using BFS. When I switched later to DFS, everything was much easier.

### First Try - Changing the minimum amount of code

Part 1 worked perfectly with a simple BFS.

So my first attempt was just a small change: I kept track of the devices visited by each path. Then, when reaching `out`, I incremented the counter **only if** that path had visited both `dac` and `fft`.

```rust
#[derive(Debug)]
struct PathExplorer {
    device: String,
    visited: HashSet<String>,
}

let graph = read_rotations();

let mut paths_count = 0;
let mut deque: VecDeque<PathExplorer> = VecDeque::new();

deque.push_back(PathExplorer {
    device: "svr".to_string(),
    visited: HashSet::from(["svr".to_string()]),
});

while let Some(p) = deque.pop_front() {
    if p.device == "out" {
        if p.visited.contains("dac") && p.visited.contains("fft") {
            paths_count += 1;
        }
    } else {
        for device in graph.get(&p.device).unwrap() {
            if !p.visited.contains(device.as_str()) {
                let mut next_p = PathExplorer {
                    device: device.clone(),
                    visited: p.visited.clone(),
                };
                next_p.visited.insert(device.clone());
                deque.push_back(next_p);
            }
        }
    }
}

println!("{:?}", paths_count);
```

But this didn’t work: the `deque` just kept growing and growing forever.

### Maybe there’s a loop?

At first I suspected a cycle somewhere in the graph. I added logic to prevent loops and some logging. But there were loops.

### Rule Number 1: If you don’t know, visualize

So I generated a `.dot` file from the input and rendered the graph.

Here’s what it looks like 🤯

<img src="/advent_of_code_2025_day_11_part_2_explained/day11.svg" />

At the bottom in green, we can see the device `out`.

The 2 devices in pink are our entry points: `svr` and `you`.

The 2 devices in red are `dac` and `fft`.

The first thing to notice is that the graph we had to explore in Part 1 was **tiny** compared to Part 2.

This explains why the `deque` blows up: the number of possible paths is now huge.

Another thing that stands out in the visualization: `dac` and `fft` are a bit off to the side.

### A new idea

Maybe a lot of nodes can be skipped. Maybe we can shrink the graph enough to reuse the Part-1 algorithm.

My idea was to eliminate any node that doesn’t eventually lead to `out`, `dac`, or `fft`.

Then, in this loop:

```rust
for device in graph.get(&p.device).unwrap() {
```

…I’d only push the devices that are *eligible*:

```rust
let eligible = upstream_out.contains(device)
    && (p.visited.contains("dac") || upstream_dac.contains(device))
    && (p.visited.contains("fft") || upstream_fft.contains(device));
```

Meaning:

- the `device` must be able to reach `out`
- if `dac` hasn’t been visited yet, the `device` must be able to reach `dac`
- if `fft` hasn’t been visited yet, the `device` must be able to reach `fft`

But this didn’t help much, the graph was still too big.

### Added some logs to understand the `deque`

I decided to log every `PathExplorer` popped from the `deque`, and it quickly became obvious: we were visiting the same subpaths **way** too many times. That’s why the `deque` kept growing endlessly.

I tried to think of some way to make memoization for BFS, but my brain needed some fresh air.

### Rule Number 2: if visualization fails, go for a walk

While walking in the park, I decided I would try DFS instead.

It simply makes more sense here:

- We go all the way down a path
- We immediately know whether we reached `out` with both `dac` and `fft` visited
- If yes → +1, we found one valid path
- Then we backtrack one step, explore the other children, backtrack again, and so on…

For this challenge, DFS just feels more natural. And I decided to implement it **recursively**, which is usually simpler.

The only thing I didn’t know at that point was how I would do memoization. But it happened naturally.

```rust
fn dfs(
    device: String,
    fft: bool,
    dac: bool,
    graph: &HashMap<String, Vec<String>>,
    memo: &mut HashMap<(String, bool, bool), usize>,
) -> usize {
    if device == "out" {
        return if fft && dac { 1 } else { 0 };
    }

    let mut paths_count = 0;

    let next_fft = fft || device == "fft";
    let next_dac: bool = dac || device == "dac";

    for next_device in graph.get(&device).unwrap() {
        if let Some(m) = memo.get(&(next_device.to_string(), next_fft, next_dac)) {
            paths_count += m;
        } else {
            let r = dfs(next_device.to_string(), next_fft, next_dac, graph, memo);
            paths_count += r;
            memo.insert((next_device.clone(), next_fft, next_dac), r);
        }
    }

    paths_count
}
```

For the memoization, we want to remember:

“I’ve already reached this node with this state → here’s the result of exploring the rest of the graph from here with this state.”

And the state is just the arguments of the `dfs` function:

```rust
device: String,
fft: bool,
dac: bool,
```

That’s all.
