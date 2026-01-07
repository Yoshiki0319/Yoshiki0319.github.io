---
title: "Indexers for ECS265 on Apache ResilientDB"
excerpt: "Prototype of native vector value indexing in ResilientDB using HNSW-based similarity search and a Python bridge for embedding and index management."
collection: portfolio
---

This project extends **ResilientDB**, a permissioned blockchain / distributed database system, with **native vector value indexing** to enable similarity search on non-primary attributes. The goal is to move beyond simple key–value lookups and support efficient querying based on semantic similarity, such as searching by text descriptions instead of exact keys.  

Our design introduces a new value indexing layer that:

- Uses **sentence-transformer–based embedding models** to convert input data into semantic vector representations.  
- Stores these embeddings in **RocksDB** alongside ResilientDB’s existing key–value store.  
- Maintains a high-performance **HNSW (Hierarchical Navigable Small World) index** for approximate nearest neighbor search.  
- Keeps the HNSW index persisted at each replica by serializing it back into ResilientDB’s storage.
