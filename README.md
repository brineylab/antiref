[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.7474336.svg)](https://doi.org/10.5281/zenodo.7474336)
[![Made withJupyter](https://img.shields.io/badge/Made%20with-Jupyter-orange?logo=Jupyter)](https://jupyter.org/try)
![GitHub](https://img.shields.io/github/license/briney/antiref)


# AntiRef: reference clusters of human antibody sequences

[AntiRef](https://zenodo.org/record/7474336) (**Anti**body **Ref**erence Clusters), which was inspired by [UniRef](https://academic.oup.com/bioinformatics/article/23/10/1282/197795), provides clustered datasets of filtered human antibody sequences. The Jupyter notebooks in this repository contain all code necessary to recreate AntiRef entirely from scratch:

* **[download](download.ipynb)**: downloads raw data from the Observed Antibody Space (OAS) repository. Note that the combined total of these datasets is quite large -- nearly 4TB after decompression.
* **[filter](filter.ipynb)**: filters the raw sequences to ensure only productive, full-length sequences are used to compile AntiRef.
* **[cluster](cluster.ipynb)**: performs a nested clustering procedure using several identity thresholds. This process is similar to that used by UniRef, although the thresholds were optimized for antibody sequence data rather than general protein seqeunces. 

### What is *nested clustering*?
AntiRef is a series of antibody sequence datasets, each clustered at an identity threshold of decreasing stringency. Rather than clustering the filtered input dataset using each threshold in parallel, we perform the clustering sequentially using the output from the previous round as input for the subsequent clustering iteration:

![iterative clustering schematic](https://github.com/briney/antiref/blob/main/img/antiref_iterative-clustering.jpg)

This has two primary benefits. First and most importantly, it ensures that cluster and sequence names are conserved across all AntiRef datasets. Each cluster is named after its representative sequence (as determined by `mmseqs`), and by using the output of one clustering round as input for the next, we can ensure that the representative sequence will be present in all previous clustering outputs. For example, if we separately clustered the input dataset at 99% and 98% identity, there is the possibility that some cluster representatives in the 98% dataset are not present in the 99% dataset because these sequences were not selected as representatives for their respective 99% cluster.

### Why AntiRef?
Biases in the human antibody repertoire result in publicly available antibody sequence datasets containing many duplicate or highly similar sequences. These redundant sequences are a barrier to rapid similarity searches and reduce the efficiency with which these datasets can be used to train statistical or machine learning models of human antibodies. Identity-based clustering provides a solution, however, the extremely large size of available antibody repertoire datasets make such clustering operations computationally intensive and potentially out of reach for many scientists and researchers who would benefit from such data.

Starting from a dataset of ~335M unique, full-length, productive human antibody sequences from the Observed Antibody Space repository, several AntiRef cluster sets were generated. Due to the modular nature of recombined antibody genes, the clustering thresholds used by UniRef (100, 90 and 50 percent identity) to cluster general protein sequences are suboptimal for antibody clustering. AntiRef provides reference antibody sequence datasets clustered at a range of relevant identity thresholds: 100, 99, 98, 96, 94, 92 and 90 percent. AntiRef90, which uses the lowest clustering threshold of any AntiRef dataset, is roughly one-third the size of the filtered input dataset and less than half the size of the non-redundant AntiRef100.

### Where can I download AntiRef datasets?
AntiRef datasets are available on Zenodo and can be downloaded at the following links:

* [AntiRef100](https://doi.org/10.5281/zenodo.7474657): representative sequences resulting from clustering all filtered AntiRef input sequences at 100% identity.
* [AntiRef99](https://doi.org/10.5281/zenodo.7475961): representative sequences resulting from clustering AntiRef100 at 99% identity.
* [AntiRef98](https://doi.org/10.5281/zenodo.7476040): representative sequences resulting from clustering AntiRef99 at 98% identity.
* [AntiRef96](https://doi.org/10.5281/zenodo.7487182): representative sequences resulting from clustering AntiRef98 at 96% identity.
* [AntiRef94](https://doi.org/10.5281/zenodo.7487199): representative sequences resulting from clustering AntiRef96 at 94% identity.
* [AntiRef92](https://doi.org/10.5281/zenodo.7487264): representative sequences resulting from clustering AntiRef94 at 92% identity.
* [AntiRef90](https://doi.org/10.5281/zenodo.7487298): representative sequences resulting from clustering AntiRef92 at 90% identity.

### How should I cite AntiRef?
Zenodo provides a DOI for every deposited dataset, and recommends using the DOI that corresponds to the version of AntiRef you used. The DOI of the current version (`v2022.12.14`) is `10.5281/zenodo.7474336`, so an appropriate citation would be:

```
Briney, Bryan. (2022). AntiRef: reference clusters of human antibody sequences (v2022.12.14) 
[Data set]. Zenodo. https://doi.org/10.5281/zenodo.7474336
```

