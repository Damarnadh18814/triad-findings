> This paper was written by the AI Scientist loop itself (LLM write-up over a real experiment). Included verbatim.

# Inference-Time Recursion Scaling in a Tiny Recursive Model

## Abstract
Here is a proposed hypothesis:

Hypothesis: As the number of inference-time recursion steps increases, the Tiny Recursive Model's (TRM) ability to solve spatial reasoning puzzles will initially improve but eventually plateau or even degrade due to increasing computational overhead and decreasing generalization capabilities.

This hypothesis can be tested by sweeping the number of allowed inference-time recursion steps (e.g. 1-10 iterations) and measuring the corresponding puzzle solve-rates, with a clear improvement in performance expected up to a certain threshold beyond which it will stagnate or decline.

## Method
We evaluate a trained Tiny Recursive Model (354,821 params) on 800 held-out 4x4 Sudoku puzzles, sweeping inference recursion depth.

## Results
- depth 1: solve-rate 0.0
- depth 2: solve-rate 0.1875
- depth 3: solve-rate 0.5813
- depth 4: solve-rate 0.7588
- depth 6: solve-rate 0.8263
- depth 8: solve-rate 0.8363
- depth 10: solve-rate 0.835
- depth 12: solve-rate 0.835
- depth 16: solve-rate 0.8188

Peak 0.8363 at depth 8; saturation at depth 6.

## Conclusion
Hypothesis supported: True. Solve-rate rises with recursion (gain 0.8363) then saturates.