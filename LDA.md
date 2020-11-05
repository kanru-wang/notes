### Steps to convert: Document->Sentences->(lowercase)Tokens->POS->Lemmas->CountVectorizer

### Document to sentences

- Splitting each document into sentences seems to be required for word_tokenize() and pos_tag().
- Two ways to split documents into sentences. See https://www.nltk.org/api/nltk.tokenize.html
    - Use PunktSentenceTokenizer

            splitter = nltk.data.load('tokenizers/punkt/english.pickle')
            splitter.tokenize(doc)

    - Use sent_tokenize

### Lowercasing

- Do not lowercase the words till after splitting documents into sentences, as lowercasing may impact the accuracy of detecting the sentence boundaries of messy text.

### Tokenization

- Use word_tokenize() and it assumes that the text has already been segmented into sentences.
- word_tokenize() will turn `don't` into `do` and `n't`, and `they'll` into `they` and `'ll`. No need to correct them, since `n't` and `'ll` are not meaningful.

### POS Tagging + Lemmatization

- A lemmatizer performs better with POS tags than without them.
- Use nltk.pos_tag() and it can be inferred from https://www.nltk.org/api/nltk.tag.html that pos_tag() expects one sentence instead of one paragraph.
- From: https://stackoverflow.com/questions/15586721 and https://www.machinelearningplus.com/nlp/lemmatization-examples-python, nltk.pos_tag() needs a function to map its POS tag types to WordNetLemmatizer's POS tag types.
- Lowercasing need to be done so that Lemmatization can be done correctly. E.g. Lemmatization won't fix `Researchers`, but `researchers` can become `researcher`. A word with a capital first letter may be out of the vocabulary.
- Punctuations will not be removed, but can be removed by CountVectorizer as the next step.