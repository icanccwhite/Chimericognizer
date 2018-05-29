

# Chimericognizer Software



# DESCRIPTION

Chimericognizer is a novel and accurate chimeric contigs correction tool which can correct chimeric contigs and chimeric optical map molecules by each other. In Chimericognizer, the problem is modeled into weighted vertex cover problem for deciding whether to correct contigs or optical map molecules when they conflict. Experiments show that our tool can outperform manual work done by experienced researcher of genome assembly problem using much less time.
Chimericognizer is written in python. Until now, Chimericognizer can only run on Unix/Linux systems.  



# DEPENDENCY


1. python   
The majoy part of Chimericognizer is written in Python, so Python has to be installed. 
Python2.7(or above) is suggested.  

2. perl   
In Chimericognizer, we use one perl script "fa2cmap_multi.pl" of a scaffolding tool Irys-scaffolding.
So perl has to be installed.

3. RefAligner   
RefAligner is a tool developed by BioNano company to align contigs to optical maps. It is called by Chimericognizer, so it has to be installed. 
RefAligner can be found from http://www.bnxinstall.com/RefalignerAssembler/Linux/ 

4. fa2cmap_multi.pl   
In Chimericognizer, we use one perl script "fa2cmap_multi.pl" of a scaffolding tool Irys-scaffolding. 
The users need to download this script from https://github.com/i5K-KINBRE-script-share/Irys-scaffolding/blob/e8e8f177dce2bf59421bd00c517ab7dc683e25d4/KSU_bioinfo_lab/assemble_XeonPhi/third-party/fa2cmap_multi.pl
and then input it as a parameter after "-y".




# PARAMETERS


There are two kinds of parameters, functional parameters and performance related parameters. 
Functional parameters are the ones that users may have to set by themselves. For performance related parameters, many users may not be able to understand the meaning of some of them. For these users, we suggest them to use default parameters. For all the parameters about address, absolute paths are preferred. 

1. functional parameters  

-f: specify the parameter file which lists all the input paramerters.
Chimericognizer offers two ways to input parameters for users as follow:

(1) first way
e.g. 

$cd ./Chimericognizer

$python ./scripts/main.py -x /home/stelo/BIONANO_in_progress/tools/RefAligner -y /home/weihua/Chimericognizer/tools/fa2cmap_multi.pl -i /home/weihua/cowpea/fastas_cowpea_eight.txt -o /home/weihua/cowpea/eight_chimeric_multioptmap -m /home/weihua/cowpea/optmaps_for_cowpea.txt -p 32 -a 1.5 -b 1 -d 25 -e 50000 -h 50000 -r 80000

(2) second way  
e.g.

$cd ./Chimericognizer

$python ./scripts/main.py -f ~/cowpea/parameters.txt

Then in parameters.txt, the parameters are listed line by line as follows:

-x /home/stelo/BIONANO_in_progress/tools/RefAligner 

-y /home/weihua/Chimericognizer/tools/fa2cmap_multi.pl 

-i /home/weihua/cowpea/fastas_cowpea_eight.txt 

-o /home/weihua/cowpea/eight_chimeric_multioptmap 

-m /home/weihua/cowpea/optmaps_for_cowpea.txt 

-p 32 

-a 1.5 

-b 1 

-d 25 

-e 50000 

-h 50000 

-r 80000

-i: specify the fasta list file which lists the address of input fasta files line by line.  
e.g. fasta_list.txt 

/home/stelo/cowpea/falcon.fasta  

/home/weihua/cowpea/canu.fasta  

/home/weihua/cowpea/falcon.fasta  

This parameter is required to be specified by users.   

-o: specify the output dirctory which will contains all of the output files and intermediate files generated. This parameter is required to be specified by users.   

-m: specify the optical map list file which lists the type of restriction enzyme and address of all the optical maps line by line. 

e.g. optmaps_for_cowpea.txt

BspQI   /home/stelo/BIONANO_in_progress/vu_162_180K.cmap

BssSI   /home/stelo/BIONANO_in_progress/vu_bsss1_102.cmap

This parameter is required to be specified by users.   

-p: specify the number of threads. The default value is "32".  

-x: specify the address of Refaligner executable program on your machine. e.g. /home/weihua/tools/RefAligner
If you can run RefAligner in any directory of your machine by command like "RefAligner [parameters]", then you can put "RefAligner" as this parameter after "-x". Anyway, this parameter should be the first term of the command your input when you run RefAligner on your machine. The default value is "RefAligner".   

-y: specify the address of fa2cmap_multi.pl file. e.g. /home/weihua/Chimericognizer/tools/fa2cmap_multi.pl


2. performance related parameters

-a: specify threshold_1 which shows the global quality score for optical maps compared with the global quality score of contigs (threshold_2). For example, "threshold_1=1.5 and threshold_2=1" shows that the quality of optical maps are 1.5 times of the quality of contigs, which means that when a contig conflicts with an optical map, the prior probability that the contig is wrong is 1.5 times of the probability that the optical map is wrong.  The default value is "1.5". 

-b: specify threshold_2 which shows the global quality score for contigs compared with the global quality score of optical maps (threshold_1). Details could be seen in explanation of "-a". The default value is "1".   

-d: specify threshold_3 which is used in pre-stitch processing. Only the alignments whose confidence from Refaligner is larger than threshold_3 are used. The default value is "25".  

-e: specify threshold_4 which is used when we try to decide the candidate cutting sites. threshold_4 is a threshold of the length of optical map's overhang. For one alignment between optical map molecule and contig, if the left overhang of optical map is larger than threshold_4 and left overhang of contig is larger than threshold_5, then we think the left end point of this alignment is a candidate cutting site, and either optical map or contig will be cut at this site. It's same for the right side. The default value is "50000". 

-h: specify threshold_5 which is used when we try to decide the candidate cutting sites. threshold_5 is a threshold of the length of contig's overhang. The details could be seen in explanation of "-e". The default value is "50000". 

-r: specify threshold_6 which is used when merging candidate cutting sites. The candiates cutting sites whose distance smaller than threshold_6 are seen as the same site.



# OUTPUT FILES

There are two kinds of output files, result files and log files. corrected contigs and optical maps are contained in the result file, while the log files contain the information which shows the whole process of running. All of the output files are stored in output directory specified by users. There are some other intermediate files inside, which could be ignored by users.
 
1. result files

nochi_all.fasta: It's a fasta file which contains all the corrected contigs.

nochi_*_*.fasta: fasta files which contain the corrected contigs for corresponding input fasta files

nochi_optmap_*_*.cmap: corrected optical map files for corresponding input optical map files

2. log files and some intermediate files  

parameters.log: shows the values of all parameters used. 

input.log: shows all the input fasta files  

input_seqs.log: shows the id of each input contig

optmap_info.log: shows all the input optical maps

chimeric_pairs_*.log: candidate cutting sites for each optical map

vertices.log: vertex information of the graph built for vertex cover model

edges.log: edge information of the graph built for vertex cover model

cover.log: optimal cover of vertex cover model

qry_cuts.txt: the real cutting sites for contigs

ref_cuts_*.txt: the real cutting sites for optical maps

cuts_info.log: the cutting information for contigs

optmap_cut_*.log: the new optical map id corresponding to old one.  

refaligner/ directory: storing the ourput of fa2cmap_multi.pl and refaligner

alms_used.log: alignments left after pre-processing
