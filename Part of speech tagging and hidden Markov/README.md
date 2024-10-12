


## Example:

Let's consider an example with a simple set of POS tags:

- POS tags: NN (noun), VB (verb), O (other)
- Sentences: 
  - $${\color{orange} \< s \> \space \color{lightblue}in \space a \space \color{lightgreen}station  \space \color{lightblue}of  \space the \space \color{lightgreen}metro }$$

  - $${\color{orange} \< s \> \space \color{lightblue}the \space \color{lightgreen}apparation  \space \color{lightblue}of \space these \space \color{lightgreen}faces \space \color{lightblue}in \space the \space \color{lightgreen} crowd    }$$

  - $${\color{orange} \< s \> \space \color{lightgreen}petals  \space \color{lightblue}on \space a \space  wet, \space black \space  \color{lightgreen}bough}$$



  


### Transition Matrix A:

|         | NN   | VB   | O    |
|---------------------|---------------------------|---------------------|-----------------------|
| π       | 1  | 0  | 2  |
| NN      | 0 | 0 | 6 |
| VB      | 0 | 0  | 0  |
| O       | 6 | 0  | 8 |

To compute $$P(NN|O)$$ we have to add the sum column to get the probabilities then A becomes

### Transition Matrix A:

|         | NN   | VB   | O    | Sum
|---------|------|------|------|---------|
| π       | 1  | 0  | 2  | 3 |
| NN      | 0 | 0 | 6 | 6 |
| VB      | 0 | 0  | 0  | 0 |
| O       | 6 | 0  | 8 |14  | 


```math
P(NN \mid O) = \frac{C(O, NN) }{\sum_{j=1}^{N} C(O, t_j)    } =  \frac{6} {14 }
```

 but sometimes probability is 0 to avoid division by zero we use the following formula instead(that includes smoothing):

 ```math
P(t_i \mid t_{i-1}) = \frac{C(t_{i-1}, t_i) + \varepsilon}{\sum_{j=1}^{N} C(t_{i-1}, t_j) + N \cdot \varepsilon}
```

 
### Populating the Emission Matrix B:

referring to the same example above we will use each words and check if association between words and POS, this is more apparent when talking about play as a verb and as a noun for example. 

|         | in   | a  | station  | of | the | metro | appraition | these | faces |  crowd | petals | on | a | wet | black | bough | 
|---------|-------|-------|-------|-------  |---------|-------|-------|-------| ------|---------|-------|-------|-------|------|-------|-------|
| NN      | 0   | 0   | 1   |
| VB      | 0   | 0   | 0   |
| O       | 2  | 2   | 0   | 


a more sophisticated example to further explain the population of this matrix 

- Sentences:
  - $${\color{orange} \< s \> \space \color{lightblue}I  \space \color{lightgreen}watch  \space \color{lightblue}a  \space the \space \color{lightgreen}play }$$

  - $${\color{orange} \< s \> \space \color{lightblue}I \space \color{violet}play  \space \color{lightgreen}volleyball}$$
In that case the matrix will look like this


|         | I   | watch  | a  | play | volleyball 
|---------|-------|-------|-------|-------  |---------|
| NN      | 0   | 0   |  0  | 1 | 1 |
| VB      | 0   | 1   | 0   | 1 | 0 |
| O       | 2  | 0   | 1   |  0 | 0 |


here we will deal with the probabilities using smoothing as the previous table: 

 ```math
P(w_i \mid t_i) = \frac{C(t_{i}, w_i) + \varepsilon}{\sum_{j=1}^{V} C(t_{i}, w_j) + N \cdot \varepsilon} =  \frac{C(t_{i}, w_i) + \varepsilon}{ C(t_{i}) + N \cdot \varepsilon}
```



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





