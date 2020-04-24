# REFINED

![REFINED_Doagram](REFINED_Diagram.jpg)

This repository is made to share the [REFINED](https://arxiv.org/abs/1912.05687) (Representation of Features as Images with NEighborhood Dependencies). To use the REFINED and create images corresponding to your dataset you neet to use the following codes step by step. REFINED contains two main cores, initial MDS and hill climbing, where both are eucliadean distance based. First the initiall coordinates will be created by MDS initializer part, then the coordinates will be updated by the hill climbing. Multiple functions are being used in the process, hence a Toolpox.py is provided which includes all the function needed. 
**We added small subset of processed NCI60 data in the data folder for the users to be able to use the code. Note that due to the size of the dataset so the performance is much lower than the paper.**

### Reuqired python packages
To use all the provided code, installing following packages is required.
1. Numpy
2. Pandas
3. Scipy
4. Scikit-learn
5. Tensorflow
6. Keras

## 1. Toolbox
The main functions provided in the toolbox are performing below tasks:
- two_d_eq:
The MDS function reduces dimensionality by preserving the distance in the new space as close as possible to the initial space.  Therefore, when it is being used for generating coordinates in 2-D space for features, the coordinates will have arbitrary values. Hence, the first step to create images is bringing down the arbitrary values in the range [0,N], where N is the square root of the number features. This process is handled by **two_d_eq**.
- Assign_features_to_pixels:
As the provided coordinates generated by MDS are extremely overlapped, the generated image will be extremely sparse. Therefore we locate the overlapped features in a closest empty pixel.

## 2. Initial MDS
The **Initial_MDS.py** provides the none-overlaping coordinates generated by post MDS processing, plus euclidean distance matrix and feature lists (descriptor names) as input of the hill climbing. Note that the provided coordinates also can be used to generate image to train a CNN. The initial MDS uses some of the toolbox functions integrated with the following steps:

- Loading the data
This section loads the input data, no anottation is needed (unsupervised). The **"normalized_padel_feats_NCI60_672_small.csv"** is provided to test the code.

- MDS
This part of the code applies MDS and its post processing steps for transposed dimensionality reduction and creating inital image coordinates.

- Saving hill climbing inputs
As the hill climbing algorithms works based on minimizing the Euclidean distance, beside the the MDS image coordinates, a euclidean distance of features as matrice need to saved as inputs of the hill climbing. Therefore all the hill climbing required inputs are saved as a pickle.

## 3. Hill climbing
The hill climbing section of REFINED is written based on using Message Passing Interface (MPI) of python to use HPCC resource very efficiently. To run this code make sure to install **mpi4py** library of Python. The hill climbing algorithm section was written based master-slave control process where the first processor is the master and other processors will slave. The master processor distribute, scatter and receive data from slave processors. Slave processors do the computational task. Some computational functions needed to run Hill climbing code should be imported from **paraHill.py**
To run the hill climbing algorithm one need to use the **mpiHill_Hardcoded.py**. 

- Input
Input of hill climbing is the initial MDS output saved as pickle file. It includes three parameter;  **`gene_names`**: feature names, **`dist_matr`**: Euclidean distance matrix of features in initial space, **`init_map`**: feature's coordinate created by initial MDS.

- Parameters
The only parameter that user of this algorithm need to choose is number of iterations. Number of iterations is basically how many times the hill climbing goes over the entire features and check each feature exchange cost. Defaults is NI = 5.

- Output
The feature names, REFINED coordinates, and intial map will be save as the output of hill climbing in a pickle file. REFINED are coordinates are saved in **`coords`**.
## 4. Training a CNN
After features coordinates are found by REFINED, they can be used to convert data into images, then trai a CNN. To this end we provided a small subset of the NCI60 dataset drug descriptors and responses in the `data` folder, and the architectures that we used to train for NCI regression and classification tasks of the paper in the `train` folder. The code that we used to train the REFINED CNN model on the GDSC dataset is also provided under the `train` folder as **GDSC_train.py**.

## 5. Data
**`NCI60`**: We downloaded the GI50 data from [NCI60_download](https://dtp.cancer.gov/databases_tools/bulk_data.htm) and the associated drug's chemical information from [Pubchem](https://pubchem.ncbi.nlm.nih.gov/).

**`GDSC`**: We downloaded the IC50 responses and cell line screened data from [GDSC_download](https://www.cancerrxgene.org/downloads/bulk_download), and the drug's chemical information from [Pubchem](https://pubchem.ncbi.nlm.nih.gov/).

**`PaDel`**: To convert the chemical information of each drug to their descriptors, we used [PaDel](http://www.yapcwsoft.com/dd/padeldescriptor/) software.

## Cite
If you use [REFINED](https://arxiv.org/abs/1912.05687) for research, please cite the following paper:

> REFINED (REpresentation of Features as Images with NEighborhood Dependencies): A novel feature representation for Convolutional Neural Networks
O Bazgir, R Zhang, SR Dhruba, R Rahman, S Ghosh, R Pal
arXiv preprint arXiv:1912.05687

