
## Morphology Definition
- Study of word formation and relationships between words.
- Involves breaking words into **morphemes** (stems, prefixes, suffixes).

---

## Key Concepts
- **Inflection**:  
  - Adds grammatical info without changing word class (e.g., "fox" → "foxes", "walk" → "walked").  
- **Derivation**:  
  - Changes word class (e.g., "computer" (noun) → "computerize" (verb)).  
- **Orthographic Rules**:  
  - General spelling changes (e.g., "city" → "cities").  
- **Morphological Rules**:  
  - Exceptions (e.g., "goose" → "geese").  

---

## Porter Stemmer (Overview)
- Rule-based algorithm to **stem** words (reduce to root form).  
- Uses heuristic replacement rules applied in **sequential steps**.  

### How it Works (Simplified)
1. **Step 1**: Replace suffixes (e.g., "sses" → "ss", "ies" → "i").  
2. **Step 2**: Handle verb forms (e.g., "singing" → "sing").  
3. **Cleanup**: Remove remaining suffixes (e.g., "computer" → "compute").  

### Examples
- `computers` → `computer` → `compute`  
- `controlling` → `control`  

### Limitations
- Heuristic; may produce errors (e.g., "elephants" → "eleph" instead of "elephant").