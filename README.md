# KIRP-topological-features
In this project, we propose a novel bioimage informatics pipeline for automatically characterizing the topological organ-ization of different cell patterns in the tumor microenvironment, using stacked sparse autoencoder and Delaunay triangulation graph. We apply this pipeline to the only publicly available large histopathology image dataset for a cohort of 190 patients with papillary renal cell carci-noma obtained from The Cancer Genome Atlas project. Experimental results show that the proposed topological features can successfully stratify early- and middle-stage patients with distinctive survival, and show superior performance than traditional clinical features and cellular morphological features.

The code in this progject will reproduce the results in our paper submitted to Bioinformatics, "Identification of Topological Features in Renal Tumor Microenvironment Associated with Patient Survival". Code is written in matlab and R.

**Outline**
* Method Overview
* Downloading histological images
* How to run code
* Contact information

Method overview
-----
Our method consists of two modules: 1) learning cell patterns using stacked sparse autoencoder followed by kmeans clustering, and 2) constructing bag-of-edge histogram based on Delaunay triangulation graph. To learn different cell patterns, we first segment cell nuclei and extract a 41 by 41 cell patch at the center of each segmented nucleus. Then these cell patches are fed into stacked sparse autoencoder to learn high-level feature representations. Finally we use kmeans algorithm on these high-level features to learn K cell patterns. To characterize spatial arrangement of different cell patterns for an image, we first assign each cell nucleus to one of the K learned cell patterns using the above steps in module 1. Then based on the centroids of segmented nuclei, a Delaunay triangulation graph is constructed. Each edge in the graph contains two nuclei. Since we have learned K nucleus patterns, the possible number of edge types will be K + K*(K-1)/2. K comes from where the two nuclei are of different cell patterns while K*(K-1)/2 comes from where the two nuclei are of the same cell patterns. The frequency of different types of edges occurring is used as feature representation of the image. 

At last we investigate what specific edge types are related patient survival. We also compare the proposed topological features with traditional cellular morphological features, tumor stage, and subtypes, and find our topological features can better predict patient prognosis.

Downloading histological images
-----
Image data can be downloaded at [here](https://doi.org/10.6084/m9.figshare.4700101.v1) (3.37GB). If you don't want to downlad the data and just want to see the codes and resutls, that is okay, because all intermediate and final results are already included.

How to run code
-----
Generally, execution order of code in a folder is indicated by filenames. For exeample, m1_xxx.m (or .R) should be run first, and then m2_xxx.m (or .R).

Folder or file descriptions (they should be run in the following order)
1. "KIRP_image" <br> 
Images should be downloaded and uncompressed in this folder.

2. "clinicalData" <br> 
Reading clinical data.

3. "bagOfEdgeHistogram" <br> 
Extracting bag of edge histogram features.

4. "morphologicalFeatures" <br> 
Extracting cellular morphological features.

5. "m1_findCommonPatients.m" <br> 
Intersecting image data and clinical data to find patients having both these two types of data.

6. "survivalAnalysis" <br> 
Univariate survival analysis.

7. "lassoCoxRegression" <br>
We first select the top 6 features that are mostly significantly related to survival using univariate Cox regression. Then we use PCA to reduce noise and decorrelate these features. The first 2 components that contribute at least 75% of the total variance are retained. Finally we train a Cox regression model with cross validation on the 2 features. Patients are divided into a low-risk or high-risk group based on the risk indices predicted by the Cox regression model. Then we test the survival difference between the two groups using log-rank test. 

8. "ROCAnalysis" <br>
We also conduct receiver-operator characteristic (ROC) curve analysis for binary outcome of 5-year survival to determine the prognosis prediction capability for stage, tumor subtype, and the risk index of Cox regression model. Areas of ROC curves are calculated, and ROC curves are plotted.


Contact information
-----
If you have any questions, feel free to contact me.<br>
Jun Cheng, Southern Medical University, Guangzhou, China. Email: chengjun583@qq.com
