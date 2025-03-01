
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

$$d = \arg\max_d \sum \log P_{KN}(w_i|w_{i-1}) \text{ on held-out data}$$1[4](https://en.wikipedia.org/wiki/Kneser%E2%80%93Ney_smoothing)

## Stupid Backoff

Uses **fixed λ (0.4)** without training:

S(w_i|context) = \begin{cases} \frac{c(context,w_i)}{c(context)} & \text{if count > 0} \\ 0.4 \cdot S(w_i|shorter\ context) & \text{otherwise} \end{cases}$$[3][7] | Method | λ Training | Formula | |-----------------|-------------------------------------|-------------------------------------------------------------------------| | Kneser-Ney | Derived from counts | λ dependent on context diversity and discount factor | | Linear Interpolation | Trained via held-out optimization | $\arg\max_{\lambda} \sum \log(\lambda_1P_3 + \lambda_2P_2 + \lambda_3P_1)$ [1] | | Stupid Backoff | Fixed (0.4) | No probability normalization, pure backoff with penalty | Key differences: - Kneser-Ney λ is **context-sensitive** and mathematically derived - Stupid Backoff uses **constant penalty** (0.4) per backoff step - Linear interpolation (different from backoff) trains λ weights using EM on held-out data[1][8]

### Citations:

1. [https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/50683439/b5e0a5e8-6394-4505-b846-1b4ff76f0111/lec4-1.pdf](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/50683439/b5e0a5e8-6394-4505-b846-1b4ff76f0111/lec4-1.pdf)
2. [https://iq.opengenus.org/kneser-ney-smoothing-absolute-discounting/](https://iq.opengenus.org/kneser-ney-smoothing-absolute-discounting/)
3. [https://stackoverflow.com/questions/16383194/stupid-backoff-implementation-clarification](https://stackoverflow.com/questions/16383194/stupid-backoff-implementation-clarification)
4. [https://en.wikipedia.org/wiki/Kneser%E2%80%93Ney_smoothing](https://en.wikipedia.org/wiki/Kneser%E2%80%93Ney_smoothing)
5. [https://cran.r-project.org/web/packages/sbo/vignettes/sbo.html](https://cran.r-project.org/web/packages/sbo/vignettes/sbo.html)
6. [https://www.scaler.com/topics/nlp/backoff-in-nlp/](https://www.scaler.com/topics/nlp/backoff-in-nlp/)
7. [https://1cademy.com/node/stupid-backoff/CfjdPOnpZoC4lfo3d27t](https://1cademy.com/node/stupid-backoff/CfjdPOnpZoC4lfo3d27t)
8. [http://aritter.github.io/courses/5525_slides/language%20models.pdf](http://aritter.github.io/courses/5525_slides/language%20models.pdf)
9. [https://www.researchgate.net/post/A-simple-numerical-example-for-Kneser-Ney-Smoothing](https://www.researchgate.net/post/A-simple-numerical-example-for-Kneser-Ney-Smoothing)
10. [https://stats.stackexchange.com/questions/246011/a-simple-numerical-example-for-kneser-ney-smoothing](https://stats.stackexchange.com/questions/246011/a-simple-numerical-example-for-kneser-ney-smoothing)
11. [https://www.youtube.com/watch?v=ody1ysUTD7o](https://www.youtube.com/watch?v=ody1ysUTD7o)
12. [https://rpubs.com/pferriere/dscapreport](https://rpubs.com/pferriere/dscapreport)
13. [https://www.cs.cornell.edu/courses/cs5740/2019sp/lectures/07-lm.pdf](https://www.cs.cornell.edu/courses/cs5740/2019sp/lectures/07-lm.pdf)
14. [https://www.cse.iitd.ac.in/~mausam/courses/csl772/autumn2014/lectures/07-lm.pdf](https://www.cse.iitd.ac.in/~mausam/courses/csl772/autumn2014/lectures/07-lm.pdf)
15. [https://www.studocu.com/in/document/vels-institute-of-science-technology-and-advanced-studies/natural-language-processing/smoothing-and-back-off/105257436](https://www.studocu.com/in/document/vels-institute-of-science-technology-and-advanced-studies/natural-language-processing/smoothing-and-back-off/105257436)
16. [https://web.stanford.edu/~jurafsky/slp3/3.pdf](https://web.stanford.edu/~jurafsky/slp3/3.pdf)
17. [https://web.eecs.umich.edu/~wangluxy/archive/neu_courses/cs6120_sp2018/slides_cs6120_sp18/lm.pdf](https://web.eecs.umich.edu/~wangluxy/archive/neu_courses/cs6120_sp2018/slides_cs6120_sp18/lm.pdf)
18. [http://smithamilli.com/blog/kneser-ney/](http://smithamilli.com/blog/kneser-ney/)
19. [https://stackoverflow.com/questions/35242155/kneser-ney-smoothing-of-trigrams-using-python-nltk](https://stackoverflow.com/questions/35242155/kneser-ney-smoothing-of-trigrams-using-python-nltk)
20. [https://github.com/yandexdataschool/nlp_course/issues/30](https://github.com/yandexdataschool/nlp_course/issues/30)
21. [https://www.youtube.com/watch?v=9SlJ76HtjoE](https://www.youtube.com/watch?v=9SlJ76HtjoE)
22. [https://web.stanford.edu/~jurafsky/slp3/slides/LM_4.pdf](https://web.stanford.edu/~jurafsky/slp3/slides/LM_4.pdf)
23. [https://github.com/hanks/Natural-Language-Processing/blob/master/pa2-autocorrect-v1/python/StupidBackoffLanguageModel.py](https://github.com/hanks/Natural-Language-Processing/blob/master/pa2-autocorrect-v1/python/StupidBackoffLanguageModel.py)

---

Answer from Perplexity: [pplx.ai/share](https://www.perplexity.ai/search/pplx.ai/share)