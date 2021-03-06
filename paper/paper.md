---
title: 'Visualizing genome synteny with xmatchview'
tags:
 - Genome visualization
 - DNA sequence alignments
 - Smith Waterman Alignments
 - cross_match
 - xmatchview
authors:
 - name: René L. Warren
   orcid: 0000-0002-9890-2293
   affiliation: 1
affiliations:
 - name: BC Cancer Agency, Genome Sciences Centre, Vancouver, BC, Canada
   index: 1
date: 6 December 2017
bibliography: paper.bib
---

# Summary

In genomics research, the visual representation of DNA sequences is of prime importance. When displayed with additional information, or tracks, showing the position of annotated genes, alignments of sequence of interest, etc., these displays facilitate our understanding of genome and gene structure, and become powerful tools to assess the relationship between various sequence data. They can be used for troubleshooting sequence assemblies, in-depth sequence analysis, and eventually find their way in publications and oral presentations as they often translate complex and abundant data succinctly, with esthetically pleasing images. Here, I introduce xmatchview and xmatchview-conifer, two python applications for comparing genomes visually and assessing their synteny.
	In bioinformatics, daily use of ENSEMBL (https://www.ensembl.org) [@hubbard2002], the UCSC genome browser (https://genome.ucsc.edu) [@Karolchik2002], and IGV [@Robinson2011] for visualizing such data is routine. The former allows for an easy-to-use visual navigation of the ENSEMBL genome databases. The latter two are customizable and flexible tools that can be used to situate sequence [read] alignments within a genome reference or draft assembly context, either online (UCSC) or as a stand-alone tool (IGV). These tools are also useful to bioinformatics software development and debugging code as well as in *de novo* genome sequencing projects as they are incredibly effective for troubleshooting sequence assemblies. Circos, a highly cited stand-alone visualization tool, represents information and associated relationships as concentric circles and ribbons, allowing for abundant data (eg. human genome scale) to be depicted succinctly in full, within a computer screen window [@Krzywinski2009]. The success of circos is attributable in part due to its flexibility, versatility and customization in representing complicated relationships between data of all sorts, not just genomics. As attractive and convenient as circles are for displaying relationships between data, linear representations of synteny blocks between two DNA sequences remain intuitive.
	Here, I introduce xmatchview (https://github.com/warrenlr/xmatchview), a tool for visualizing DNA sequence alignments produced by cross_match (unpublished, http://www.phrap.org/), a robust implementation of the sensitive Smith-Waterman algorithm for DNA alignments. The software requires python and the python imaging library (PIL) to produce publication-ready images in a variety of formats (PNG, TIFF, GIF and JPEG) and cross_match for performing the DNA alignments. With xmatchview, users can compare any two DNA sequences, including but not limited to gene reconstructions, genome assemblies, cDNA, nanopore reads, etc and visually 1) identify collinear blocks, 2) assess the relationship between them, 3) analyze the sequence identity between repeated segments, and 4) view their frequency at given coordinates.

![alt text](https://raw.githubusercontent.com/warrenlr/xmatchview/master/paper/images/Fig1.png)
**Figure 1. Genome sequence synteny between _E. coli_ strains O157:H7 str. Sakai and K12 str. MC4100 (Genbank accessions BA000007.2 and HG738867.1)**. The alignment was done with cross_match (options _-minmatch_ 29 _-minscore_ 59 _-masklevel_ 101) and rendered with xmatchview (options _-m_ 10 _-b_ 10 _-r_ 10 _-c_ 2000 _-a_ 200). The genomes of these two _E. coli_ strains are largely co-linear, with the exception of a large inversion seen in one relative to the other. Although both strains comprise unique genomic sequences, strain O157:H7 has longer sequence stretches (up to 100 kb) not found in the K12 strain. Open reading frames in both genomes are displayed in yellow.

As seen on Fig. 1, the xmatchview display consists of three main components: 1) The sequence objects, represented as black rectangles. Additional features such as exons, coding sequences, mRNA, ORFs, etc., are provided to xmatchview via a simple tab-separated file enumerating each start and end coordinates with, optionally, the color of each feature (plotted as yellow rectangles by default). Stretches of Ns in the reference and query sequences, when applicable, are shown in red on the sequence objects. 2) Relationships between co-linear block of sequences are represented by blue and pink polygons between the two black rectangles/sequence objects, depending on their direct or inverted associations, respectively. 3) A histogram on top of the reference sequence (upper most) black rectangle shows the sequence identity (top to bottom, from 0 to 100%) with the query sequence (lower rectangle). When a sequence is repeated, the color of the histogram changes to reflect its frequency. Visualizing repeat frequency is a feature unique to xmatchview that can be used to readily assess sequence complexity between (Fig. 1) or within (Fig. 2) sequences.

