# Pipeline-of-transcriptome


This workshop recorded the whole processing steps of transcriptome analysis in CC-LY Lab, including alignment, quality control from fasta files. In this page, ```GenomicAlignments``` and ```Rsamtools``` were used to quantify the counts of transcriptome  data. In old version, we used FPKM and TPM for heatmap visualization and gene set enrichment analysis, however, in latest version,  DESeq2 normalized data , which was much better to reduce the effect of gene body and library size, were used to describe the expression pattern of each gene. And the pathways enrichment also based on the DESeq2 normalized data, especially for GSEA processing. 

Here, ```DESeq2 pipeline``` also was used to identify the differentiated expressed genes. There were some essential parameters to set the cutoff of DEG detecting in this pipeline. The detail information would be explained in following pages. To direct visualize the DEGs' function, ```clusterprofiler``` was implemented in this pipeline. GO/KEGG database could be enriched by DEGs with default parameter.  Besides, we also integrated the GSEA processing in following page.

- Before, we used this pipeline, there were some softwares should be installed: 

~~~shell
#STAR
STAR_2.6.0a

#Rscript
R scripting front-end version 3.5.1 (2018-07-02)
~~~

- And the major pipeline of transcriptome analysis includes two parts:

  ##  [Step1. The alignment of bulk RNA-seq](step1.md)

  ## [Step2. The quantification of genes and the identification of DEG ](step2.md)

- And sometimes, you want to quantify the expression levels of each transcripts in bulk RNA-seq, I suggest you follow next pipeline, quantified by ``stringtie`` and/or `RSEM`. 

  ## [Step3. The quantification of transcripts by stringtie ](step3.md)

  ## [Step4. The quantification of transcripts by RSEM](step4.md)
  
  

## Keep updating of Pipeline-of-transcriptome

- And this pipeline is flexible, you could broaden more analysis steps based on the output of ```RNAseq_pipeline```, such as GSEA analysis, TF enrichment, deconvolution and et al. 

- And this `Pipeline-of-transcriptome` would keep updating, welcome anyone others to add comments and provide request. 

  

## **The analysis pipeline had been written**

- [x] Alignment
- [x] Transcription quantification
  - [x] `GenomicsFeatures` and `Rsamtools`
  - [x] `Stringtie`
  - [x] `RSEM`
- [x] DEG identification
- [x] GO/KEGG enrichment
- [x] GSEA
- [ ] Alternative splicing
- [ ] RNA editing
- [ ] Mutation











