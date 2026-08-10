## Netflix Real-Time Distributed Graph (RDG) — Kafka + Flink

**Source:** [Scale Engineer](https://scaleengineer.com/blog/how-netflix-built-a-real-time-distributed-graph-to-connect-billions-of-member-interactions) | [Netflix Tech Blog](https://netflixtechblog.com/how-and-why-netflix-built-a-real-time-distributed-graph-part-1-ingesting-and-processing-data-80113e124acc)

### The Problem Netflix Solved

A member watches Stranger Things on phone → continues on TV → plays a Netflix game on tablet. Three separate events, three separate systems, one member. Traditional data warehouses processed these in batch — too slow for real-time recommendations. Netflix needed to connect these interactions *as they happened*.

### Why a Graph Model

- Entities: members, titles, devices, games
- Relationships: interactions between them (watched, played, continued)
- Advantage over relational DB: no expensive joins to traverse relationships
- Flexible: new node/edge types added without redesigning the schema

### Architecture — Three Layers

This article covers **layer 1 only** — ingestion and processing.

### The Pipeline: Kafka → Flink → Data Mesh

**Kafka** — ingestion backbone
- Every member action hits the API Gateway → written to Kafka topics
- Each topic handles ~1 million messages/second
- Events encoded in Apache Avro, schemas in a central registry
- Kafka doesn't store forever → overflow backed up to Apache Iceberg tables

**Apache Flink** — stream processing
- Consumes from Kafka, transforms events into graph nodes and edges
- Each Flink job: filter → enrich → transform → buffer/deduplicate → publish
- One member interaction can produce dozens of nodes and edges

**Data Mesh** — publishes processed graph data to downstream storage

### Key Engineering Decision — 1:1 Kafka Topic to Flink Job

**First attempt:** one large Flink job consuming all Kafka topics
**Problem:** different topics have wildly different traffic patterns — impossible to tune CPU, memory, parallelism for all workloads in one job

**Solution:** one dedicated Flink job per Kafka topic
- More jobs to manage but each is independently tunable
- Same principle applied to graph data — each node type and edge type gets its own Kafka topic

### Core Principle

> Scalability comes from separation of concerns — one job per workload, one topic per entity type. More moving parts, but each part is understandable and tunable independently.

### Interview Line

> "Netflix's RDG is a good example of the tradeoff between operational simplicity and scalability. One job is simpler to manage but impossible to tune at scale. Splitting into 1:1 job-to-topic mappings increases complexity but makes each component independently observable and scalable — the same principle behind microservices."

### Why This is Relevant to AI Engineering

- Stream processing with Kafka + Flink is the data backbone that feeds real-time features into recommendation models
- The graph model is directly applicable to knowledge graphs in RAG systems
- Deduplication within a time window is the same pattern as semantic caching in FinSight


