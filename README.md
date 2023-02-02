# Emotion Detection
<a target="_blank" href="https://colab.research.google.com/github/giuseppe-tanzi/Emotion-Detection/blob/main/Emotion_Detection.ipynb">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
</a>

Project work for the "Deep Learning" course of the Artificial Intelligence Master's Degree at University of Bologna.

## Problem

The task consists in creating an Emotion Detection model, able to recognise the emotion expressed in a given
text/sentence.

## Dataset

The [dataset](./data) is composed of 58k texts labelled manually to assign them an emotion:

- Anger
- Digust
- Fear
- Joy
- Sadness
- Surprise
- Neutral

## Preprocessing

The following processing step have been performed:

- Expansion of the contracted forms;
- Conversion of emoji to text;
- Removal of the Saxon genitive;
- Specific preprocessing for Gloove;
- Specific tokenization for twitter text.

## First Model

The model applied in this study use the long-short term memory networks (LSTM) in their bidirectional variation and the convolutional neural netowrks (CNN) mediated by a max pooling approach. Furthermore they use a self-attention layer to allow the system to capture distant relationships among words with a different weight depending theirs contribute for classification.

In particural the architecture is the following:
- the first layer is an embedding layer that has the pre-trained Gloove embedding matrix;
- the second layer is a Bi-LSTM layer that considers the intrinsic sequential relationship between the terms of a sentence;
- the third layer is a Self-Attention layer that provides the model ability to weight the vectors of single words of the sentence differently, according to the similarity of the neighboring tokens;
- the fourth layer is a CNN layer, ideal for working on data with a shape of grid; the result is a grid more dense and smaller of the previous that captures the hidden relations among cells that fall in the kernel dimension;
- it has been applied a between the output of the CNN layer and the output of the previous Bi-LSTM layer in order to letting the model to better conceptualize both local and long-term features;
- then there are operations of max pooling followed by operations of global average pooling;
- finally there are dense layers that estimates the probability distribution of each of the emotional classes of the dataset.


I added some regularizer into the self attention layer in order to prevent overfitting

## Second Model

The proposed TCN model is inspired by Christof Henkel<sup>3</sup>, one of the grandmasters on Kaggle.

The model consists of:
- Two TCN blocks stacked with the kernel size of 3 and dilation factors of 1, 2, and 4;
- The first TCN block contains 128 filters, and the second block uses 64 filters; the input features will be based on Word Embedding;
- Each block’s result will take the form of a sequence;
- The final sequence is then passed to two different global pooling layers;
- Next, both results are concatenated and passed into a dense layer of 16 neurons, and pass to the output.

## Results

The best model for the F1-score metric is the first model for better score, since it reaches a F1-score of 0.61 with
about 7M parameters to train.

## Bibliography

<sup>1</sup> Polignano, Marco & Basile, Pierpaolo & de Gemmis, Marco & Semeraro, Giovanni. (2019). "A Comparison of
Word-Embeddings in Emotion Detection from Text using BiLSTM, CNN and Self-Attention". \\
https://www.researchgate.net/publication/333740389_A_Comparison_of_Word-Embeddings_in_Emotion_Detection_from_Text_using_BiLSTM_CNN_and_Self-Attention

<sup>2</sup> S.bai, J. Kolter, and V. Koltun, "An empirical Evaluation of Generic Convolutional and Recurrent Networks
for Sequence Modelling", *arXiv*, April, 2018

<sup>3</sup> C.Henkel, "Temporal Convolutional Network", *Kaggle*,
https://www.kaggle.com/code/christofhenkel/temporal-convolutional-network/notebook
