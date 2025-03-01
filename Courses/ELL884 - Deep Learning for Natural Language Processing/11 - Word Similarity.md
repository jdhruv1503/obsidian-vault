

## Terminology
- **Lemma**: Base/dictionary form (e.g., "bank")
- **Wordform**: Inflected form (e.g., "banks", "sung")
- **Sense**: Specific meaning variant of a lemma (e.g., bank=financial institution vs. riverbank)

## Key Relationships
### Homonymy (Unrelated Meanings)
$$
\text{Example: } \text{bass}_{1} \text{(fish)} \text{ vs. } \text{bass}_{2} \text{(low voice)}
$$

### Polysemy (Related Meanings)
*Test via zeugma*:  
`✗ "The bank holds investments and river sediment"` → Two senses

### Synonymy
- **Perfect synonyms** rarely exist (register/tone differences)  
`purchase vs. buy` (formal vs. casual)

### Antonymy (Opposites)
$$
\begin{array}{lll}
\text{Complementary: } \text{alive/dead} & \text{Scalar: } \text{hot/cold} \\
\text{Reversive: } \text{rise/fall} 
\end{array}
$$

### Hyponymy/Hypernymy (IS-A)
$$
\text{car} \xrightarrow{\text{hyponym}} \text{vehicle} \xleftarrow{\text{hypernym}} \text{bus}
$$

### Meronymy (Part-Whole)
$$
\text{wheel} \xrightarrow{\text{meronym}} \text{car} \xleftarrow{\text{holonym}} \text{engine}
$$

---

## WordNet Structure
### Synset Example
```

{
"lemma": "chump",
"synset": ["chump1", "fool2", "gull1"...],
"gloss": "a gullible person",
"relations": {
"hypernym": "person",
"hyponym": ["dupe", "patsy"]
}
}

```

### Supersenses (Top-Level Categories)
![](https://cdn.mathpix.com/cropped/2025_03_01_39bbbd7418e7f50241c6g-24.jpg?height=1207&width=1978&top_left_y=200&top_left_x=89)

---

## Word Similarity Metrics

### Path-Based Similarity
$$
\text{sim}_{\text{path}}(c_1,c_2) = \frac{1}{\text{pathlen}(c_1,c_2)}
$$
**Example**:  
![](https://cdn.mathpix.com/cropped/2025_03_01_39bbbd7418e7f50241c6g-38.jpg?height=752&width=1278&top_left_y=319&top_left_x=1220)  
`sim(nickel, money)` uses path length 6 → $1/6 ≈ 0.17$

---

### Information Content (IC)
$$
\text{IC}(c) = -\log P(c)
$$
Where $P(c)$ = probability of encountering concept $c$ in corpus:
$$
P(c) = \frac{\sum_{w \in \text{words}(c)} \text{count}(w)}{N}
$$

---

### Resnik Similarity (Common Information)
$$
\text{sim}_{\text{Resnik}}(c_1,c_2) = \text{IC}(\text{LCS}(c_1,c_2))
$$
**LCS**: Lowest common subsumer in hierarchy  
**Example**:  
For `hill` (IC=-ln(0.0000189)) and `coast` (IC=-ln(0.0000216)):  
LCS = `geological-formation` (IC=-ln(0.00176)) → similarity = 6.34

---

### Lin Similarity (Ratio Metric)
$$
\text{sim}_{\text{Lin}}(c_1,c_2) = \frac{2 \times \text{IC}(\text{LCS}(c_1,c_2))}{\text{IC}(c_1) + \text{IC}(c_2)}
$$
**Derivation**: Balances common vs. total information  
**Example** (Hill/Coast):  
$$
\frac{2 \times 6.34}{10.51 + 10.86} ≈ 0.59
$$

---

### Jiang-Conrath Distance
$$
\text{dist}_{\text{JC}}(c_1,c_2) = \text{IC}(c_1) + \text{IC}(c_2) - 2 \times \text{IC}(\text{LCS}(c_1,c_2))
$$
*Inversely related to similarity*

---

### Extended Lesk Algorithm
Score = ∑ (overlapping n-grams$^2$) between:  
- Node glosses  
- Hypernym/hyponym glosses  

**Example**:  
`Decal` vs. `Drawing paper`: score += 5 (for "specially prepared paper")

---

## Evaluation
### Intrinsic (Human Correlation)
- **Dataset**: Wordsim353 (0-10 similarity ratings)  
`plane-car` = 5.77 human vs. algorithm score

### Extrinsic (Task Performance)
- **TOEFL Test**: Choose synonym from options  
*Levied* → *imposed* (algorithm accuracy ≈ 92%)