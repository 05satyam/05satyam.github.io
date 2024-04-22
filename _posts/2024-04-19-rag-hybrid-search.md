---
title: "RAG - Hybrid Search"
date: 2024-04-19
categories: DeepLearning
---

# RAG - BASED ON HYBRID SEARCH
 1. Hybrid search most commonly refer to use of two or more algorithms to make a search and hence improve the search results.
 2. Most commonly in RAG - keyword based search and vector based search usually combined and called as hybrid search
    
## Keyword Based Search :
 1. Often uses uses `Sparse Embeddings` and sometime also known as `Sparse vector search`.
 2. Sparse Embeddings: Vectors with mostly ZERO values and very few NON-ZERO values.
    Example: [0, 0, 0, 0, 0, 1, 0, 0, 0, 4, 0, 0, 0, 0, ...]
 3. Several algorithms are there to generate these Sparse Embeddings. Most common one is `BM25 (Best match 25)` - Designed upon the TF-IDF (Term Frequency-Inverse Document 
    Frequency)
    => In simple terms BM25 creates Sparse Embeddings based on "terms frequency in a document relative to their frequency across all documents"

## Vector Based Search :
 1. Modern search technique
 2. These vector embeddings are usually desenly packed with very few ZERO values i.e. Dense Vector Embeddings
    Example: [0.3,0.5,0.1,0.6,0,.....0.11]
 3. Vectors Embeddings containes : Are, numerical representation of data objects in various modalities (text, images, etc.) 
 4. Vector Embeddings calculate similarity scores based on similarity metric like cosine distance etc

## Combining search results :
  1. Usually, first get the searched scores.
  2. Calculate a hybrid score: 
     => hybrid_score = (1 - alpha) * sparse_score + alpha * dense_score
        alpha = 1: Pure vector search
        alpha = 0: Pure keyword search
     => This Paper list ways to combine score: Benham, R., & Culpepper, J. S. (2017). Risk-reward trade-offs in rank fusion. In Proceedings of the 22nd Australasian Document 
        Computing Symposium 

---

## [.ipynb Notebook](https://github.com/05satyam/large_language_models/blob/main/rag/HybridSearch.ipynb)