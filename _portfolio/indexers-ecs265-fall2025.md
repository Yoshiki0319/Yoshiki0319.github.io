---
title: "Ongoing - Indexers for ECS265 on Apache ResilientDB"
excerpt: "Prototype of native vector value indexing in ResilientDB using HNSW-based similarity search and a Python bridge for embedding and index management.
collection: portfolio
---

This project extends **ResilientDB**, a permissioned blockchain / distributed database system, with **native vector value indexing** to enable similarity search on non-primary attributes. The goal is to move beyond simple key–value lookups and support efficient querying based on semantic similarity, such as searching by text descriptions instead of exact keys.  

Our design introduces a new value indexing layer that:

- Uses **sentence-transformer–based embedding models** to convert input data into semantic vector representations.  
- Stores these embeddings in **RocksDB** alongside ResilientDB’s existing key–value store.  
- Maintains a high-performance **HNSW (Hierarchical Navigable Small World) index** for approximate nearest neighbor search.  
- Keeps the HNSW index persisted at each replica by serializing it back into ResilientDB’s storage.

Technically, the system is a **multi-layer architecture** that leverages ResilientDB’s existing `pybind11` integration:

1. A C++ transaction in ResilientDB triggers Python code through the pybind11 bridge.  
2. In Python, the data is **vectorized** into embeddings (e.g., using sentence-transformers).  
3. The embeddings are stored in RocksDB and inserted into an **HNSW index** (via `faiss-hnsw` or similar libraries).  
4. The updated index is serialized and written back to ResilientDB so that each replica maintains its own local copy.  
5. A **user-facing CLI tool** (with a Web UI as a stretch goal) allows users to submit text queries and retrieve the *N* most similar stored items.

By combining ResilientDB’s blockchain-backed resilience with vector indexing, this prototype aims to support applications like recommendation systems, resume–job requirement matching, and semantic document search, all without relying on an external secondary database.
