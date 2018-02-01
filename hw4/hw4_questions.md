Homework #4
============

Step #1
============

First, complete the [samtools tutorial](http://quinlanlab.org/tutorials/samtools/samtools.html) and feel free to play around with the data a bit to get your feet wet.

Step #2
============
Install IGV on your laptop by following the instructions [here](http://software.broadinstitute.org/software/igv/download). I recommend using the "Java Web Start" version with 1.2Gb of RAM.
This should work nicely with both Windows and OSX. Note that if you have any problems you may need to install a newer version of Java on your computer.

Step #3
===========
Complete this excellent [IGV tutorial](https://github.com/griffithlab/rnaseq_tutorial/wiki/IGV-Tutorial)

Step #4
============
Download the sorted [BAM](https://s3.amazonaws.com/samtools-tutorial/sample.sorted.bam) and [index](https://s3.amazonaws.com/samtools-tutorial/sample.sorted.bam.bai) file from Amazon to your laptop desktop. These are the same files you created by doing the tutorial.

Question #1
=============
Using IGV, how many sequences aligned to exon 17 of the ATM gene?

*NOTE*: make sure you set the reference genome to "Human hg19"

Question #2
=============
At what position is the apparent SNP in exon 17 of the ATM gene? What allele is observed for this individual?  

Question #3
=============
Following question #2, what do you think the individual's genotype is at this site based on the alignments?  You may need to refer back to earlier lectures if you have forgotten about genotypes. 

Question #4
=============
What do you think the individual's genotype is at the SNP in following locus: chr17:41,244,367-41,244,497
What are the Phred quality scores associated with the observed alternate allele?
Does this make you more or less confident in your genotype assesment?

Question #5
=============
What is your assessment of the two apparent SNPs at this locus: chr7:55,227,815-55,228,328
Are they any less or more confident than the SNP in question #4?
What else can you say about the inheritance of these two SNPs? 

Question #6
=============
Is there a SNP at position chr7:55,224,334?  Why or why not?

    