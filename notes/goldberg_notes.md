# A Primer on Neural Network Models for Natural Language Processing

Summary: https://machinelearningmastery.com/primer-neural-network-models-natural-language-processing/

## Architectures

### Fully connected NN/Feed-forward NN
- basic non-linear classifier, can be used as a replacement wherever linear learner is used

todo insert pic

- brain metaphor: network of neurons which each take a scalar input, multiplies input by its weight, results are summed, nonlinear function is applied to final result
- each circle is a neuron, neurons are arranged in layers

#### Fully connected layer
- h = xW where W_(ij) = weight on arrow from ith neuron to jth neuron in next layer
- transform result with nonlinearity g()
- g(xW) is the whole computation done by the first layer

#### Perceptron
- linear function of inputs
- NN_perceptron(x) = xW + b where x is an input vector of dim d_(in), W is a weight matrix of dim (d_in x d_out), and b is a bias vector of dim d_out

* to go beyond linear functions, introduce a non-linearity (activation?) function g applied elementwise on output of layer *

- if network's output is dimension d=1, can use for regression/scoring
- if dimension of output is > 1, can use for classification (scoring, distribution over classes)

- multilayer perceptron = "universal approximator" - can hypothetically represent anything, but may be impractical (e.g. may need giant architectures or a lot of time)

- softmax + perceptron = *multinomial logistic regression classifier* (aka max-entropy classifier)

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
   - max(0,x)
- hard tanh
  - approximation of tanh
  - -1 if x < -1
  - x if x > 1
  - x otherwise
- tanh
  - tanh(x) = (e^(2x)-1)/(e^(2x)+1)
- sigmoid
  - sigmoid(x) = 1/(1 + e^(-x))
  
## Output Transformations
### Softmax
- for x = x_1 ... x_k
   - softmax(x_i) = sum from j = 1 to k (e^(x_i)/e^(x_j))
- make output like a discrete probablity distribution over k outcomes
- use in conjunction with "probabalistic training objective such as cross-entropy" (?)

## Loss Functions
- L(y_real, y_predicted)
- training objective is to minimize the loss function
### Hinge (binary) loss
- aka SVM
- hinge(y_real, y_predicted) = max(0, 1-y_real * y_predicted) where y_real = {-1,+1}
- want y_real and y_ predicted to share the same sign

.. (continue this section) 


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

#### Variable number of features: continuous bag of words (CBOW)
- feed forward nets expect fixed sized input, but number of features isn't always known in advance
- need a way to represent unbounded number of features in fixed sized vector
- CBOW
  - sum or average the embeddings of the features (reduces to normal bag of words if the embeddings are 1hot vectors)
(todo insert equation)

#### Embedding layers
- c(.) = function from core features to input vectors (extract embeddings from core features and concatenate them e.g.)
- some networks treat c as part of the network as a "lookup" or "embedding" layer

( todo copy remainder of notes on this, too many equations - take pic?)

## General structure for Feed-Forward NLP classification system
1. extract set of core linguistic features f_1...f_k that are relevant to the output class
2. for each feature f_i retrieve corresponding feature vector v(f_i)
3. combine vectors (via concatenation, summation, etc.) into input vector x
4. feed x into nonlinear classifier (feedforward NN)

- Network takes care of learning interactions between combos of features using nonlinearities
- For multiclass classification with k classes, output is k-dim vector with score for each class

