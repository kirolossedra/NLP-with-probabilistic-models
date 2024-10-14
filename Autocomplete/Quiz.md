I understand. To prevent GitHub from rendering `<s>` and `</s>` as strikethrough tags, we can use backticks (`) to display them as inline code. Here's the updated version of the quiz with this change:

# Language Model Quiz

### Question 1
In the context of our corpus, what is the probability of the word "papers" following the phrase "it in the"?
- [ ] $$P(papers|it in the) = 0$$
- [ ] $$P(papers|it in the) = 1$$
- [ ] $$P(papers|it in the) = 2/3$$
- [ ] $$P(papers|it in the) = 1/2$$

### Question 2
Given these conditional probabilities:
- $$P(Mary) = 0.1$$
- $$P(likes) = 0.2$$
- $$P(cats) = 0.3$$
- $$P(Mary|likes) = 0.2$$
- $$P(likes|Mary) = 0.3$$
- $$P(cats|likes) = 0.1$$
- $$P(likes|cats) = 0.4$$
Approximate the probability of the sentence "Mary likes cats" using bigrams.
- [ ] $$P(Mary likes cats) = 0.003$$
- [ ] $$P(Mary likes cats) = 0.008$$
- [ ] $$P(Mary likes cats) = 1$$
- [ ] $$P(Mary likes cats) = 0$$

### Question 3
Given these conditional probabilities:
- $$P(Mary) = 0.1$$
- $$P(likes) = 0.2$$
- $$P(cats) = 0.3$$
- $$P(Mary|`<s>`) = 0.2$$
- $$P(`</s>`|cats) = 0.6$$
- $$P(likes|Mary) = 0.3$$
- $$P(cats|likes) = 0.1$$
Approximate the probability of the sentence "`<s>` Mary likes cats `</s>`" using bigrams.
- [ ] $$P(`<s>` Mary likes cats `</s>`) = 0.0036$$
- [ ] $$P(`<s>` Mary likes cats `</s>`) = 1$$
- [ ] $$P(`<s>` Mary likes cats `</s>`) = 0.003$$
- [ ] $$P(`<s>` Mary likes cats `</s>`) = 0$$

### Question 4
Given the logarithm of these conditional probabilities:
- $$log(P(Mary|`<s>`)) = -2$$
- $$log(P(`</s>`|cats)) = -1$$
- $$log(P(likes|Mary)) = -10$$
- $$log(P(cats|likes)) = -100$$
Approximate the log probability of the sentence "`<s>` Mary likes cats `</s>`" using bigrams.
- [ ] $$log(P(`<s>` Mary likes cats `</s>`)) = -112$$
- [ ] $$log(P(`<s>` Mary likes cats `</s>`)) = -113$$
- [ ] $$log(P(`<s>` Mary likes cats `</s>`)) = 113$$
- [ ] $$log(P(`<s>` Mary likes cats `</s>`)) = 2000$$

### Question 5
Given the logarithm of these conditional probabilities:
- $$log(P(Mary|`<s>`)) = -2$$
- $$log(P(`</s>`|cats)) = -1$$
- $$log(P(likes|Mary)) = -10$$
- $$log(P(cats|likes)) = -100$$
Assuming our test set is W="`<s>` Mary likes cats `</s>`", what is the model's perplexity?
- [ ] $$log PP(W) = -113$$
- [ ] $$log PP(W) = (-1/5) * (-113)$$
- [ ] $$log PP(W) = (-1/4) * (-113)$$
- [ ] $$log PP(W) = (-1/5) * 113$$

### Question 6
Given the training corpus and minimum word frequency = 2, how would the vocabulary for the corpus preprocessed with `<UNK>` look like?
- Corpus: "`<s>` I am happy I am learning `</s>` `<s>` I am happy I can study `</s>`"
- [ ] $$V =$$ (I, am, happy)
- [ ] $$V =$$ (I, am, happy, learning, can, study)
- [ ] $$V =$$ (I, am, happy, I, am)
- [ ] $$V =$$ (I, am, happy, learning, can, study, `<UNK>`)

### Question 7
Corpus: "I am happy I am learning"
In the context of our corpus, what is the estimated probability of the word "can" following the word "I" using the bigram model and add-k-smoothing where k=3?
- [ ] P(can|I) = 0
- [ ] P(can|I) = 1
- [ ] P(can|I) = 3 / (2 + 3 * 4)
- [ ] P(can|I) = 3 / (3 * 4)

### Question 8
Which of the following are applications of n-gram language models?
- [ ] Speech recognition
- [ ] Auto-complete
- [ ] Auto-correct
- [ ] Augmentative communication
- [ ] Sentiment Analysis

### Question 9
The higher the perplexity score, the more our corpus will make sense.
- [ ] True
- [ ] False

### Question 10
The perplexity score increases as we increase the number of `<UNK>` tokens.
- [ ] True
- [ ] False

This version uses backticks around `<s>`, `</s>`, and `<UNK>` to ensure they are displayed as intended without being interpreted as HTML tags or markdown syntax. The rest of the formatting remains the same, preserving the LaTeX-style math expressions using double dollar signs ($$).
