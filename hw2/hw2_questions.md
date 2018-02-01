Homework #2
============

Setup
=====
First, we need to download the FASTA file for chr22 from build 38 of the human genome.

    curl http://hgdownload.cse.ucsc.edu/goldenPath/hg38/chromosomes/chr22.fa.gz > chr22.fa.gz
    gzip -d chr22.fa.gz

Question #1
============
Recall question #5 and #6 from [homework 1](https://gist.github.com/arq5x/c0eb84bce2086fbfbe9184668ef87b31#file-hw1-md).  Create a UNIX pipeline that answers both question #5 and #6 with a _single_ command

Show your work by providing the _single_ command you used.

Question #2
============
What is the GC content of chr22? Hint: recall that your denominator should exclude all of the Ns on the chromosome.
Show your work by providing the command(s) you used.

Question #3
============
How many lines in the chr22 file have exactly 15 cytosines?
Show your work by providing the command(s) you used.

Question #4
============
How many lines in the chr22 file have >=15 cytosines? Note: checkout the -n option for the sort command.
Show your work by providing the command(s) you used.

Question #5
============
Which gene from the file used in Homework 1 has the most transcripts/isoforms? Note: checkout the -n option for the sort command.
Show your work by providing the command(s) you used.

Question #6
============
Which isform from the file used in Homework 1 has the most exons?
Show your work by providing the command(s) you used.

Question #7
============
Restriction enzymes cleave DNA molecules at or near a specific sequence of bases. For example, the HindIII (Hin D 3) enzyme cuts at the "/" in either this motif: 5'A/AGCTT3' or its reverse complement, 3'TTCGA/A5'.
How many perfectly matching HindIII restriction enzyme cut sites are there on chr22? Don't worry about sites that span two lines - just care about sites that are fully contained on a single line.

Question #8
============
How many HindIII cut sites are there on chr22, assuming that a mutant form of HindIII will tolerate a mismatch in the second position? Think about ways in which you could best test for all the possible DNA combinations. There are a few valid approaches. Don't worry about sites that span two lines - just care about sites that are fully contained on a single line.

Question #9
===========
Given the chr22 fasta file we have used for the rest of this homework, devise a conceptual strategy for using Unix commands you know of so far (and possibly a wish list of methods that you are not yet aware of) to describe how you would conduct an *in silico* digest of  chr22 using HindIII. That is, if HindIII cuts at the sites described above, how could you predict exactly the sequences and their lengths that would result from chr22?

Question #10
============
Describe in prose an approach to finding all occurences of the motif ATTCCGAATCAGGGT on chromosome 22. You also need to be able to find any motif that is one mismatch away (e.g., *T*TTCCGAATCAGGGT) from this motif.

Bonus (not req'd but worth extra points) 
=======================================================
(1 pt) 1. What is the longest consecutive simple repeat of CAG occuring on a single line in the FASTA file? Hint: you will need to do some Googling to figure out how to do more sophisticated regular expression (regex) matches with grep.
(1 pt) 2. How many distinct gaps (continuous runs of Ns) are there on chromosome 22? Show your work.