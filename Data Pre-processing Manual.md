# Manual for Data Pre-processing

**Data preparation**<br/>
→```mkdir 2.data_preprocess```<br/> 
→ create R project (with dataset, scrips, output folders) and Python script in your current working directory<br/> 
→ ```cp 1.dataset/H1N1_swineIAV.FASTA 1.dataset/H1N1_swineIAV.XLS 2.data_preprocess/```<br/>
→```mkdir 3.seq_tosubmit```<br/> 
→ ```cp 1.dataset/DNAvaccineseq.pep 1.dataset/vaccine_epitopes_classI.txt 1.dataset/vaccine_epitopes_classII.txt 3.seq_tosubmit/```<br/>

**Pre-processing of nucleotide sequence data**<br/>
1.	```cd 2.data_preprocess```<br/>
2.	To check how’s the sequence file look like<br/>
```head H1N1_swineIAV.FASTA```<br/>
3.	To check the total number of sequences provided<br/>
```grep ">" H1N1_swineIAV.FASTA -c```<br/>
4.	To check sequence duplication by extracting header of each sequences and load the output header information into R<br/>
```grep ">" H1N1_swineIAV.FASTA > header_ H1N1_swineIAV.csv```<br/>
```cp header_H1N1_swineIAV.csv H1N1_swineIAV_preanalysis/dataset/```<br/>
5.	R script – to check duplicates. There are 72 complete genome of H1N1 swine flu (each possess 8 protein segments, i.e. complete genome). Any strains that more than 8 count means there are duplicates, if less than 8 meaning the particular strain is of partial genome.<br/>
```cp H1N1_swineIAV_analysis/output/proteinsegment.txt ./```<br/> 
6.	Since we are looking for full genome strain, we can determine that we are considering all 576 sequences.<br/>
7.	Next is to proceed to extract all 576 sequences to 8 separate files according to their protein segments by using Python.<br/>
8.	Gather all sequences that needed to be translated<br/>
```mkdir ../seq_tosubmit/totranslate```<br/>
```mv *.fasta ../seq_tosubmit/totranslate```<br/>
9.	Translate each DNA fasta files on a [web-based translational tool](http://www.bioinformatics.org/sms2/translate.html) by copying and pasting the DNA sequences.<br/>
→ Opt for open reading fram (ORF) 1 for complete protein sequence translation and click the button ‘submit’, taking _4.fasta_ (HA protein) as an example.<br/>
→ Save the translated result as swinefluH1N1_segment4_HA.pep in  seq_tosubmit/ folder.<br/>
*Table 1. Naming format of the file after sequence translation*<br/>

| DNA fasta files | Protein sequence file (.pep) |<br/>
| --- | --- |<br/>
| 1.fasta | swinefluH1N1_segment1_PB2.pep |<br/>
| 2.fasta | swinefluH1N1_segment2_PB1.pep |<br/>
| 3.fasta | swinefluH1N1_segment3_PA.pep |<br/>
| 4.fasta | swinefluH1N1_segment4_HA.pep |<br/>
| 5.fasta | swinefluH1N1_segment5_NP.pep |<br/>
| 6.fasta | swinefluH1N1_segment6_NA.pep |<br/>
| 7.fasta | swinefluH1N1_segment7_M.pep |<br/>
| 8.fasta | swinefluH1N1_segment8_NS.pep |<br/>

10.	To remove stop codons ‘\*’ and before doing so, screen through the file by searching ‘\*’. You are expected to have a total of 72 ‘\*’ (one stop codon for one sequence in a .pep file) but if more than 72 meaning there are some sequences that are not chosen from the best ORF. Screen through every sequences in the file and inspect manually for sequences that have more than one stop codons ‘\*’, then choose other reading frame for the particular sequences and/or blast its nucleotide sequences against the respective amino acid sequences to obtain only the coding region (CDS).<br/>
 a. Take note especially for this sequence: _‘gb:MF116358’ (A/swine/Kansas/A01378027/2017)_<br/>
 Chose reading frame 3<br/> 

>\>rf 3 gb:MF116358|Organism:Influenza A virus (A/swine/Kansas/A01378027/2017(H1N1))|Strain Name:A/swine/Kansas/A01378027/2017|Segment:4|Subtype:H1N1|Host:Swine QKQGKTKATKMKAILVVLLYTFTTANADTLCIGYHANNSTDTVDTVLEKNVTVTHSVNLL EDKHNGKLCKLRGVAPLHLGKCNIAGWILGNPECESLSTASSWSYIVETSNSDNGTCYPG DFINYEELREQLSSVSSFERFEIFPKTSSWPNHDSNKGVTAACPHAGAKSFYKNLIWLVK KGNSYPKLNQSYINDKGKKVLVLWGIHHPSTTADQQSLYQNADAYVFVGTSRYSKKFKPE IATRPKVRDQEGRMNYYWTLVEPGDKITFEATGNLVVPRYAFTMERNAGSGIIISDTPVH DCNTTCQTPEGAINTSLPFQNIHPITIGKCPKYVKSTKLRLATGLRNVPSIQSRGLFGAI AGFIEGGWTGMVDGWYGYHHQNEQGSGYAADLKSTQNAIDKITNKVNSVIEKMNTQFTAV GKEFNHLEKRIENLNKKVDDGFLDIWTYNAELLVLLENERTLDYHDSNVKNLYEKVRNQL KNNAKEIGNGCFEFYHKCDNTCMESVKNGTYDYPKYSEEAKLNREKIDGVKLESTRIYQI LAIYSTVASSLVLVVSLGAISFWMCSNGSLQCRICI*H*DFR<br/>

 b. Perform [blastx](https://blast.ncbi.nlm.nih.gov/Blast.cgi?PROGRAM=blastx&PAGE_TYPE=BlastSearch&LINK_LOC=blasthome) to be certain of the sequence CDS (screenshot) which started with ‘MKAILVVLLYTF’ and ended with ‘SLQCRICI’.<br/>
![blastx result](/2.data_preprocess/blastx.png)<br/> 

 c. Then, make necessary edits to the ‘problematic’ sequence, i.e. trim away ‘QKQGKTKATK’ and ‘H\*DFR’<br/>
 Showing before and after (see attached screenshots) removing \* in ‘swinefluH1N1_segment4_HA.pep’<br/>
 **Before**<br/>
 ![sequence edit 1](/2.data_preprocess/seq_edit1.png)<br/>

 **After**<br/>
 ![sequence edit 2](/2.data_preprocess/seq_edit2.png)<br/>
11.	Proceed to removing the 72 ‘\*’ and save the file following the format in Table 1.<br/>
12.	Repeat step 8 - 10 for the remaining 7 other proteins.<br/>
13.	Next, modify and shorten the header. The reason being, there are certain criteria that has to comply for a file to be successfully uploaded to the web-based toolkit.<br/>
>The criteria are:<br/>
•	File to be uploaded should be in .pep format, in which the files were all saved as .pep according to Table 1.<br/>
•	Sequence headers should only contain two special characters, ‘/’ and ‘-’. Other characters such as ‘:’, ‘.’, ‘|’, ‘*’ etc will throw an error.<br/>
•	Sequence header should be kept short.<br/>
14.	Now, to further edit sequence headers, loop through all 8 file to make the following modification, i.e. shorten sequence header:<br/>
```cd ../seq_tosubmit/```<br/>
```{awk }
for i in 1 2 3 4 5 6 7 8; 
do for j in PB2 PB1 PA HA NP NA M NS;
do cat swinefluH1N1\_segment$i\_$j.pep | awk '/^>/{split($0,a,"|"); print a[1]"|"a[3]"|"a[4]; next}{print}' 
> swinefluH1N1\_segment$i\_$j\_v2.pep; 
done; done
```
  *```mkdir seq_foruse```<br/>
  *```mv swinefluH1N1_segment1_PB2_v2.pep swinefluH1N1_segment2_PB1_v2.pep swinefluH1N1_segment3_PA_v2.pep swinefluH1N1_segment4_HA_v2.pep swinefluH1N1_segment5_NP_v2.pep swinefluH1N1_segment6_NA_v2.pep swinefluH1N1_segment7_M_v2.pep swinefluH1N1_segment8_NS_v2.pep seq_foruse/```<br/>
  *```rm *v2.pep```<br/>
15.	```cd seq_foruse/```<br/>
16.	Manually further edit sequence header using vi editor<br/>
* ```vi <filename>```<br/>
    *For example: ```vi swinefluH1N1_segment4_HA_v2.pep```<br/>
    *Hit ‘Esc’ to enter coding mode everytime (do not hit ‘I’) and things to edit are as below:<br/>
     *```:%s/rf 1 //g```<br/>
     *```:%s/rf 3 //g```<br/>
     *```:%s/Strain Name://g```<br/>
     *```:%s/:/_/g```<br/>
     *```:%s/ //g```<br/>
     *```:%s/\//_/g```<br/>
     *```:%s/|/-/g```<br/>
    *After done all the above, hit ‘Esc’ again and type the command below to save<br/>
     *```:wq!```<br/>
17.	After done sequence header editing, proceed to submit all sequences in the folder *seq_foruse/* and *3.seq_tosubmit/DNAvaccineseq.pep* to the web-based tool.<br/>



