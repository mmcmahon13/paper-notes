# Named Entity Recognition with Bidirectional LSTMs

*Jason Chiu, Eric Nichols*

- Main goal = avoid costly feature engineering and lexicons for NER
- Instead, use a hybrid CNN-LSTM architecture to automatically detect word and character-level features
- Encode "partial lexicon matches" in networks
- Model is competitive on both CoNLL-2003 and OntoNotes 5.0, establishes new state-of-the-art when lexicons are used

## Intro

- Collobert et. al. (2011) -> neural model to learn features from word embeddings
    - limitations:
        - uses feed-forward NN, restricts size of window (can't capture long-distance relationships between words)
        - doesn't capture character-level features (which could be useful for rare words)
 - RNN -> has long term memory, can handle variable-length input
     - speech recongition (Graves, et. al. 2013)
     - machine translation (Cho et. al. 2014)
     - language modeling (Miklov et al. 2011)
 - LSTM -> has forget gate, can learn long-distance dependencies
     - ** bidirectional LSTMs can take into account infinit context on both sides **
     - good for sequential labeling tasks like speech recognition and NER
 - Char-based CNN
     - NER, POS (Santos et. al. 2015) (Labeu et al. 2015)
     - semantic role labeling (Collobert 2011)
