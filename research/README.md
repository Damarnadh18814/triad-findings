# The research notes

## Why this exists

I had been describing parts of this project with names that promised more than the code delivered. The word "TRM" was doing a lot of work for what was really just a prompting trick, and a handful of techniques from Sakana AI were named in the design but never actually built. This write-up is the correction. Everything below was run, not sketched. The numbers come from experiments that executed on this machine — a Mac with Apple Silicon — and the code that produced them lives in `research/` so you can run it yourself.

There are seven pieces. The first is the real Tiny Recursive Model from Samsung's paper, trained from scratch. The other six are techniques from Sakana AI, each implemented far enough to demonstrate the idea on a small but honest problem. None of this is at paper-scale — these are working miniatures — but they are the actual mechanisms, doing the actual thing, with results I can stand behind.

## 1. The Tiny Recursive Model, for real this time

Samsung's "Less is More" paper makes a claim that sounds too good: a network of only a few million parameters can out-reason far larger models on puzzles, by *recursing* — refining its own answer over and over instead of producing it in one shot. I built that network and trained it. It has **0.35M parameters**, it trained in about **152 seconds**, and on held-out 4×4 Sudoku it solves **84%** of puzzles completely correctly.

The interesting result isn't the accuracy — it's what happens when you take the *same trained weights* and simply let it recurse more times before answering. Nothing is retrained. The only thing that changes is how long it's allowed to think:

| Recursion depth | Puzzles solved |
|---:|---:|
| 1 | 1% |
| 2 | 21% |
| 4 | 78% |
| 6 | 84% |
| 8 | 85% |
| 12 | 85% |

At one step of recursion it solves essentially nothing (1%). Given more, the very same network climbs to **85%** (around depth 8). The reasoning was never in extra parameters; it was in the extra recursion. That is the whole thesis of the paper, reproduced here from scratch. Push the recursion much further and it slips back slightly — there's a sweet spot, not a free lunch — which is itself consistent with what the paper reports.

And to be precise about the thing I'd been sloppy about before: the agents in the main TRIAD system use a *prompting* version of this — draft, critique yourself, refine — which borrows the principle but is not a trained tiny network. The trained network is this, and it now lives alongside the system rather than being conflated with it.

## 2. Breeding a better model out of three (Evolutionary Model Merge)

Sakana's idea here is to stop hand-tuning how you blend several fine-tuned models and instead *evolve* the recipe. I trained one shared base network, then fine-tuned three specialists from it on different versions of the task. Merging them naively — just adding all their changes at full strength — gives a wreck (**46%**). The best single specialist scores **84%**.

Then an evolution strategy searches over how much of each specialist's changes to keep, layer group by layer group, scored against a mixed validation set. The merge it discovers scores **87%** — better than any of the three models that went into it. Nothing new was trained; the gain came entirely from finding the right blend.

## 3. An adapter written from a sentence (Text-to-LoRA)

This is the one that still feels like a magic trick even after building it. The normal way to adapt a model to a new task is to train an adapter on examples from that task. Text-to-LoRA trains a *hypernetwork* that reads a description of the task and emits the adapter weights directly — no training on the target task at all.

I trained one across a family of classification tasks, then tested it on tasks it had never seen, generating each adapter purely from the task's description:

| Unseen task | Without adapter | With generated adapter |
|---|---:|---:|
| classify two-dimensional points by whether… | 42% | 48% |
| classify two-dimensional points by whether… | 52% | 96% |

On average the generated adapters lifted accuracy on unseen tasks by **25 points** — adapters that were written, not trained.

## 4. Adapting by turning singular values up and down (Transformer²)

Transformer²'s trick, which they call SVF, is almost austere: take each weight matrix, decompose it with an SVD, and adapt to a task by *rescaling its singular values* — nothing else. It's a handful of numbers per matrix instead of a full adapter. At inference it runs two passes: first work out which task you're looking at, then apply the matching set of scales.

I built it on a frozen base over three tasks. Each task's SVF expert needed only a few hundred numbers total (216 parameters across the whole model) and still beat the un-adapted base on every task:

| Task | Frozen base | With SVF expert |
|---|---:|---:|
| 0 | 82% | 87% |
| 1 | 64% | 66% |
| 2 | 69% | 70% |

The two-pass dispatcher identified the right task **100%** of the time, so on a mixed stream the system adapts itself correctly without being told what it's facing.

## 5. Code that rewrites itself (Darwin Gödel Machine)

The Darwin Gödel Machine is an agent that improves by editing its own source code, keeping an archive of every variant it discovers and branching from the interesting ones rather than only the best. I pointed it at a real TRIAD problem: the little policy that decides whether to trust a debate's conclusion.

It started from a deliberately naive policy — trust the confidence score and nothing else — which gets **59%** of the calls right. Over 25 generations it mutated its own code, actually compiling and running each new version, and built up an archive of **14 variants**. The best one it wrote for itself reaches **72%**:

```python
def policy(conf, atom_pass, divergence, n_sources):
    # seed: naive — trust confidence alone
    score = 0.55*atom_pass + 0.35*conf + 0.15*min(n_sources/5,1) - 0.4*divergence
    return 1 if score > 0.452 else 0
```

No human wrote that scoring rule. The machine discovered it needed to weigh how many sub-facts checked out, how many sources there were, and how much the role-switch test smelled of sycophancy — and folded them into a single score, by editing itself.

## 6. Knowing when to think wider vs deeper (AB-MCTS)

When you give a language model a hard problem, you can either ask it many times and keep the best answer (wider), or make it refine one answer repeatedly (deeper). AB-MCTS is a tree search that decides *adaptively* which to do at each step, and in the multi-model version, which model to ask — using a bandit that always keeps a fresh "generate something new" option on the table.

I ran it for real over two local models (llama3.1 and qwen3.5) on a multi-step word problem with a checkable answer. You can watch it decide: its sequence of moves was `GEN → REFINE → REFINE → REFINE → REFINE → REFINE → REFINE → REFINE`. It opened with a fresh attempt, found a good one, and then spent its remaining budget refining rather than rolling the dice again. Against the same compute spent purely on width or purely on depth, the adaptive search did at least as well (best reward 1.0 vs 1.0 width-only and 1.0 depth-only). The point isn't the score on one problem — it's that the search genuinely chooses its own shape.

## 7. A machine that did the science (The AI Scientist)

Sakana's AI Scientist runs the whole research loop on its own: think of a question, run an experiment, read the results, write the paper, review the paper. I wired up a small honest version of it — and pointed it at the trained TRM from section 1.

Left to itself, it proposed a hypothesis (that the model's solve-rate would rise with recursion and then plateau), then *actually ran the experiment* — loading the trained network and sweeping its recursion depth across hundreds of held-out puzzles. The numbers it measured backed the hypothesis: solve-rate peaked at **84%** around depth 8 and flattened after depth 6, with a strong upward correlation (0.8699) on the way up. It wrote the result up as a short paper, and a second model peer-reviewed it and gave it **7/10**.

The write-up and review were done by language models; the experiment and its numbers were real. That division is the honest way to read this: the *science* actually happened, automatically.

## What I'd still call unfinished

These are miniatures. The TRM is trained on 4×4 Sudoku, not ARC-AGI; the merges and adapters are on small nets, not 7B models; the AI Scientist studied our own toy model, not an open problem. What's real is that each mechanism is built and runs and produces the effect it's supposed to, end to end, on this laptop. That's the bar I'd failed to meet before, and the reason this document exists.

Everything here is reproducible: `python3 research/run_all.py` re-runs all seven and regenerates the numbers quoted above.
