# Data Analysis Project: NLP based on a Financial Consumer Complaints Dataset

This portfolio is based on the “Financial Consumer Complaints Dataset” downloaded as a csv file from Kaggle (https://www.kaggle.com/datasets/sherrytp/consumer-complaints).
The aim is to apply NLP to extract the most prevalent topics using different NLP techniques.
Focus is on the column “Consumer complaint narrative” containing unstructured free-text field data.

To make the procedure trackable, different versions of code are available.
V6 contains the complete final code including all pre-processing steps as well as vectorization and topic modeling techniques.
V7 is the final version in terms of only applying the techniques leading to the best NLP performance based on the results of V6.

## Table of Contents

1. [Data Preprocessing with nltk and gensim](#Data-Preprocessing-with-nltk-and-gensim)
2. [Vectorization with gensim](#Vectorization-with-gensim)
3. [Topic Modeling with gensim](#Topic-Modeling-with-gensim)
4. [Topic Modeling Results](#Topic-Modeling-Results)

## Data Preprocessing with nltk and gensim

To start with (V1), the following techniques have been used for pre-processing the data: 
(1)	Tokenization including removal of stopwords, capitalization and restriction on alphabetic values 
(2)	Lemmatization with POS tagging 
(3)	Extraction of most common bigrams
(4)	Removal of words which are occurring with a frequency > 0.5
When searching for ways to improve the coherence score, the following changes in pre-processing have been made thereby always working with a subset of the data to minimize loading times and monitoring the results of the different topic models:
V2: POS tagging to only consider nouns and adjectives (no major improvement but also no decrease)
V3: V2+Replacement of lemmatization by stemming (Porter) (slight improvement for LSI and LDA)
V4: V3+Replacement of most common bigrams by trigrams (discarded due to performance decrease)
V5: V3+Transformation of all tokens into bigrams (major improvement over all models)

## Vectorization with gensim
*(a)	Bag of Words (BoW) – doc2bow (gensim)*
BoW displays the frequency of each word for each consumer narrative in a vectorial form.
*(b)	Term Frequency-Inverse Document Frequency (TF-IDF) –TfidfModel&doc2bow (gensim)*
Addressing the shortcomings of BoW of only considering one customer narrative at a time without looking at the whole corpus, TF-IDF extends BoW (=TF) by adding a second component, IDF, which evaluates how rare a word is across all relevant narratives. 

## Topic Modeling with gensim
For topic modeling, the following three unsupervised machine learning techniques have been used each once with BoW and once with TF-IDF as input. 
*(a)	Latent Semantic Analysis (LSA) / Latent Semantic Indexing (LSI) – LsiModel (gensim)*
LSA is based on the idea of dimensionality reduction to be able to extract the most important topics of a dataset. Sparsity and noise of the BoW-/TF-IDF-based matrix are reduced with the help of singular value decomposition grouping frequent word occurrences to the same topic.
*(b)	Latent Dirichlet Allocation (LDA) – LdaModel (gensim)*
LDA uses the Dirichlet distribution and groups words to the same topic based on the idea that (a) each topic is a mix over words and (b) each document is a mix over topics.
*(c)	Hierarchical Dirichlet Process (HDP) – HdpModel (gensim)*
HDP is an extension to LDA which, in contrast to LSA and LDA, infers the number of topics from the data while training the corpus and dictionary.

## Topic Modeling Results
*LDA with BoW* has shown the highest and most consistent coherence score.
Six topics have been chosen to be extracted and the following topics could be derived:
(1) Inaccurate late payment credit reporting
(2) Breach of privacy rights and authorization concepts
(3) Inaccurate hard inquiries
(4) Incorrect/unkown credit accounts
(5) Identify theft
(6) Missing/late responses on deletion of inaccurate accounts

