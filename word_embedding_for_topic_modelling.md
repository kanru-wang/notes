# Word embedding for topic modelling

## Word embedding

A few ways to get word embedding. All use neural networks for training and their resulting weight matrix is the word embedding.

From: https://towardsdatascience.com/nlp-101-word2vec-skip-gram-and-cbow-93512ee24314 and https://towardsdatascience.com/nlp-101-negative-sampling-and-glove-936c88f3bc68

- Skip Gram
    - Use a word to predict its surrounding words.
    - Works well with small amount of the training data, represents well even rare words or phrases.
    - Neural Network details:
        - Input one-hot vector 1xV — where V is the number of words in the vocabulary
        - Hidden layer size VxE, where E is the size of the word embedding and is a hyper-parameter
        - Output from the hidden layer size 1xE will be fed into an softmax layer.
        - Output layer will be 1xV, where each value in the vector will be the probability score of the target word at that position.
        
- Continuous Bag of Words (CBOW)
    - Use a word's surrounding words to predict that word
    - Several times faster to train than the skip-gram, slightly better accuracy for the frequent words
    - Neural Network details:
        - The dimension of our hidden layer and output layer will remain the same. Only the dimension of our input layer and the calculation of hidden layer activations will change, if we have 4 context words for a single target word, we will have 4 1xV input vectors. Each will be multiplied with the VxE hidden layer returning 1xE vectors. All 4 1xE vectors will be averaged element-wise to obtain the final activation which then will be fed into the softmax layer.

- Negative Sampling

- GloVe

From: https://stackoverflow.com/questions/38287772/cbow-v-s-skip-gram-why-invert-context-and-target-words

    - Here is my oversimplified and rather naive understanding of the difference:

    - As we know, CBOW is learning to predict the word by the context. Or maximize the probability of the target word by looking at the context. And this happens to be a problem for rare words. For example, given the context yesterday was a really [...] day CBOW model will tell you that most probably the word is beautiful or nice. Words like delightful will get much less attention of the model, because it is designed to predict the most probable word. This word will be smoothed over a lot of examples with more frequent words.

    - On the other hand, the skip-gram model is designed to predict the context. Given the word delightful it must understand it and tell us that there is a huge probability that the context is yesterday was really [...] day, or some other relevant context. With skip-gram the word delightful will not try to compete with the word beautiful but instead, delightful+context pairs will be treated as new observations.
    
## Word embedding for topic modelling

- Word embedding + K-means
    - https://ai.intelligentonlinetools.com/ml/text-clustering-word-embedding-machine-learning/
    - http://xplordat.com/2018/12/14/want-to-cluster-text-try-custom-word-embeddings/
- lda2vec
    - https://towardsdatascience.com/lda2vec-word-embeddings-in-topic-models-4ee3fc4b2843
    - https://multithreaded.stitchfix.com/blog/2016/05/27/lda2vec ("if you want human-readable topics just use LDA")
    - https://github.com/cemoody/lda2vec ("it's still research software. I wouldn't run this in production")
