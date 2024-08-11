
A super naive version of candidate elimination.
\
The LIST-THEN-ELIMINATE algorithm first initializes the version space to con-
tain all hypotheses in H, then eliminates any hypothesis found inconsistent with
any training example. The version space of candidate hypotheses thus shrinks
as more examples are observed, until ideally just one hypothesis remains that is
consistent with all the observed examples. 

This, presumably, is the desired target
concept. If insufficient data is available to narrow the version space to a single
hypothesis, then the algorithm can output the entire set of hypotheses consistent
with the observed data.

In principle, the LIST-THEN-ELIMINATE algorithm can be applied whenever
the hypothesis space H is finite. It has many advantages, including the fact that it
is guaranteed to output all hypotheses consistent with the training data. Unfortu-
nately, it requires exhaustively enumerating all hypotheses in H-an unrealistic
requirement for all but the most trivial hypothesis spaces.

![[Pasted image 20240812041723.png]]