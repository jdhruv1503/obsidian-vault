

# Probabilistic Context-Free Grammars (PCFGs)

## Key Components of CFG
Formally defined as quadruple:  
$$G = (T, N, S, R)$$
- **T**: Terminal symbols (words)
- **N**: Non-terminal symbols (grammatical categories)
- **S**: Start symbol  
- **R**: Production rules $X \rightarrow \gamma$ where $X \in N$, $\gamma \in (N \cup T)^*$

## Extended PCFG Model
$$G = (T, C, N, S, L, R, P)$$
Adds:
- **C**: Preterminal symbols (POS tags)
- **L**: Lexicon rules $X \rightarrow w$ where $X \in C$, $w \in T$
- **P**: Probability function satisfying:
$$\forall X \in N,\sum_{X \rightarrow \gamma \in R} P(X \rightarrow \gamma) = 1$$

## Probability Calculations
### Tree Probability
For parse tree $t$ with rules $r_1,...,r_n$:
$$P(t) = \prod_{i=1}^n P(r_i)$$

### Sentence Probability
Sum over all valid parses:
$$P(s) = \sum_{t \in \text{parses}(s)} P(t)$$

## Chomsky Normal Form (CNF)
### Transformation Steps
1. **Remove ε-productions**:
   - Replace $A \rightarrow ε$ with combinations of parent rules
   
2. **Eliminate unit productions**:
   - For $A \rightarrow B$, add all $A \rightarrow \gamma$ where $B \rightarrow \gamma$
   
3. **Binarize n-ary rules**:
   Convert $X \rightarrow Y_1 Y_2 ... Y_n$ into:
```

X → Y_1 @X_1
@X_1 → Y_2 @X_2
...
@X_{n-2} → Y_{n-1} Y_n

```

### CNF Example Transformation
Original rule:
```

VP → V NP PP

```
Becomes:
```

VP → V @VP_1
@VP_1 → NP PP

```

## CKY Parsing Algorithm
### Dynamic Programming Table
For sentence $w_1...w_n$:

| Span Length | Cell [i,j] Contents                   |
|-------------|---------------------------------------|
| 1           | All POS tags for $w_i$                |
| >1          | All possible constituents spanning i-j |

### Recursive Calculation
For span [i,j] and split point k:
$$\text{score}[i,j,X] = \max_{\substack{Y,Z,k}} \text{score}[i,k,Y] \times \text{score}[k,j,Z] \times P(X \rightarrow YZ)$$

### Viterbi Semi-Ring Version
Keeps backpointers to reconstruct max-probability parse:
$$\text{back}[i,j,X] = \arg\max_{Y,Z,k} (\text{prob})$$

## Parsing Example
**Sentence**: "people fish tanks with rods"

### Probability Calculation Steps
For parse tree $t_1$:
$$
\begin{aligned} 
P(t_1) &= 1.0 \times 0.7 \times 0.4 \times 0.5 \times 0.6 \times 0.7 \times 1.0 \times 0.2 \times 1.0 \times 0.7 \times 0.1 \\ 
&= 0.0008232 
\end{aligned}
$$

## Evaluation Metrics
### Constituency Parsing
- **Labeled Precision**: 
  $$\frac{\text{Correct Brackets}}{\text{Predicted Brackets}}$$
  
- **Labeled Recall**: 
  $$\frac{\text{Correct Brackets}}{\text{Gold Brackets}}$$
  
- **F1**: 
  $$2 \times \frac{P \times R}{P + R}$$

### Example Calculation
| Metric        | Calculation   | Result  |
|---------------|---------------|---------|
| Precision     | 3/7           | 42.9%   |
| Recall        | 3/8           | 37.5%   |
| F1            | 2*(.429*.375)/.804 | 40.0% |

## Key Algorithms
```

# CKY Pseudocode

def cky_parse(sentence):
n = len(sentence)
chart = init_3d_array(n+1, n+1, num_nonterminals)

    # Base case - lexical lookup
    for i in 0..n-1:
        for (X -> w) in lexicon:
            if w == sentence[i]:
                chart[i][i+1][X] = P(X->w)
    
    # Recursive case
    for span in 2..n:
        for i in 0..n-span:
            j = i + span
            for k in i+1..j-1:
                for (X -> Y Z) in grammar:
                    prob = chart[i][k][Y] * chart[k][j][Z] * P(X->YZ)
                    if prob > chart[i][j][X]:
                        chart[i][j][X] = prob
                        back[i][j][X] = (k,Y,Z)
    
    return chart, back
```

## Key Observations
- Binarization reduces time complexity to **O(n³|N|³)**
- CNF enables efficient parsing but requires careful detransformation
- Practical implementations handle unary rules through iterative relaxation:

```

while changes:
for all spans [i,j]:
if A → B and B exists in [i,j]:
add A with prob P(A→B)*chart[i,j,B]

```