# 20440 project
## Aim 0: Creating Working DataFrame
## Aim 1: Using a heatmap and dendrogram 
### load in data
Read in the csv file created from the DataFrame created in Aim 0: Creating Working DataFrame.ipynb. This file contains information about the merged datasets of various cancer neoepitope types. Specifically, each cancer neoepitope has an associated amino acid substitution, cancer classification, gene ID, length normalized variant frequency, neoantigen frequency, predicted error in pathogenicity calculation, protein length, variant frequency, categorized base pair substitution, and categorized pathogenicity classification. 
### import packages to use 
For the purposes of creating the heatmap and dendrogram in this Aim, the imported packages are pandas (as pd) to create the dataframe, seaborn (as sb) and matplotlib.pyplot (as plt) to graphically visualize groupings, sklearn (preprocessing, decomposition, cluster, ensemble, model_selection, metrics) to standardize the data before analysis and scipy (cluster, spatial) to create groupings for use in the dendrogram and heatmap. 
### store numerical data in a pandas dataframe and standardize data for further processing
Drop all columns that contain qualitative variables so that the dataframe only contains numerical datatypes that can be standardized. The variables with qualitative variables (Amino Acid Substitution, Cancer Classification, gene ID) are rewritten as categorical variables so that the information is not lost. 
### create a base pair sub key
For the creation of categorical variables, a key was created to reference the numerical values assigned for each qualitative entry in BasePair Substitution, Pathogenicity and Amino Acid Substitution. Base Pair substitutions were grouped as A to T, C to G, etc resulting in 12 groupings. Amino Acid Substitutions were groups as non-polar neutral to polar neutral, non-polar neutral to polar acidic, polar acidic to polar basic, etc. 

| Number | Base Pair Substitution | Pathogenicity | Amino Acid Sub |
| --- | --- | --- | --- |
| 0 | None | Pathogenic | None |
| 1 | A to T | Neutral/Unknown | NP neutral to P neutral |
| 2 | A to G | N/A | NP neutral to P acidic |
| 3 | A to C | N/A | NP neutral to P basic |
| 4 | T to A | N/A | P neutral to NP neutral |
| 5 | T to G | N/A | P neutral to P acidic |
| 6 | T to C | N/A | P neutral to P basic |
| 7 | G to A | N/A | P acidic to NP neutral |
| 8 | G to T | N/A | P acidic to P neutral |
| 9 | G to C | N/A | P acidic to P basic |
| 10 | C to A | N/A | P basic to NP neutral |
| 11 | C to T | N/A | P basic to P neutral |
| 12 | C to G | N/A | P basic to P acidic |

