# Experimental Evoluntion Strains
Bioinformatics project on Rhizobium leguminosarum 

Work done on INFO server, node info114. 

# Questions to ask during the Monday meeting

# Step 1 - Genome Assembly using SPAdes <br>

## Before Assembly
Create symbolic link for raw reads:
```
ln -s /2/scratch/batstonelab/RltEE2020-PE_reads/*gz .
```
Check the number of files:
```
ls *.gz | wc -l
```

### 1. Run FastQC for raw data
Versions: FastQC v0.11.5

**Sample: 1_1_2**

Run FastQC:
```
nohup fastqc -o /home/xingyuan/rhizo_ee/fastQC_raw_reads 1_1_2* &
```
**All 363 samples (726 files)**
```
nohup fastqc -o /home/xingyuan/rhizo_ee/fastQC_raw_reads *gz &
```

### 2. Run MultiQC for raw data
```
multiqc . 
```
### 3. Run Trimmomatic 
Version: 0.39


**Sample: 1_1_2**
```
nohup java -jar /usr/local/trimmomatic/Trimmomatic-0.39/trimmomatic-0.39.jar PE /home/xingyuan/rhizo_ee/raw_reads/1_1_2_GAACTGAGCG-CGCTCCACGA_L002_R1_001.fastq.gz /home/xingyuan/rhizo_ee/raw_reads/1_1_2_GAACTGAGCG-CGCTCCACGA_L002_R2_001.fastq.gz /home/xingyuan/rhizo_ee/trimmomatic_reads/1_1_2_GAACTGAGCG-CGCTCCACGA_L002_R1_P_001.fastq.gz /home/xingyuan/rhizo_ee/trimmomatic_reads/1_1_2_GAACTGAGCG-CGCTCCACGA_L002_R1_UP_001.fastq.gz /home/xingyuan/rhizo_ee/trimmomatic_reads/1_1_2_GAACTGAGCG-CGCTCCACGA_L002_R2_P_001.fastq.gz /home/xingyuan/rhizo_ee/trimmomatic_reads/1_1_2_GAACTGAGCG-CGCTCCACGA_L002_R2_UP_001.fastq.gz ILLUMINACLIP:/usr/local/trimmomatic/Trimmomatic-0.39/adapters/NexteraPE-PE.fa:2:30:10:2:TRUE HEADCROP:15 CROP:130 LEADING:3 TRAILING:3 MINLEN:36 &
```
**All 363 samples (726 files)**
```
nano run_trimmomatic.sh
```
```
#!/bin/bash 
for R1 in *R1* 
do 
R2=${R1//R1_001.fastq.gz/R2_001.fastq.gz} 
R1_P=${R1//001.fastq.gz/P_001.fastq.gz} 
R1_UP=${R1//001.fastq.gz/UP_001.fastq.gz} 
R2_P=${R2//001.fastq.gz/P_001.fastq.gz} 
R2_UP=${R2//001.fastq.gz/UP_001.fastq.gz} 

java -jar /usr/local/trimmomatic/Trimmomatic-0.39/trimmomatic-0.39.jar PE /home/xingyuan/rhizo_ee/raw_reads/$R1 /home/xingyuan/rhizo_ee/raw_reads/$R2 /home/xingyuan/rhizo_ee/trimmomatic_reads/$R1_P /home/xingyuan/rhizo_ee/trimmomatic_reads/$R1_UP /home/xingyuan/rhizo_ee/trimmomatic_reads/$R2_P /home/xingyuan/rhizo_ee/trimmomatic_reads/$R2_UP ILLUMINACLIP:/usr/local/trimmomatic/Trimmomatic-0.39/adapters/NexteraPE-PE.fa:2:30:10:2:TRUE HEADCROP:15 CROP:130 LEADING:3 TRAILING:3 MINLEN:36
done
```
```
nohup bash run_trimmomatic.sh &
```

### 4. Run FastQC for trimmed reads 
**Sample: 1_1_2**

Run FastQC: 
```
fastqc -o /home/xingyuan/rhizo_ee/fastQC_trimmomatic_reads *_P_* 
```

**All 363 samples (726 files)**
```
nohup fastqc -o /home/xingyuan/rhizo_ee/fastQC_trimmomatic_reads *_P_* &
```

### 5. Run MultiQC for trimmed reads
Version: MultiQC v1.9

**All 363 samples (726 files)**
```
multiqc . 
```

## During Assembly 
### 1. Run SPAdes 
Version: 3.15.2

**Sample: 1_1_2**
```
nohup spades.py --careful -1 1_1_2_GAACTGAGCG-CGCTCCACGA_L002_R1_P_001.fastq.gz -2 1_1_2_GAACTGAGCG-CGCTCCACGA_L002_R2_P_001.fastq.gz -o /home/xingyuan/rhizo_ee/spades_assembly/1_1_2 &
```

