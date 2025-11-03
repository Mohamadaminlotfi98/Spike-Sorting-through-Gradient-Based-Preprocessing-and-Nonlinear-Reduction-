# 🧠⚡ Automated Spike Sorting Optimization
```bash
![Pipeline Overview](https://via.placeholder.com/800x300.png?text=Optimization+Pipeline+Flowchart)
```
*Example workflow diagram*

A **color-coded optimization pipeline** for spike sorting with interactive parameter selection and cluster quality visualization.

## 🚀 Features

| Feature              | Description                                                                 | Visualization Example        |
|----------------------|-----------------------------------------------------------------------------|-------------------------------|
| Gradient Processing  | `apply_gradient(data, n=2)`<br>Enhances spike features through differentiation | ![Gradient](https://via.placeholder.com/150x100.png?text=Raw+vs+1st+Gradient) |
| Dimensionality Reduction | UMAP/Spectral Embedding<br>`n_components=2-3`                                | ![Embeddings](https://via.placeholder.com/150x100.png?text=2D+Embedding) |
| Clustering           | Agglomerative (Ward/Average/Complete)<br>`n_clusters=2-10`                  | ![Clusters](https://via.placeholder.com/150x100.png?text=Cluster+Separation) |

## 🛠️ Installation

```bash
# Color-coded installation guide
pip install \
  numpy \           # ➡️ Array processing (v1.23+)
  scikit-learn \    # ➡️ Clustering & metrics (v1.2+)
  umap-learn        # ➡️ Non-linear embedding (v0.5+)
```

## 📈 Workflow Overview

```mermaid
graph TD
    A[Raw Spike Data] --> B{Apply Gradient?}
    B -->|Yes| C[Gradient Transform]
    B -->|No| D[Embedding]
    C --> D
    D --> E[Clustering]
    E --> F[Metric Calculation]
    F --> G{Optimal?}
    G -->|Yes| H[Return Parameters]
    G -->|No| I[Next Combination]
```

## 🎛️ Parameter Configuration Guide

### 1️⃣ Gradient Settings
```python
# Recommended for 3000+ spikes 🚀
gradient_range = range(0, 3)  # 0=Raw, 1=1st deriv, 2=2nd deriv
```

### 2️⃣ UMAP Parameters
```python
{
  'n_neighbors': 25,       
  'min_dist': 0.1,         
  'n_components': 2        
}
```

### 3️⃣ Clustering Options
```python
AgglomerativeClustering(
  n_clusters=5,            # 🔢 Start with sqrt(n_spikes)
  linkage='ward'           # ⛓️ Ward=var, Complete=max
)
```

##  Evaluation Metrics Comparison

| Metric            | Best For                  | Ideal Range | Color Code      |
|-------------------|---------------------------|-------------|-----------------|
| Silhouette Score  | Balanced cluster density  | 0.6-1.0     | 🟢 High is good |
| Davies-Bouldin    | Separated cluster centers | 0-0.5       | 🔵 Low is good  |
| Calinski-Harabasz | Large datasets            | 300+        | 🟡 Higher=better|

## 💻 Example Run (Color Output)

```python
from SortingPipeline import get_optimal_parameters, run_grid_search

# Get optimal parameters
param_grid, metric = get_optimal_parameters(your_data)
best_params, best_score = run_grid_search(your_data, param_grid, metric)
```
# Apply optimal parameters to your full dataset

```python
import numpy as np
from SortingPipeline import apply_gradient, get_optimal_parameters, run_grid_search

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

##  Best Practices

1. **Parameter Ranges**:
   - 🔄 Start broad then narrow down
   - 📈 Use logarithmic scales for large ranges

2. **Visual Checks**:
   ```python
   import matplotlib.pyplot as plt
   plt.scatter(embedded[:,0], embedded[:,1], c=labels, cmap='tab20')
   plt.title(f'Cluster Separation (Score: {best_score:.2f})')
   plt.show()
   ```

## 🚨 Common Errors

| Error Type           | Solution                  | Color Code |
|----------------------|---------------------------|------------|
| Memory Error         | Reduce gradient order     | 🟠 Warning |
| Cluster Collapse     | Increase min_dist         | 🔴 Critical|
| Metric Contradiction | Try different evaluation  | 🟡 Caution |

##  References

- [UMAP Documentation](https://umap-learn.readthedocs.io) 📘
- [Scikit-learn Clustering](https://scikit-learn.org/stable/modules/clustering.html) 📗
