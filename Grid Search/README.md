Usage
Basic Workflow
Prepare your spike data as a NumPy array (shape: [n_spikes, n_features])

Run parameter optimization:
{{{
python
Copy
from your_module import get_optimal_parameters, run_grid_search

# Get optimal parameters
param_grid, metric = get_optimal_parameters(your_data)
best_params, best_score = run_grid_search(your_data, param_grid, metric)
Apply optimal parameters to your full dataset
}}}
Example Code
python
Copy
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
Parameters Guide
