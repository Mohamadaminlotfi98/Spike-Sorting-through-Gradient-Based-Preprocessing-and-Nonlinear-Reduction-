## 🧬 MEA Data Synthesizer
# Generate Synthetic Multi-Electrode Array (MEA) Recordings with Ground Truth Spiking Activity

![image](https://github.com/user-attachments/assets/2cfe13a5-83bc-4705-a553-c03c075659f0)

A Python pipeline for synthesizing realistic extracellular recordings using MEArec and SpikeInterface. Generate ground-truth spiking activity with precise control over probe geometry, neuronal templates, noise levels, and firing dynamics. Ideal for benchmarking spike sorting algorithms like GSA-Spike and GUA-Spike.

# 🔑 Key Features
Probe Simulation: Model Neuropixel, tetrode, or custom electrode layouts.

Template Generation: Create realistic spike waveforms from recorded or synthetic templates.

Noise Injection: Add Gaussian noise, transient artifacts, or drifting baselines.

Ground Truth Metadata: Includes precise spike times, neuron IDs, and template positions.

Seamless Integration: Compatible with SpikeInterface for preprocessing, sorting, and validation.

📥 Installation
```
{
bash
Copy
pip install mearec spikeinterface

```
🚀 Quickstart
Generate a Neuropixel-like Dataset
python
Copy
import MEArec as mr  
import spikeinterface as si  

# 1. Create a 24-channel Neuropixel probe  
probe = mr.generate_recording_probe(neuropixel=True, num_columns=2)  

# 2. Simulate 10-minute recording with 20 neurons  
recording_gen = mr.simulate_recordings(  
    duration=600,  
    probe=probe,  
    num_neurons=20,  
    firing_rate=5.0,  # Hz  
    noise_level=15.0,  # μV  
    seed=42  
)  

# 3. Save synthetic data  
recording_gen.save("dataset/neuropixel_synthetic")  

# 4. Visualize templates  
mr.plot_templates(recording_gen.templates)  
}
# 📂 Dataset Structure
Copy
neuropixel_synthetic/  
├── recordings.h5         # Raw traces (μV)  
├── spike_trains.h5       # Ground truth spike times & neuron IDs  
├── templates.h5          # Waveform templates per neuron  
├── probe_positions.csv   # 3D electrode coordinates  
└── params.yaml           # Simulation parameters  
📊 Advanced Customization
Add Bursting Activity and Drift
python
Copy
{
from MEArec import simulate_recordings  

recording_gen = simulate_recordings(  
    ...,  
    bursting=True,  
    burst_duration=2.0,  # seconds  
    drift_speed=5.0,     # μm/min  
    max_drift=100.0      # μm  
)  
Inject High-Frequency Noise
python
Copy
recording_gen = simulate_recordings(  
    ...,  
    noise_color="pink",  # "white", "brown", or custom PSD  
    noise_floor=10.0,    # μV  
    seed=42  
)  
}
## 📜 Citation
Please cite the MEArec and SpikeInterface papers:

bibtex
Copy
@article{Buccino2021MEArec,  
  title={MEArec: A Fast and Customizable Testbench Simulator for Ground-Truth Extracellular Spiking Activity},  
  author={Buccino, Alessio Paolo and Einevoll, Gaute T},  
  journal={Neuroinformatics},  
  volume={19},  
  pages={185--204},  
  year={2021},  
  doi={10.1007/s12021-020-09467-7}  
}  

@article{Buccino2020SpikeInterface,  
  title={SpikeInterface: A Unified Framework for Spike Sorting},  
  author={Buccino, Alessio Paolo and Hurwitz, Cole Lincoln and Garcia, Samuel and Magland, Jeremy and Siegle, Joshua H and Hurwitz, Christoph and Hennig, Matthias H},  
  journal={eLife},  
  volume={9},  
  pages={e61834},  
  year={2020},  
  doi={10.7554/eLife.61834}  
}  
# 🌐 Resources
MEArec Documentation: https://mearec.readthedocs.io

SpikeInterface Documentation: https://spikeinterface.readthedocs.io

Example Notebooks: examples/


# 🙏 Acknowledgments
The MEArec and SpikeInterface teams for their groundbreaking tools.

This README provides clear instructions for generating synthetic MEA data while properly crediting MEArec and SpikeInterface
