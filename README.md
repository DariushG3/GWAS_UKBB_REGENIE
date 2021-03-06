# GWAS_UKBB_REGENIE
Running GWAS on Kidney's latent phenotypes via REGENIE v1.0.6.7.

- In qcUKBB.sh you'll find the commands for format conversion of UKBB genotype file (BGEN) using plink.

- Fo Step 1 of GWAS via REGENIE, we need a merged file contating all the genotypes from autosomes (chromosome 1 to 22) in one single file in BGEN or plink{.bed/.bam/.fam) formats. 

- To prepare such file, we are using plink to to convert the BGEN files to plink and meanwhile impose our criteria for filtering SNPs based on their Minor Allele Frequency (MAF) and Allel Count (AC).

- Before merging the converted plink files, we need to store their names in text file, here called "myfilesnames.txt".

- Then, we use plink2 to merged the QC-ed plink files. This merged file consisting all the the autosomes genotypes is going to be used for step1 of GWAS using REGENIE where we need to find the most important SNPs across all SNPs of the study to make a prediction file for Step 2 og GWAS.

- File format conversion using plink2 was unsuccessful! Plink couldn't merge multiallelic SNPs positions using genotype input in plink format. 

- I now move on by taking the alternative approach which utilizes bgenix tool to convert the genotype files from BGEN to VCF file. One can also makes use of plink2 to conduct the conversion if they don't have bgenix installed on their Unix machine. (As in parallel I tested BGEN to VCF format conversion via Plink2 -also filtered SNPs based on MAF/MAC- and it worked too.)

- Additionally, I directly piped the converted file in VCF format to vcftools for pruning SNPs by MAF/MAC, and then to bcftools for reheadering file. So, they all worked well.

- It's been around one week and the submitted slurm-jobs with 4 seperate CPUs and mem-per-cpu=16Gb are still being done on the EURAC servers!!! This just file format conversion and initial SNPs pruning (QC process)!!! GWAS is remained!!!  :|
- I'm dubious wheather bgenix could have correctly converted the bgen files to VCF, as I have found small number of SNPs remained for some of the CHRs after filtering SNPs (MAF05/AC10)! I'm now trying again to convert BGEN->VCF with Plink2 and compare the generated VCF file with the bgenix VCF file for the same CHR in terms of size, number of SNPs, and etc. 
- Persumabley the CHRs have been correctly converted to VCF file using Plink2.
- Now we can merge the QCed VCF files and run the first step of REGENIE GWAS. First I need to check and test the VCFmerge function for CHR21 and CHR22. If it worked, I would stick to it and proceed for Step 1 of REGENIE GWAS.

## DNAnexus

- The UK Biobank dataset is a uniquely rich resource containing over 10 petabytes of genetic and health data from over 500,000 volunteer participants. With the launch of RStudio Workbench Trial Version on the Research Analysis Platform, approved UK Biobank researchers can now analyze this extensive dataset using their programming language of choice.

- Experts from RStudio, UK Biobank and DNAnexus walk through using RStudio Workbench to analyze the extensive UK Biobank dataset, available to all UK Biobank researchers free of charge until August 31. This webinar provides an overview on how to create notebooks, dashboards and incorporate Shiny apps all in the cloud on the Research Analysis Platform. -> https://www.youtube.com/watch?v=iy22sxlj5Ik