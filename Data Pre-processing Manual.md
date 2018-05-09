# Manual for Data Pre-processing

Dataset: H1N1_swineIAV.FASTA, H1N1_swineIAV.xls, DNAvaccineseq.pep, vaccine_epitopes_classI.pep, vaccine_epitopes_classII.pep

mkdir 2.data_preprocess → create R project and Python script in your current working directory → put H1N1_swineIAV.FASTA, H1N1_swineIAV.XLS in this newly created folder

mkdir 3.seq_tosubmit → put DNAvaccineseq.pep, vaccine_epitopes_classI.txt, vaccine_epitopes_classII.txt

```cd 2.data_preprocess```
Pre-processing of nucleotide sequence data
1.	To check how’s the sequence file look like
head H1N1_swineIAV.FASTA
2.	To check the total number of sequences provided
grep ">" H1N1_swineIAV.FASTA -c
3.	To check sequence duplication by extracting header of each sequences and load the output header information into R
grep ">" H1N1_swineIAV.FASTA > header_ H1N1_swineIAV.csv
cp header_H1N1_swineIAV.csv H1N1_swineIAV_analysis/dataset/
4.	R script – to check duplicates. There are 72 complete genome of H1N1 swine flu (each possess 8 protein segments, i.e. complete genome). Any strains that more than 8 count means there are duplicates, if less than 8 meaning the particular strain is of partial genome.
cp H1N1_swineIAV_analysis/output/proteinsegment.txt ./ 
5.	Since we are looking for full genome strain, we can determine that we are considering all 576 sequences.
6.	Next is to proceed to extract all 576 sequences to 8 separate files according to their protein segments by using Python.
7.	Gather all sequences that needed to be translated
mkdir ../seq_tosubmit/totranslate
mv *.fasta ../seq_tosubmit/totranslate
8.	Translate each DNA fasta files on a web-based translational tool (http://www.bioinformatics.org/sms2/translate.html) by copying and pasting the DNA sequences. Opt for open reading fram (ORF) 1 for complete protein sequence translation and click the button ‘submit’, taking 4.fasta (HA protein) as an example. Save the translated result as swinefluH1N1_segment4_HA.pep in  seq_tosubmit/ folder.
Table 1. Naming format of the file after sequence translation.
DNA fasta files	Protein sequence file (.pep)
1.fasta	swinefluH1N1_segment1_PB2.pep
2.fasta	swinefluH1N1_segment2_PB1.pep
3.fasta	swinefluH1N1_segment3_PA.pep
4.fasta	swinefluH1N1_segment4_HA.pep
5.fasta	swinefluH1N1_segment5_NP.pep
6.fasta	swinefluH1N1_segment6_NA.pep
7.fasta	swinefluH1N1_segment7_M.pep
8.fasta	swinefluH1N1_segment8_NS.pep

9.	To remove stop codons ‘*’ and before doing so, screen through the file by searching ‘*’. You are expected to have a total of 72 ‘*’ (one stop codon for one sequence in a .pep file) but if more than 72 meaning there are some sequences that are not chosen from the best ORF. Screen through every sequences in the file and inspect manually for sequences that have more than one stop codons ‘*’, then choose other reading frame for the particular sequences and/or blast its nucleotide sequences against the respective amino acid sequences to obtain only the coding region (CDS).
Take note especially for this sequence: ‘gb:MF116358’ (A/swine/Kansas/A01378027/2017)
Chose reading frame 3 
>rf 3 gb:MF116358|Organism:Influenza A virus (A/swine/Kansas/A01378027/2017(H1N1))|Strain Name:A/swine/Kansas/A01378027/2017|Segment:4|Subtype:H1N1|Host:Swine QKQGKTKATKMKAILVVLLYTFTTANADTLCIGYHANNSTDTVDTVLEKNVTVTHSVNLL EDKHNGKLCKLRGVAPLHLGKCNIAGWILGNPECESLSTASSWSYIVETSNSDNGTCYPG DFINYEELREQLSSVSSFERFEIFPKTSSWPNHDSNKGVTAACPHAGAKSFYKNLIWLVK KGNSYPKLNQSYINDKGKKVLVLWGIHHPSTTADQQSLYQNADAYVFVGTSRYSKKFKPE IATRPKVRDQEGRMNYYWTLVEPGDKITFEATGNLVVPRYAFTMERNAGSGIIISDTPVH DCNTTCQTPEGAINTSLPFQNIHPITIGKCPKYVKSTKLRLATGLRNVPSIQSRGLFGAI AGFIEGGWTGMVDGWYGYHHQNEQGSGYAADLKSTQNAIDKITNKVNSVIEKMNTQFTAV GKEFNHLEKRIENLNKKVDDGFLDIWTYNAELLVLLENERTLDYHDSNVKNLYEKVRNQL KNNAKEIGNGCFEFYHKCDNTCMESVKNGTYDYPKYSEEAKLNREKIDGVKLESTRIYQI LAIYSTVASSLVLVVSLGAISFWMCSNGSLQCRICI*H*DFR

and also perform blastx (https://blast.ncbi.nlm.nih.gov/Blast.cgi?PROGRAM=blastx&PAGE_TYPE=BlastSearch&LINK_LOC=blasthome) to be certain of the sequence CDS (screenshot)
 
which is started with ‘MKAILVVLLYTF’ and ended with ‘SLQCRICI’.
Then, make necessary edits to the ‘problematic’ sequence, i.e. trim away ‘QKQGKTKATK’ and ‘H*DFR’
Showing before and after (see attached screenshots) removing * in ‘swinefluH1N1_segment4_HA.pep’
Before
 

After
 
10.	Proceed to removing the 72 ‘*’ and save the file following the format in Table 1.
11.	Repeat step 8 - 10 for the remaining 7 other proteins.
12.	Next, modify and shorten the header. The reason being, there are certain criteria that has to comply for a file to be successfully uploaded to the web-based toolkit. 
The criteria are:
•	File to be uploaded should be in .pep format, in which the files were all saved as .pep according to Table 1.
•	Sequence headers should only contain two special characters, ‘/’ and ‘-’. Other characters such as ‘:’, ‘.’, ‘|’, ‘*’ etc will throw an error.
•	Sequence header should be kept short.
13.	Now, to further edit sequence headers, loop through all 8 file to make the following modification, i.e. shorten sequence header:
$cd ../seq_tosubmit/
$for i in 1 2 3 4 5 6 7 8; 
do for j in PB2 PB1 PA HA NP NA M NS; 
do cat swinefluH1N1\_segment$i\_$j.pep | awk '/^>/{split($0,a,"|"); print a[1]"|"a[3]"|"a[4]; next}{print}' 
> swinefluH1N1\_segment$i\_$j\_v2.pep; 
done; done
$mkdir seq_foruse
$mv swinefluH1N1_segment1_PB2_v2.pep swinefluH1N1_segment2_PB1_v2.pep swinefluH1N1_segment3_PA_v2.pep swinefluH1N1_segment4_HA_v2.pep swinefluH1N1_segment5_NP_v2.pep swinefluH1N1_segment6_NA_v2.pep swinefluH1N1_segment7_M_v2.pep swinefluH1N1_segment8_NS_v2.pep seq_foruse/
$rm *v2.pep
14.	cd seq_foruse/
15.	Manually further edit sequence header using vi editor
$vi <filename>, for example 
$vi swinefluH1N1_segment4_HA_v2.pep
Hit ‘Esc’ to enter coding mode everytime (do not hit ‘I’) and things to edit are as below:
:%s/rf 1 //g
:%s/rf 3 //g
:%s/Strain Name://g
:%s/:/_/g
:%s/ //g
:%s/\//_/g
:%s/|/-/g
after done all the above, hit ‘Esc’ again and type the command below to save
:wq!
16.	After done sequence header editing, proceed to submit all sequences in the folder seq_foruse/ and seq_tosubmit/DNAvaccineseq.pep to the web-based tool.



