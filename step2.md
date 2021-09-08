# Step2. The quantification and DEG identification in bulk RNA-seq

Here, we integrated expression quantification, counts data normalization, differentiation expression gene identification and pathways enrichments, into our pipelines, named ```RNAseq_pipeline```.

And this pipeline is flexible, you could broaden more analysis steps based on the output of ```RNAseq_pipeline```, such as GSEA analysis, TF enrichment, deconvolution and et al. 

- Then, we begin to learn the parameters in this pipeline :

~~~shell
Rscript /mnt/data/user_data/xiangyu/programme/R_PACKAGES/my_code/RNAseq_pipeline --help

        Xiangyu Pan
        8/29/2021
      The R Script merge cnvkit res

      Mandatory arguments:
      --info=sample_info            - sample info list
      --bam=bam_files               - the pathways of .bam files
      --case=case_group             - case group names
      --ctrl=control_group          - control group names
      --cutoff=significant_cutoff   - the significant cutoff value
      --spe=species                 - the species of samples (mm10,GRCm38,hg19,hg38,GRCh38)
      --ref=reference_pathways      - the pathway of ebg and txdb reference
      --out=output_pathways         - the pathway of RNAseq analysis output
      --se_files=se_files_pathways  - the pathway of se file (if se hasn't generate, remove this parameter)
      --help                - print this text

        WARNING : you should install the R packages Rsamtools, GenomicFeatures,
            GenomicAlignments, BiocParallel, pheatmap, RColorBrewer, PoiClaClu, org.Mm.eg.db,
            AnnotationDbi, DOSE, clusterProfiler, topGO, pathview, org.Hs.eg.db, AnnotationDbi,
            DOSE, clusterProfiler, topGO, ggplot2, DESeq2,

  Example:
          Rscript ./RNAseq_pipeline
            --info=./sample_sampletable1.csv
            --bam=./
            --case=case
            --ctrl=ctrl
            --cutoff=1
            --spe=mm10
            --ref=./WORKFLOW_RNAseq/
            --out=./sortedByCoord.out.bam
            --se_files=./case_vs_ctrl_3_vs_3_work_file/PXY_0_RNAseq_se.RData
~~~

- The [reference files](WORKFLOW_RNAseq) had been attached in this page, and you could download them by clicking [here](WORKFLOW_RNAseq), including ```mouse genome (mm10,GRCm38)```, ```human genome (hg19/GRCh38)```, ```drosophila genome (dm6)``` and ```armadillidium vulgare genome (Arma_vul)```. The genome version should be consistent in alignment processing and subsequent quantification analysis. 

- And you also could generate the txdb and ebg reference for your specific species, as following R codes:

~~~R
gtffile <- file.path("./Drosophila/Drosophila_melanogaster/UCSC/dm6/Annotation/Genes","genes.gtf")
txdb <- makeTxDbFromGFF(gtffile, format = "gtf", circ_seqs = character())
txdb
ebg <- exonsBy(txdb, by="gene")
ebg
save(txdb,file="./WORKFLOW_RNAseq/dm6_anno/txdb_dm6.RData")
save(ebg,file="./WORKFLOW_RNAseq/dm6_anno/ebg_dm6.RData")
~~~

- Here, we show an example to run the pipeline for RNA-seq analysis:

~~~shell
Rscript /mnt/data/user_data/xiangyu/programme/R_PACKAGES/my_code/RNAseq_pipeline \
  --info=/mnt/data/user_data/xiangyu/workshop/RNAseq/RNAseq_85_WBH_20210809_9samples/sortedByCoord.out.bam/sample_sampletable1.csv \
  --bam=/mnt/data/user_data/xiangyu/workshop/RNAseq/RNAseq_85_WBH_20210809_9samples/GRCm38_version \
  --case=T3 \
  --ctrl=T1 \
  --cutoff=.5 \
  --spe=GRCm38 \
  --ref=/mnt/data/user_data/xiangyu/workshop/WORKFLOW_RNAseq \
  --out=/mnt/data/user_data/xiangyu/workshop/RNAseq/RNAseq_85_WBH_20210809_9samples/GRCm38_version/ \
  --se_files=null
~~~

- And then, you would get the new folder named `T3_vs_T1_3_vs_3_work_file`, and its tree structure was as following:

~~~shell
── T3_vs_T1_3_vs_3_work_file
    ├── 0_RNAseq_se.RData
    ├── 10_T3_VS_T1_KEGG_all.pdf
    ├── 11_T3_VS_T1_GO_UP.csv
    ├── 12_T3_VS_T1_GO_DOWN.csv
    ├── 13_T3_VS_T1_GO_UP.pdf
    ├── 14_T3_VS_T1_GO_DOWN.pdf
    ├── 1_cluster_similarity_pca.pdf
    ├── 1_cluster_similarity_rld_pheatmap.pdf
    ├── 1_cluster_similarity_vsd_pheatmap.pdf
    ├── 2_DESeq2_count.csv
    ├── 3_T3_VS_T1_result.csv
    ├── 4_T3_VS_T1_symbol_and_anno.csv
    ├── 5_T3_VS_T1_count_DESeq2_count_symbol_and_anno.csv
    ├── 6_T3_VS_T1_KEGG_UP.csv
    ├── 7_T3_VS_T1_KEGG_DOWN.csv
    ├── 8_T3_VS_T1_KEGG_UP.pdf
    └── 9_T3_VS_T1_KEGG_DOWN.pdf
~~~

---

- If you had executed the pipeline, you could get the same output files  [T3_vs_T1_3_vs_3_work_file](T3_vs_T1_3_vs_3_work_file), and you also could access this templet output files by clicking [here](T3_vs_T1_3_vs_3_work_file)
- Here, we will introduce the meaning of each file in [T3_vs_T1_3_vs_3_work_file](T3_vs_T1_3_vs_3_work_file) 
  - `0_RNAseq_se.RData` include all the raw data quantified by `Rsamtools` and `GenomicAlignments`, it's a S4 class, and the sample information could be extracted be using `colData(se)`
  
  - `1_cluster_similarity_pca.pdf`, the graph record the PCA distribution of all sample. ![image-20210908151246141](D:\github_clone\Pipeline-of-transcriptome\step2.assets\image-20210908151246141.png)
  
  - `1_cluster_similarity_rld_pheatmap.pdf` and `1_cluster_similarity_vsd_pheatmap.pdf`, as graphs showed the similarity of each sample on whole transciptome.
  
  - `2_DESeq2_count.csv` include the DESeq2 normalization data for GSEA analysis and heat-map visualization
  
  - `3_T3_VS_T1_result.csv` include the all differentiation expression gene without filter. 
  
  - `4_T3_VS_T1_symbol_and_anno.csv` include the all differentiation expression gene with detail gene annotation. 
  
  - **5_T3_VS_T1_count_DESeq2_count_symbol_and_anno.csv** include all the raw data, DESeq2 normalization data and  differentiation expression value with detail gene annotation.
  
  - `6_T3_VS_T1_KEGG_UP.csv` include all the KEGG enrichment results in case samples up-regulated signatures. 
  
  - `7_T3_VS_T1_KEGG_DOWN.csv` include all the KEGG enrichment results in control samples up-regulated signatures. 
  
  - `8_T3_VS_T1_KEGG_UP.pdf`, `9_T3_VS_T1_KEGG_DOWN.pdf` and `10_T3_VS_T1_KEGG_all.pdf` show the top KEGG enrichment results
  
  - `11_T3_VS_T1_GO_UP.csv`  include all the GO enrichment results in case samples up-regulated signatures. 
  
  - `12_T3_VS_T1_GO_DOWN.csv` include all the GO enrichment results in control samples up-regulated signatures. 
  
  - `13_T3_VS_T1_GO_UP.pdf` and `14_T3_VS_T1_GO_DOWN.pdf` show the top GO enrichment results![image-20210908152133341](D:\github_clone\Pipeline-of-transcriptome\step2.assets\image-20210908152133341.png)
  
    

 
