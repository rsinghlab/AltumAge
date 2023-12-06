# AltumAge

AltumAge is a pan-tissue DNA methylation epigenetic clock based on deep learning. For the link to our paper published in npj Aging, please click [here](https://www.nature.com/articles/s41514-022-00085-y).

## [New] AltumAge is available on pyaging

The easiest way to use AltumAge with your methylation data is through [pyaging](https://github.com/rsinghlab/pyaging), our newly release aging clock package. It is avaiable on PyPi and can easily be installed via ```pip install pyaging```. The tutorial for DNA methylation age prediction is available [here](https://pyaging.readthedocs.io/en/latest/tutorial_dnam.html).

## Usage

In order to use AltumAge for age prediction, please follow the steps in example.ipynb. The example file also contains simple instructions to use Horvath's 2013 model for ease of comparison.

The main instructions to use AltumAge are as follows:

#### (1) Load required python packages:

The following packages must be installed. As of note, the model was trained with ```tensorflow``` 2.5.0, so beware of possible compatibility issues with other versions.

```python
import tensorflow as tf
import numpy as np
import pandas as pd
from sklearn import linear_model, preprocessing
```

#### (2) Load list of CpGs, methylation data, scaler, and AltumAge model:

From your Illumina 27k, 450k or EPIC array data, select the 20318 CpG sites from the file "CpGsites.csv" in the correct order.

```python
cpgs = np.array(pd.read_pickle('example_dependencies/multi_platform_cpgs.pkl'))
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

Finally, load ```AltumAge```:

```python
AltumAge = tf.keras.models.load_model('example_dependencies/AltumAge.h5')
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


## PyTorch compatibility

AltumAge's h5 tensorflow model has also been converted to the latest PyTorch 2.1 version. To use, just ```torch.load``` the AltumAge.pt file under the dependencies folder. Follow all of the preprocessing steps and just use the loaded model as usual. 


## Supplementary Results

The summary files are CSVs containing detailed information regarding the performance of AltumAge and Horvath's 2013 model by data set in the test set.

## Data availability

To access the raw data and metadata from Array Express and Gene Expression Omnibus (GEO) or the organized, non-normalized methylation data, please access our Google Drive [here](https://drive.google.com/drive/folders/1RH2JYmhOmsScaj_WMQfVwYjubkNTh5Oq?usp=sharing_eip&ts=60c67fb4).

## Citation

To cite our study, please use the following:

de Lima Camillo, L.P., Lapierre, L.R. & Singh, R. A pan-tissue DNA-methylation epigenetic clock based on deep learning. npj Aging 8, 4 (2022). [https://doi.org/10.1038/s41514-022-00085-y](https://doi.org/10.1038/s41514-022-00085-y)

BibTex citation:
```
@article {de_Lima_Camillo_AltumAge,
	author = {de Lima Camillo, Lucas Paulo and Lapierre, Louis R and Singh, Ritambhara},
	title = {A pan-tissue DNA-methylation epigenetic clock based on deep learning},
	year = {2022},
	doi = {10.1038/s41514-022-00085-y},
	publisher = {Springer Nature},
	URL = {https://doi.org/10.1038/s41514-022-00085-y},
	eprint = {https://www.nature.com/articles/s41514-022-00085-y.pdf},
	journal = {npj Aging}
}
```

## License

Copyright 2021, Brown University, Providence, RI.

All Rights Reserved

Permission to use, copy, modify, and distribute this software and
its documentation for any purpose other than its incorporation into a
commercial product or service is hereby granted without fee, provided
that the above copyright notice appear in all copies and that both
that copyright notice and this permission notice appear in supporting
documentation, and that the name of Brown University not be used in
advertising or publicity pertaining to distribution of the software
without specific, written prior permission.

BROWN UNIVERSITY DISCLAIMS ALL WARRANTIES WITH REGARD TO THIS SOFTWARE,
INCLUDING ALL IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR ANY
PARTICULAR PURPOSE.  IN NO EVENT SHALL BROWN UNIVERSITY BE LIABLE FOR
ANY SPECIAL, INDIRECT OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES
WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN
ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF
OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.

