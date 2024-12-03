# CatBench
Benchmark Framework for Graph Neural Networks in Adsorption Energy Predictions

## Installation

```bash
pip install catbench
```

## Overview
CatBench is a comprehensive benchmarking framework designed to evaluate Graph Neural Networks (GNNs) for adsorption energy predictions. It provides tools for data processing, model evaluation, and result analysis.

## Usage Workflow

### 1. Data Processing
CatBench supports two types of data sources:

#### A. Direct from Catalysis-Hub

```python
import catbench

# Convert JSON data from Catalysis-Hub to PKL format
catbench.json2pkl("AraComputational2022")
```

#### B. User Dataset
For custom datasets, prepare your data structure as follows:
```
data/
├── gas/
│   ├── H2gas/
│   │   ├── CONTCAR
│   │   └── OSZICAR
│   └── H2Ogas/
│       ├── CONTCAR
│       └── OSZICAR
└── surface1/
    ├── slab/
    │   ├── CONTCAR
    │   └── OSZICAR
    ├── H/
    │   ├── CONTCAR
    │   └── OSZICAR
    └── OH/
        ├── CONTCAR
        └── OSZICAR
```

Then process using:

```python
import catbench

coeff_setting = {
    "H": {
        "slab": -1,
        "adslab": 1,
        "H2gas": -1/2,
    },
    "OH": {
        "slab": -1,
        "adslab": 1,
        "H2gas": +1/2,
        "H2Ogas": -1,
    },
}

catbench.process_output("data", coeff_setting)
catbench.vasp2pkl("data")
```

### 2. Execute Benchmark

#### A. Multiple Calculations

```python
import catbench
from your_calculator import Calculator

# Prepare calculator list
calculators = [Calculator() for _ in range(5)]

config = {}
catbench.execute_benchmark(calculators, **config)
```

#### B. Single Calculation

```python
import catbench
from your_calculator import Calculator

calculator = Calculator()

config = {}
catbench.execute_benchmark_single(calculator, **config)
```

### 3. Analysis

```python
import catbench

config = {}
catbench.analysis_GNNs(**config)
```

## Results
[Images will be added here]

## Configuration Options

### execute_benchmark
| Option | Description | Default |
|--------|-------------|---------|
| GNN_name | Name of your GNN | Required |
| benchmark | Name of benchmark dataset | Required |
| F_CRIT_RELAX | Force convergence criterion | 0.05 |
| N_CRIT_RELAX | Maximum number of steps | 999 |
| rate | Fix ratio for surface atoms | 0.5 |
| disp_thrs_slab | Displacement threshold for slab | 1.0 |
| disp_thrs_ads | Displacement threshold for adsorbate | 1.5 |
| again_seed | Seed variation threshold | 0.2 |
| damping | Damping factor for optimization | 1.0 |
| gas_distance | Cell size for gas molecules | 10 |
| optimizer | Optimization algorithm | "LBFGS" |

### execute_benchmark_single
| Option | Description | Default |
|--------|-------------|---------|
| GNN_name | Name of your GNN | Required |
| benchmark | Name of benchmark dataset | Required |
| gas_distance | Cell size for gas molecules | 10 |

### analysis_GNNs
| Option | Description | Default |
|--------|-------------|---------|
| Benchmarking_name | Name for output files | Current directory name |
| calculating_path | Path to result directory | "./result" |
| GNN_list | List of GNNs to analyze | All GNNs in result directory |
| target_adsorbates | Target adsorbates to analyze | All adsorbates |
| specific_color | Color for plots | "black" |
| min | Plot y-axis minimum | Auto-calculated |
| max | Plot y-axis maximum | Auto-calculated |
| figsize | Figure size | (9, 8) |
| mark_size | Marker size | 100 |
| linewidths | Line width | 1.5 |
| dpi | Plot resolution | 300 |
| legend_off | Toggle legend | False |
| error_bar_display | Toggle error bars | False |

## License
[License information]

## Citation
[Citation information]