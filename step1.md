# Step1. The alignment of bulk RNA-seq

Firstly, we should download the raw data of ```.fasta``` files from ```Sequencing company```. Nowadays, we had much more cooperations with Novogene. Hence, we show the codes line to download the raw data from them:

~~~SHELL
./programme/lnd login \
-u X101SC21030878-Z03-F027 \  #登录账号
-p kf6mmxcj                   #登录密码

./programme/lnd list          #list oss path

./programme/lnd cp oss://CP2020120100066 -d ./ -b 1000  #downlaod the raw data
~~~

Here, we showed the murine AML data as examples. And after downloading, the tree structure of raw data is as following:

~~~shell
.
├── CP2020120100066
│   └── H101SC21030878
│       └── RSCS3300
│           └── X101SC21030878-Z03
│               └── X101SC21030878-Z03-F027
│                   ├── 1.rawdata
│                   │   ├── checkSize.xls
│                   │   ├── T1GFP-L1_FRAS210115518-1r
│                   │   │   ├── MD5.txt
│                   │   │   ├── T1GFP-L1_FRAS210115518-1r_1.fq.gz
│                   │   │   └── T1GFP-L1_FRAS210115518-1r_2.fq.gz
│                   │   ├── T1GFP-L2_FRAS210115519-3r
│                   │   │   ├── MD5.txt
│                   │   │   ├── T1GFP-L2_FRAS210115519-3r_1.fq.gz
│                   │   │   └── T1GFP-L2_FRAS210115519-3r_2.fq.gz
│                   │   ├── T1GFP-L3_FRAS210115520-1r
│                   │   │   ├── MD5.txt
│                   │   │   ├── T1GFP-L3_FRAS210115520-1r_1.fq.gz
│                   │   │   └── T1GFP-L3_FRAS210115520-1r_2.fq.gz
│                   │   ├── T2GFP-N_FRAS210115521-1r
│                   │   │   ├── MD5.txt
│                   │   │   ├── T2GFP-N_FRAS210115521-1r_1.fq.gz
│                   │   │   └── T2GFP-N_FRAS210115521-1r_2.fq.gz
│                   │   ├── T2GFP-R1_FRAS210115522-3r
│                   │   │   ├── MD5.txt
│                   │   │   ├── T2GFP-R1_FRAS210115522-3r_1.fq.gz
│                   │   │   └── T2GFP-R1_FRAS210115522-3r_2.fq.gz
│                   │   ├── T2GFP-R3_FRAS210115523-2r
│                   │   │   ├── MD5.txt
│                   │   │   ├── T2GFP-R3_FRAS210115523-2r_1.fq.gz
│                   │   │   └── T2GFP-R3_FRAS210115523-2r_2.fq.gz
│                   │   ├── T3GFP-L4_FRAS210115524-2r
│                   │   │   ├── MD5.txt
│                   │   │   ├── T3GFP-L4_FRAS210115524-2r_1.fq.gz
│                   │   │   └── T3GFP-L4_FRAS210115524-2r_2.fq.gz
│                   │   ├── T3GFP-R2_FRAS210115525-2r
│                   │   │   ├── MD5.txt
│                   │   │   ├── T3GFP-R2_FRAS210115525-2r_1.fq.gz
│                   │   │   └── T3GFP-R2_FRAS210115525-2r_2.fq.gz
│                   │   └── T3GFP-R4_FRAS210115526-2r
│                   │       ├── MD5.txt
│                   │       ├── T3GFP-R4_FRAS210115526-2r_1.fq.gz
│                   │       └── T3GFP-R4_FRAS210115526-2r_2.fq.gz
│                   ├── 2.cleandata
│                   │   ├── checkSize.xls
│                   │   ├── T1GFP-L1_FRAS210115518-1r
│                   │   │   ├── MD5.txt
│                   │   │   ├── T1GFP-L1_FRAS210115518-1r_1.clean.fq.gz
│                   │   │   └── T1GFP-L1_FRAS210115518-1r_2.clean.fq.gz
│                   │   ├── T1GFP-L2_FRAS210115519-3r
│                   │   │   ├── MD5.txt
│                   │   │   ├── T1GFP-L2_FRAS210115519-3r_1.clean.fq.gz
│                   │   │   └── T1GFP-L2_FRAS210115519-3r_2.clean.fq.gz
│                   │   ├── T1GFP-L3_FRAS210115520-1r
│                   │   │   ├── MD5.txt
│                   │   │   ├── T1GFP-L3_FRAS210115520-1r_1.clean.fq.gz
│                   │   │   └── T1GFP-L3_FRAS210115520-1r_2.clean.fq.gz
│                   │   ├── T2GFP-N_FRAS210115521-1r
│                   │   │   ├── MD5.txt
│                   │   │   ├── T2GFP-N_FRAS210115521-1r_1.clean.fq.gz
│                   │   │   └── T2GFP-N_FRAS210115521-1r_2.clean.fq.gz
│                   │   ├── T2GFP-R1_FRAS210115522-3r
│                   │   │   ├── MD5.txt
│                   │   │   ├── T2GFP-R1_FRAS210115522-3r_1.clean.fq.gz
│                   │   │   └── T2GFP-R1_FRAS210115522-3r_2.clean.fq.gz
│                   │   ├── T2GFP-R3_FRAS210115523-2r
│                   │   │   ├── MD5.txt
│                   │   │   ├── T2GFP-R3_FRAS210115523-2r_1.clean.fq.gz
│                   │   │   └── T2GFP-R3_FRAS210115523-2r_2.clean.fq.gz
│                   │   ├── T3GFP-L4_FRAS210115524-2r
│                   │   │   ├── MD5.txt
│                   │   │   ├── T3GFP-L4_FRAS210115524-2r_1.clean.fq.gz
│                   │   │   └── T3GFP-L4_FRAS210115524-2r_2.clean.fq.gz
│                   │   ├── T3GFP-R2_FRAS210115525-2r
│                   │   │   ├── MD5.txt
│                   │   │   ├── T3GFP-R2_FRAS210115525-2r_1.clean.fq.gz
│                   │   │   └── T3GFP-R2_FRAS210115525-2r_2.clean.fq.gz
│                   │   └── T3GFP-R4_FRAS210115526-2r
│                   │       ├── MD5.txt
│                   │       ├── T3GFP-R4_FRAS210115526-2r_1.clean.fq.gz
│                   │       └── T3GFP-R4_FRAS210115526-2r_2.clean.fq.gz
│                   ├── checkSize.xls
│                   └── Report-X101SC21030878-Z03-J036-21.zip
└── errors.log
~~~

