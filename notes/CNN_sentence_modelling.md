# A Convolutional Neural Network for Modelling Sentences

**Kalchbrenner, N., Grefenstette, E., & Blunsom, P. (2014). A Convolutional Neural Network for Modelling Sentences. Acl, 655–665.**

* Dynamic Convolutional Neural Network (DCNN) architecture used for semantic modelling of sentences
* Network uses "Dynamic k-max pooling"
    * global pooling over linear sequences
* Handles senteces of varying length, can capture short and long-range feature relations
* Doesn't rely on parse tree

## Intro
* General problem = classify or generate sentence based on its semantic content
* Represent sentence in terms of word features and short n-grams
* Feature function defines how sentence features are extracted from word/n-gram features
* Advantages of neural sentence models:
    * can be trained to obtain generic vectors for words and phrases (embeddings?)
    * can fine-tune these vectors for specific tasks
* Their CNN interleaves 1D conv layers and dynamic k-max pooling layers (generalization of normal max pooling)
    * return k maximum values instead of just one
    * choose k dynamically based on other aspects of network/input
    * conv layers apply 1D filters across each row of features in sentence matrix
    * compute multiple feature maps with different filters
    * "the weights at these layers form an order-4 tensor." (?)
* Multiple layers induce "structured feature graph" over input sentence
    * e.g. "small filters at higher layers can capture syntactic/semantic relatiosn between non-continuous phrases that are far apart in input"

## Background
### Related Neural Sentence Models
* Neural Bag-of-words (NBoW)
    * projection layer to map words, sub-word units, n-grams to high dim. embeddings
    * combine embeddings component-wise by summing or some other operation
    * classify the resulting combined vector
* Recursive Neural Network (RecNN)
    * combine left and right children of a node with a "classical" layer (what is this?)
    * share weights of layer across all tree nodes
    * layer at root node = representation for sentence
* Recurrent Neural Network (RNN)
    * special case of RecNN with a linear chain
    * primarily used as langaue model, but can also be used as a sentence model
    * layer computed at last word represents the sentence
* TDNN

