# KmerBased_GenomeSurvey

## FastQC 
FastQC is a quality control tool used to assess the quality of raw sequencing data (FASTQ files). FastQC is performed before downstream analyses to identify potential sequencing issues, low-quality reads, adapter contamination and other biases. This helps determine whether the data require preprocessing (e.g. quality trimming or adapter removal) before proceeding with the analysis.
```bash
conda activate assembly
#[assembly]
fastqc SRR6746345664_1.fastq.gz SRR6746345664_2.fastq.gz
```

### Trimmomatic
Trimmomatic is a fast, multithreaded command line tool that can be used to trim and crop Illumina (FASTQ) data as well as to remove adapters. These adapters can pose a real problem depending on the library preparation and downstream application. 
```bash
#[assembly]
trimmomatic PE -threads 8 -phred33 SRR11672503_1.fastq.gz SRR11672503_2.fastq.gz SRR11672503_1_paired.fastq SRR11672503_1_unpaired.fastq SRR11672503_2_paired.fastq SRR11672503_2_unpaired.fastq ILLUMINACLIP:/opt/miniforge3/envs/assembly/share/trimmomatic-0.40-0/adapters/TruSeq3-PE.fa:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36 2> stats_trimmomatic.log
```

Where:

- 'PE' : specifies that trimmomatic should run in paired-end mode
- '-threads 20' : use 20 threads for parallel processing
- '-phred33' : type of quality score format
- the next six are input and output names that MUST be given in a specific order:
  - first input (1)
  - second imput (2)
  - first output (first input) for trimmed and paired forward reads reads (1)
  - second output (first input) for the trimmed and unpaired forward reads (those that cannot be paired) (1)
  - first output (second input) for trimmed and paired forward reads reads (2)
  - second output (second input) for the trimmed and unpaired forward reads (those that cannot be paired) (2)
- 'ILLUMINACLIP:TruSeq3-PE.fa:2:30:10'
  - path to adapters fasta;
  - seedMismatches: specifies the maximum mismatch count which will still allow a full match to be performed;
  - palindromeClipThreshold: specifies how accurate the match between the two ‘adapter ligated’ reads must be for PE palindrome read alignment;
  - simple clip threshold: the minimum score of the full alignment between adapter andcd ../ read for the clipping to take place
- 'LEADING:3' #Removes bases from the beginning of a read if their quality score is below 3 or N.
- 'TRAILING:3' #Removes bases from the end of a read if their quality score is below 3 or N.
- 'SLIDINGWINDOW:4:15'
  - windowSize: specifies the number of bases to average across;
  - requiredQuality: specifies the average quality required.
- 'MINLEN:36': Discards reads shorter than 36 bases after trimming.
- '2> stats_trimmomatic': standard error redirection to a file named stats_trimmomatic

For a detailed description of the available parameters and their functions, please refer to the Trimmomatic [manual](http://www.usadellab.org/cms/uploads/supplementary/Trimmomatic/TrimmomaticManual_V0.32.pdf)

**Post-trimming quality control:** After trimming, it is recommended to run FastQC again on the trimmed FASTQ files to assess whether read quality has improved and to verify that adapter contamination and low-quality bases have been successfully removed. This step helps confirm that the preprocessing was effective before proceeding with downstream analyses.
