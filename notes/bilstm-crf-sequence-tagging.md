# Bidirectional LSTM-CRF Models for Sequence Tagging

_Huang, et. al._

- Explores LSTMs, BiLSTMs, LSTM-CRF, and BiLISTM-CRFs for sequence tagging
- BiLISTM-CRF can "efficiently use past and future input features" (BiLSTM component) while also using sentence-level tag information (CRF layer)
- BiLSTM-CRF = close to state-of-the-art resutls on POS, chunking, NER
- robust, less dependence on word embedding quality

## Intro
- Sequence tagging = POS taggint, chunking, NER, etc.
- Conventional approaches:
    - HMM
    - MEMMs
    - CRFs
- CNNs
    - Collobert (2011)
    - Conv-CRF (consists of conv-net and CRF layer (sentence level LL) on output)
- RNN, etc.
   - Graves

- this paper = comparison of many variations on LSTMs

## Models

### RNN/LSTM
- input layer x, hiddlen layer h, output layer y
- input = features at time t (1-hot word encoding, dense word vectors, sparse features)
- output = distribution over tags at time t
- LSTM (Hochreiter and Schmidhuber, 1997; Graves et al., 2005)
    - replace hidden layer with "purpose-built memory cell"
    - better at exploiting long-range dependencies

### Bidirectional LSTM
- use past features (via forward states) and future features (via backward states)
- train via backprop through time (BPTT)

### CRF
- 2 ways to use neighboring tags to predict current tags:
    - predict distribution of tags for each timestep, use "beam-like decoding to find optimal tag sequences" (MEMM)
    - focus on sentence-level rather than individual positions (CRFs)
    
### LSTM-CRF
- add lines connecting consecutive output layers
- CRF layer has state-transition matrix as params
- use past and future tags to predict current tag

### Training
- SGD forward-backward training process
- batches of size 100 (sentences of length < 100?)
- run forward pass for LSTM-CRF, which includes forward/backward state of LSTM
- get output score for all tag positions
- run CRF layer forward and backward pass to get gradients for network output and state-transition edges
- update params (state transition matrix and original LSTM params)

## Experiments
### Data
- Penn TreeBank POS tagging, CoNLL 2000 chunking, CoNLL 2003 NER

### Features
#### Spelling 
- whether start with a capital letter
- whether has all capital letters
- whether has all lower case letters
- whether has non initial capital letters
- whether mix with letters and digits
- whether has punctuation
- letter prefixes and suffixes (with window size of 2 to 5)
- whether has apostrophe end (’s)
- letters only, for example, I. B. M. to IBM
- non-letters only, for example, A. T. &T. to ..&
- word pattern feature, with capital letters, lower case letters, and digits mapped to ‘A’, ‘a’ and ‘0’ respectively, for example, D56y-3 to A00a-0
- word pattern summarization feature, similar to word pattern feature but with consecutive identical characters removed. For example, D56y-3 to A0a-0

#### Context
- unigram, bigram, trigram features

#### Word Embedding
- 50d collobert embeddings

#### Feature "connection tricks"
- instead of treating spelling and context features the same as word features, directly connect spelling and context features to output?

### Results
- train all models, using both random and SENNA embeddings
- baselines are LSTM (worst), BiLSTM (better on POS/chunk), and CRF (better on NER)
- best model = BiLSTM-CRF
- when hand-engineered features are removed, CRF performance degrades, but BILSTM is more robust
- BILSTM-CRF can achieve good tagging accuracy without resorting ot embeddings



