## 4. Attack

For each row in the grid, apply every selected attack that fits the element. After **every** action, check the oracles. Keep an action log: step number, action, element, result.

For monkey attacks, generate the action order from the seed with a small script in the scratchpad (a seeded PRNG such as mulberry32 over the grid rows and the attack inputs), save the generated list, and play it back step by step, so the same seed replays the same run.

On a break: screenshot, capture console/log output, record minimal repro steps, then reset to a clean state and continue. Never stop at the first break.

Done when every grid row × selected attack is marked `break`, `held`, or `n/a` with a reason.