**Samples: 52 samples from 2020 strains in Rhizobium_leguminosarum_EE2021-Single_strain_experiment Google sheets**
```
vi running-spades.sh
```
```
#!/bin/bash 
for R1 in 10_1_8_*R1_P_* 10_1_9_*R1_P_* 10_7_6_*R1_P_* 11_4_2_*R1_P_* 11_4_4_*R1_P_* 11_5_6_*R1_P_* 13_4_1_*R1_P_* 14_4_6_*R1_P_* 14_5_3_*R1_P_* 15_4_4_*R1_P_* 15_4_6_*R1_P_* 16_1_6_*R1_P_* 16_1_7_*R1_P_* 16_1_8_*R1_P_* 16_4_2_*R1_P_* 16_4_3_*R1_P_* 16_6_6_*R1_P_* 17_2_1_*R1_P_* 17_2_8_*R1_P_* 17_2_9_*R1_P_* 18_1_4_*R1_P_* 18_1_5_*R1_P_* 19_1_1_*R1_P_* 19_5_8_*R1_P_* 2_2_5_*R1_P_* 2_5_2_*R1_P_* 2_6_4_*R1_P_* 3_1_5_*R1_P_* 3_2_1_*R1_P_* 3_2_3_*R1_P_* 3_2_6_*R1_P_* 3_2_7_*R1_P_* 3_3_5_*R1_P_* 3_3_7_*R1_P_* 3_3_9_*R1_P_* 4_1_2_*R1_P_* 4_1_4_*R1_P_* 4_2_1_*R1_P_* 6_4_5_*R1_P_* 6_4_7_*R1_P_* 6_7_5_*R1_P_* 7_1_2_*R1_P_* 7_1_5_*R1_P_* 7_6_3_*R1_P_* 7_6_9_*R1_P_* 7_7_2_*R1_P_* 7_7_3_*R1_P_* 8_4_10_*R1_P_* 8_4_4_*R1_P_* 9_3_7_*R1_P_* 9_7_6_*R1_P_* 9_7_9_*R1_P_* 
do
R2=${R1//R1_P_001.fastq.gz/R2_P_001.fastq.gz}

spades.py --careful -1 $R1 -2 $R2 -o /home/xingyuan/rhizo_ee/spades_assembly/${R1%_*_L002_*gz}
done
```
```
nohup bash running-spades.sh &
```
manipulating string (useful for writing loop): https://mywiki.wooledge.org/BashFAQ/100, https://tldp.org/LDP/abs/html/string-manipulation.html

### 2. Run plasmidSPAdes
**Sample: 1_1_2**
```
nohup spades.py --plasmid -1 1_1_2_GAACTGAGCG-CGCTCCACGA_L002_R1_P_001.fastq.gz -2 1_1_2_GAACTGAGCG-CGCTCCACGA_L002_R2_P_001.fastq.gz -o /home/xingyuan/rhizo_ee/spades_assembly/1_1_2-plasmids &
```

## After Assembly 
### 1. Run Quast on scaffolds
Version: 5.2.0, 3d87c606

**Sample: 1_1_2** <br>
```
quast.py -m 0 scaffolds.fasta
```
**Samples: 52 samples from 2020 strains in Rhizobium_leguminosarum_EE2021-Single_strain_experiment Google sheets**
```
quast -m 0 scaffolds.fasta 
```

| sample | # scaffolds | total length | largest scaffold | GC (%) | N50 | N90 | auN | L50 | L90 | # N's per 100 kbp  |
| :----: | :----:    | :----:       | :---:          | :--:   | :--:|:---:|:--: |:--: |:--: | :--:               |
| 10_1_8 | 269 |  7698930 | 928458 | 60.61 | 400806 | 116845 | 436794.2 | 7 | 21 | 5.03 |
| 10_1_9 | 119 | 7377880  | 1009279 | 60.75 | 430333 | 173037 | 461108.1 | 6 | 18 |  7.94  |
| 10_7_6 | 229 | 7561496 | 966116 | 60.71 | 447903 |  86934 | 445840.7 | 7 | 19 |  5.20 | 
| 11_4_2 | 227 | 7490241 | 856458 | 60.69 | 334660 | 65380 |  364679.9 | 8 | 24 | 5.19 |
| 11_4_4 | 113 | 7526111 | 1446462 | 60.60 | 654969 | 159952 | 678186.6 | 4 | 14 |  6.52  |
| 11_5_6 | 136 | 7531401 | 1117631 | 60.60 | 590789 | 205988 | 654709.3  |  5  | 12 | 7.81 | 
| 13_4_1 | 216 | 7296160 | 1167888 | 60.75 | 500937 | 120765 | 535202.8 | 5 | 17 |  9.42 |
| 14_4_6 (has warning)| 150 | 7534545 | 1101709 | 60.60 | 654969 | 228788 | 677489.1 | 5 | 11 |  7.78 |
| 14_5_3 | 217 | 7299266 | 1165591 | 60.76 | 526898 | 120765 | 559835.2 | 5 | 15 | 9.43 |
| 15_4_4 | 274 | 7699117 | 970156 | 60.61 | 409389 |  105773  | 486538.2 | 6 | 19 | 6.34 | 
| 15_4_6 | 253 | 7654724 | 869297 | 60.62 | 445323 | 114888 | 453915.5 | 6 |  19 | 3.85 | 
| 16_1_6 | 350 | 7619799 | 899177 | 60.96 | 291640 | 50655 | 396217.0 | 8 | 28 | 2.73 |
| 16_1_7 | 270 | 7696264 | 970290 | 60.62 | 414354 | 105773 | 455952.9 | 6 | 21 | 7.68 |
| 16_1_8 | 286 | 7316283 | 1492913 | 60.70 | 517446 | 61754 | 582815.2 | 5 | 20 | 9.38 |
| 16_4_2 | 216 | 7561806 | 883671 | 60.70 | 459276 | 74856 | 424792.2 | 7 | 21 | 6.39 | 
| 16_4_3 | 425 | 7357352 | 1509109 | 60.69 | 508829 | 69558 | 586323.5 | 5 | 20 | 9.35 |
| 16_6_6 (has warning) | 
