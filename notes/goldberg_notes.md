# A Primer on Neural Network Models for Natural Language Processing

Summary: https://machinelearningmastery.com/primer-neural-network-models-natural-language-processing/

## Architectures

### Fully connected NN
- basic non-linear classifier, can be used as a replacement wherever linear learner is used

### CNN
- "location invariant classifier" (like advanced ngram)
- useful when there are "strong local clues regarding class membership, but these clues can appear in different places in the input" (eg phrase in a doc or n-grams that are predictive but we don't know where they occur)


### RNN/LSTM
- chain/tree structure prediction classfiers
- preserves structural informationw when working with sequences or trees

### Recursive NN
- generalization of RNN that can handle trees


## Non-linearities
- reLu
- hard tanh
- tanh
- sigmoid

## Features
- CBOW, weighted CBOw, word embeddings, etc.
- "feature" = concrete linguistic input (word, POS tag, etc.)
- input vector = actual input fed into NN

### Feature Representation
- represent input X (words, POS, liguistic info) as a dense vector (rather than a 1-hot encoding)
- "embed" core featuress into d-dimensional space and train them along with the other params of the neural net

One-hot | Dense reperesentations
--------|-----------------------
each feature has own dimension | each feature has dimensionality d
dimensionality = num features | similar features means similar vectors, generalize better
features are computed independently of each other | computationally easier to deal with, lower dimension
