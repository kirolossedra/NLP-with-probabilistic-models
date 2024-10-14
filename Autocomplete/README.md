Here's a reformatted and improved version of the content, with equations enclosed in $$:

# Trigrams and the Problem of Missing Context

## Introduction to Trigram Models

In a trigram model, we calculate the probability of a word based on the previous two words. However, this approach faces a challenge at the beginning of a sentence where there aren't enough preceding words.

## The Problem Illustrated

Consider the sentence: "The dog runs."

In a trigram model:
- For "The": We need two preceding words, but there are none.
- For "dog": We only have "The" as context, but we need two words.

This lack of context makes it impossible to calculate trigram probabilities for the first two words using the standard approach.

## Solution: Start Tokens for Trigrams

To address this issue, we introduce two start tokens ($$S$$) at the beginning of the sentence. This provides the necessary context for the first two words.

### Example:

Original sentence: "The dog runs."
Modified with start tokens: $$S\ S\ \text{"The dog runs."}$$

Now we can calculate trigram probabilities for each word:

1. First word ("The"): $$P(\text{The}|S,S)$$
   - "The" is conditioned on two start tokens.

2. Second word ("dog"): $$P(\text{dog}|S,\text{The})$$
   - "dog" is conditioned on one start token and "The".

3. Third word ("runs"): $$P(\text{runs}|\text{The},\text{dog})$$
   - We now have two preceding words to form a complete trigram.

## Sentence Probability with Trigrams

The probability of the entire sentence becomes the product of these trigram probabilities:

$$P(S,S,\text{"The dog runs"}) = P(\text{The}|S,S) \cdot P(\text{dog}|S,\text{The}) \cdot P(\text{runs}|\text{The},\text{dog})$$

## Why Not Use Unigrams and Bigrams for the First Words?

The goal is to maintain model consistency. By using start tokens, we ensure that:

1. We always calculate trigram probabilities throughout the entire sentence.
2. We avoid mixing different n-gram models (unigrams, bigrams, and trigrams) within the same sentence probability calculation.

This approach provides a uniform method for handling context across the entire sentence.

## Summary

1. Start tokens ($$S$$) are added at the beginning of a sentence to provide context for the initial words in n-gram models.
   - For bigram models: One start token
   - For trigram models: Two start tokens

2. This method ensures that:
   - Even the first two words have sufficient context for probability calculations.
   - The sentence probability is consistently computed as the product of trigram probabilities for each word, including those at the beginning of the sentence.

3. By using start tokens, we maintain a consistent approach to probability calculation across the entire sentence, avoiding the need to mix different n-gram models for different parts of the sentence.
























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
