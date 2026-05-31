# Findings

This is where I keep the write-ups from TRIAD, a little system I've been building that checks claims by making language models argue with each other instead of trusting any single one of them.

The idea is simple. One model makes the best case for a claim. Another is told to pull it apart and go hunting for anything that contradicts it. A third reads both sides and rules. Before any of that happens, the claim gets chopped into the smallest facts it's made of, and each fact is checked against pages the system actually fetches and reads. Nothing here is taken on faith, including the easy stuff.

Each finding below is written up as a short paper rather than dumped as data. There's a PDF if you want to read it properly and a Markdown version if you'd rather skim it here.

## The papers so far

- **[Mount Everest is a mountain located within the Himalayas, which spans across the region near the Nepal-China border](papers/mount-everest-is-a-mountain-located-within-the-himalayas-whi.md)** — We tried to knock this down and couldn't. Here's what we checked and why it stands. _(stands, 95% confidence)_ &nbsp;·&nbsp; [PDF](papers/mount-everest-is-a-mountain-located-within-the-himalayas-whi.pdf)
- **[The Eiffel Tower is a wrought-iron lattice tower](papers/the-eiffel-tower-is-a-wrought-iron-lattice-tower.md)** — We tried to knock this down and couldn't. Here's what we checked and why it stands. _(stands, 85% confidence)_ &nbsp;·&nbsp; [PDF](papers/the-eiffel-tower-is-a-wrought-iron-lattice-tower.pdf)

## How to read the confidence numbers

A number like 85% isn't a popularity score. It's roughly how much I'd bet the claim is right given what survived the argument. A fact can be perfectly true and still land below 100% if the only sources online phrase it loosely or aren't primary. There's a longer explanation of the method in [notes/how-this-works.md](notes/how-this-works.md).

---

*Last updated May 31, 2026. Findings are added automatically as the system confirms them.*
