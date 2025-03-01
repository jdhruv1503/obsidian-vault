
## **1. Syntax Fundamentals**
- **Definition**: Study of how words are arranged to form grammatical structures.  
- **Applications**: Grammar checking, machine translation, information extraction.  

---

## **2. Constituency (Phrase Structure)**
### **Key Concepts**
- **Constituent**: Group of words acting as a single unit (e.g., noun phrase "the brown fox").  
- **Phrase Structure Grammar**: Uses context-free grammar (CFG) rules to model nested constituents.  

### **Example CFG Rules**
$$
\begin{align*}
S &\rightarrow NP\ VP \\
NP &\rightarrow DT\ NN \mid NN \\
VP &\rightarrow VB\ NP \\
DT &\rightarrow \text{"the"} \mid \text{"a"} \\
NN &\rightarrow \text{"fox"} \mid \text{"dog"} \\
VB &\rightarrow \text{"jumps"} \mid \text{"runs"} \\
\end{align*}
$$

### **Constituency Tree Example**
**Sentence**: "Fed raises interest rates."  
```
(S
  (NP (N Fed))
  (VP
    (V raises)
    (NP (N interest) (N rates))
  )
)
```

---

## **3. Dependency Structure**
### **Key Concepts**
- **Dependency Grammar**: Represents binary relations between words (head-dependent).  
- **Dependency Labels**:  
  - `nsubj` (nominal subject), `dobj` (direct object), `det` (determiner), `amod` (adjectival modifier).  

### **Dependency Tree Example**
**Sentence**: "The boy put the tortoise on the rug."  
```
root
└── put (verb)
    ├── boy (nsubj)
    │   └── The (det)
    ├── tortoise (dobj)
    │   └── the (det)
    └── on (prep)
        └── rug (pobj)
            └── the (det)
```

---

## **4. Classical vs. Statistical Parsing**
### **Classical Parsing (Pre-1990)**
- **Approach**: Handcrafted CFG rules + categorical constraints.  
- **Limitations**:  
  - Low coverage (30% sentences unparsable).  
  - Exponential parses for simple sentences (e.g., 592 parses for a 10-rule grammar).  

### **Statistical Parsing**
- **Solution**: Use treebanks to train probabilistic models.  
- **Advantages**:  
  - Handles ambiguity via probability scores.  
  - Broad coverage with loose grammars.  

---

## **5. Treebanks**
- **Definition**: Corpus of sentences paired with correct parse trees.  
- **Penn Treebank**:  
  - 1M words from Wall Street Journal (1987-1989).  
  - Used to train/evaluate parsers and taggers.  

### **Treebank Utility**
- Provides annotated data for:  
  - Grammar induction.  
  - Statistical models (e.g., PCFG).  
  - Evaluation benchmarks.  

---

## **6. Attachment Ambiguity**
### **Problem**
- Multiple ways to attach phrases (e.g., PPs, infinitives).  
- **Example**: "John wrote the book with a pen in the room."  
  - PP attachments grow exponentially with phrases.  

### **Catalan Numbers**
- Counts distinct bracketings (parses) for \( n \) phrases:  
$$
C_n = \frac{(2n)!}{(n+1)!n!}
$$
- **Growth**:  
  - \( C_3 = 5 \), \( C_4 = 14 \), \( C_5 = 42 \) (exponential).  

### **Example Ambiguity**
**Sentence**: "The board approved its acquisition by Royal Trustco Ltd. of Toronto for $27 a share at its meeting."  
- Each PP ("by...", "of...", "for...", "at...") creates branching ambiguity.  

---

## **7. Parsing Algorithms**
### **Key Challenges**
1. **Avoid Repeated Work**: Use dynamic programming (e.g., CKY algorithm).  
2. **Resolve Ambiguity**: Select highest-probability parse using statistical models.  

### **CKY Algorithm**
- **Input**: Sentence + CNF grammar.  
- **Output**: Parse table with possible constituents.  
- **Complexity**: \( O(n^3) \) for sentence length \( n \).  

---

## **8. Probabilistic Context-Free Grammar (PCFG)**
- **Definition**: CFG with probabilities on rules.  
- **Probability of a Parse Tree**:  
$$
P(T) = \prod_{r \in T} P(r)
$$
- **Example**:  
  - Rule probabilities: $$ P(VP \rightarrow VB\ NP) = 0.7 ,  P(VP \rightarrow VB) = 0.3 $$

---

## **9. Evaluation Metrics**
- **PARSEVAL Measures**:  
  - **Precision**: \( \frac{\text{Correct Constituents}}{\text{Proposed Constituents}} \).  
  - **Recall**: \( \frac{\text{Correct Constituents}}{\text{Gold Constituents}} \).  
  - **F1 Score**: Harmonic mean of precision/recall.  