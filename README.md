# DSP539_BigData_FinalProject

## Introduction
This is a study on swine vaccination and the project collaborators need to produce an inactivated vaccine to immunize pigs. Prior to do so, the pigs have to be challenged with a virus strain in order to test the efficacy of experimental inactivated vaccine. 

By using the previous published swine DNA vaccine as reference, the collaborator intended to know which circulating swine influenza A virus (IAV) is suitable to serve as the challenge strain.

## Analysis End Goals
1. To screen H1N1 influenza A virus (IAV) sequences (provided by the collaborator) against the epitopes in the DNA vaccine, for both MHC Class I and Class II to find good matches using two approaches: Epitope Content Comparison (EpiCC) and JanusMatrix (JMX).
2. Rank the viruses to select top matches to be candidate for the challenge strain.

## Analysis Workflow 
This analysis is divided into 3 main stages:
1. Data pre-processing (R, Python)
2. Subsequent data analysis using a web-based immunoinformatics tool
   *EpiCC (Epitope Content Comparison)
   *JMX (JanusMatrix) 
3. Post data processing and analysis (R)

## Folders Description
*Follow the numbering of each folders*

**Part I** 
Folder: 1.dataset - A placeholder to put all original/raw files
Contents:
1. H1N1_swineIAV.FASTA
2. H1N1_swineIAV.xls
3. DNAvaccineseq.pep
4. vaccine_epitopes_classI.pep
5. vaccine_epitopes_classII.pep

**Part II**
Folder: 2.data_preprocess - A working folder to put all input / output files that are related to data preprocessing part 
Manual to follow: 'Data Pre-processing Manual.md' 

**Part III**
Folder: 3.seq_tosubmit - A placeholder and also a working folder to put all files that are ready to be uploaded to the web-based tool

**Part IV**
Folder: 4.data_postprocess - A placeholder to put all web-based downloaded files and also serve as working folder for post data analysis
Contents:
1. R project
2. EpiCC results for Class I/II (.csv files)
3. JMX results for Class I/II (.html files)
