# Phylogenetic-analysis
Phylogenetic analysis of opsin genes - script that I used for Balkan project

# Opsin sequence alignment preparation in Geneious
Align all sequences (MAAFT). Careful, all sequences must be DNA (i. e. they must not contain U isntead of T)!  
Use Danio rerio VA opsin as outgroup. Careful, outgroup has to be the first sequence in the alignment!  
Trim alignment according to LWS opsin (if present). At the 5' end, trim LWS exon 1. At the 3' end, trim LWS exon 6 (i. e. exon 5 for other opsins).  
Export in .nex format.

# Adjusting the NEXUS file for Mr Bayes
NEXUS file generated from Geneious has to be edited before being submitted to MrBayes.

Edit the beginning of the file so that it looks like this:

    #NEXUS
    begin data;
    	dimensions ntax=66 nchar=1231;
    	format datatype=dna missing=N gap=-;
    	matrix
    	Danio_rerio_VA_opsin_CDS	ATGGAGTCGTTG...

Append MrBayes block to the end of the file (paste MrBayes block on a new line after end;)    
MrBayes block:

        begin mrbayes;

        	set autoclose=yes nowarn=yes;
        	lset nst=6 rates=invgamma;

        	mcmc ngen=10000000 printfreq=1000 samplefreq=1000 nchains=4 nruns=2 savebrlens=yes filename=NAME;

         	sumt filename=NAME burnin=2500 conformat=simple;
        	sump filename=NAME burnin=2500;

        end;

MrBayes block contains instructions for MrBayes software and sets parameters for the phylogenetic analysis.
Explanation of MrBayes block:

        begin mrbayes;

            set autoclose=yes nowarn=yes; #Setting general options

            lset nst=6 rates=invgamma #Model specification. Here using the GTR+I+G model
        
            mcmc #Run the MCMC analysis
                ngen=10000000 #Sets number of generations
                printfreq=1000 #Determines how often MrBayes prints progress updates to the screen (i.e., the terminal or log)
                samplefreq=1000 #Determines how often MrBayes samples trees and parameter values during the MCMC run and writes them to the output files (e.g., .p and .t files)
                nchains=4 
                nruns=2 
                savebrlens=yes #Saves lenghts of the branches
                filename=NAME; #Sets name of the output file (file with consensus tree)

            sumt #Summarizes the tree results
                filename=NAME #Sets name of the output file (run.t file)
                burnin=2500 #Discards 2500 first trees
                conformat=simple; #Specifies output as a simplified streamlined consensus tree output (the branch lengths represent the average branch lengths across the sampled trees)

            sump #Summarizes parameter estimates with a burn-in
                filename=NAME #Sets name of the output file (run.p file)
                burnin=2500; #Summarizes parameters in a couple of files after discarding 2500 first entries

            end;

# Running MrBayes on Metacentrum
Prepare new folder for each run. This folder must contain .nex file with instructions for MrBayes.    Submit the job using the following script:

        #!/bin/bash
        #PBS -N NAME.pbs
        #PBS -l select=1:ncpus=12:mem=100gb:scratch_local=100gb
        #PBS -l walltime=24:00:00

        cd path/to/your/file

        module load mrbayes/3.2.7a-intel-19.0.4-s2htn4w

        mb NAME.nex
