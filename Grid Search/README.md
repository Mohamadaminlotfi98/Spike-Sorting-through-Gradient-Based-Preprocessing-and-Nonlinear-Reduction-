## Automated Spike Sorting Optimization Tool

A configurable pipeline for optimizing spike sorting parameters through gradient transformations, dimensionality reduction, and hierarchical clustering with automated grid search.

## Features

- **Gradient Transformation**: Apply 1st to Nth-order gradients to enhance spike features
- **Dimensionality Reduction**:
  - UMAP (non-linear embedding)
  - Spectral Embedding (linear/non-linear hybrid)
- **Clustering**: Agglomerative Hierarchical Clustering with multiple linkage methods
- **Parameter Optimization**: Grid search with three evaluation metrics:
  - Silhouette Score
  - Davies-Bouldin Index
  - Calinski-Harabasz Index

## Installation

1. **Requirements**:
   - Python 3.7+
   - numpy
   - scikit-learn
   - umap-learn

2. **Install dependencies**:
   ```bash
   pip install numpy scikit-learn umap-learn
   ```

###   Usage
Basic Workflow
Prepare your spike data as a NumPy array (shape: [n_spikes, n_features])

Run parameter optimization:

```
from your_module import get_optimal_parameters, run_grid_search

# Get optimal parameters
param_grid, metric = get_optimal_parameters(your_data)
best_params, best_score = run_grid_search(your_data, param_grid, metric)

```

3. **Apply optimal parameters to your full dataset**

```
import numpy as np
from your_module import apply_gradient, get_optimal_parameters, run_grid_search

# Load your spike data
spike_data = np.load('spikes.npy')  # Shape: [n_spikes, n_features]

# Find optimal parameters
param_grid, metric = get_optimal_parameters(spike_data)
best_params, best_score = run_grid_search(spike_data, param_grid, metric)

# Apply best parameters
processed_data = apply_gradient(spike_data, best_params['gradient_order'])

# Apply optimal embedding
if best_params['embedding_params']['method'] == 'UMAP':
    embedder = UMAP(**best_params['embedding_params'])
else:
    embedder = SpectralEmbedding(**best_params['embedding_params'])
    
embedded_data = embedder.fit_transform(processed_data)

# Apply clustering
clusterer = AgglomerativeClustering(**best_params['clustering_params'])
labels = clusterer.fit_predict(embedded_data)

```

## Parameters Guide

When prompted during optimization:

1. **Number of Spikes**: Total spikes in your dataset
2. **Gradient Range**:
   - Suggested: 0-3 (auto-calculated based on spike count)
   - Higher orders emphasize spike shape changes
3. **Cluster Range**:
   - Suggested: 2-√n_spikes (auto-calculated)
   - Start with 2-10 for initial explorations
4. **Embedding Method**:
   - UMAP: Better for non-linear structures (default)
   - Spectral: Better for linear relationships
5. **Evaluation Metric**:
   - Silhouette: Best for compact, separated clusters
   - Davies-Bouldin: Good for density comparisons
   - Calinski-Harabasz: Best for variance ratios

## Output Interpretation

The optimization returns:
- `best_score`: Optimal metric value found
- `best_params`: Dictionary containing:
  - `gradient_order`: Optimal number of gradient applications
  - `embedding_params`: Optimal embedding configuration
  - `clustering_params`: Optimal cluster count + linkage method

## Best Practices

1. **Start with Defaults**: Use suggested parameter ranges first
2. **Progressively Refine**: 
   - First run: Wide ranges, coarse steps
   - Subsequent runs: Narrow ranges around best candidates
3. **Metric Selection**:
   - Use Silhouette for balanced cluster evaluation
   - Use Davies-Bouldin when cluster separation is critical
   - Use Calinski-Harabasz for large datasets (>10k spikes)

## Limitations

1. **Computational Complexity**:
   - Parameter combinations grow exponentially - start with ≤5 values per parameter
2. **Memory Requirements**:
   - UMAP requires ≥16GB RAM for >50k spikes
3. **Cluster Shape Bias**:
   - Hierarchical clustering favors spherical clusters

## References

1. UMAP: McInnes et al. (2018)
2. Spectral Embedding: Ng et al. (2002)
3. Clustering Metrics: Scikit-learn documentation



