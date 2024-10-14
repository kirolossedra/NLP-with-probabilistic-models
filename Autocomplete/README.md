Trigrams and the Problem of Missing Context
In a trigram model, we look at the previous two words to calculate the probability of a word. However, for the first two words in a sentence, there isn’t enough context—because there aren’t two preceding words.

Example with Trigrams
Consider the sentence: "The dog runs."

In a trigram model, to calculate the probability of "The", we would normally look at the previous two words. But there are no words before "The", so the trigram probability for "The" would be undefined.

Similarly, to calculate the probability of "dog", we would need the two preceding words, but we only have "The". So we don’t have enough context for the first two words.

Solution: Start Tokens for Trigrams
To solve this, we introduce two start tokens 
𝑆
S at the beginning of the sentence. This ensures we have enough context for the first two words.

For example, we modify the sentence:

Original: "The dog runs."
With two start tokens: ( S \ S \ "The dog runs."
Now, we can calculate the probabilities using trigrams:

First word ("The"): Use the trigram probability 
𝑃
(
The
∣
𝑆
,
𝑆
)
P(The∣S,S). This means "The" is conditioned on the two start tokens, which serve as a context for the beginning of the sentence.
Second word ("dog"): Use 
𝑃
(
dog
∣
𝑆
,
The
)
P(dog∣S,The). This means the probability of "dog" is conditioned on the start token 
𝑆
S and the word "The".
Third word ("runs"): Use 
𝑃
(
runs
∣
The
,
dog
)
P(runs∣The,dog). Now we have two preceding words to form a trigram.
Sentence Probability with Trigrams
Now, the probability of the sentence becomes the product of the trigram probabilities:

𝑃
(
𝑆
,
𝑆
,
"The dog runs"
)
=
𝑃
(
The
∣
𝑆
,
𝑆
)
⋅
𝑃
(
dog
∣
𝑆
,
The
)
⋅
𝑃
(
runs
∣
The
,
dog
)
P(S,S,"The dog runs")=P(The∣S,S)⋅P(dog∣S,The)⋅P(runs∣The,dog)
Why Don’t You Use Unigrams and Bigrams for the First Words?
The idea here is to keep the model consistent. Instead of using a combination of unigrams for the first word, bigrams for the second word, and trigrams for the rest, you handle the lack of context by adding start tokens to make the sentence long enough for the trigram model to work from the very beginning. This way, you are always calculating trigram probabilities throughout the entire sentence.

Summary
Start tokens 
𝑆
S are added at the beginning of a sentence to give context for the first one or two words when using a bigram or trigram model.
For bigram models, one start token 
𝑆
S is used to calculate the probability of the first word.
For trigram models, two start tokens 
𝑆
 
𝑆
SS are used so that even the first two words have enough context to compute their probabilities.
The sentence probability is the product of the trigram probabilities of each word, starting from the first one and accounting for the start tokens

























Certainly! I'll reformat the content, improve its organization, and enclose equations in $$. Here's the revised version:

# Add-k Smoothing in Language Models

## Introduction to Add-One Smoothing

Add-one smoothing is a technique used to prevent zero probabilities in language models. It works by adding 1 to the count of each n-gram, whether it's seen or unseen.

The add-one smoothing formula for a bigram is:

$$P(w_i|w_{i-1}) = \frac{C(w_{i-1}, w_i) + 1}{C(w_{i-1}) + V}$$

Where:
- $$C(w_{i-1}, w_i)$$ is the count of the bigram $$(w_{i-1}, w_i)$$
- $$C(w_{i-1})$$ is the count of the previous word $$w_{i-1}$$
- $$V$$ is the size of the vocabulary (number of distinct words)

## Limitations of Add-One Smoothing

While add-one smoothing prevents zero probabilities, it can over-correct for unseen n-grams, especially in larger corpora. Adding 1 uniformly to every n-gram can distort the probabilities of frequently occurring n-grams.

## Introduction to Add-k Smoothing

Add-k smoothing generalizes add-one smoothing by allowing you to choose the value of $$k$$, providing more control over the amount of smoothing.

The formula for add-k smoothing is:

$$P(w_i|w_{i-1}) = \frac{C(w_{i-1}, w_i) + k}{C(w_{i-1}) + kV}$$

Where $$k$$ is a parameter you can choose, typically a small value (e.g., $$k = 0.01$$, $$k = 0.1$$, etc.).

## Advantages of Add-k Smoothing

1. **Better for Larger Corpora**: A smaller $$k$$ ensures that seen n-grams dominate the probability distribution.

2. **Fine Control**: Smaller $$k$$ values place less emphasis on unseen n-grams while still avoiding zero probabilities.

3. **Improved Proportionality**: Scaled-down smoothing maintains higher probabilities for frequent n-grams compared to unseen ones.

## Example: Add-One vs Add-k Smoothing

Consider two bigrams: ("cat", "sleeps") unseen, and ("dog", "runs") seen 100 times.

### Add-One Smoothing:

$$P("sleeps"|"cat") = \frac{0 + 1}{C("cat") + V} = \frac{1}{C("cat") + V}$$

$$P("runs"|"dog") = \frac{100 + 1}{C("dog") + V} = \frac{101}{C("dog") + V}$$

### Add-k Smoothing (k = 0.1):

$$P("sleeps"|"cat") = \frac{0 + 0.1}{C("cat") + 0.1V} = \frac{0.1}{C("cat") + 0.1V}$$

$$P("runs"|"dog") = \frac{100 + 0.1}{C("dog") + 0.1V} = \frac{100.1}{C("dog") + 0.1V}$$

Note how add-k smoothing with a small $$k$$ value makes a much smaller adjustment for the unseen bigram compared to add-one smoothing.

## Summary

Add-k smoothing offers more flexibility than add-one smoothing. By choosing a smaller $$k$$, you can:
1. Give less emphasis to unseen n-grams
2. Ensure no zero probabilities
3. Better preserve the probabilities of frequently occurring n-grams

This approach helps strike a balance between smoothing and maintaining the integrity of observed frequency distributions, especially in larger datasets.
