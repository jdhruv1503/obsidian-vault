
A weighted FSM with probabilities on the arcs. All outbound $P$ must sum to 1.

*Markov chain*: A special case of WFST in which input seq uniquely determines which states the automaton will go through. i.e. The states are actually observations. Essentially, you can assign a probability to any unambiguous sequence.

*Hidden Markov Model*: States are hidden! We don't know what state we're in

$$
Q=q_1,q_2... states \\
A=a_{11}, a_{12}... \text{transition probablility matrix}
$$