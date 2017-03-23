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

## Model

- Inspired by Collobert 2011: lookup tables from word/character -> continuous vector represnetations, concatenated and fed into network
- Use BiLSTM instead of feed-forward network, with CNN to induce char-level features

### Sequence labelling with BiLSTM

- Follows speech-recognition approach by Graves et. al (2013)
- Feed word embeddings, CNN extracted features into a forward LSTM and a backward LSTM
- Linear layer decodes output of each network and log-softmax layer to produce probabilities over labels
- Add the two vectors together to produce final output

### Core Features
#### Word Embeddings
- 50d Collobert embeddings (wikipeida, reuters)
- lowercase the words, pass them through lookup layer to get embeddings
- allow fine-tuning of word embeddings during training

#### Character Embeddings
- randomly initialize char embeddings in range [-0.5, 5.0] (25d)
- add additional PADDING and UNKNOWN tokens to char set

### Additional Word-Level Features
- Capitalization features (shape)
#### Lexicons
- for each lexicon category:
    - Match every n-gram (up to lenght of longest entry) against entries in lexicon
    - Success = matching prefix/suffix of an entry that is at least half the length of the entry
    - Discard matches of less than two tokens (except for Person category)
    - Prefer exact over partial matches
    - For each token in match, encode feature using BIOLU annotation (e.g. token is B-person or something?)
    - This performs better than Collobert (who treats partial and exact matches equally, marks tokens with Y/N)

### Additional Character-Level Features
- use lookup table to produce 4d vector representing character type (upper, lower, punctuation, other)

### Training and Inference 
- done on per-sentence level
- initialize network with 0 vectors
- randomly initialize lookup tables with stdnorm (except pretrained embeddings)
- Objective = maximize sentence-level log-likelihood
- (see paper 2.6 for equations, or notebook notes)
- At inference time, use viterbi to find tag sequence that maximizes score given the outputs of the network

#### Tagging Scheme
- BIOLU annotations on the output tags

#### Learning Algorithm
- minibatch SGD, fixed learning rate
- apply dropout to output nodes of each LSTM layer to reduce overfitting

### Evaluation
- CoNLL 2003
- Ontonotes 5.0
- trained a BiLSTM, BiLSTM-CNN, BiLSTM-CNN+lexicon

#### Hyperparam optimization (see paper)

- training takes ~6 hours for ontonotes?

#### Character-level CNNs vs. Character Type and Capitalization Features
- BiLSTM-CNN outperfoms BLSTM - suggests that char-level CNNs can replace handcrafted character features in some cases

## Related Research (NER w/ Neural Nets)
- Collobert (2011) - SENNA (deep FFNN and word embeddings for near state-of-the-art on POS tagging, chunking, NER, SRL)
- Santos (2015) - CharWNN (augments collobert, et al with chara-level CNNs for spanish/portuguese NER)
- Huang et. al. (2015) - BiLSTM for POS tagging, chunking, NER tasks (used heavy feature engineering instead of CNN for char-level features)
- Ling et. al. (2015) uses word and char-level BiLSTMs for english POS tagging



    

