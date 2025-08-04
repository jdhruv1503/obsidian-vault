
Compound proposition T for all inputs -> Tautology
Compound proposition F for all inputs -> Contradiction

**Satisfiable formula:** Compound proposition is satisfiable if there exists a truth assignment to the propsitional  variables in it, that makes it evaluate to T.

A formula is valid if it is true for all values of its terms. Satisfiability refers to the existence of a combination of values to make the expression true. So in short, a proposition is satisfiable if there is at least one `true` result in its truth table, valid if all values it returns in the truth table are `true`.

**Satisfiability** -the other way of interpretation  
A propositional statement is satisfiable if and only if, its truth table is not contradiction.  
Not contradiction means, it could be a tautology also.

Hence, every tautology is also Satisfiable.  
However, Satisfiability doesn't imply Tautology.  

Another thing to note is, if a propositional statement is Tautology, then its always valid.

Thus, Tautology implies ( Satisfiability + Validity ).

---
# p -> q: 

Equivalent to p' v q (p' OR q)

| P   | Q   | P → Q |
| --- | --- | ----- |
| T   | T   | T     |
| T   | F   | F     |
| F   | T   | T     |
| F   | F   | T     |


p implies q; if p then q; q when p; p is a sufficient condition for q; q is a necessary condition for p

(p -> q is the same as q' -> p')

---

## Logical Equivalences

### Identity laws
$$P \land T = P, \quad P \lor F = P$$

### Domination laws
$$P \lor T = T, \quad P \land F = F$$

### Idempotent laws
$$P \land P = P, \quad P \lor P = P$$

### Negation law
$$P \land \neg P = F, \quad P \lor \neg P = T$$

### Double negation law
$$\neg (\neg P) = P$$

### Commutativity
$$P \land Q = Q \land P, \quad P \lor Q = Q \lor P$$

### Associativity
$$(P \land Q) \land R = P \land (Q \land R), \quad (P \lor Q) \lor R = P \lor (Q \lor R)$$

### Distributivity
$$P \land (Q \lor R) = (P \land Q) \lor (P \land R)$$
$$P \lor (Q \land R) = (P \lor Q) \land (P \lor R)$$

### De Morgan's laws
$$\neg (P \land Q) = \neg P \lor \neg Q$$
$$\neg (P \lor Q) = \neg P \land \neg Q$$


---