### Convolution
* 1D convolution = operation between vector of weights **m** (size m) and vector of inputs **s** (size s)
* **m** = filter
* take dot product of vector **m** with each m-gram in sentece to get sequence **c**
* narrow vs. wide convolutions
    * narrow = |s| >= |m|, gives sequence of size s-m+1
    * wide = gives sequence of size s+m-1
    * (i don't really understand this, look it up somewhere else..)
* trained filter weights = "linguistic feature detector that learns to recongize a specific class of n-grams"
* wide convolution ensures that all weights reach entire sentence, including margin words, and that **c** is always non-empty

### Time-Delay Neural Networks
* Sequence of inputs **s** with a set of weights **m**
* Sequence has a time dimension, convolution is applied over time dimension
* each s<sub>j</sub> is often a vector of d values, so that **s** is a matrix of size dxs
* **m** is a matrix of size dxm of weights
* Convolve each row of **m** with corresponding row of **s** (usually narrow)
* take resulting sequence **c** as input to next layer
* Max-TDNN model = based on TDNN
* apply narrow conv layer to sentence matrix **s**, where each column of **s** is a feature vector w<sub>i</sub> of word in sentence
     * *todo add images of this*
* Take max of each row in c to get a vector of d values (capture most relevant feature for each dimension d in c)
* pass **c<sub>max</sub>** to fully-connected classification layer
* Advantages of Max-TDNN
    * Sensitive to word order in sentence, doesn't need dependency/parse trees
    * Gives equal-ish importance to signal from each word in sentence (except margins)
* Limits of Max-TDNN
    * Range of feature detectors is limited by spans of filters
    * Stacking narrow conv layers or increasing m makes range of feature detectors larger, but makes it harder to include the margins, increases minimum size of s that can be convolved
* Limits of Max-pooling
    * Can't distinguish between a relevant feature occurring once or multiple times
    * Doesn't remember the order that the features occur in
    * Pooling factor is "excessive"

## Convolutional Neural Networks with Dynamic k-Max Pooling

### DCNN
* Alternate wide convolution layers with dynamic k-max pooling layers
* Width of a feature map at intermediate layer varies depending on length of input sentence

### Wide Convolution
* First layer: take embedding **w<sub>i</sub>** (dim d) for each word, construct d x s sentence matrix 
* Embeddings are params that are optimized during training
* Conv layer: convolve matrix of weights **m** (size d x m) with matrix of activations at previus layer
* *d* and *m* are network hyperparams
* **c** has dimensions *d* x (*s* + *m* - 1)

### k-max Pooling
* Instead of selecting max value in each dim, take the top k features, preserving their order
* Apply at topmost conv layer (to make sure that the input to the rest of the network is independent sentence length)
* Dynamically select k at lower layers to allow "smooth extraction of higher-order and longer-range features"

### Dynamic K-max Pooling
* k-max pooling where k is a function of the length of the sentence and the depth of the network
* k = max(*k<sub>top</sub>*, (*L*-*l*)*s*/*L*) where *l* is the number of the current conv layer and *L* is the total number of conv layers, *k<sub>top</sub>* is the fixed param at the topmost layer

### Non-linear Feature Function
* apply nonlinearity *g* component-wise to pooled matrix and add bias after pooling (one bias value for each row of pooled matrix)
* (see paper, there is math that I don't understand/can't type out here)

### Multiple Feature Maps
* **F<sup>i</sup>** = feature map of ith order
* Compute multiple feature maps of order i in parallel on same layer 
* Compute a certain feature map for layer i by convolving filters with each feature map from previous layer and summing results
    * (see paper for mathematical definition of this)

### Folding
* Sum every two rows in feature map component-wise (after convoution, before pooling) to allow feature detectors to depend on multiple rows? (don't really get this)

## Properties of the Sentence Model

### Word and n-gram Order
* Want the model to be able to distinguish whether an n-gram occurs in input, and the relative position of relevant n-grams
* First layer filters can learn to recognize n-grams that are shorted than m
* Generalized pooling induces "invariance to absolute positions", but maintains order/relevant positions
* This differs from NBoW which is insensitive to word order, and RNNs which use word order but are biased towards more recent words

### Induced Feature Graph
* Node from layer is connected to node from next higher layer if lower node is used in convolution that computes higher node
* Nodes that aren't selected by pooling operation at a layer are dropped from graph
* Last nodes after last pooliing layer are connected to topmost root
* induces a DAG with eighted edges and a root node
* Generalization of other things (like parse tree, etc)

## Experiments

### Training
* Fully-connected layer followed by softmax nonlinearity to give probability distribution over classes
* Objective = minimize cross-entropy of predicted and true distribution
* include L2 regularization over params in loss
* params = word embeddings, filter weights, weights from fully-connected layers
* minibatch SGD with Adagrad update

### Sentiment Prediction in Movie Reviews
* Two experiments on Stanford Sentiment Treebank (one with binary output variable, the other has 5 outcomes)
* Randomly initialize word vectors, of d = 48
* (see paper for details of network params)
* use tanh nonlinearity, dropout on the second to last layer after last tanh
* DCNN outperforms other models

### Question Type Classification
* TREC dataset
* use d=32 for word vecs, initialize with pre-trained word embeddings
* difference between DCNN and other models' performance is not significant

### Twitter Sentiment Prediction w/ Distance Supervision
* dataset of tweets labeled pos or neg based on the emoticons in them
* randomly initialize embeddings of d=60
* DCNN outperforms other models

### Visualizing Feature Detectors
* they visualize the top five 7-grams at four feature detectors in first layer of network

## NOTES:
*I don't understand the Time-Delay networks at all, might have to look those up somewhere else?*
    
