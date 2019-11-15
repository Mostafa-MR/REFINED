# REFINED
This repository is made temporarily to share the REFINED (Representation of Features as Images with NEighborhood Dependencies). Therefore the code is not provided as an easy-to-use package. To use the REFINED to create images corresponding to your dataset you neet to use the following codes step by step. REFINED contains two main cores, initial MDS and hill climbing, where both are eucliadean distance based. First the initiall coordinates will be created by MDS initializer part, then the coordinates will be updated by the hill climbing. Multiple functions are being used in the process, hence a Toolpox.py is provided which includes all the function needed. 
## 1) Toolbox
The main functions provided in the toolbox are performing below tasks:
### a) two_d_eq:
The MDS function reduces dimensionality by preserving the distance in the new space as close as possible to the initial space. Therefore, when it is being used for generating coordinates in 2-D space for features, the coordinates will have arbitrary values. Hence, the first step to create images is bringing down the arbitrary values in the range [0,N], where N is the square root of the number features. This process is handled by **two_d_eq**.
