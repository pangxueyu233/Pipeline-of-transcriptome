# The identification of alternative splicing events

Here, we summarized the [rMats pipeline](https://github.com/Xinglab/rmats-turbo) of alternative splicing events identification in  this page. Firstly, you should install the right version of `rMats` in your ubuntu system.

Then, we could run  `rMats` with modified parameters. In previous, we had a really tough time to install and execute the  `rMats`, because its environment is so hard to be compatible in our server. But now, the latest version was developed based on `Python (3.6.12 or 2.7.15)` which strongly simplified the installing steps. After downloading the package and run one-line code is okay.

~~~R
./build_rmats
~~~

However, I found the latest version of `rMats` always generated the empty output files with the default parameter. After I learning all the detail parameters of `rMats`, I found the `--allow-clipping` is the most important variable in processing our bam files generated from [Step1.](step1.md) 

Now you could following the pipeline to identify the alternative splicing events in your RNAseq data.

Firstly, you should record the absolute position of your bam files:

~~~shell
mkdir /mnt/data/user_data/xiangyu/workshop/RNAseq/RNAseq_85_WBH_20210809_9samples/sortedByCoord.out.bam/rMATS
cd /mnt/data/user_data/xiangyu/workshop/RNAseq/RNAseq_85_WBH_20210809_9samples/sortedByCoord.out.bam/rMATS

cd /mnt/data/user_data/xiangyu/workshop/RNAseq/RNAseq_85_WBH_20210809_9samples/sortedByCoord.out.bam
vim T1_AML.txt
/mnt/data/user_data/xiangyu/workshop/RNAseq/RNAseq_85_WBH_20210809_9samples/sortedByCoord.out.bam/T1_L1.Aligned.sortedByCoord.out.bam,/mnt/data/user_data/xiangyu/workshop/RNAseq/RNAseq_85_WBH_20210809_9samples/sortedByCoord.out.bam/T1_L2.Aligned.sortedByCoord.out.bam,/mnt/data/user_data/xiangyu/workshop/RNAseq/RNAseq_85_WBH_20210809_9samples/sortedByCoord.out.bam/T1_L3.Aligned.sortedByCoord.out.bam

vim T2_AML.txt
/mnt/data/user_data/xiangyu/workshop/RNAseq/RNAseq_85_WBH_20210809_9samples/sortedByCoord.out.bam/T2_N.Aligned.sortedByCoord.out.bam,/mnt/data/user_data/xiangyu/workshop/RNAseq/RNAseq_85_WBH_20210809_9samples/sortedByCoord.out.bam/T2_R1.Aligned.sortedByCoord.out.bam

vim T3_AML.txt
/mnt/data/user_data/xiangyu/workshop/RNAseq/RNAseq_85_WBH_20210809_9samples/sortedByCoord.out.bam/T3_R2.Aligned.sortedByCoord.out.bam,/mnt/data/user_data/xiangyu/workshop/RNAseq/RNAseq_85_WBH_20210809_9samples/sortedByCoord.out.bam/T3_R4.Aligned.sortedByCoord.out.bam
~~~

Then, you could begin to run the `rMats pipeline`

~~~SHELL
mkdir /mnt/data/user_data/xiangyu/workshop/RNAseq/RNAseq_85_WBH_20210809_9samples/sortedByCoord.out.bam/rMATS/tmp
python /mnt/data/user_data/xiangyu/programme/rmats-turbo-master/rmats.py \
--b1 /mnt/data/user_data/xiangyu/workshop/RNAseq/RNAseq_85_WBH_20210809_9samples/sortedByCoord.out.bam/T2_AML.txt \
--b2 /mnt/data/user_data/xiangyu/workshop/RNAseq/RNAseq_85_WBH_20210809_9samples/sortedByCoord.out.bam/T1_AML.txt \
--gtf /mnt/data/public_data/reference/Mus_musculus_UCSC/UCSC/mm10/Annotation/Genes/genes.gtf \
--od /mnt/data/user_data/xiangyu/workshop/RNAseq/RNAseq_85_WBH_20210809_9samples/sortedByCoord.out.bam/rMATS/T2_VS_T1_AS \
--tmp /mnt/data/user_data/xiangyu/workshop/RNAseq/RNAseq_85_WBH_20210809_9samples/sortedByCoord.out.bam/rMATS/tmp \
-t paired --nthread 20 --readLength 150 --allow-clipping

mkdir /mnt/data/user_data/xiangyu/workshop/RNAseq/RNAseq_85_WBH_20210809_9samples/sortedByCoord.out.bam/rMATS/tmp2
python /mnt/data/user_data/xiangyu/programme/rmats-turbo-master/rmats.py \
--b1 /mnt/data/user_data/xiangyu/workshop/RNAseq/RNAseq_85_WBH_20210809_9samples/sortedByCoord.out.bam/T3_AML.txt \
--b2 /mnt/data/user_data/xiangyu/workshop/RNAseq/RNAseq_85_WBH_20210809_9samples/sortedByCoord.out.bam/T1_AML.txt \
--gtf /mnt/data/public_data/reference/Mus_musculus_UCSC/UCSC/mm10/Annotation/Genes/genes.gtf \
--od /mnt/data/user_data/xiangyu/workshop/RNAseq/RNAseq_85_WBH_20210809_9samples/sortedByCoord.out.bam/rMATS/T3_VS_T1_AS \
--tmp /mnt/data/user_data/xiangyu/workshop/RNAseq/RNAseq_85_WBH_20210809_9samples/sortedByCoord.out.bam/rMATS/tmp2 \
-t paired --nthread 20 --readLength 150 --allow-clipping

mkdir /mnt/data/user_data/xiangyu/workshop/RNAseq/RNAseq_85_WBH_20210809_9samples/sortedByCoord.out.bam/rMATS/tmp3
python /mnt/data/user_data/xiangyu/programme/rmats-turbo-master/rmats.py \
--b1 /mnt/data/user_data/xiangyu/workshop/RNAseq/RNAseq_85_WBH_20210809_9samples/sortedByCoord.out.bam/T3_AML.txt \
--b2 /mnt/data/user_data/xiangyu/workshop/RNAseq/RNAseq_85_WBH_20210809_9samples/sortedByCoord.out.bam/T2_AML.txt \
--gtf /mnt/data/public_data/reference/Mus_musculus_UCSC/UCSC/mm10/Annotation/Genes/genes.gtf \
--od /mnt/data/user_data/xiangyu/workshop/RNAseq/RNAseq_85_WBH_20210809_9samples/sortedByCoord.out.bam/rMATS/T3_VS_T2_AS \
--tmp /mnt/data/user_data/xiangyu/workshop/RNAseq/RNAseq_85_WBH_20210809_9samples/sortedByCoord.out.bam/rMATS/tmp3 \
-t paired --nthread 20 --readLength 150 --allow-clipping
~~~

And you would get the output files like following (the `T3_VS_T2_AS` file was as an example)

~~~shell
tree -lh /mnt/data/user_data/xiangyu/workshop/RNAseq/RNAseq_85_WBH_20210809_9samples/sortedByCoord.out.bam/rMATS/T3_VS_T2_AS
.
├── [199K]  A3SS.MATS.JCEC.txt
├── [198K]  A3SS.MATS.JC.txt
├── [156K]  A5SS.MATS.JCEC.txt
├── [155K]  A5SS.MATS.JC.txt
├── [151K]  fromGTF.A3SS.txt
├── [112K]  fromGTF.A5SS.txt
├── [503K]  fromGTF.MXE.txt
├── [ 17K]  fromGTF.novelJunction.A3SS.txt
├── [ 18K]  fromGTF.novelJunction.A5SS.txt
├── [482K]  fromGTF.novelJunction.MXE.txt
├── [ 534]  fromGTF.novelJunction.RI.txt
├── [2.3M]  fromGTF.novelJunction.SE.txt
├── [ 102]  fromGTF.novelSpliceSite.A3SS.txt
├── [ 102]  fromGTF.novelSpliceSite.A5SS.txt
├── [ 140]  fromGTF.novelSpliceSite.MXE.txt
├── [ 108]  fromGTF.novelSpliceSite.RI.txt
├── [ 104]  fromGTF.novelSpliceSite.SE.txt
├── [ 31K]  fromGTF.RI.txt
├── [2.5M]  fromGTF.SE.txt
├── [ 40K]  JCEC.raw.input.A3SS.txt
├── [ 31K]  JCEC.raw.input.A5SS.txt
├── [192K]  JCEC.raw.input.MXE.txt
├── [8.9K]  JCEC.raw.input.RI.txt
├── [1.0M]  JCEC.raw.input.SE.txt
├── [ 40K]  JC.raw.input.A3SS.txt
├── [ 30K]  JC.raw.input.A5SS.txt
├── [190K]  JC.raw.input.MXE.txt
├── [8.6K]  JC.raw.input.RI.txt
├── [1.0M]  JC.raw.input.SE.txt
├── [971K]  MXE.MATS.JCEC.txt
├── [969K]  MXE.MATS.JC.txt
├── [ 43K]  RI.MATS.JCEC.txt
├── [ 43K]  RI.MATS.JC.txt
├── [4.6M]  SE.MATS.JCEC.txt
├── [4.5M]  SE.MATS.JC.txt
├── [ 389]  summary.txt
└── [4.0K]  tmp
    ├── [4.0K]  JC_A3SS
    │   ├── [ 70K]  rMATS_result_FDR.txt
    │   ├── [5.0K]  rMATS_result_ID.txt
    │   ├── [ 32K]  rMATS_result_I-L.txt
    │   ├── [ 35K]  rMATS_result_INP.txt
    │   ├── [ 54K]  rMATS_result_P-V.txt
    │   └── [102K]  rMATS_result_.txt
    ├── [4.0K]  JC_A5SS
    │   ├── [ 55K]  rMATS_result_FDR.txt
    │   ├── [3.7K]  rMATS_result_ID.txt
    │   ├── [ 25K]  rMATS_result_I-L.txt
    │   ├── [ 26K]  rMATS_result_INP.txt
    │   ├── [ 42K]  rMATS_result_P-V.txt
    │   └── [ 80K]  rMATS_result_.txt
    ├── [4.0K]  JCEC_A3SS
    │   ├── [ 71K]  rMATS_result_FDR.txt
    │   ├── [5.0K]  rMATS_result_ID.txt
    │   ├── [ 32K]  rMATS_result_I-L.txt
    │   ├── [ 35K]  rMATS_result_INP.txt
    │   ├── [ 55K]  rMATS_result_P-V.txt
    │   └── [103K]  rMATS_result_.txt
    ├── [4.0K]  JCEC_A5SS
    │   ├── [ 55K]  rMATS_result_FDR.txt
    │   ├── [3.7K]  rMATS_result_ID.txt
    │   ├── [ 25K]  rMATS_result_I-L.txt
    │   ├── [ 27K]  rMATS_result_INP.txt
    │   ├── [ 42K]  rMATS_result_P-V.txt
    │   └── [ 81K]  rMATS_result_.txt
    ├── [4.0K]  JCEC_MXE
    │   ├── [337K]  rMATS_result_FDR.txt
    │   ├── [ 23K]  rMATS_result_ID.txt
    │   ├── [142K]  rMATS_result_I-L.txt
    │   ├── [169K]  rMATS_result_INP.txt
    │   ├── [262K]  rMATS_result_P-V.txt
    │   └── [479K]  rMATS_result_.txt
    ├── [4.0K]  JCEC_RI
    │   ├── [ 16K]  rMATS_result_FDR.txt
    │   ├── [ 926]  rMATS_result_ID.txt
    │   ├── [7.0K]  rMATS_result_I-L.txt
    │   ├── [8.0K]  rMATS_result_INP.txt
    │   ├── [ 12K]  rMATS_result_P-V.txt
    │   └── [ 23K]  rMATS_result_.txt
    ├── [4.0K]  JCEC_SE
    │   ├── [1.4M]  rMATS_result_FDR.txt
    │   ├── [162K]  rMATS_result_ID.txt
    │   ├── [749K]  rMATS_result_I-L.txt
    │   ├── [874K]  rMATS_result_INP.txt
    │   ├── [1.2M]  rMATS_result_P-V.txt
    │   └── [2.2M]  rMATS_result_.txt
    ├── [4.0K]  JC_MXE
    │   ├── [335K]  rMATS_result_FDR.txt
    │   ├── [ 23K]  rMATS_result_ID.txt
    │   ├── [142K]  rMATS_result_I-L.txt
    │   ├── [167K]  rMATS_result_INP.txt
    │   ├── [260K]  rMATS_result_P-V.txt
    │   └── [477K]  rMATS_result_.txt
    ├── [4.0K]  JC_RI
    │   ├── [ 15K]  rMATS_result_FDR.txt
    │   ├── [ 921]  rMATS_result_ID.txt
    │   ├── [6.9K]  rMATS_result_I-L.txt
    │   ├── [7.7K]  rMATS_result_INP.txt
    │   ├── [ 12K]  rMATS_result_P-V.txt
    │   └── [ 22K]  rMATS_result_.txt
    └── [4.0K]  JC_SE
        ├── [1.4M]  rMATS_result_FDR.txt
        ├── [162K]  rMATS_result_ID.txt
        ├── [747K]  rMATS_result_I-L.txt
        ├── [868K]  rMATS_result_INP.txt
        ├── [1.2M]  rMATS_result_P-V.txt
        └── [2.2M]  rMATS_result_.txt

11 directories, 96 files
~~~

Then, you could use `maser` to quantify the alternative splicing events and `IGV` to visualize the specific events. Besides, you could take the expression data of each isoform into following analysis. 

