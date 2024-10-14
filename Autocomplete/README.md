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


























Add-One Smoothing
Add-one smoothing solves this by adding 1 to the count of each n-gram, whether it’s seen or unseen. This prevents any n-gram from having a count of zero, ensuring no zero probabilities.

The add-one smoothing formula for a bigram (for example) looks like this:

𝑃
(
𝑤
𝑖
∣
𝑤
𝑖
−
1
)
=
𝐶
(
𝑤
𝑖
−
1
,
𝑤
𝑖
)
+
1
𝐶
(
𝑤
𝑖
−
1
)
+
𝑉
P(w 
i
​
 ∣w 
i−1
​
 )= 
C(w 
i−1
​
 )+V
C(w 
i−1
​
 ,w 
i
​
 )+1
​
 
Where:

𝐶
(
𝑤
𝑖
−
1
,
𝑤
𝑖
)
C(w 
i−1
​
 ,w 
i
​
 ) is the count of the bigram 
(
𝑤
𝑖
−
1
,
𝑤
𝑖
)
(w 
i−1
​
 ,w 
i
​
 ),
𝐶
(
𝑤
𝑖
−
1
)
C(w 
i−1
​
 ) is the count of the previous word 
𝑤
𝑖
−
1
w 
i−1
​
 ,
𝑉
V is the size of the vocabulary (the number of distinct words).
This formula ensures that every possible bigram gets some probability, including those that haven’t been seen.

Why Use 
𝑘
k (Add-
𝑘
k Smoothing)?
While add-one smoothing prevents zero probabilities, it has a side effect: It can over-correct for unseen n-grams, especially when dealing with larger corpora. Adding 1 uniformly to every n-gram (even those that appear frequently) can distort the probabilities of the n-grams that actually occur frequently.

Add-
𝑘
k smoothing generalizes add-one smoothing by allowing you to choose the value of 
𝑘
k, giving more control over the amount of smoothing. Here's the formula:

𝑃
(
𝑤
𝑖
∣
𝑤
𝑖
−
1
)
=
𝐶
(
𝑤
𝑖
−
1
,
𝑤
𝑖
)
+
𝑘
𝐶
(
𝑤
𝑖
−
1
)
+
𝑘
𝑉
P(w 
i
​
 ∣w 
i−1
​
 )= 
C(w 
i−1
​
 )+kV
C(w 
i−1
​
 ,w 
i
​
 )+k
​
 
Where 
𝑘
k is a parameter you can choose, typically a small value (e.g., 
𝑘
=
0.01
k=0.01, 
𝑘
=
0.1
k=0.1, etc.).

Why Scale by 
𝑘
k?
The value of 
𝑘
k allows you to control the amount of smoothing. Here's why that’s important:

Larger Corpora: When you have a large corpus, many n-grams will have very high counts. In such cases, adding 1 (as in add-one smoothing) can skew the probabilities too much by overemphasizing the unseen n-grams. A smaller 
𝑘
k (like 
𝑘
=
0.01
k=0.01) ensures that the seen n-grams still dominate the probability distribution.

Fine Control: By using a smaller 
𝑘
k, you place less emphasis on the unseen n-grams (because you're adding a smaller value), while still ensuring that they don’t have zero probability. This provides a balance between avoiding zero probabilities and keeping the real counts dominant.

Proportionality: Add-one smoothing can give too much weight to rare or unseen n-grams, especially when the vocabulary size 
𝑉
V is large. By using a smaller 
𝑘
k, the smoothing is scaled down proportionally, so that seen and frequent n-grams maintain higher probabilities compared to unseen ones.

Example of the Difference:
Let’s say in a bigram model, the word pair ("cat", "sleeps") has not been seen in your corpus, while the word pair ("dog", "runs") has been seen 100 times.
With add-one smoothing:

𝑃
(
"sleeps"
∣
"cat"
)
=
0
+
1
𝐶
(
"cat"
)
+
𝑉
=
1
𝐶
(
"cat"
)
+
𝑉
P("sleeps"∣"cat")= 
C("cat")+V
0+1
​
 = 
C("cat")+V
1
​
 
𝑃
(
"runs"
∣
"dog"
)
=
100
+
1
𝐶
(
"dog"
)
+
𝑉
=
101
𝐶
(
"dog"
)
+
𝑉
P("runs"∣"dog")= 
C("dog")+V
100+1
​
 = 
C("dog")+V
101
​
 
With add-
𝑘
k smoothing (let’s use 
𝑘
=
0.1
k=0.1):

𝑃
(
"sleeps"
∣
"cat"
)
=
0
+
0.1
𝐶
(
"cat"
)
+
0.1
𝑉
=
0.1
𝐶
(
"cat"
)
+
0.1
𝑉
P("sleeps"∣"cat")= 
C("cat")+0.1V
0+0.1
​
 = 
C("cat")+0.1V
0.1
​
 
𝑃
(
"runs"
∣
"dog"
)
=
100
+
0.1
𝐶
(
"dog"
)
+
0.1
𝑉
=
100.1
𝐶
(
"dog"
)
+
0.1
𝑉
P("runs"∣"dog")= 
C("dog")+0.1V
100+0.1
​
 = 
C("dog")+0.1V
100.1
​
 
Notice that with add-
𝑘
k smoothing, the adjustment for the unseen bigram is much smaller than with add-one smoothing. This preserves the real counts more effectively in large datasets, while still ensuring that every possible bigram has a non-zero probability.

Summary
Why scale by 
𝑘
k? To control the amount of smoothing, especially in larger corpora. By choosing a smaller 
𝑘
k, you give less emphasis to unseen n-grams while still ensuring no zero probabilities.
Add-
𝑘
k smoothing is more flexible than add-one smoothing and helps strike a balance between ensuring all n-grams have some probability, while not distorting the probabilities of frequently occurring n-grams.
