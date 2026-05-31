# How this works

A plain-language account of what happens before anything ends up in this repo.

## Three models, on purpose

Most fact-checking with a language model has one obvious flaw: you're asking the same model that might be wrong to tell you whether it's wrong. It tends to agree with itself. So instead of one model, there are three, and they're deliberately picked to come from different training lineages so they don't share the same blind spots.

One model is the proposer. Its job is to make the strongest honest case for a claim and to go find sources for it. The second is the challenger, and its instructions are blunt: disagree, look for counter-evidence, try to break this. The third is the judge. It forms its own view first, before reading either side's full argument, so it isn't just anchored to whoever spoke last. Then it weighs both and rules.

## Claims get broken into pieces first

“Mount Everest is in the Himalayas, on the Nepal–China border” is really three separate facts wearing one sentence. Each is checked on its own. That way a claim can't sneak through because the gist sounds right while one piece is actually wrong.

Every piece gets checked against pages the system fetches itself. The match score you see in the papers is how closely the source text actually supports the fact. A low score isn't an accusation; often it just means the source says the right thing in different words, or that the clean primary source isn't sitting on the open web.

## The anti-agreement checks

There's a specific test for sycophancy, the habit models have of telling you what you want to hear. After a claim is argued, the model that defended it is asked to switch sides and argue the opposite. If it flips easily, that's a warning sign that it never really held the position. If the two stances stay far apart, the original view was probably held for reasons. The papers report that divergence number where it applies.

## What a verdict means

The judge can only return one of three things: confirmed, rejected, or contested, and it can't confirm anything below 60% confidence or without checking at least one source itself. It's also required to write down, in advance, what would change its mind. That last part matters: a claim you can't imagine being wrong isn't a finding, it's a belief.
