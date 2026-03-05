# Work Log - March 2026

## Week 1

###  March 2-4, 2026
- vacation

---

### Thursday, March 5, 2026

#### mpox analysis
[A guide to sequencing for genomic epidemiology](https://artic.network/viruses/mpxv/artic-mpxv-guide.html) 


![41392_2023_1675_Fig3_HTML](https://github.com/user-attachments/assets/2ded63c6-a0d9-4388-9228-6cf0b0536eb8)

The genome structure and potential antiviral targets of Mpox virus. The Mpox virus genome consists of a double-stranded linear DNA comprising approximately 196,858 base pairs. It consists of a central recognition region, two variable region, and two terminal inverted terminal repeats (ITRs) (Monkeypox virus strain Zaire, GenBank accession number: AF380138.1, web link: https://www.ncbi.nlm.nih.gov/nuccore/17529780). In the genome map, target genes implicated in the interaction between Mpox virus and antiviral drugs are listed. Most essential genes are located in the central region of the genome

- background reading
  - Mpox virus is a large double-stranded DNA virus (~200 kb). wgs is more challenging than for small RNA virus because of genome size, repoeats, and terminal regions
  - It has low GC content of ~33% which tranlates to a higher frequency of A/T hompolymeric tract regions. nanopore R9 chemistry is poor at resovling homopolymeric tracts above 6 bps, R10 is better and can be reliable up to around 9-10 bps
  - MPXV genome contains a large (6.5 - 17.5 kb) ITR (inverted tandem repeat) at each end of the genome
  - large deletions are commonly observed (e.g. a large deletion distinguishes Clade I and Clade Ib) amplicon methods may not easily differentiate between biological deletiions and amplicon drop-outs
  - mpxv genome contains short tandem repeats and exists genome rearrangements, which pose difficulties to amplicon sequencing approaches
  - sequencing and analyis strategy:
    - Because amplicon sequencing introduces synthetic primer sequences into the sequencing data which do not necessarily reflect the sequence of the target, these must be systematically identified and removed from analysis. So, it is absolutely vital that the correct primer scheme and reference genome are specified, or results may not be correct
    - A second, common and pervasive problem with amplicon sequencing is that laboratory contamination with PCR amplification products can render sequencing results unreliable, because sequences may spill between different samples. This is most commonly seen in low genome copy number samples (high Ct), or in regions that have dropped-out due to mutations in the primer-binding site. The most common way of detecting contamination is to ensure that positive and negative controls are included in each sequencing batch. Positive control sequences should be checked that they are the expected sequence on each sequencing run, and negative controls should be generally “clean” (the total number of reads in a negative control acceptable is hard to specify, but generally it should be <0.1% of the average number of reads in a real sample).
    - bioinformatics recommendation
      - for nanopore amplicon sequecne data generated with the ARTIC protocol or with the ARTIC protocol using the rapid kit , recommend using fieldbioinformatics 1.4.0
      - for illumina amplicon sequence, the artic team recommend using [artic-mpxv-illumina-nf](https://github.com/artic-network/artic-mpxv-illumina-nf) which is an EPI3ME wrapper for the [BCCDC's illumina amplicon bioinformatics pipeline](https://github.com/BCCDC-PHL/mpxv-artic-nf)
      - other options: An overview of some other options for bioinformatics pipelines can be found at the PHA4GE Github: [https://github.com/pha4ge/pipeline-resources/blob/main/docs/mpxv-bioinfo-solutions.md](https://github.com/pha4ge/pipeline-resources/blob/main/docs/mpxv-bioinfo-solutions.md)
      - phylogenetic reconstruction: mask the parts of genomes that are likely to contain errors, particularly low-complexity and repetitive areas. [Squirrel](https://artic.network/software/squirrel) provides best-practice MPXV phylogenetics and APOBEC3-reconstruction. 
---
