
This is the first part of [syntax]!

Splitting into noun, pronoun, preposition, adverb, etc!
Important! It helps understand chunking of a sentence - info about word and neighbors

---
## Lists of POS

Also called - Word classes, Morphological class, Lexical tag

| Tagset                       | Classes     |
| ---------------------------- | ----------- |
| Penn, Treebank (early 2000s) | 43 classes  |
| C5 tagset                    | x classes   |
| Brown corpus                 | 87 classes  |
| C7 tagset                    | 146 classes |

---
### Broad subcategories

- *Closed class*: Prepositions etc. Mostly stay the same
- *Open class*: Whose entities can be expanded


## Open classes

### Nouns (open)

- Proper
- Common (countable or mass)

### Verb (open)

- Doing

### Adjectives (open)

- Describe stuff

### Adverb (open)

- Modify things (often verbs)
- Directional, degree, manner, temporal

## Closed classes

- Prepositions
- Determiner
- Pronoun
- Conjunctions
- Auxilary verbs
- Numerals
- Interjections
- Negations
- Politeness markers
- Greetings
---
# The task

Take a string of words and a tagset. Automatically assign a tag (DISAMBIGUATE)

---
# The solutions

Few ways:

- *Rule-based* taggers: Use a dict to assign potential POS to words; use handwritten disambiguation rules to narrow down. e.g. ENGTWOL tagger (1995)
- *Stochastic* taggers: [[Hidden Markov Model]]
- *Hybrid* taggers: i.e. Brill tagger
---
