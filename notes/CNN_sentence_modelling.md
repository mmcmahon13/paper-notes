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


