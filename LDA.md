## Document to sentences

- Steps to convert: Document->Sentences->(lowercase)Tokens->POS->Lemmas
- Two ways to split documents into sentences. See https://www.nltk.org/api/nltk.tokenize.html
    - Use PunktSentenceTokenizer
            splitter = nltk.data.load('tokenizers/punkt/english.pickle')
            splitter.tokenize(doc)
    - Use sent_tokenize

## Tokenization

- Use word_tokenize() and it assumes that the text has already been segmented into sentences.
- word_tokenize() will turn `don't` into `do` and `n't`, and `they'll` into `they` and `'ll`.

## Lemmatization

- Do not lowercase the words till after splitting documents into sentences, as lowercasing may impact the accuracy of detecting the sentence boundaries of messy text.
- Lowercasing need to be done so that POS + Lemmatization can be done correctly. E.g. POS + Lemmatization won't fix `Researchers`, but `researchers` can become `researcher`.
- From: https://stackoverflow.com/questions/15586721 and https://www.machinelearningplus.com/nlp/lemmatization-examples-python, nltk.pos_tag needs a function to map its pos tag names to WordNetLemmatizer's pos tag names.
- Punctuations will not be removed, but can be removed by CountVectorizer as the next step.