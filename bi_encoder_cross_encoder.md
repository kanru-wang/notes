# LLM + Bi-encoder + Cross-encoder - A New Method for Text Classification

- Traditionally we apply Gradient Boost Tree on TFIDF vectors (one vector represents one sample text) for any text classification task. One area for improvement is that, with this modeling methodology we treat each label as a class (e.g., 1, 2, 3, etc.), and ignore the inherent meaning of the label.

- If we use a Bi-encoder, we (1) convert each class/label to a vector, (2) compare the distance between the complaint vector and each of the class/label vectors, and (3) choose the closest class/label vector as the predicted class. With Bi-encoder, we are able to leverage useful prior human knowledge (i.e., concatenate highly useful keywords to the class/label string). This method is particularly useful for classes with insufficient training samples and for a group of classes that are confusing. However, we mention in below why Gradient Boost can still be better than Bi-encoder.

- It is possible to improve the accuracy by applying a Cross-encoder after the Bi-encoder (or Gradient Boost). A Cross-encoder is a pre-trained BERT model that can determine how relevant are two BERT-tokenized sentences. In our case, the input for the BERT Cross-encoder would be:
  ```
  [CLS] complaint_tokenized_sentence [SEP] class_n_tokenized_sentence [SEP]
  ```
  To reiterate the process, after using Bi-encoder (or Gradient Boost) to retrieve e.g., the top 5 closest class/label vectors, run BERT Cross-encoder 5 times, and the complaint-class sentence pair with the highest score is the final predicted class. The use of Cross-encoder after Bi-encoder is called "Re-ranking."

- The inference of Bi-encoder is relatively fast, but not accurate enough. The inference of Cross-encoder is slow (need to run for each complaint-class sentence pair), but has a focus on sentence pair relevance and accuracy. A hybrid of Bi-encoder and Cross-encoder achieves both accuracy and speed.

- In fact, in the realm of Information Retrieval, it is common to use an algorithm similar to Bi-encoder that focuses on Recall, followed by a Cross-encoder-like algorithm that focuses on Precision. When you think about Text Classification from a different perspective, it is also an Information Retrieval task.

- The Cross-encoder is downloadable as a pre-trained model, and can be further fine-tuned with data of this project (not recommended, for simplicity).

- From experience,
  - Keywords are extremely useful for text classification, so it is necessary to use some Term Frequency embedding for retrieving top K classes (instead of word embedding where keywords would be indirectly represented), before the Cross-encoder re-ranking step.
  - We should use Gradient Boost models rather than Bi-encoder because (1) Gradient Boost models can leverage keyword features better than Bi-encoder models (keywords dimensions are overwhelmed by other dimensions when a Bi-encoder model calculates the distance), and (2) Gradient Boost models can also leverage structured-data features.
  - Therefore, a new approach to classify text can be: Gradient Boost on TFIDF + BERT Cross-encoder.
  - If many classes suffer from insufficient training samples, we should use: Bi-encoder + BERT Cross-encoder.

---

### An LLM + Bi-encoder + Cross-encoder Solution for Text Classification:
1. Query decomposition (use LLM to answer 5 guiding questions).
2. Query re-write (write a summary with 5 answers in mind, similar to "Hypothetical document embedding").
3. Use a vector database to search most similar k embedding vectors of k classes (retrieve top 10 most similar classes. This is a Bi-encoder method).
4. Re-rank the k embedding vectors (Re-rank the top 10 to get the top 1. Usually, a Cross-encoder is used to do this step, but can use LLM for this step as well).
5. An optional catch-all step that uses LLM to directly classify low certainty samples (low certainty with vector database search and reranking).

- For this LLM solution, there is no Generation step; what LLM really helped is the summarizing step (Step 1 & 2) that normalizes/denoises the original text.
