
## **Backoff & Interpolation**
- **Backoff**: Prefer higher-order $n$-grams if available:  
  $$P_{\text{backoff}}(w_i | w_{i-2}w_{i-1}) = \begin{cases} 
  P_{\text{trigram}} & \text{if count}(w_{i-2}w_{i-1}w_i) > 0 \\
  \lambda P_{\text{bigram}} & \text{otherwise}
  \end{cases}$$
- **Linear Interpolation**: Weighted mix of $n$-grams:  
  $$P_{\text{interp}}(w_i | w_{i-2}w_{i-1}) = \lambda_1 P_{\text{trigram}} + \lambda_2 P_{\text{bigram}} + \lambda_3 P_{\text{unigram}}$$  
  - $\lambda$s optimized on held-out data.

---

## **Stupid Backoff**
- No discounting; use relative frequencies with fallback:  
  $$P_{\text{stupid}}(w_i | w_{i-k+1}^{i-1}) = \begin{cases} 
  \frac{c(w_{i-k+1}^i)}{c(w_{i-k+1}^{i-1})} & \text{if } c(w_{i-k+1}^i) > 0 \\
  0.4 \cdot P_{\text{stupid}}(w_i | w_{i-k+2}^{i-1}) & \text{otherwise}
  \end{cases}$$

---

## **Handling OOV Words**
- Replace OOV tokens with `<UNK>`:  
  - Train $P(\text{<UNK>})$ like other words.  
  - During decoding: $P(w) = P(\text{<UNK>})$ if $w \notin$ training vocabulary.

---

## **Good-Turing Smoothing**
- **Intuition**: Estimate unseen events using frequency of rare events.  
  - Let $N_c =$ number of $n$-grams with count $c$.  
  - Adjusted count for unseen ($c=0$):  
    $$c^*_{\text{unseen}} = \frac{N_1}{N}, \quad \text{where } N = \sum_c N_c \cdot c$$  
  - For seen $n$-grams:  
    $$c^* = (c + 1) \frac{N_{c+1}}{N_c}$$  
  - Example (Church & Gale 1991):  

| $c$ | $c^*$    |     |
| --- | -------- | --- |
| 0   | 0.000027 |     |
| 1   | 0.446    |     |
| 2   | 1.26     |     |

---

## **Absolute Discounting**
- Subtract fixed discount $d$ from counts:  
  $$P_{\text{AD}}(w_i | w_{i-1}) = \frac{\max(c(w_{i-1}, w_i) - d, 0)}{c(w_{i-1})} + \lambda(w_{i-1}) P_{\text{cont}}(w_i)$$  
  - $\lambda(w_{i-1})$ ensures probabilities sum to 1.

---

## **Kneser-Ney Smoothing**
- Uses **continuation probability** for better unseen $n$-gram estimation:  
  $$P_{\text{KN}}(w_i | w_{i-1}) = \frac{\max(c(w_{i-1}, w_i) - d, 0)}{c(w_{i-1})} + \lambda(w_{i-1}) P_{\text{cont}}(w_i)$$  
  - **Continuation Probability**:  
    $$P_{\text{cont}}(w_i) = \frac{|\{w_{i-1}: c(w_{i-1}, w_i) > 0\}|}{|\text{all bigrams}|}$$  

---

## **Perplexity**
- **Definition**: Inverse probability of test data, normalized by word count:  
  $$\text{PP}(W) = P(w_1, \dots, w_n)^{-\frac{1}{n}}$$  
- Using **Chain Rule** and **Bigram LM**:  
  $$\text{PP}(W) = \left( \prod_{i=1}^n \frac{1}{P(w_i | w_{i-1})} \right)^{\frac{1}{n}}$$  
- Lower perplexity = better model.

---

## **Problems with Statistical LMs**
1. **Data Sparsity**: Rare $n$-grams lead to zeros.  
2. **Fixed Context**: Limited to $n-1$ previous words.  
3. **Memory**: Storage grows as $O(V^n)$.  
4. **Generalization**: Fails on unseen word combinations.

---

## **Transition to Neural LMs**
- **Word Embeddings**: Continuous vectors capture semantic similarity.  
- **Neural Networks**: Model long-range dependencies (e.g., RNNs, Transformers).  
- **Scalability**: Handle large vocabularies and datasets efficiently.

---
## Training λ in Kneser-Ney vs Stupid Backoff

## Kneser-Ney Smoothing

The λ parameter is **not directly trained** but derived from counts and discounting:
$$
\lambda(w_{i-1}) = \frac{d}{c(w_{i-1})} \cdot |\{w: c(w_{i-1},w) > 0\}| $$

- **d**: Fixed discount factor (typically 0.75) 
- **c(w_{i-1})**: Count of context 
- **|\{w: ...\}|**: Number of unique words following context 

The discount **d** can be optimized using held-out data:

$$d = \arg\max_d \sum \log P_{KN}(w_i|w_{i-1}) \text{ on held-out data}$$

## Stupid Backoff

Uses **fixed λ (0.4)** without training.