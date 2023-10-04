# README Steps

## Formatting Data for Running the Active Learning Model

1- Prepare a csv file of the entire pool of compounds with a SMILES string for each compound. The entire pool of compounds should include both labeled and unlabeled compounds. 

2- Within your dataset, there should be a unique id for each compound. This uniqueness ID could be a unique smiles string or integer indexing. 

3- Generate features (e.g. MorganFP 1024 using rdkit) for each compound. Add this feature as a string for each compound as a column in your csv dataset.

4- Cluster your compounds using a preferred clustering algorithm. Butina-Taylor was used to cluster and generate dissimalirty matrix within this codebase. 
Within this repository, [active_learning_dd/utils/generate_bt_clustering.py](active_learning_dd/utils/generate_bt_clustering.py) is a file that will generate your cluster ids for your compounds as well as computing the dissimilarity matrix. It is helpful to keep this output file to reduce the runtime of some methods by saving the resources needed to recalculate this matrix. 

-- If using a separate clustering method, make sure there is a cluster ID column within your csv dataset and your dissimilarity matrix. Otherwise, the dissimilarity matrix will be generated using Butina-Taylor, and the cluster assignments might not be the same.

5- The csv dataset should have the following format:

| Index ID | Features (Fingerprint) | Cluster ID | SMILES String | Hit Column |
|:--------:|:----------------------:|:----------:|:-------------:|:----------:|
| Index 1  | Feature 1              | Cluster 1  | String 1      | Hit 1      |
| .        | .                      | .          | .             | .          |
| Index N  | Feature N              | Cluster N  | String N      | Hit N      |

One thing to note is that the hit column can be empty, but the column should be present. In addition, the columns do not need to be in this order, but these columns should be present within the dataset. [Fact check this]

6- There should exist some structure for where your training pool csv files and the unlabeled pool csv files are located. We recommend matching the sample data set file structure within this respository. 