![alt text](https://raw.githubusercontent.com/warrenlr/xmatchview/master/paper/images/Fig2.png)
**Figure 2. Sequence repeats within the human TP53 gene (Genbank accession U94788.1)**. The alignment was done with cross_match (options _-minmatch_ 29 _-minscore_ 59 _-masklevel_ 101) and displayed with xmatchview (options _-m_ 20 _-b_ 10 _-r_ 10 _-c_ 10 _-a_ 200). TP53 mRNA sequence coordinates within the gene are shown with yellow rectangles.

When the same sequence is given as input to xmatchview, internal repeats within that sequence are shown instead, representing only the reference sequence and the relationships between repeated blocks as arcs, instead of polygons (Fig. 2). Images from xmatchview have been used in a number of peer-reviewed publications to showcase genome co-linearity and/or highlight differences between DNA sequences [@Bakkeren2006] [@DSouza2011] [@Warren2013] [@Coombe2016]. A modified version developed specifically for comparing conifer sequences with an evergreen tree representation, xmatchview-conifer, is co-released with xmatchview. The conifer tree representation differs from that of xmatchview. In the former, the sequence identity is color-coded within each synteny block relationship (light to dark shades of green tracking with increased sequence conservation), instead of a histogram (Fig. 3). Typically, sequences less than 10 Mbp in length are compared with cross_match and displayed in less than a few minutes using these pipelines, depending on your system. Both xmatchview and xmatchview-conifer are implemented in python and released under GPLv3.

![alt text](https://raw.githubusercontent.com/warrenlr/xmatchview/master/paper/images/Fig3.png)
**Figure 3. Sequence comparisons of the flowering locus gene FTL1 in selected conifer species of the order Pinales**. The FTL1 gene for Sitka spruce (_Picea sitchensis_, Genbank accession KT263970.1) was compared to that of the A) Norway spruce (_Picea abies_, Genbank accession JN039333.1), B) Japanese black pine (_Pinus thunbergii_, Genbank accession KT263877.1) and C) Chinese mountain tree - yin shan (_Cathaya argyrophylla_, Genbank accession KT263875.1). The alignment was done with cross_match (options _-minmatch_ 5 _-minscore_ 10 _-masklevel_ 101) and rendered with xmatchview-conifer (options _-m_ 99 _-b_ 10 _-r_ 1 _-c_ 2 _-l_ FLT1 _-a_ 200). The position of exons is indicated by the black rectangles outside on the outer edge of the tree representation. When comparing the FTL1 gene between the most distinct species (B and C comparisons), FTL1 sequence conservation (85-94% sequence identity) is mostly seen between exons.

# Funding

This work has been partly supported by the National Human Genome Research Institute of the National Institutes of Health (under award number R01HG007182). Additional funds were received through Genome Canada, Genome Quebec, Genome British Columbia and Genome Alberta for the Spruce-Up (243FOR) project (www.spruce-up.ca). The content reported here is solely the responsibility of the author, and does not necessarily represent the official views of the National Institutes of Health or other funding organizations.

# References
