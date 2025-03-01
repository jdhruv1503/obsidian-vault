
## **Markov Chains**
- **Definition**: A Markov chain is a stochastic model describing a sequence of states where the probability of transitioning to the next state depends **only on the current state** (Markov Assumption).  
- **Components**:
  - **States**: $Q = \{q_1, q_2, \dots, q_N\}$  
  - **Transition Matrix**: $A = [a_{ij}]$, where $a_{ij} = P(q_{t+1}=j \mid q_t=i)$ and $\sum_j a_{ij} = 1$.  
  - **Initial Distribution**: $\pi = [\pi_1, \pi_2, \dots, \pi_N]$, where $\pi_i = P(q_1=i)$.  

### **Example: Weather Prediction**
- States: $\{\text{HOT}, \text{COLD}\}$  
- Transition Matrix:  
  $$A = \begin{bmatrix}
  0.6 & 0.4 \\
  0.3 & 0.7 \\
  \end{bmatrix}$$  
- Initial Distribution: $\pi = [0.5, 0.5]$  

**Probability of a Sequence**:  
For sequence $Q = (\text{HOT}, \text{HOT}, \text{COLD})$:  
$$
P(Q) = \pi_{\text{HOT}} \cdot a_{\text{HOT→HOT}} \cdot a_{\text{HOT→COLD}} = 0.5 \times 0.6 \times 0.4 = 0.12
$$
![[Pasted image 20250301150451.png]]

---

## **2. Hidden Markov Models (HMMs)**
- **Extension of Markov Chains**: States are **hidden**, and observations depend on states.  
- **Components**:  
  - **States**: $Q = \{q_1, \dots, q_N\}$  
  - **Observations**: $O = \{o_1, \dots, o_T\}$ from vocabulary $V$.  
  - **Transition Matrix**: $A = [a_{ij}]$, $a_{ij} = P(q_{t+1}=j \mid q_t=i)$.  
  - **Emission Probabilities**: $B = [b_j(o_t)]$, $b_j(o_t) = P(o_t \mid q_t=j)$.  
  - **Initial Distribution**: $\pi = [\pi_1, \dots, \pi_N]$.  

### **Key Assumptions**:
1. **Markov Assumption**:  
   $$P(q_t \mid q_{1:t-1}) = P(q_t \mid q_{t-1})$$  
2. **Output Independence**:  
   $$P(o_t \mid q_{1:t}, o_{1:t}) = P(o_t \mid q_t)$$  ![[Pasted image 20250301150524.png]]

---

## **3. Three Fundamental Problems for HMMs**
### **Problem 1: Likelihood Computation (Forward Algorithm)**
**Goal**: Compute $P(O \mid \lambda)$, the probability of observations $O$ given model $\lambda = (A, B)$.  

#### **Forward Algorithm**:
1. **Initialization**:  
   $$\alpha_1(j) = \pi_j \cdot b_j(o_1) \quad \forall j \in Q$$  
2. **Recursion**:  
   $$\alpha_t(j) = \sum_{i=1}^N \alpha_{t-1}(i) \cdot a_{ij} \cdot b_j(o_t) \quad \forall t > 1, j \in Q$$  
3. **Termination**:  
   $$P(O \mid \lambda) = \sum_{i=1}^N \alpha_T(i)$$  

**Example**:  
For $O = (3, 1, 3)$ and hidden states $\{\text{HOT}, \text{COLD}\}$:  
$$
\alpha_3(\text{COLD}) = \alpha_2(\text{HOT}) \cdot a_{\text{HOT→COLD}} \cdot b_{\text{COLD}}(3)
$$

---

### **Problem 2: Decoding (Viterbi Algorithm)**
**Goal**: Find the most probable hidden state sequence $Q^* = \arg\max_Q P(Q \mid O, \lambda)$.  

#### **Viterbi Algorithm**:
1. **Initialization**:  
   $$v_1(j) = \pi_j \cdot b_j(o_1), \quad \text{backpointer}_1(j) = 0 \quad \forall j$$  
2. **Recursion**:  
   $$v_t(j) = \max_{i=1}^N \left[ v_{t-1}(i) \cdot a_{ij} \cdot b_j(o_t) \right]$$  
   $$\text{backpointer}_t(j) = \arg\max_{i=1}^N \left[ v_{t-1}(i) \cdot a_{ij} \cdot b_j(o_t) \right]$$  
