# AltumAge

AltumAge is a pan-tissue DNA methylation epigenetic clock based on deep learning. For the pre-print of the paper, please click [here](https://www.biorxiv.org/content/10.1101/2021.06.01.446559v2).

## Usage

In order to use AltumAge for age prediction, please follow the steps in example.ipynb. The example file also contains simple instructions to use Horvath's 2013 model for ease of comparison.

The main instructions to use AltumAge are as follows:

#### (1) Load required python packages:

The following packages must be installed. As of note, the model was trained with ```tensorflow``` 2.4.0, so beware of possible compatibility issues with other versions.

```python
import tensorflow as tf
import numpy as np
import pandas as pd
from sklearn import linear_model, preprocessing
```

#### (2) Load list of CpGs, methylation data, scaler, and AltumAge model:

From your Illumina 27k or 450k array data, select the 21368 CpG sites from the file "CpGsites.csv" in the correct order.

```python
cpgs = pd.read_csv('example_dependencies/CpGsites.csv').iloc[:,1]
```

Load the BMIQCalibration-normalized methylation data. It is crucial that the methylation beta values are normalized according to BMIQCalibration in R from "Horvath, S. DNA methylation age of human tissues and cell types." Genome Biol 14, 3156 (2013). [https://doi.org/10.1186/gb-2013-14-10-r115](https://doi.org/10.1186/gb-2013-14-10-r115). Moreover, reading the pickled example data only works in python version >= 3.8.

```python
data = pd.read_pickle('example_dependencies/example_data.pkl')
real_age = data.age
methylation_data = data[cpgs]
```

Load the scaler, which transforms the distribution of beta values of each CpG site to mean = 0 and variance = 1.

```python
scaler = pd.read_pickle('example_dependencies/scaler.pkl')
```

Finally, load ```AltumAge```. There are two similar ways of reading the model. The first, which works with arm64 SoCs, is simply using the ```AltumAge``` folder contained in this GitHub repository.

```python
AltumAge = tf.keras.models.load_model('AltumAge.h5')
```

#### (3) Scale the methylation data:

Scale the beta values of each CpG with sklearn robust scaler.

```python
methylation_data_scaled = scaler.transform(methylation_data)
```

#### (4) Age prediction:

Finally, to predict age, simply use the following. The ```.flatten()``` command might be needed to transform the output into a 1D array.

```python
pred_age_AltumAge = AltumAge.predict(methylation_data_scaled).flatten()
```

Voilà!


## Supplementary Results

The summary files are CSVs containing detailed information regarding the performance of AltumAge and Horvath's 2013 model by data set in the test set.

## Data availability

To access the raw data and metadata from Array Express and Gene Expression Omnibus (GEO) or the organized, non-normalized methylation data, please access our Google Drive [here](https://drive.google.com/drive/folders/1RH2JYmhOmsScaj_WMQfVwYjubkNTh5Oq?usp=sharing_eip&ts=60c67fb4).

## Citation

To cite our study, please use the following:

Lucas Paulo de Lima Camillo, Louis R Lapierre, Ritambhara Singh. AltumAge: Improving epigenetic clock performance and interpretation with deep learning. bioRxiv 2021.06.01.446559; doi: [https://doi.org/10.1101/2021.06.01.446559](https://doi.org/10.1101/2021.06.01.446559)

