---
title: R is overtaking Python in microbes analysis
author: Anicet Ebou
date: '2019-10-26'
slug: r-is-overtaking-python-in-microbes-analysis
---

Microbial analysis using amplicon genes is a good field for data analysis. The fact that microbes are ubiquitous in our environments and that high throughput sequencing causes a data deluge are the main causes.

Crucial steps in microbial analysis are primers removing, demultiplexing, filtering and trimming of sequences, fitting of error models, sequences clustering and taxonomic assignment.

Among above steps sequences clustering and taxonomic assignments appears as the most challenging. Sequences clustering, aims to reduce inherent PCR and DNA sequencing errors. 
Traditionaly, sequences were clustered in operational taxonomic unit (OTUs) following a 97% threshold. But as recent studies ( [Callahan et al., 2016](https://www.ncbi.nlm.nih.gov/pubmed/27214047), [Amir et al., 2016](https://msystems.asm.org/content/2/2/e00191-16), [Edgar RC, 2018](https://www.ncbi.nlm.nih.gov/pubmed/29506021)) outline it, olygotyping is now the proper way to cluster amplicon sequences using an update threshold into amplicon sequences variants (ASVs).
And to do it the two major algorithms has been written in R (dada2) and Python (deblur). But Dada2 seems to be the most used.

For taxonomic assignment, the [RDP classifier](https://rdp.cme.msu.edu/classifier/classifier.jsp) which is always widely used is like the reference for classifying and it has it own implementation in R (assignTaxonomy function in dada2 package). [IdTaxa](https://microbiomejournal.biomedcentral.com/articles/10.1186/s40168-018-0521-5) that reports a better accuracy than all previous taxonomic classifier, is written in R and part of the DECIPHER package.

It's like when you want to do data analysis of biological data you are required to know the R language.

The leadership of R in microbial analysis reveal the great power of adaptation of R and it is an amazing language for manipulating biological data.
