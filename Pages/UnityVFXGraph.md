# Unity VFX Graph

## Common Issues and Corner Cases in VFX Graph

### Random Seed Convergence on Apple silicon

When generating child particles from a single parent particle via GPU Event,
the particle IDs and seeds in each child particle converge to a few variants.
This issue only occurs in Editor during play mode and does not reproduce in
edit mode or a build player.

A minimal project to reproduce the issue can be found here:
https://github.com/keijiro/VfxRandomTest

I've already reported the issue, but haven't found a workaround yet.

https://issuetracker.unity3d.com/issues/random-number-operator-in-a-child-system-only-has-set-limited-seeds-for-particle-spawning-in-a-visual-effect-when-entering-the-play-mode

The issue is going to be resolved by a fix for another GPU Event issue.
