
## **Definitions**
- **Edit Distance**: Minimum number of operations (insertion, deletion, substitution) required to transform string `X` into string `Y`.  
- **Levenshtein Distance**: A variant where substitution costs **2**, while insertion/deletion cost **1** (as defined in the PDF).  
  *(Note: Standard Levenshtein often uses substitution cost = 1; this course uses a modified version.)*

![[Pasted image 20250301140417.png]]
---

## **Complexity**
- **Time**: \(O(nm)\) (filling the table).  
- **Space**: \(O(nm)\) (storing the table).  
- **Backtrace**: \(O(n + m)\) (path reconstruction).

---

## **Applications**
- Spell correction (e.g., "Appl" → "Apple").  
- Sequence alignment in computational biology.  
- Machine translation evaluation.  
- Speech recognition error analysis.