---
title: MICRO'16 Cambricon-X Review
date: 2017-07-18 16:05:26
tags:
---


Detailed Comments
=================

Background and Motivation
-------------------------

An intuitive optimization is to directly integrate sparsity encoding/decoding modules into existing DianNao or DaDianNao architecture, so as to reduce off-chip memory accesses as well. However, this solution may not be efficient for two main reasons: 

1. the number of total operations remain the same, because the pruned synapses are still filled with zero values, incurring significant waste of computational resource,

2. neither the centralized architecture (e.g., DianNao) nor the symmetric tiled architecture (e.g., DaDianNao) can adapt to irregularity of sparse NNs.

Accelerator Design
------------------

All the PEs are connected in a topology of Fat-tree in order to avoid wiring congestion.

As the number of synapses of different neurons may significantly differ from each other, we allow SBs in different PEs to load new data from the memory asynchronously to improve overall efficiency.

**IM.** Instead of distributing an indexing module to each PE, we design a centralized indexing module in the BC and only transfer the indexed neurons to PEs, which can significantly reduce the bandwidth requirement between the neural buffer and PEs because the number of data after indexing is much smaller in sparse networks.

Aside: Fortunately, systolic array doesn't have this problem. We could deploy an IM in each PE, as the inter-PE connection could greatly reduce the bandwidth between PE and neuron buffers.
