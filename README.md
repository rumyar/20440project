# 20440 project
## Aim 1: Using a heatmap and dendrogram 
#### load in data from github
#### store data in a pandas dataframe 
#### import the following packages: 
##### import matplotlib.pyplot as plt
  ##### import sklearn
  ##### from sklearn import preprocessing, decomposition, cluster, ensemble, model_selection, metrics, tree
  ##### import scipy as sp
  ##### from scipy import cluster, spatial
  ##### from scipy.spatial import distance
  ##### from scipy.spatial.distance import pdist
  ##### from scipy.cluster import hierarchy 
  ##### from scipy.cluster.hierarchy import dendrogram, linkage, fcluster
  #### drop data that is not numerical from the dataframe
### create a base pair sub key
[ ]
# Base-Pair Sub Key
key = pd.DataFrame(data = ['None','A to T', 'A to G', 'A to C', 'T to A', 'T to G', 'T to C', 'G to A', 'G to T', 'G to C', 'C to A', 'C to T', 'C to G'], columns = ['Base Pair Substitution'], index  = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12])
key['Pathogenicity'] = ['Pathogenic', 'Neutral/Unknown', 'N/A', 'N/A', 'N/A', 'N/A', 'N/A', 'N/A', 'N/A', 'N/A', 'N/A', 'N/A', 'N/A']
key['Amino Acid Sub'] = ['None', 'NP neutral to P neutral', 'NP neutral to P acidic', 'NP neutral to P basic', 'P neutral to NP neutral', 'P neutral to P acidic', 'P neutral to P basic', 'P acidic to NP neutral', 'P acidic to P neutral', 'P acidic to P basic', 'P basic to NP neutral', 'P basic to P neutral', 'P basic to P acidic']
[ ]
def dendrogram_data(df, cancer_type):
    num_values = df.loc[df['Cancer Classification'] == cancer_type].shape[0]
    array = []
    for i in range(1,13):
        array.append((data.loc[(data['Cancer Classification'] == cancer_type) & (data['Base Pair Substitutions'] == i)]).shape[0])
    for j in range(1,13):
        array.append((data.loc[(data['Cancer Classification'] == cancer_type) & (data['Amino Acid Classification'] == j)]).shape[0])
    array.append((data.loc[(data['Cancer Classification'] == cancer_type) & (data['Pathogenicity Classification'] == 1)]).shape[0])
    array[:] = [x/num_values for x in array]
    return array
[ ]
import numpy as np
cancer_freq = pd.DataFrame()
cancer_types = ['ALL','AML', 'breast', 'bladder', 'cervix', 'CLL', 'colorectum', 'esophageal', 'glioblastoma', 'glioma low grade', 'head and neck', 'kidney clear cell', 'kidney papillary', 'liver', 'lung adenocarcinoma', 'lung small cell', 'lung squamous', 'medulloblastoma', 'melanoma', 'neuroblastoma', 'ovary', 'pancreas', 'prostate', 'thyroid', 'uterus']
for cancer in cancer_types:
    cancer_freq[cancer] = dendrogram_data(data, cancer)
indices = np.array(['A to T', 'A to G', 'A to C', 'T to A', 'T to G', 'T to C', 'G to A', 'G to T', 'G to C', 'C to A', 'C to T', 'C to G', 'NP neutral to P neutral', 'NP neutral to P acidic', 'NP neutral to P basic', 'P neutral to NP neutral', 'P neutral to P acidic', 'P neutral to P basic', 'P acidic to NP neutral', 'P acidic to P neutral', 'P acidic to P basic', 'P basic to NP neutral', 'P basic to P neutral', 'P basic to P acidic', 'Pathogenic']) 
cancer_freq.set_index(indices)
cancer_freq.to_csv('cancerfreq.csv')
[ ]
sb.heatmap(cancer_freq, yticklabels = indices)
plt.show()
cancer_freq_std = sklearn.preprocessing.scale(cancer_freq)
cancer_freq_std = pd.DataFrame(data = cancer_freq_std, columns = cancer_freq.columns, index =  cancer_freq.index)
sb.heatmap(cancer_freq_std, yticklabels = indices)
plt.show()

[ ]
#cancer_freq_std = cancer_freq_std.drop(['COSMIC'], axis = 1)
plt.figure(figsize=(12, 5)) 
sb.clustermap(cancer_freq_std, metric = 'euclidean', method = 'average', yticklabels = indices)
plt.title('Heatmap of Hierarchially Clustered Values')
plt.show()
​
plt.figure(figsize=(12, 5))  
linked = linkage(cancer_freq_std, 'ward')
labelList = cancer_freq.columns
dendrogram(linked, orientation='top',labels=labelList, distance_sort='descending',show_leaf_counts=True)
plt.xlabel('Cancer Types')
plt.ylabel('Distance')
plt.title('Dendrogram of Hierarchially Clustered Data')
plt.show() 

[ ]
linkages = ['complete', 'average', 'weighted', 'ward']
distances = ['euclidean', 'cityblock', 'cosine', 'correlation', 'hamming', 'jaccard', 'chebyshev', 'canberra']
array = []
for distance in distances:
    for linked in linkages:
        distance_1 = sp.spatial.distance.pdist(cancer_freq_std, metric = distance)
        linked_1 = linkage(cancer_freq_std, method = linked)
        array.append(hierarchy.cophenet(linked_1, distance_1)[0])
