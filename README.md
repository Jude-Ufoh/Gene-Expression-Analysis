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
   ~~~~
      #get metadata
      gse <- getGEO(GEO = 'GSE183947', GSEMatrix = TRUE)

      metadata <-pData(phenoData(gse[[1]]))
      head(metadata)

      # modifying the metadata by selecting some columns

      metadata.modified <- metadata %>%
  
                #selecting some columns
                select(1,10,11,17) %>%
                #renaming of columns
                rename(tissue = characteristics_ch1)%>%
                 rename(metastasis = characteristics_ch1.1)%>%
                #changing  some values in a column
                mutate(tissue =gsub("tissue: ","",tissue))%>%
                mutate(metastasis =gsub("metastasis: ","",metastasis))
- **Manipulating the Gene Expression Data**: the main gene expression data stored as 'dat' is reshaped to make for  a long file renamed 'dat.long' having three columns which are the gene  samples FPKM
  ~~~~
      #reshaping Data
      dat.long <- dat%>%
          rename(gene =X) %>%
          gather(key ='samples', value ='FPKM', -gene)
![image](https://github.com/user-attachments/assets/057e1f95-5ae6-4788-bf5f-f60b11d30433)
- **Combining gene expression data and metadata**: the two datasets were combined using left_join
    ~~~~~
         dat.long<-dat.long %>%
            left_join(., metadata.modified, by =c("samples" = "description"))
            
- **Brief Statistical Analysis**: The data for BRCA1 and BRCA2 genes were filtered, grouped by gene and tissue and the mean and median values for each group.
  ~~
    dat.long %>%
        filter(gene =="BRCA1" | gene =='BRCA2' )%>%
        group_by(gene, tissue)%>%
        summarize(mean_FPKM =mean(FPKM),
            median_FPKM = median(FPKM))%>%
        arrange(-mean_FPKM)

 

## Visualisation
**Barplot** : In the context of the gene expression data I was working with, the bar plots were used to visualize the FPKM values of specific genes.
![image](https://github.com/user-attachments/assets/6be4f38b-74b0-49e5-8253-4faa48fa63a6)

**Density Plot**: This was helpful for identifying genes that are differentially expressed, as well as understanding the overall distribution of gene expression within the dataset
![image](https://github.com/user-attachments/assets/5eff55ae-e0a8-43c5-bf39-5f851e45f9fe)

**Boxplot**: This enabled me to compare the distributions of gene expression between different groups such as metastasis.
![image](https://github.com/user-attachments/assets/d63c7da1-3b7e-4c00-a6df-3bce789c85c0)

**scatterplot**: This allowed for the visualization of the pattern, direction, and strength of the relationship between the two genes BRCA1 and BRCA2.
![image](https://github.com/user-attachments/assets/aefaa11e-53ad-4079-90f3-446f14e4e429)



## Conclusion
Gene expression data analysis, manipulation, and visualization are essential in biological research for understanding disease mechanisms, biomarker discovery, drug development, personalized medicine, biological pathway analysis, and data visualization. Critical analysis of gene expression data can give insight into the molecular basis of diseases, help to identify potential therapeutic targets and advance personalised medicine.








- 
