
## **N-gram Models**
- **Goal**: Compute probability of a word sequence $P(W)$ or next word $P(w_i | w_1, \dots, w_{i-1})$.  
- **Markov Assumption**: Approximate using limited history:  
  $$P(w_i | w_1, \dots, w_{i-1}) \approx P(w_i | w_{i-k}, \dots, w_{i-1})$$  
  - **Unigram**: $P(W) \approx \prod_i P(w_i)$  
  - **Bigram**: $P(w_i | w_{i-1})$  
  - **N-gram**: Extends to $n$ previous words.

---

## **Maximum Likelihood Estimation (MLE)**
- **Bigram Probability**:  
  $$P_{\text{MLE}}(w_i | w_{i-1}) = \frac{c(w_{i-1}, w_i)}{c(w_{i-1})}$$  
  - $c(w_{i-1}, w_i)$: Count of bigram $(w_{i-1}, w_i)$.  
  - Example:  
    $$P(\text{am} | \text{I}) = \frac{c(\text{I, am})}{c(\text{I})} = \frac{2}{3} \text{ (from corpus)}$$

---

## **Laplace (Add-1) Smoothing**
- **Purpose**: Handle zero probabilities for unseen N-grams.  
- **Formula**:  
  $$P_{\text{Add-1}}(w_i | w_{i-1}) = \frac{c(w_{i-1}, w_i) + 1}{c(w_{i-1}) + V}$$  
  - $V$: Vocabulary size.  
- **Reconstituted Counts**: Adjust raw counts post-smoothing:  
  $$c^*(w_{n-1}w_n) = \frac{(c(w_{n-1}w_n) + 1) \times c(w_{n-1})}{c(w_{n-1}) + V}$$  

### **Example (Berkeley Restaurant Corpus)**
- Raw: $c(\text{want}, \text{to}) = 608$  
- Smoothed:  
  $$P_{\text{Add-1}}(\text{to} | \text{want}) = \frac{608 + 1}{927 + 8} \approx 0.66$$  

---

## **Practical Considerations**
- **Log Probabilities**: Avoid underflow by summing logs:  
  $$\log P(W) = \sum_i \log P(w_i | \text{context})$$  
- **Sparsity**: Higher-order N-grams (e.g., quadrigrams) suffer from data sparsity:  
  - Shakespeare corpus: 99.96% of possible bigrams unseen.  

---

## **Issues with N-grams**
1. **Zero Probabilities**: Unseen N-grams break models.  
   - Smoothing redistributes probability mass.  
2. **Generalization**: Models overfit to training corpus.  
   - Test corpus often differs (e.g., "denied the offer" unseen in training).  
3. **Blunt Smoothing**: Add-1 assigns uniform probability to all unseen events.  
   - Better methods: Backoff, Interpolation, Kneser-Ney.  

---

## **Key Formulas Summary**
| Concept | Formula |  
|---|---|  
| MLE Bigram | $$P_{\text{MLE}}(w_i \| w_{i-1}) = \frac{c(w_{i-1}, w_i)}{c(w_{i-1})}$$ |  
| Laplace Smoothed Bigram | $$P_{\text{Add-1}}(w_i \| w_{i-1}) = \frac{c(w_{i-1}, w_i) + 1}{c(w_{i-1}) + V}$$ |  
| Log Probability | $$\log P(W) = \sum_i \log P(w_i \| \text{context})$$ |  