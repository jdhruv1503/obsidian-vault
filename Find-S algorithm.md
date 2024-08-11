One way is to begin
with the most specific possible hypothesis in H, then generalize this hypothesis
each time it fails to cover an observed positive training example. (We say that
a hypothesis "covers" a positive example if it correctly classifies the example as
positive.)

This is the **Find-S algo**:

![[Pasted image 20240812040737.png]]

Because we are moving from **the most specific** hypothesis and assume that $c$ exists in $H$, we never need to look at negative examples!! $c$ will always be less or equal specificity as compared to our current hypothesis, and so if training samples are well formed wrt $c$, we are guaranteed to never false-positive a negative example.

### Limitations about Find-S

• Has the learner converged to the current target concept?
• No way to determine if it has found the _only_ hypothesis that is consistent with
the target concept
• Or there are many other consistent hypotheses as well
• Why prefer the most specific hypothesis?
• In case of multiple hypotheses consistent with the target concept, why to
consider the most specific one?
• Are the training example consistent?
• What if a few training instances are corrupted?
• What if there are several maximally specific consistent hypotheses?
• Find-S should be backtracked to generalize the hypothesis!!