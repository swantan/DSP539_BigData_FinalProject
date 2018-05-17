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
2. Subsequent data analysis using a web-based immunoinformatics tool<br/>
   *EpiCC (Epitope Content Comparison)<br/>
   *JMX (JanusMatrix)<br/>
3. Post data processing and analysis (R)

## Folders Description
*Follow the numbering of each folders*

**Part I** <br/>
Folder: 1.dataset - A placeholder to put all original/raw files<br/>
Contents:<br/>
1. H1N1_swineIAV.FASTA
2. H1N1_swineIAV.xls
3. DNAvaccineseq.pep (data not shown)
4. vaccine_epitopes_classI.pep
5. vaccine_epitopes_classII.pep

**Part II**<br/>
Folder: 2.data_preprocess - A working folder to put all input / output files that are related to data preprocessing part<br/>
Manual to follow: 'Data Pre-processing Manual.md'<br/>
Contents:<br/>
1. R project<br/>
2. Python script - to open on Jupyter Notebook

**Part III**<br/>
Folder: 3.seq_tosubmit - A placeholder and also a working folder to put all files that are ready to be uploaded to the web-based tool

**Part IV**<br/>
Folder: 4.data_postprocess - A placeholder to put all web-based downloaded files and also serve as working folder for post data analysis<br/>
Contents:<br/>
1. R project
2. EpiCC results for Class I/II (.csv files)
3. JMX results for Class I/II (.html files)

## Result Summary
Refer to **POSTER_SwineFluProject.pdf**
