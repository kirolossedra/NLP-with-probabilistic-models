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
