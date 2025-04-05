# Spike-Sorting-through-Gradient-Based-Preprocessing-and-Nonlinear-Reduction-with Agglomerative Clustering

Here’s a polished, detailed GitHub repository description for your spike sorting methods (GSA-Spike and GUA-Spike). I’ve structured it to highlight technical strengths, usability, and reproducibility:

## 🧠 SpikeSorting
#Unsupervised Spike Sorting via Gradient-Based Preprocessing, Nonlinear Dimensionality Reduction, and Agglomerative Clustering

A robust, training-free pipeline for spike sorting in electrophysiology, leveraging gradient-based feature enhancement, UMAP/Spectral Embedding for nonlinear dimensionality reduction, and agglomerative clustering. Designed for high-accuracy sorting of overlapping and noisy neural spikes, even in high-density recordings (e.g., Neuropixels). Outperforms traditional methods like PCA, LDA-GMM, and Wave_Clus by 12–18% in challenging scenarios.

# 🔑 Key Features
Gradient-Based Preprocessing: Extract noise-robust features using nth-order temporal derivatives to amplify spike dynamics (slopes, inflection points).

# Nonlinear Dimensionality Reduction:

UMAP: Preserve local/global structure for complex, overlapping spike clusters.

Spectral Embedding: Graph-based manifold learning for datasets with nonlinear correlations.

Hierarchical Clustering: Agglomerative clustering with silhouette-guided cluster count optimization.

GPU Acceleration Support: Optional GPU-accelerated UMAP via RAPIDS.ai for large datasets.

Benchmarked Performance: Validated on synthetic (MEArec) and real datasets, including Quiroga’s Dataset1 and hippocampal recordings.
