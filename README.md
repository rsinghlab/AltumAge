# AltumAge


The summary files are CSVs containing detailed information regarding the performance of AltumAge and Horvath's 2013 model by data set.

The instructions to use DeepAge are as follows:

(1) From your Illumina 27k or 450k data, select the 21368 CpG sites from the file "CpGsites.csv" in the correct order.

(2) Normalize the beta values using BMIQCalibration in R from "Horvath, S. DNA methylation age of human tissues and cell types. Genome Biol 14, 3156 (2013). https://doi.org/10.1186/gb-2013-14-10-r115".

(3) Scale the data using scikit-learn StandardScaler() object in "scaler.pkl".

(4) Load DeepAge from the "deep_age_keras" folder into tensorflow.

(5) Use the predict function of the model with the normalized beta values to obtain the predicted age.
