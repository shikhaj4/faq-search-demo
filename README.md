# FAQ Search with Embeddings + ANN + Reranking

This project demonstrates how **Airbnb-style search systems** can be built at a smaller scale.

## Concepts
- **Embeddings** → Convert text (questions/answers) into numerical vectors in high-dimensional space.
- **IVF ANN (Inverted File Index for Approximate Nearest Neighbor)** → Speeds up similarity search by searching only in the most relevant vector clusters.
- **Contrastive Learning (used in large-scale systems like Airbnb)** → Models are trained to bring semantically similar pairs closer and push dissimilar ones apart.
- **Reranking** → After ANN search, we reorder candidates by semantic similarity for better accuracy.

## Workflow
1. Extract Q&A pairs from dataset.
2. Generate embeddings with `all-MiniLM-L6-v2`.
3. Index vectors with **Faiss IVF ANN**.
4. Query search with ANN → rerank results.
5. Return top answers.


Dataset: [rjac/e-commerce-customer-support-qa](https://huggingface.co/datasets/rjac/e-commerce-customer-support-qa) from Hugging Face.  
Embedding model: `sentence-transformers/all-MiniLM-L6-v2`  
Reranker model: `cross-encoder/ms-marco-MiniLM-L-6-v2`

---

## How to Run

1. Clone repo
   ```bash
   git clone https://github.com/shikhaj4/faq-search-demo.git
   cd faq-search-demo

2. Install dependencies
pip install -r requirements.txt

3. Run Jupyter notebook
jupyter notebook faq_search.ipynb

Pipeline::
Load dataset → Extract questions and answers from JSON column qa.
Embeddings → Convert to vectors using a pretrained SBERT model.
ANN Index (FAISS IVF) → Index vectors for scalable search.
Retrieve → Find top-k matches for a query.
Rerank → Use cross-encoder to rescore and reorder retrieved matches.

Example Query
Query: "I got the wrong item, what should I do?"

Rank 1 (reranked):  
Q: What should I do to return a wrong item I received?  
A: Initiate the return process, provide order number, get a return label via email, and send the item back. Shipping is free.

