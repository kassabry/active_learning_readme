# README Steps

## Formatting Data for Running the Active Learning Model

1- Prepare a csv file of the entire pool of compounds with a SMILES string for each compound. The entire pool of compounds should include both labeled and unlabeled compounds. 

2- Within your dataset, there should be a unique id for each compound. This uniqueness ID could be a unique smiles string or integer indexing. 

3- Generate features (e.g. MorganFP 1024 using rdkit) for each compound. Add this feature as a string for each compound as a column in your csv dataset. (Rewrite the code in [here](https://github.com/gitter-lab/pria-ams-enamine/blob/master/preprocessing/preprocess_datasets_chtc.py#L85-L91) to make a streamlined preprocessing code)

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

6- There should exist some structure for where your training pool csv files and the unlabeled pool csv files are located. We would recommend that you match the sample data set file structure within this repository. 

## Running the Active Learning Model

1. Before running the active learning model, you will need to modify a config file to match the format the model takes in. We recommend modifying [active-learning-drug-discovery/param_configs/general_pipeline_config.json](active-learning-drug-discovery/param_configs/general_pipeline_config.json) that has most of what is needed for an initial already properly formatted. The main things to update within the training_data_params and unlabeled_data_params sections of the config file are the data paths to your data paths and the corresponding column names to match the column names within your data. 


## Notes

It seems like the config variable names don't matter? If the variables don't match the specific names within the code, it will error out. It will run without even checking the config??

The active learning code will default to computing the Tanimoto dissimilarity matrix if you don't already have a dissimilarity matrix, but both the BT clustering and Tanimoto both error out. 

I think there's something wrong with the conda environment, but I need to investigate it more to find out for sure. 

All the indices for the data must start at 0 and count up. The indices cannot exceed the len of data. So no cutting up data in multiple files. 

## Errors

### BT Clustering
![image](https://github.com/kassabry/active_learning_readme/assets/33270781/240142e0-3b71-40a8-9e0b-1a2af21079b0)

1). With the dissimilarity matrix path defined, you will get the OS error, I think because the file hasn't been made yet. (Should remove from the example on how to run at top of the code)

2). Without the dissimialrity matrix path defined, there is an assignment error, where the variable is referenced before the assignment. 

The weird aspect of this, is the only difference between the two is the file path for the uncreated matrix .dat file, but for some reason the code will run up until the writing of the .dat file when the filepath is defined as in (1). But when there is no file path defined as in (2), the code won't even run. Which means I think that the get_features method is broken, whereas, the get_n_instances method is fine, but writing the file is messed up somewhere. 
