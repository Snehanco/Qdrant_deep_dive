Hybrid search, a method that combines different search approaches in a single pipeline to improve retrieval quality. It highlights the strengths of both dense vectors and sparse vectors and how they can be used together.

Dense Vectors are produced by neural encoders and are excellent at associating meaning with inputs, making them suitable for natural language queries where the exact words might differ but the intent is the same (e.g., "elevator" vs. "lift"). They can also support multiple languages if trained accordingly.

Sparse Vectors, often described as keyword-based or lexical search, are best for cases where an exact match matters, such as searching for a specific product identifier or model number.

The video emphasizes that users have diverse search behaviors: some are domain experts using precise terminology, while others use vague natural language. Hybrid search addresses this by not forcing a choice between the two methods.

There's no single definition of hybrid search, but it generally involves combining at least two different search methods. This can include:

Chaining: Using dense vector search to prefetch candidates and then re-ranking them with sparse vectors, or vice versa. The initial retrieval method is crucial as the re-ranker can only work with what's already selected.
Fusion: This approach integrates signals from both dense and sparse retrieval methods, especially when their scores are not directly comparable (e.g., cosine similarity vs. BM25 scores).
Reciprocal Rank Fusion (RRF) is a common fusion technique. Instead of comparing the raw scores from different methods, RRF uses only the ranks of the documents returned by each individual method to calculate a new, unified score. This new score then defines the final ranking. The k parameter in the RRF formula can adjust the impact of lower-ranked documents. Qdrant has RRF built-in, simplifying its implementation.

The video also touches upon Qdrant's Universal Query API, introduced in version 1.10, which consolidates other APIs and simplifies the creation of complex hybrid search pipelines. This API allows for nesting multiple prefetch operations and applying fusion at any level.

Beyond chaining and fusion, hybrid search can involve more complex multi-stage pipelines with faster initial retrieval methods and slower, more precise models for re-ranking. Re-ranking is a general term for methods applied to results of intermediate search operations, and fusion is just one way to tackle it. Other options include using stronger neural models like cross-encoders, multi-vector representations like Colbert, or custom business rules.

The video concludes by stating that the next lesson will focus on building a hybrid search pipeline using Qdrant's Universal Query API.
