## Packages

- Gensim has `LdaModel` and `LdaMulticore`.
- Apart from `pyLDAviz`, the evaluation can be done using gensim's `CoherenceModel`.


## Processing steps for each doc: Doc -> Sentences -> (lowercased)Tokens -> POS -> Lemmas -> CountVectorizer

### Document to sentences

- Splitting each document into sentences seems to be required for `word_tokenize()` and `pos_tag()`.
- Two ways to split documents into sentences. See https://www.nltk.org/api/nltk.tokenize.html
    - Use `PunktSentenceTokenizer`

            splitter = nltk.data.load('tokenizers/punkt/english.pickle')
            splitter.tokenize(doc)

    - Use `sent_tokenize`

### Lowercasing

- Do not lowercase the words till after splitting documents into sentences, as lowercasing may impact the accuracy of detecting the sentence boundaries of messy text.

### Tokenization

- Use `nltk.word_tokenize()` and it assumes that the text has already been segmented into sentences.
- `word_tokenize()` will turn `don't` into `do` and `n't`, and `they'll` into `they` and `'ll`. No need to correct them, since `n't` and `'ll` are not meaningful.

### POS Tagging + Lemmatization

- A lemmatizer (`nltk.stem.WordNetLemmatizer`) performs better with POS tags than without them.
- Use `nltk.pos_tag()` and it can be inferred from https://www.nltk.org/api/nltk.tag.html that `pos_tag()` expects one sentence instead of one paragraph.
- From: https://stackoverflow.com/questions/15586721 and https://www.machinelearningplus.com/nlp/lemmatization-examples-python, `nltk.pos_tag()` needs a function to map its POS tag types to WordNetLemmatizer's POS tag types.
- Lowercasing need to be done so that Lemmatization can be done correctly. E.g. Lemmatization won't fix `Researchers`, but `researchers` can become `researcher`. A word with a capital first letter may be out of the vocabulary.
- Punctuations will not be removed.

### CountVectorizer

- Stop words (from nltk.corpus) and punctuations can be removed at this stage by specifying them in arguments.

### Generate corpus ready for gensim's LDA

- `gensim.matutils.Sparse2Corpus`, to get: for each doc, a list of (token_id, token_count) pairs.


## Outcome

- Calculate the Coherence Score vs Number of Topics using `CoherenceModel`
- Top words per topic once optimum number of topics selected
- Most probable docs per topic
- Most probable topics per doc


## About pyLDAviz

From: https://nbviewer.jupyter.org/github/bmabey/hacker_news_topic_modelling/blob/master/HN%20Topic%20Model%20Talk.ipynb

### Saliency

- Top 30 most relevant (in terms of saliency) terms for topic n.
- `saliency(term w) for topic t = frequency(w) * [sum_t p(t | w) * log(p(t | w)/p(t))]`
- The percetage of the red bar length out of the total bar length stands for how informative the specific term w is for determining the generating topic, versus a randomly-selected term.

### Relevance

- Adjustable relevance metric λ
- `relevance(term w | topic t) = λ * p(w | t) + (1 - λ) * p(w | t)/p(w)`
- λ determines the weight given to the probability of term w under topic k relative to its lift. 
- Setting λ to 1 results in the familiar ranking of terms in decreasing order of their topic-specific probability, and setting λ to 0 ranks terms solely by their lift.
- The optimal value of λ was about 0.6.