Then, we used the STAR as the major aligner to process the ```.fasta``` files of bulk RNA-seq. If your data was generated from Novogene,  you could directly used the clean data, ending with ```.clean.fq.gz```, for following analysis. if your data hasn't been processed with any quality control, you should use ``faatp`` to cut the adaptors and low quality reads. 

Now, we begin our alignment pipelines of bulk RNA-seq analysis.

Firstly, we should prepare a ```files``` to record the sample info, especially match the the short-name and full names of each sample. 

~~~shell
vim config.raw1
T1_L1 T1GFP-L1_FRAS210115518-1r
T1_L2 T1GFP-L2_FRAS210115519-3r
T1_L3 T1GFP-L3_FRAS210115520-1r
T2_N T2GFP-N_FRAS210115521-1r
T2_R1 T2GFP-R1_FRAS210115522-3r
T2_R3 T2GFP-R3_FRAS210115523-2r
T3_L4 T3GFP-L4_FRAS210115524-2r
T3_R2 T3GFP-R2_FRAS210115525-2r
T3_R4 T3GFP-R4_FRAS210115526-2r
~~~

 The first column is the short-name of each sample, and the second column  is the full name of each sample. And we used ```vim``` to edit the ```config.raw1```.

Before we begin the alignment of each bulk RNA-seq data, we need to generate the ```STAR index``` for the specific/matched genome reference. Here, we used the ```GRCm38`` genome as example: 

~~~R
#here, we need to assign the pathways of tools
STAR_tools=/usr/bin/STAR

#And then, we begin with STAR
$STAR_tools \
--runThreadN 20 \                          #how many cores you plan to use
--runMode genomeGenerate \                 #use to generate the genome index for specific reference
--genomeDir /mnt/data/public_data/reference/Mus/Mus_musculus_GRCm38_150bp \ #the output name and pathways of genome index
--genomeFastaFiles /mnt/data/public_data/reference/Mus/Mus_musculus_GRCm38/Mus_musculus.GRCm38.dna.primary_assembly.fa \ #the fasta file of specific genome
--sjdbGTFfile /mnt/data/public_data/reference/Mus/Mus_musculus_GRCm38/Mus_musculus.GRCm38.89.gtf \  #the .gtf file of specific genome
--sjdbOverhang 149  #the length of your sequncing data, such as pired-end 150 (PE150) of each sample, this parameter should set the 149
~~~

And then, we begin our alignment with STAR in each sample:

~~~shell
#here, we need to assign the pathways of tools, refrence and output files 
STAR_tools=/usr/bin/STAR
sequence_data_path=./CP2020120100066/H101SC21030878/RSCS3300/X101SC21030878-Z03/X101SC21030878-Z03-F027/2.cleandata
refrence=/mnt/data/public_data/reference/Mus_musculus_GRCm38_150bp
output_path=./workshop/RNAseq/RNAseq_85_WBH_20210809_9samples/

#And then, we begin our alignment in each sample
cat config.raw1  |while read id;
do echo $id
arr=($id)
path=${arr[1]}
fq2=${arr[1]}'_2.clean.fq.gz'
fq1=${arr[1]}'_1.clean.fq.gz'
sample=${arr[0]}
echo $fq2
echo $fq1
echo $sample
echo $path
$STAR_tools  --readFilesCommand zcat \       #input with .fq.gz files/
--outSAMtype BAM SortedByCoordinate \        #output files should be the bam files not sam files.
--runThreadN 20 \                            #how many cores you plan to use
--readFilesIn \
$sequence_data_path/$path/$fq1 \             #input with ._1.clean.fq.gz
$sequence_data_path/$path/$fq2 \             #input with ._2.clean.fq.gz, if you are not the paired data, you just delete this line
--genomeDir $refrence \                      #genome refence with STAR index 
--outFileNamePrefix $output_path/$sample. ;  #the pathways and names of output files
done

~~~

And the tree files you would generate as following:

~~~shell
.
├── config.raw1
├── config.raw2
├── config.raw3
├── T1_L1.Aligned.sortedByCoord.out.bam
├── T1_L1.Log.final.out
├── T1_L1.Log.out
├── T1_L1.Log.progress.out
├── T1_L1.SJ.out.tab
├── T1_L2.Aligned.sortedByCoord.out.bam
├── T1_L2.Log.final.out
├── T1_L2.Log.out
├── T1_L2.Log.progress.out
├── T1_L2.SJ.out.tab
├── T1_L3.Aligned.sortedByCoord.out.bam
├── T1_L3.Log.final.out
├── T1_L3.Log.out
├── T1_L3.Log.progress.out
├── T1_L3.SJ.out.tab
├── T2_N.Aligned.sortedByCoord.out.bam
├── T2_N.Log.final.out
├── T2_N.Log.out
├── T2_N.Log.progress.out
├── T2_N.SJ.out.tab
├── T2_R1.Aligned.sortedByCoord.out.bam
├── T2_R1.Log.final.out
├── T2_R1.Log.out
├── T2_R1.Log.progress.out
├── T2_R1.SJ.out.tab
├── T2_R3.Aligned.sortedByCoord.out.bam
├── T2_R3.Log.final.out
├── T2_R3.Log.out
├── T2_R3.Log.progress.out
├── T2_R3.SJ.out.tab
├── T3_L4.Aligned.sortedByCoord.out.bam
├── T3_L4.Log.final.out
├── T3_L4.Log.out
├── T3_L4.Log.progress.out
├── T3_L4.SJ.out.tab
├── T3_R2.Aligned.sortedByCoord.out.bam
├── T3_R2.Log.final.out
├── T3_R2.Log.out
├── T3_R2.Log.progress.out
├── T3_R2.SJ.out.tab
├── T3_R4.Aligned.sortedByCoord.out.bam
├── T3_R4.Log.final.out
├── T3_R4.Log.out
├── T3_R4.Log.progress.out
├── T3_R4.SJ.out.tab
~~~

To better visualize the ```.bam``` files in ```IGV```, you should index these ```.bam``` files or transfer them into ```.bw``` files. Here, we only showed the index steps. 

~~~shell
ls | while read id ; do
echo $id 
samtools index -@ 20 $id ;
done
~~~







