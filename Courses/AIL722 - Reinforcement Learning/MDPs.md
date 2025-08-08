
Finite discrete fully-observable time MDP is a tuple (S, A, D, T, R)

S: finite set of possible states (state space)
A: finite set of all actions an agent can take
D: finite/infinite set of natural numbers (timesteps 1,2,3,...n)
T: SxAxSxD -> [0,1] is a transition function, probability of going to state s2 if action a when in state s1. written as T(s1, a, s2, 1)
R:

## Policy

Policy; rule for action selection that works in any state
Mapping from each state s to an action a

History dependent: history H x A -> [0,1] Can give a distrib over actions for EVERY possible history, or deterministic (H -> A map). Difficult because potentially infinite domain

Markovian: Probabilistic/deterministic history dependent that does not depend on history, only current state and timestamp

Stationary Markovian: Depends only on the state! Not timestamp. Practically interesting MDPs have at least one optimal stationary Markovian solution.

## Value function

Mapping H to [-inf, inf]

If stationary Markovian: Mapping S to [-inf, inf]

Some utility function of the sequence of rewards you get by following a policy -> can be expected sum, etc

## Optimality

Optimal MDP solution: A policy $\pi$ where its value function (Called optimal value function) dominates all other value functions for all other histories h. For all histories h, $V^{*}(h) >= V^{\pi}(h)$

## Finite Horizon MDP

Finite number of timesteps