3. **Termination**:  
   $$P^* = \max_{i=1}^N v_T(i), \quad q_T^* = \arg\max_{i=1}^N v_T(i)$$  
4. **Backtrace**: Reconstruct path using backpointers.  

**Example**:  
For $O = (3, 1, 3)$, the Viterbi path might be $(\text{HOT}, \text{HOT}, \text{COLD})$.  

---

### **Problem 3: Learning (Forward-Backward/Baum-Welch Algorithm)**
**Goal**: Learn HMM parameters $A$ and $B$ from unlabeled data.  

#### **Forward-Backward Algorithm**:
1. **Expectation Step (E-Step)**:  
   - Compute forward probabilities $\alpha_t(j)$ and backward probabilities $\beta_t(j)$.  
   - Calculate $\xi_t(i,j)$ (probability of transitioning $i→j$ at time $t$):  
     $$\xi_t(i,j) = \frac{\alpha_t(i) \cdot a_{ij} \cdot b_j(o_{t+1}) \cdot \beta_{t+1}(j)}{P(O \mid \lambda)}$$  
   - Calculate $\gamma_t(j)$ (probability of being in state $j$ at time $t$):  
     $$\gamma_t(j) = \frac{\alpha_t(j) \cdot \beta_t(j)}{P(O \mid \lambda)}$$  
2. **Maximization Step (M-Step)**:  
   - Update transition probabilities:  
     $$\hat{a}_{ij} = \frac{\sum_{t=1}^{T-1} \xi_t(i,j)}{\sum_{t=1}^{T-1} \sum_{k=1}^N \xi_t(i,k)}$$  
   - Update emission probabilities:  
     $$\hat{b}_j(v_k) = \frac{\sum_{t=1 \text{ s.t. } o_t=v_k}^T \gamma_t(j)}{\sum_{t=1}^T \gamma_t(j)}$$  

---

## **4. Applications of HMMs**
### **Part-of-Speech (POS) Tagging**
- **States**: POS tags (e.g., Noun, Verb).  
- **Observations**: Words in a sentence.  
- **Transition Probabilities**: $P(\text{tag}_t \mid \text{tag}_{t-1})$.  
- **Emission Probabilities**: $P(\text{word}_t \mid \text{tag}_t)$.  

**Example**:  
For sentence *"The koala put the keys on the table"*:  
- **Transition**: $P(\text{NOUN} \mid \text{DET}) = 0.49$ (from corpus counts).  
- **Emission**: $P(\text{"koala"} \mid \text{NOUN}) = 0.8$.  

#### **Viterbi Decoding for POS Tagging**:
$$
\hat{t}_1^n = \arg\max_{t_1^n} \prod_{i=1}^n P(w_i \mid t_i) \cdot P(t_i \mid t_{i-1})
$$

---

## **5. Key Figures**
### **HMM Trellis for Forward Algorithm**
- **Structure**: Grid with time steps $t$ on one axis and states $j$ on the other.  
- **Cells**: $\alpha_t(j)$ represents the probability of being in state $j$ at time $t$.  
- **Edges**: Weighted by $a_{ij} \cdot b_j(o_t)$.  

### **Viterbi Trellis**  
- Similar to forward trellis but tracks **maximum path probabilities** and **backpointers**.  

### **Baum-Welch Expectation Step**  
- **Forward-Backward Pass**: Combines $\alpha$ and $\beta$ to compute $\xi$ and $\gamma$ for parameter updates.  

---

## **6. Summary of Equations**
| Concept | Formula |  
|---|---|  
| **Forward Probability** | $$\alpha_t(j) = \sum_{i=1}^N \alpha_{t-1}(i) \cdot a_{ij} \cdot b_j(o_t)$$ |  
| **Viterbi Probability** | $$v_t(j) = \max_{i=1}^N \left[ v_{t-1}(i) \cdot a_{ij} \cdot b_j(o_t) \right]$$ |  
| **Transition Re-estimation** | $$\hat{a}_{ij} = \frac{\sum_{t=1}^{T-1} \xi_t(i,j)}{\sum_{t=1}^{T-1} \sum_{k=1}^N \xi_t(i,k)}$$ |  
| **Emission Re-estimation** | $$\hat{b}_j(v_k) = \frac{\sum_{t: o_t=v_k}^T \gamma_t(j)}{\sum_{t=1}^T \gamma_t(j)}$$ |  
