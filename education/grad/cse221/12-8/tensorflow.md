---
layout: page
title: CSE 221 - Operating Systems - Notes on "TensorFlow; Large-Scale Machine Learning on Heterogeneous Distributed Systems"
permalink: /education/grad/cse221/12-8/tensorflow/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, OS, google, machine, learning, tensorflow, heterogeneous, clusters
description: Notes for "TensorFlow; Large-Scale Machine Learning on Heterogeneous Distributed Systems"
mathjax: true
---


## Lecture Notes

- MapReduce not suitable for graph computation - overhead too large
- lots of other deep learning packages
    - pytorch, caffe, theano, tensorflow, mxnet, cuDNN, etc.
- compared to other frameworks, tensorflow took off in popularity when released
- Neural network --> $$$y = \phi(\Sum_{i=1}^N w_i x_i - b)$
    - sum inputs with bias, then pass through an activation function.
- tensors are represented by by edges in the computation graph
- computations are represented as graphs
    - nodes are operations
    - edges are tensors
- programs consist of two phases
    - construction (assembling the graph)
    - execution (pushing data)
- tensorflow - like theano with inter-device comm. as first class citizen
    - send and recv with specific implementations for particular devices
    - DMA for GPU-GPU impl
    - network for host-host
- objectives
    - ease of expression
    - scalability
    - portability
    - reproducibility
    - production readiness
- 