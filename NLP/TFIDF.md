# Introduction to TF-IDF
**TF-IDF** stands for “*Term Frequency — Inverse Data Frequency*”. First, we will learn what this term means mathematically.

**Term Frequency (tf)**: gives us the frequency of the word in each document in the corpus. It is the ratio of number of times the word appears in a document compared to the total number of words in that document. It increases as the number of occurrences of that word within the document increases. Each document has its own tf.

<p align="center">
  <img src="/images/tf.PNG"/>
</p>

**Inverse Data Frequency (idf)**: used to calculate the weight of rare words across all documents in the corpus. The words that occur rarely in the corpus have a high IDF score. It is given by the equation below.

<p align="center">
  <img src="/images/idf.PNG"/>
</p>

Combining these two we come up with the TF-IDF score (w) for a word in a document in the corpus. It is the product of tf and idf:

<p align="center">
  <img src="/images/tfidf.PNG"/>
</p>