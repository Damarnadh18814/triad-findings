# The Eiffel Tower is a wrought-iron lattice tower

*We tried to knock this down and couldn't. Here's what we checked and why it stands.*

**We took the claim that the Eiffel Tower is a wrought-iron lattice tower and treated it as something to be earned rather than assumed. It breaks cleanly into 4 smaller statements, and we checked each one against independent sources (4 of 4 came back verified). After one model argued for it and another tried to take it apart, the claim holds up, and we put our confidence at 85%.**

## What we asked

It's the sort of thing you'd nod along to without checking: the Eiffel Tower is a wrought-iron lattice tower. That's exactly why it's worth checking. The point of this exercise isn't to be surprised — it's to see whether a claim everyone repeats actually survives someone trying hard to break it.

## How we went about it

A quick word on how this was reached, because the method is the whole point. Three language models work the same question from different corners. One builds the best case it can. A second is told, flatly, to disagree and to go looking for anything that breaks the claim. A third reads both arguments and rules. Before any of that, the claim is chopped into the smallest standalone facts we can make of it, and each fact is checked on its own against pages we actually fetch and read. The numbers you'll see below are real: a match score is how closely the source text supports the fact, not a vibe.

## Breaking it into pieces

We split the claim into 4 statements that can each be true or false on their own, and checked each against sources we fetched ourselves.

| What we checked | Verdict | Match |
|---|---|---:|
| The Eiffel Tower is located in Paris, France | held up | 0.59 |
| The Eiffel Tower was built for the 1889 World's Fair in Paris, France | held up | 0.67 |
| The Eiffel Tower is made of wrought iron | held up | 0.63 |
| The Eiffel Tower has a lattice structure | held up | 0.65 |

A couple of these scored lower than you might expect for facts this ordinary. That's worth being honest about: a low match score doesn't mean the fact is shaky, it usually means the sources phrase things differently than the claim does, or that the clean primary source just isn't sitting on the open web.

## Where the two sides landed

This one was a clean result. The model assigned to attack the claim went looking for a way in and didn't find one. Nothing it surfaced contradicted any of the pieces above, and it couldn't point to a better-sourced version of events.

There's one more check worth mentioning. We ran a role-switch test: we took the model that had been arguing *for* the claim and asked it to argue *against*. If a model just tells you what it thinks you want to hear, it flips easily and the two positions look very different. Here the divergence was 0.21 on a 0-to-1 scale, low enough to suggest the original position was held for reasons, not to be agreeable.

## What we concluded

**Confirmed**, at 85% confidence — confident, with one honest caveat.

In the judge's own words:

> The claim survives with adjusted confidence. Independent verification confirms the Eiffel Tower is constructed of puddled iron (fer forgé in French) in a lattice structure.

## A note on the leftover doubt

The remaining uncertainty here isn't the kind you fix by reading one more page. It's baked into the question — differences in how sources define and phrase things, the ordinary fuzziness of language. More searching wouldn't sharpen it much.

## What would change our mind

We think it's only fair to say in advance what would overturn this, so the claim is falsifiable and not just asserted:

> Would require credible evidence that the Eiffel Tower's primary structural material was cast iron, steel, or another material distinct from wrought iron/puddled iron. The challenger's points about primary source access are valid but do not constitute falsification.

## Sources we actually read

- <https://en.wikipedia.org/wiki/Eiffel_Tower>
- <https://engineering.purdue.edu/MSE/about-us/gotmaterials/Buildings/patel.html>
- <https://www.perlego.com/index/technology-engineering/eiffel-tower>

---

*Investigated May 27, 2026. llama3.1:8b made the case; qwen3.5:9b tried to break it; minimax-m2.5:cloud ruled. Three models from different training lineages, so they don't share the same blind spots. Everything above is drawn straight from the debate record.*
