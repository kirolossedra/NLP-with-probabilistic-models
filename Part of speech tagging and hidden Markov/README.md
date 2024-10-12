


## Example:

Let's consider an example with a simple set of POS tags:

- POS tags: NN (noun), VB (verb), O (other)
- Sentences: 
  - $${\color{orange} \< s \> \space \color{lightblue}in \space a \space \color{lightgreen}station  \space \color{lightblue}of  \space the \space \color{lightgreen}metro }$$

  - $${\color{orange} \< s \> \space \color{lightblue}the \space \color{lightgreen}apparation  \space \color{lightblue}of \space these \space \color{lightgreen}faces \space \color{lightblue}in \space the \space \color{lightgreen} crowd    }$$

  - $${\color{orange} \< s \> \space \color{lightgreen}petals  \space \color{lightblue}on \space a \space  wet, \space black \space  \color{lightgreen}bough}$$



  


### Transition Matrix A:

|         | NN   | VB   | O    |
|---------|------|------|------|
| π       | 1  | 0  | 2  |
| NN      | 0 | 0 | 6 |
| VB      | 0 | 0  | 0  |
| O       | 6 | 0  | 8 |

To compute $$P(NN|O)$$ we have to add the sum column to get the probabilities

### Emission Matrix B:

|         | dog   | runs  | fast  |
|---------|-------|-------|-------|
| NN      | 0.8   | 0.1   | 0.1   |
| VB      | 0.1   | 0.7   | 0.2   |
| O       | 0.1   | 0.2   | 0.7   |

This matrix shows the probability of each word being emitted by a particular POS tag. For instance, the word "runs" is most likely associated with the VB tag (0.7 probability).

### Probability Matrix C (for three words):

|         | dog   | runs  | fast  |
|---------|-------|-------|-------|
| NN      | 0.48  |       |       |
| VB      | 0.03  |       |       |
| O       | 0.01  |       |       |

For the first word, "dog", the initial probabilities are calculated using the initial state (π) and emission probabilities.

### Backpointer Matrix D:

|         | dog   | runs  | fast  |
|---------|-------|-------|-------|
| NN      | 0     |       |       |
| VB      | 0     |       |       |
| O       | 0     |       |       |

For the first word, the backpointer values are initialized to 0. For subsequent words, the backpointers will store the index of the most likely previous tag.



Here's the reformatted text with LaTeX formatting for the equations, followed by an example to explain how each matrix might look like.

---

## Transition Matrix A:

### Purpose:
Maps the probability of transitioning from one part-of-speech (POS) tag to another.

### Equation:
```math
P(t_i \mid t_{i-1}) = \frac{C(t_{i-1}, t_i) + \varepsilon}{\sum_{j=1}^{N} C(t_{i-1}, t_j) + N \cdot \varepsilon}
```


### In-depth explanation:
- **A** is a square matrix where both rows and columns represent POS tags (e.g., NN, VB, O) plus an initial state (π). Each element
```math
  A[i,j]
```

$P(t_i \mid t_{i-1}) = \frac{C(t_{i-1}, t_i) + \varepsilon}{\sum_{j=1}^{N} C(t_{i-1}, t_j) + N \cdot \varepsilon}$

  represents the probability of transitioning from tag  $i$ to tag $j$.
  
- The equation incorporates **smoothing** to avoid zero probabilities:
  - $C(t_{i-1}, t_i)$ is the count of transitions from tag $i$ to $j$
  - $\varepsilon$  is a small positive value added to all counts (smoothing factor)
  - \( N \) is the number of possible tags

- The denominator normalizes the probabilities.

This smoothing is crucial because:
- It prevents zero probabilities, which could cause issues in log-likelihood calculations.
- It allows for unseen transitions in the training data to have a small, non-zero probability.
- It improves the model's generalization to new, unseen data.

---

## Emission Matrix B:

### Purpose:
Maps words to their probabilities of being associated with each POS tag.

### Equation:
$$
P(w_i \mid t_i) = \frac{C(t_i, w_i) + \varepsilon}{C(t_i) + N \cdot \varepsilon}
$$

### In-depth explanation:
- **B** has dimensions \( (\text{num\_tags}, \text{num\_words}) \). Each element \( B[i,j] \) represents the probability of word \( j \) being emitted by tag \( i \).

- The equation is a smoothed version:
  - \( C(t_i, w_i) \) is the count of word \( w_i \) being tagged as \( t_i \)
  - \( C(t_i) \) is the total count of tag \( t_i \)
  - \( N \) is the vocabulary size
  - \( \varepsilon \) is the smoothing factor

This smoothing addresses the same issues as in the transition matrix, allowing for words not seen with a particular tag in the training data to have a small, non-zero probability of emission.

---

## Probability Matrix C:

### Purpose:
Stores the probabilities of each word belonging to each POS tag, considering both transition and emission probabilities.

### Equation:
For the first word:
$$
c_{i,1} = \pi_i \cdot b_{i,\text{cindex}(w_1)} = a_{1,i} \cdot b_{i,\text{cindex}(w_1)}
$$

For subsequent words:
$$
c_{i,t} = \max(c_{j,t-1} \cdot a_{j,i} \cdot b_{i,\text{cindex}(w_t)})
$$

### In-depth explanation:
- **C** has dimensions \( (\text{num\_tags}, \text{num\_words}) \). It's used in the Viterbi algorithm to compute the most likely sequence of tags.

For the first word (column 1):
- \( \pi_i \) is the initial probability of tag \( i \)
- \( b_{i,\text{cindex}(w_1)} \) is the emission probability of the first word given tag \( i \)
- \( a_{1,i} \) is equivalent to \( \pi_i \), representing the probability of starting with tag \( i \)

For subsequent words, the Viterbi algorithm uses:
- The previous probability \( c_{j,t-1} \)
- The transition probability \( a_{j,i} \)
- The emission probability \( b_{i,\text{cindex}(w_t)} \)

---

## Backpointer Matrix D:

### Purpose:
Keeps track of the most likely previous POS tag for each word and current tag combination.

### Initialization:
$$
d_{i,1} = 0 \text{ for all } i
$$

### In-depth explanation:
- **D** has the same dimensions as **C** \( (\text{num\_tags}, \text{num\_words}) \). It's crucial for reconstructing the most likely tag sequence after running the Viterbi algorithm.

For words after the first:
$$
d_{i,t} = \arg\max(c_{j,t-1} \cdot a_{j,i} \cdot b_{i,\text{cindex}(w_t)})
$$
This stores the index \( j \) of the previous tag that maximizes the probability of reaching the current tag \( i \) for word \( t \).

The Viterbi algorithm uses **C** and **D** together:
- **Forward pass**: Fill **C** and **D** using the recurrence relations.
- **Backward pass**: Use **D** to backtrack and find the most likely tag sequence.

---


