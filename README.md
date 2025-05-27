# AltumAge

[![Paper](https://img.shields.io/badge/Paper-npj%20Aging-blue.svg)](https://www.nature.com/articles/s41514-022-00085-y)
[![DOI](https://img.shields.io/badge/DOI-10.1038%2Fs41514--022--00085--y-green.svg)](https://doi.org/10.1038/s41514-022-00085-y)
[![Python](https://img.shields.io/badge/Python-3.8%2B-yellow.svg)](https://www.python.org/)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-2.5.0-orange.svg)](https://www.tensorflow.org/)
[![License](https://img.shields.io/badge/License-MIT-lightgrey.svg)](LICENSE)

## 🧬 **AltumAge**: A Pan-Tissue DNA Methylation Epigenetic Clock Based on Deep Learning

AltumAge is a state-of-the-art epigenetic clock that predicts biological age from DNA methylation data across multiple tissue types. Built using deep learning, AltumAge demonstrates superior performance compared to traditional epigenetic clocks.

### 🎯 Key Features

- **Pan-tissue compatibility**: Works across multiple tissue types
- **Deep learning architecture**: Leverages neural networks for improved accuracy
- **Multi-platform support**: Compatible with Illumina 27k, 450k, and EPIC arrays
- **PyTorch compatibility**: Available in both TensorFlow and PyTorch formats
- **Easy integration**: Now available through the [pyaging](https://github.com/rsinghlab/pyaging) package

### 📊 Performance Highlights

- Trained, validated, and tested on 142 datasets
- Outperforms Horvath's 2013 model across multiple metrics
- Robust performance across diverse tissue types and age ranges

## 🚀 Quick Start

### Option 1: Using pyaging (Recommended)

The easiest way to use AltumAge is through [pyaging](https://github.com/rsinghlab/pyaging):

```bash
pip install pyaging
```

Then follow the [DNA methylation age prediction tutorial](https://pyaging.readthedocs.io/).

### Option 2: Standalone Usage

#### Prerequisites

```bash
pip install tensorflow==2.5.0 numpy pandas scikit-learn
```

#### Basic Usage

```python
import tensorflow as tf
import numpy as np
import pandas as pd
from sklearn import linear_model, preprocessing

# Load CpG sites
cpgs = np.array(pd.read_pickle('example_dependencies/multi_platform_cpgs.pkl'))

# Load your methylation data
data = pd.read_pickle('example_dependencies/example_data.pkl')
methylation_data = data[cpgs]

# Load scaler and model
scaler = pd.read_pickle('example_dependencies/scaler.pkl')
AltumAge = tf.keras.models.load_model('example_dependencies/AltumAge.h5')

# Scale and predict
methylation_data_scaled = scaler.transform(methylation_data)
predicted_ages = AltumAge.predict(methylation_data_scaled).flatten()
```

## 📋 Detailed Instructions

### 1. Data Preparation

AltumAge requires:
- DNA methylation beta values from Illumina arrays (27k, 450k, or EPIC)
- Selection of 20,318 specific CpG sites (provided in `CpGsites.csv`)

### 2. Model Loading

```python
# For TensorFlow users
AltumAge = tf.keras.models.load_model('example_dependencies/AltumAge.h5')

# For PyTorch users
import torch
AltumAge_pytorch = torch.load('dependencies/AltumAge.pt')
```

### 3. Preprocessing Pipeline

1. Select the required CpG sites in the correct order
2. Scale using the provided RobustScaler
3. Fill up missing values with 0 after scaling
4. Input to the model for age prediction

## 📁 Repository Structure

```
AltumAge/
├── example.ipynb                    # Complete usage example
├── example_dependencies/            # Required files for running AltumAge
│   ├── AltumAge.h5                 # TensorFlow model
│   ├── multi_platform_cpgs.pkl     # List of CpG sites
│   ├── scaler.pkl                  # Preprocessing scaler
│   └── example_data.pkl            # Example dataset
├── dependencies/
│   └── AltumAge.pt                 # PyTorch model
├── CpGsites.csv                    # Required CpG sites
└── supplementary_results/          # Detailed performance metrics
```

## 💾 Data Availability

Access our comprehensive dataset collection:
- Raw data from ArrayExpress and GEO
- Organized methylation data (non-normalized)
- [Google Drive Repository](https://drive.google.com/drive/folders/1RH2JYmhOmsScaj_WMQfVwYjubkNTh5Oq?usp=sharing_eip&ts=60c67fb4)

## 📚 Citation

If you use AltumAge in your research, please cite:

```bibtex
@article{de_Lima_Camillo_AltumAge,
    author = {de Lima Camillo, Lucas Paulo and Lapierre, Louis R and Singh, Ritambhara},
    title = {A pan-tissue DNA-methylation epigenetic clock based on deep learning},
    journal = {npj Aging},
    volume = {8},
    pages = {4},
    year = {2022},
    doi = {10.1038/s41514-022-00085-y},
    publisher = {Springer Nature},
    URL = {https://doi.org/10.1038/s41514-022-00085-y}
}
```
## 📧 Contact

For questions or collaborations, please contact:
- Lucas Paulo de Lima Camillo: [lucas_camillo@alumni.brown.edu](mailto:lucas_camillo@alumni.brown.edu)

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
