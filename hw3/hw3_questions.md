Homework #3
============

Installing new software!!!
===========================
One of the biggest barriers for new genomics researchers is the confusion and complexity associated with installing new pieces of software to run on UNIX platforms.  The basic workflow is as follows:

    # Download the code for the software with curl or wget, or git.
    # Compile the code into an executable (a.k.a. binary) program
    # Copy the executable (e.g., samtools, bedtools, etc.) into a directory that is in your PATH
    
To get your feet wet, we will install both [seqtk](https://github.com/lh3/seqtk) and [bioawk](https://github.com/lh3/bioawk) for use in this homework. First let's create a new directory in your home directory called "src". This is where we will download and compile all of the software we download. Like "bin" for your binaries, "src" is a traditional name for the directory we create for storing custom software installations.

    cd ~
    mkdir src
    cd src
    
###Installing seqtk

Download the source code from github (this is where most software is hosted now) using the `git` command.

    git clone https://github.com/lh3/seqtk

This creates a new directory called "seqtk". Navigate into it.
        
    cd seqtk
    
Now we _compile_ the source code for the seqtk binary with the `make` command, which runs a series of instructions for building the program that are outlined in the file called "Makefile" in the seqtk directory. You may see some warnings. You can ignore them.
    
    make
        
This should have created a new binary called "seqtk". Let's check.
    
    ls
       
Now copy this binary to the bin directory in your home directory.  To copy files in UNIX we use `cp`, where the convention is cp FROM TO:
    
    cp seqtk ~/bin/
       
You should now be able to run `seqtk` from any directory since the "~/bin" directory is in your PATH.
    
    seqtk
    
###Installing bioawk

The pattern is the exact same - we are just getting source code from a different "repository" on Github.

    cd ~/src
    git clone https://github.com/lh3/bioawk
    cd bioawk
    make
    cp bioawk ~/bin/


Homework setup
===========================
There is a new directory in your home directory with the prosaic name "hw3". Therein lies real live FASTQ files resulting from an Illumina paired end sequencing run of genomic DNA from C. elegans ([details](http://www.ebi.ac.uk/ena/data/search?query=SRR3750603)). Since this is from a paired-end sequencing run, there are two files --- one for each end of each DNA fragment. As such, the two files are named SRR3750603_1.fastq and SRR3750603_2.fastq. 

    # for reproducibility:
    curl ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR375/003/SRR3750603/SRR3750603_1.fastq.gz | head -n 1000000 > SRR3750603_1.fastq.gz
    curl ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR375/003/SRR3750603/SRR3750603_2.fastq.gz | head -n 1000000 > SRR3750603_2.fastq.gz
    gunzip SRR3750603_1.fastq.gz &
    gunzip SRR3750603_2.fastq.gz &

Conveniently, the sequence for each end of each fragment is consistently ordered in each file. For example, let's look at the first 12 lines (3 sequences as each sequence record occupies 4 lines) of each file:

    $ head -n 12 SRR3750603_1.fastq
    @SRR3750603.1 HWI-D00372:204:HA3E2ADXX:1:1101:1384:2116/1
    NTCGGAACATTTTTTCTTCAAAAATATGAAAAATCACCTAATTTATCTGAAAATGACATTTANNNCAGTNNNNNNNATTGGGAAAGTGCTCGATTTNCGGA
    +
    #0<FFFFFFFFFFIIIIIIFFIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII###00<B#######00<BBFFFFFBFFFFFFFFF#00BB
    @SRR3750603.2 HWI-D00372:204:HA3E2ADXX:1:1101:1290:2192/1
    TGTAATTTACTTTGTTCAGTTAGACTCTTAATTAGACTAAAAACGGTCTCAAAAAGTATAATTTCATAATGAGACACCTTTAAAAATTCTACGTTTTTATG
    +
    BBBFFFFFFBFFFFIIIIIIIIFFFIFFIIFIIIIIFFFFIIFFFIIFFIIIIFIIFFFIFFFIFIIIIIIIIIFFFFFFFFFFFFFFBBFBBBFFFFFFF
    @SRR3750603.3 HWI-D00372:204:HA3E2ADXX:1:1101:1256:2198/1
    AGTTTTCTCAAACACAGAAAACATATGGGAGTTTCTCAAACAATGGACAATGAGTGATCACCGATATTTGATACAAATCGACCAACTCGGCTCATATTCTC
    +
    BBBFFFFFFFFFFFBFFFFIIFFFIIIIIFFFFFBFFFFFFFIIIIIFFIIIIIIIIIFFFFFFIFFFFFFFFFBFFFBBFB<<BBB7<B7B7BBBFF<B<
    
    $ head -n 12 SRR3750603_2.fastq
    @SRR3750603.1 HWI-D00372:204:HA3E2ADXX:1:1101:1384:2116/2
    TCTNNAATAAAATTTATTTCAGCTGGCATATTTGTGAAAATTTTCCGAAAATCGAGCACTTTCCCAATTATTTCAACTGAAATAAATGTCATTTTCAGATA
    +
    <<<##0<BBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBB<BBBBBBBBBBBBBBBBBB<B<BBBBBBBBB<<<<<<<
    @SRR3750603.2 HWI-D00372:204:HA3E2ADXX:1:1101:1290:2192/2
    TTTGAAAAATAACTTTTAAGAAAACCTCTTTCCTCTATTAGTAAGGTAATTTAACGATTTTAAGAAAACGTTAAAACCAATTCTCTAAAAAAATTTCAAAA
    +
    BBBFFFBFFFFBFFFIIFFIIIFFB<BFFFFFFFBFFFIFIFIFIIIIIIIIIFBFFFIIIFFIIIFFFFFIIFFFB<FBBF<BBBFBFBB<<BFFBBBBB
    @SRR3750603.3 HWI-D00372:204:HA3E2ADXX:1:1101:1256:2198/2
    TTGATTTTTTCATAAATAATTTACAATCTATCACATGAACCTTTGAACAAAAAGAAGCGCTCCTGGAGAATATGAGCCGAGTTGGTCGATTTGTATCAAAT
    +
    BBBFFFFFFFBFFFFFFFFFIIIFFFIFFFIFFFFFFFIFFFFFIFFBBBFIIIIIIBFBFBFFFFBFFFFFFFFFFBBBFFBFFFBBBFFFFFFFBBBFB

Notice that aside from the /1 and /2 the sequence IDs for each record are identical in each file, indicating that they came from the two 5' ends of the same clonally amplified clusters on the Illumina flowcell.  This makes life easier for us --- remember, sorting is good!


Now, let's get to the fun part. Please note that many of these questions will need to be answered with a mixture of standard UNIX commands you have learned thus far, but also with the [seqtk](https://github.com/lh3/seqtk) and [bioawk](https://github.com/lh3/bioawk) toolkits that we just installed. Please consult their documentation websites and help menus (e.g., just type `seqtk` then Enter) for ideas of how to answer the questions. Note that seqtk has many subcommands (e.g., `seqtk comp`) and one can get further information about what the subcommands do and what their output means by typing the subcommand followed by -h (e.g., `seqtk comp -h`).


Question #1
===========
If you wanted to find all SNPs in a baby's genome with the fewest errors, what modern DNA sequencing technology would you choose?  Why?

Question #2
===========
You sequence 1000 genomes from individuals in Tibet and 1000 individuals in Hawaii.  You observe 10,000 SNPs where the alternate allele is present on 40% of Tibetan chromosomes, yet only 1% of Hawaiian chromosomes.  What processes could explain this observation?

Question #3
===========
How does a mutation become a polymorphism?

Question #4
===========
For giggles, imagine we raised a bunch of money to sequence TA Jon's genome.  We find all the genetic variation in his genome and compare it to all the known genetic variants in the human population and find that 1,356 variants observed in Jon's genome have never been observed before.  Are they all de novo mutations in his genome?  Why or why not?

Question #5
===========
How many paired end sequences resulted from this run? Show your work.

Question #6
===========
How many nucleotides were sequenced in total for this experiment? Show your work.

Question #7
===========
What is the overall GC content of the FASTQ files? How does it compare to the GC content of the C. elegans genome (you will need to look this up). Show your work.

Question #8
===========
How does the average Phred quality score for the first read (sequence) position compare to the average Phred quality score for the last position? Why is this? Show your work.

Question #9
============
Is the length of every sequence in the FASTQ files the same? Show your work.

Question #10
============
What is the longest mononucleotide run in any sequence? Show your work.

Question #11
============
In class we talked about the fact that PCR amplification can cause dupicate DNA fragments (PCR duplicates) in the resulting sequencing data. How many PCR duplicates do you find in these data? Note that a PCR duplicate would have to be identified by inspecting both ends of each sequenced fragment. What fraction of the sequenced fragments were PCR duplicates? Hint: you will need to use bioawk for this question.

Note that while not strictly necessary to answer this question, the UNIX `paste` command may come in handy. While it can do many different things (http://www.theunixschool.com/2012/07/10-examples-of-paste-command-usage-in.html), but it's in simplest form `paste` places the lines from two files side by side one another:

    $ cat a.txt
    yuge
    loud
    head
    
    $ cat b.txt
    small
    quiet
    tail
    
    $ paste a.txt b.txt
    yuge    small
    loud    quiet
    head    tail




