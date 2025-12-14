## Key Concepts:

- Brute Force Search vs. HNSW: Brute force search, which compares every vector to a query vector, is inefficient for large collections. HNSW provides a practical alternative by building a multi-layered graph for faster retrieval.
- How HNSW Works: HNSW creates a hierarchical graph where the top layers have broader connections and lower layers have more specific ones. When a query is made, the algorithm starts at the top and navigates down, progressively refining the search until it finds the nearest neighbors in the lowest layer. This significantly narrows down the search space.

### HNSW Parameters for Tuning:

- M (connections per node): Controls the density of connections in the graph. Higher values increase accuracy but also memory usage and indexing time.
- EF Construct (candidates checked during build): Determines how thoroughly the graph is built during vector insertion. Higher values lead to a more accurate graph but slower indexing.
- EF (candidates checked during search): Controls the number of neighbors evaluated during a search. Higher values increase accuracy but also query time.
- When HNSW is Used: Qdrant optimizes memory by only storing indexed vectors in the HNSW graph. For very small collections, HNSW indexing may not be triggered, and a brute force search might be used instead until indexing is needed.

Qdrant's "filterable HNSW" feature addresses the challenges of combining vector search with metadata filters in real-world applications. When applying filters, traditional HNSW graphs can break down because they assume all points are reachable, leading to missed relevant results. A naive approach of post-filtering (filtering after retrieving top K results) is inefficient, wasting compute and reducing recall.

Qdrant's filterable HNSW overcomes this by creating additional edges and subgraphs based on payload data during the index build. This maintains connectivity even when filters are applied. At query time, Qdrant uses a query planner to determine the most efficient search route:

For broad filters (matching many points), it performs a regular HNSW search, skipping non-matching nodes.
For narrow filters (matching fewer points), it pre-filters using payload indexes and searches within the resulting subgraph using HNSW.
For extra narrow filters (matching very few points), it skips HNSW entirely and uses a full scan (brute-force search).
Payload indexing is crucial for efficient filtering. Qdrant does not automatically index payload fields; users must explicitly define them. While payload indexes occupy memory, they significantly improve search speeds and resource utilization for filtered queries. It's recommended to index fields that restrict search results the most and have high cardinality (many different values) for optimal efficiency. Filterable HNSW is an enhancement to the base HNSW index for improved performance under filtering.

The key difference is when the filtering is fully applied:

Broad: Filter applied dynamically during HNSW traversal.
Narrow: Filter applied before HNSW traversal to create a smaller, relevant subgraph for the search.
