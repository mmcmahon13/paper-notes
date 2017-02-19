# Convolutional Neural Networks for Sentence Classification

*Kim, Y. (2014). Convolutional Neural Networks for Sentence Classification. Proceedings of the 2014 Conference on Empirical Methods in Natural Language Processing (EMNLP 2014), 1746–1751*

Experiments with CNNs trained on top of pre-trained word vectors.

## Intro
* Deep learning used within NLP predominantly for learning word vector representations
* Word vectors = feature extractors to encode semantic features of words in dimensions
* close words = close in vector space
* paper trains CNN with one convolutional layer on top of word vectors (trained by Miklov et al on 100 billion Google News words)
* hold vectors static, train other network params (this performs well, implying that the pretrained vectors are "universal" feature extractors)

## Model
* variant of Collobert et al. CNN architecture
* (TODO insert image from paper)
* x_i = k-dim word vector for ith word in sentence
* Represent a sentence of length n by concatenating the word vectors for the words in the sentence, padding if needed
* Convolution = applying a filter to a window of words to produce a new feature
* Feature map = result of applying filter to each possible window of words
* Pass feature map to max-over-time pooling layer
    * c_hat = max{c} (capture the most important feature for each feature map)
    * pooling deals with variable sentence lengths
* Model uses multiple filters with varying window sizes
* Resulting feature maps are passed to fully-connected softmax layer, output = probability distribution over labels
* One of their experiments has two "channels" for embeddings, one that is finetuned and one that is held static

### Regularization
* Dropout on the second to last layer with constraint on L2 norms of weight vectors
   * use a masking vector of Bernoulli random variables with prob P of being 1
   * backprop through unmasked units only
   * scale learned weight vectors by P at test time
   * constrain L2 norms by rescaling **w** so that ||**w**||_2 = s whenever ||**w**||_2 > s after each gradient descent step
   
## Datasets and Experimental Setup
* MR: movie reviews, one sentence per review, positive or negative
* SST-1: Stanford Sentiment Treebank, extension of MR with more fine-grained labels
* SST-2: removed neutral reviews, binary labels
* Subj: subjectivity dataset (is sentence subjective or objective)
* TREC: question dataset (classify into question types)
* CR: customer reviews of products, predict positive and negative
* MPQA: opinion polarity detection

### Hyperparameters/Training
* RELUs, filter windows of 3,4,5 w/ 100 feature maps each, dropout of 0.5, l2 of 3, minibatch size 50

### Pretrained Word Vectors
* word2vec (300 dim, trained with CBOW architecture)
* initialize words randomly if they don't occur in the pretrained set

### Model Variations
* CNN-rand (baseline, all words randomly initialized and then learned during training)
* CNN-static (with pretrained vectors from word2vec, kept static)
* CNN-non-static (same but vectors are fine-tuned)
* CNN-multichannel (both word2vec, apply filters to both channels, but fine-tune only one of them)

## Results
* CNN-rand does poorly
* word vectors (even static) do pretty well
* non-static word vectors are competitive with/beat state-of-the-art in some cases
* they thought the two-channel approach might prevent overfititng, but results are mixed?
* they tried Collobert Wikipedia embeddings and found that word2vec worked better
