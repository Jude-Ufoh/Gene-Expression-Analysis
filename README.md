# Gene-Expression-Analysis
## Overview
Gene Expression data is the information that shows the genes that are actively producing mRNA in a particular cell, tissue, or organism at a specific point in time. the data helps in understanding the mechanisms underlying normal development, disease progression and responses to various environmental stimuli. RNA- sequencing is one of the three ways of obtaining gene expression data and it involves sequencing the entire transcriptome of a sample. The data obtained is analysed to understand gene expression, identify differentially expressed genes, and gain insights into biological processes and regulatory mechanisms.


## Procedures
- **Data Download**: Visit to the NCBI website [here](ncbi.nim.nih.gov).Select GEO (Gene Expression Omnibus) and then download the GSE183947 dataset that contains RNA -seq data of breast cancer samples.Having selected the http option, the data is downloaded in a ,gz format which is then converted to csv. The csv file contains rows of file nrepresenting different genes and the columns representing different samples. The first column represents the gene names and the rest columns contains the gene expression levels in different formats.The gene expression levels are measured in FPKM (Fragments Per Kilobase of transcript per Million mapped reads) which is a normalised measure of gene expression. ![image](https://github.com/user-attachments/assets/8548e693-b6e6-4060-9b5a-52e35f44a377)

**Data Manipulation**: This involved normalisation,  cleaning, transformation, statistical analysis, differential expression analysis, gene enrichment analysis and data visualisation and was done using R. The following steps were involved
- loading of neccessary library in 
  ~~~
  #Load Libraries
  library(dplyr)
  library(tidyverse)
  install.packages("GEOquery")
  library(GEOquery)
  library(tidyr)
- the downloaded data was loaded into R
  ~~~~
  #read the data

  dat <- read.csv(file = "../Gene Expression Data/GSE183947_fpkm.csv")

- **Loading and Manipulating Metadata**: It was necessary to obtain the metadata which included the supplementary file, donor, metastasis and tissue for accurate interpretation and meaningful analysis. The metadata was manipulated by renaming the columns and changing some values in the column.
  ![image](https://github.com/user-attachments/assets/26782f4e-8265-47b3-914a-ab823a50bfab)
- **Manipulating the Gene Expression Data**: the main gene expression data stored as 'dat' is reshaped to make for  a long file renamed 'dat.long' having three columns which are the gene  samples FPKM
![image](https://github.com/user-attachments/assets/2eb5bfa2-1f07-4185-926e-ca408e917b6a)
![image](https://github.com/user-attachments/assets/057e1f95-5ae6-4788-bf5f-f60b11d30433)
**Combining gene expression data and metadata**: the two datasets were combined using left_join
![image](https://github.com/user-attachments/assets/ec92d29f-fb5c-4659-9f9b-1153d4fd62e4)
  **Brief Statistical Analysis**: The data for BRCA1 and BRCA2 genes were filtered, grouped by gene and tissue and the mean and median values for each group.
![image](https://github.com/user-attachments/assets/09f47fb1-5f9d-4df4-80aa-4c1a4d550f93)

## Visualisation
**Barplot**
![image](https://github.com/user-attachments/assets/6be4f38b-74b0-49e5-8253-4faa48fa63a6)

**Density Plot**
![image](https://github.com/user-attachments/assets/5eff55ae-e0a8-43c5-bf39-5f851e45f9fe)

**Boxplot**
![image](https://github.com/user-attachments/assets/d63c7da1-3b7e-4c00-a6df-3bce789c85c0)

**scatterplot**
![image](https://github.com/user-attachments/assets/aefaa11e-53ad-4079-90f3-446f14e4e429)

**Heatmap**
![image](https://github.com/user-attachments/assets/c455a2ab-1321-49cd-b331-b3ea09c7ed10)










- 
