---
title: "HDGIM: Hyperdimensional DNA Search and FeFET-style Quantization"
excerpt: "Implementation of a hyperdimensional DNA search pipeline with encoding, CDF-based quantization, noise injection, and training using similarity-based feedback."
collection: portfolio
---

This project implements, in PyTorch, a hyperdimensional computing (HDC) pipeline for **DNA sequence search**.

### Main components

The core of the project is the `HDGIM` class, which encapsulates the full pipeline:

1. **Dataset generation (`dna_dataset.Dataset`)**  
   - Generates a synthetic DNA sequence and corresponding subsequences labeled as *true* (matching) or *false* (non-matching).  
   - Provides a PyTorch `DataLoader` interface for training and evaluation.

2. **HDC encoding of DNA (`generate_base_hypervectors`, `binding`, `binding_arbitrary_sequence`)**  
   - Assigns each DNA base to a random **base hypervector** in a high-dimensional space (`dimension`).  
   - For each subsequence, performs **binding** by circularly shifting the base hypervectors according to position and multiplicatively combining them.  
   - Stacks the resulting chunk hypervectors into an **HDC library** and sums them into a single **encoded hypervector** representing the reference DNA.

3. **FeFET-style quantization using CDF mapping (`quantize`, `quantize_arbitrary_sequence`)**  
   - Sorts the encoded hypervector values, normalizes them, and maps them through a standard normal **CDF**.  
   - Discretizes the CDF values into `2^bit_precision` levels, effectively simulating limited bit precision of FeFET-based hardware.  
   - Reorders the quantized values back to the original positions to obtain a **quantized hypervector**.

4. **Noise injection to model device variation (`adding_noise`, `adding_noise_arbitrary_sequence`)**  
   - With probability `noise` per dimension, randomly increments or decrements the quantized value, while respecting boundary conditions (0 and max).  
   - Produces a **noisy quantized hypervector**, modeling device-level variability in non-ideal hardware.

5. **Similarity computation and CAM-style search proxy (`hamming_distance`)**  
   - Uses an L1-based **Hamming-like distance** between quantized hypervectors as a proxy for CAM search behavior.  
   - Smaller distance corresponds to more similar sequences.
