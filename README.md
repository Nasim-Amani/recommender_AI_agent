

---

### Phase 1: NLP foundations
**Goal:** treat text the way you treat time series, as an input you can preprocess and model.
- Text preprocessing, tokenization, TF-IDF.
- Embeddings: words → vectors, the text equivalent of feature extraction.
- Transformers for text: BERT and the Hugging Face `transformers` library. You know the architecture already; only the input type changes.
- **spaCy:** part-of-speech tags and dependency parsing. These are your "syntactic patterns".

**Deliverable:** load one Amazon Reviews 2023 category (e.g., All_Beauty), explore the text and tags, and extract adjective-noun patterns with spaCy.

**Learn from:** the Hugging Face NLP Course (chapters 1–4) and the free spaCy course.

---

### Phase 2: Recommenders and multi-label basics
**Goal:** understand the problem framing and how it's evaluated.
- Recommender types: content-based, collaborative filtering, hybrid. You only need the concepts, since your task is content-based.
- Multi-label classification: sigmoid + BCE loss, not softmax.
- Metrics: Precision@k, Recall@k, F1 (micro/macro), MAP, NDCG.
- Splits that avoid leakage: by vendor or category, like a temporal split in forecasting.

**Deliverable:** two baselines.
1. TF-IDF + One-vs-Rest logistic regression.
2. Fine-tuned BERT/DistilBERT multi-label tagger.

**Learn from:** Google's "Recommendation Systems" crash course and the scikit-learn multilabel docs.

---

### Phase 3: Embeddings and retrieval 
**Goal:** "similar products have similar tags".
- sentence-transformers for product embeddings.
- FAISS for fast nearest-neighbour search.
- kNN tag propagation: borrow tags from the top-k similar products.

**Deliverable:** a retrieval-based tagger, compared against the Phase 2 baselines.

---

### Phase 4: Tag graphs 
**Goal:** model "neighbouring tag attributes".
- Build a tag co-occurrence graph with networkx, weighting edges by PMI.
- Graph embeddings (node2vec), then optionally a GNN (GCN in PyTorch Geometric).
- Use the graph to re-rank or complete predicted tags.

**Deliverable:** graph-enhanced tagger + an ablation showing what the graph adds.

**Learn from:** Stanford CS224W lectures 1–6 (free on YouTube).

---

### Phase 5: LLMs
**Goal:** use an LLM as a reasoning tagger.
- Prompting: zero-shot, few-shot, and structured JSON output.
- RAG: put retrieved similar products into the prompt. Phase 3 becomes LLM context.
- Fine-tuning a small open model (Qwen or Llama, 1–8B) with LoRA via PEFT.
- Hallucination and abstention: teach the model to answer "NONE".

**Deliverable:** an LLM tagger on AE-110K or MAVE, compared with your baselines and the published results from your summary paper.

**Learn from:** the Hugging Face LLM Course and the PEFT/LoRA docs.

---

### Phase 6: Building the agent
**Goal:** turn your components into tools and let the LLM orchestrate them.
- Tool calling and the ReAct loop (think → act → observe → repeat).
- Your tools: `parse_syntax()` from Phase 1, `retrieve_similar()` from Phase 3, `tag_graph()` from Phase 4.
- A verifier step: check each tag's evidence and abstain if it's weak.
- Memory: store accepted and rejected tags so the agent can learn from feedback.
- Write the loop in plain Python first. Try LangGraph or smolagents only if you need them later.

**Deliverable:** a working recommender agent that runs end-to-end on one category.

**Learn from:** the Hugging Face Agents Course and Anthropic's "Building effective agents" article.

---

### Phase 7: Research experiments and writing 
**Goal:** turn the system into a paper.
- **Ablations:** agent with and without the parser, graph, retrieval and verifier.
- **Cross-domain tests:** train on one category, test on another. This is the open problem from Sabeh et al.
- **Error analysis:** hallucinations, missed tags, rare tags.
- **Cost and latency:** LLM calls per product. Reviewers will ask about this.
- Write it up using your proposal as the skeleton.
