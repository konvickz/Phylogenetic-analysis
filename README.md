# Phylogenetic-analysis
Phylogenetic analysis of opsin genes - script that I used for Balkan project

# Opsin sequence alignment preparation in Geneious
Align all sequences (MAAFT). Careful, all sequences must be DNA (i. e. they must not contain U isntead of T)!  
Use Danio rerio VA opsin as outgroup. Careful, outgroup has to be the first sequence in the alignment!  
Trim alignment according to LWS opsin (if present). At the 5' end, trim LWS exon 1. At the 3' end, trim LWS exon 6 (i. e. exon 5 for other opsins).  
Export in .nex format.

# Adjusting the NEXUS file for Mr Bayes
NEXUS file generated from Geneious has to be edited before being submitted to MrBayes.

    #NEXUS
    begin data;
    	dimensions ntax=66 nchar=1231;
    	format datatype=dna missing=N gap=-;
    	matrix
    	Danio_rerio_VA_opsin_CDS	ATGGAGTCGTTG...
