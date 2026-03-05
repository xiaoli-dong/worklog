# Work Log - February 2026

## Week 1 (Feb 2-7)

### Monday, February 2, 2026
- Sick day

---

### Tuesday, February 3, 2026

#### Meetings & Discussions
- **9:00 AM - McMaster University 16S rRNA Discussion** (Rubayet, Mohammad Hasan)
  - Challenges discussed:
    - Both Sanger and NGS experiencing issues
    - Consistency concerns (flowcell selection and reusability)
    - Biomass requirements: minimum four barcodes needed
  - Their approach:
    - Direct sample sequencing with native barcoding
    - Targeting V1-V8 regions
  - Data interpretation:
    - Organism identification based on relative read abundance across samples
    - Requires expert knowledge for call determination
    - Need to establish NGS review board
  - Nanopore applications: contaminated samples, pathogen detection, sensitivity optimization

#### Administrative
- Submitted sick day form for Monday
- Filed vacation requests:
  - February 17 (Phoenix trip)
  - February 27 - March 4 (Mexico trip)

#### Project Work
- **nf-taxflow pipeline**: Added barnarp tool and configured Matthew's bacterial classification request
- **Brollie data processing**: Reprocessed with updated parameters from Friday meeting
  - Requirements: primers on both ends
  - Length cutoff: 500-600 bp and 500-700 bp ranges

---

### Wednesday, February 4, 2026

#### SNAPPI Meeting - Mark Unger Presentation: Legionella WGS
**Timeline & Context**
- 2015: WGS research began
- 2021: WGS integrated into routine workflow
- 2023: GRDI-funded WGS for Legionella subtyping
- Transmission: primarily environmental to human (minimal human-to-human)

**LD Outbreak Investigation Approaches**
- Current gold standard: Sequence-based typing (SBT) - 7 loci (flaA-pilE-asd-mip-mompS-proA-neuA)
- New WGS methods: cgMLST, in silico SBT (Illumina and Nanopore compatible)

  <img width="450" alt="cgMLST comparison" src="https://github.com/user-attachments/assets/b053a401-74af-463e-9976-5f7ec3caef36" />
  
  <img width="450" alt="In silico SBT results" src="https://github.com/user-attachments/assets/78b5ddcc-90fd-4002-902e-567b5dcd055c" />

- Tools developed: diphtOscan

**Technical Findings**
- Rapid barcoding: not performing well
- Native barcoding: achieving single genome assembly

**Clinical Metagenomics**
- Sanger limitation: 500 bp, cannot handle mixed samples

  <img width="450" alt="Clinical metagenomics comparison" src="https://github.com/user-attachments/assets/9e63462e-7910-438a-b86e-42e9d18929d3" />

  <img width="450" alt="Workflow diagram" src="https://github.com/user-attachments/assets/cd380db3-d4e9-4f6d-8ff9-01886a552fee" />

  <img width="450" alt="Results visualization" src="https://github.com/user-attachments/assets/d0b09495-4866-49a5-9c8c-f3ed99fcc238" />

- MAB114 kit: full-length 16S rRNA with rapid chemistry

#### Genomics Huddle
- 16S rRNA discussion, MAB114 kit
- Wastewater metagenomics update (Kanti): run underperformed, failed to detect COVID

#### Project Work
- Continued OSPC data analysis pipeline refinement with modified parameters

---

### Thursday, February 5, 2026

#### Literature Review
- **Paper**: [Fitness estimates from experimental infections predict the long-term strain structure of a vector-borne pathogen in the field](https://www.nature.com/articles/s41598-017-01821-1)
  - Key findings on ospC gene classification:
    - ospC major groups (oMGs) show highly discrete genetic variation
    - oMGs differ by ≥8% DNA sequence between groups
    - Within-group variation: <2%
    - oMG alleles are robust biological categories, resistant to sequencing errors
    - Clustering protocol threshold (93-98% similarity) doesn't affect unique oMG count

#### Pipeline Development
- **ospC analysis parameter optimization**:
  - Both-end primers required
  - Length range: `minlen=500`, `maxlen=700`
  - Merge consensus: `0.92`
  - Reads consensus: `100`
  - Modified Concompra to copy locally identified chimeras from temp directory to results

#### Data Analysis
- Analyzed TB assembled genomes for Matt using nf-taxflow pipeline

---

### Friday, February 6, 2026

#### RSV Data Analysis Troubleshooting
> <sub>We were talking about where we left this off and want to get more clarity on how to follow up. We agree that it looks like some amplicons have lower amplification efficiency and thus end up with fewer reads, however, the Illumina data does show genome coverage in this region (albeit lower), so what are we removing in the nanopore data that we are retaining in the Illumina data? The main motivation behind this question is that it is not trivial to go back to the drawing board and optimize primers and PCR for full genome sequencing of RSV. If we are absolutely sure that we cannot retrieve reads for these amplicons using Viralassembly on the ONT data, we will have to go back to the drawing board. Another clarification is that when Petya mentioned that primer sequences seems "potentially modified" in a group meeting a few weeks ago, she only meant that the new primers were ordered by ProvLab, the sequences are identical to the ones given to us by Henry's lab. Let us know your thoughts and we will decide how we will proceed on the wet lab side.</sub>

---

## Week 2 (Feb 9-13)

### Monday, February 9, 2026

#### New Project Setup - nf-viroflow
- Viralassembly from NML not working with RSV data due to align_trim functions
- Need downstream analysis tool to answer Kanti's questions for RSV data
- **Storage issue**: APL_Genomics and Gev directories filled up
  - Investigation revealed: RSV analysis with `minlen=200` used 30 TB of space in consensus files
  - Root cause unknown

---

### Tuesday, February 10, 2026
- Continued work on nf-viroflow pipeline

---

### Wednesday, February 11, 2026

#### Viroflow Development
- Implemented variant calling with Clair3
- Implemented vcf_post_process

#### Documentation
- Provided ospC analysis methods section to Tarah and Kanti for Kevin's conference paper abstract:

> <sub>Nanopore ospC amplicon reads were analyzed using the in-house ospc-tiller workflow: (1) Adapter trimming and quality filtering with fastplong (Chen, 2023) with command line options: "--length_required 500 --length_limit 700 --mean_qual 10". (2) Dehosting against tick genome NW_024609835.1 using Deacon (Constantinides et al., 2025). (3) Primer filtering to only retain reads with primers at both ends using Cutadapt (Martin, 2011) and in-house Python scripts. (4) High-quality, clean reads were analyzed with CONCOMPRA (Stock et al., 2024): "MIN=500; MAX=700; MERGE_CONSENSUS=0.92; READ_CONSENSUS=100". (5) Non-chimeric consensus sequences generated with CONCOMPRA were classified using BLASTn searches against a locally curated ospC typing database with command line options "-evalue 0.01 -perc_identity 90".</sub>

**References**

**fastp**
- Chen S. (2023). Ultrafast one-pass FASTQ data preprocessing, quality control, and deduplication using fastp. *iMeta* 2: e107. https://doi.org/10.1002/imt2.107

**Cutadapt**
- Martin M. (2011). Cutadapt removes adapter sequences from high-throughput sequencing reads. *EMBnet.journal* 17(1): 10-12. https://doi.org/10.14806/ej.17.1.200

**Deacon**
- Constantinides B, Lees J, Crook DW. (2025). Deacon: fast sequence filtering and contaminant depletion. *bioRxiv* 2025.06.09.658732. https://doi.org/10.1101/2025.06.09.658732

**CONCOMPRA**
- Stock W, et al. (2024). Breaking free from references: a consensus-based approach for community profiling with long amplicon nanopore data. *Briefings in Bioinformatics* 26(1). https://doi.org/10.1093/bib/bbae660

---

### Thursday, February 12, 2026
- Timesheet due
- CPLHN Genomics working group meeting
- Continued viroflow pipeline development

---

## Week 3 (Feb 16-20)

### Monday, February 16, 2026
- Family Day long weekend

---

### Tuesday, February 17, 2026
- Vacation in Phoenix

---

### Wednesday, February 18, 2026

#### Back to Work
- Refreshed and resumed nf-viroflow pipeline work
- Pipeline verification - checking each step for proper output:
  - Minimap: working
  - align_trim: appears not to mask or trim primers properly
- Received feedback from Vince about pathogenseq2 errors - need to investigate

---

### Thursday, February 19, 2026

#### nf-viroflow - Primer Masking Investigation
- Investigated why primers were not being soft-masked

**align_trim (new version) - Key Findings:**
- **BED file format**: 7-column format; 7th column contains primer sequences
- **Primer sequences usage**: Only used for assigning reads to correct amplicon (based on primer pair)
- **Soft-masking/trimming mechanism**: Performed using amplicon coordinates, NOT actual primer sequence
- **Read end handling**: Bases outside amplicon coordinates are trimmed/soft-masked, even if primer is in middle of read
- **Padding parameter** (`-p`): Allows fuzzy matching for reads with adapters/barcodes
  - Default `-p 35` too aggressive for our Nanopore data
  - Changed to `-p 5`
  
> <sub>This controls how far outside of an amplicon boundary an alignment can be while still being considered a match to the primer. This is useful to control for instances where reads are not adapter/barcode trimmed (or incompletely trimmed) and these extra bases have some degree of homology with the reference leading to alignments extending beyond the amplicon boundary.</sub>

---

### Friday, February 20, 2026
- Continued nf-viroflow development
- Step-by-step parameter verification from mapping to consensus calling

---

## Week 4 (Feb 23-27)

### Monday, February 23, 2026

#### Group Meeting
- Asked Petya about mpox processing - she uses viralrecon, same approach as RSV data

#### RSV Workflow Development
- Working on RSV workflow with newly implemented viroflow pipeline
- **Issue identified**: `make_summary_report.py` failing because Nextclade seqName doesn't match `chromosome_coverage_depth_summary.tsv`

---

### Tuesday, February 24, 2026

#### RSV-Analyzer Integration
- Working on integrating viroflow into rsv-analyzer
- **Primer discrepancy**: Henry Wong's paper supplementary table lists 27 primer pairs, but BED file contains only 26 pairs
- **Coordinate issues**: Kingston's BED file coordinates slightly off from actual BAM file coordinates
  - Switched to PLSA BED file - coordinates more consistent with BAM coordinates
- **Reanalysis**: Nanopore ligation samples with viroflow

**Amplicon read counts by barcode:**

| Barcode | RSVA_1 | RSVA_17 | RSVA_18 | RSVA_20 | RSVA_26 |
|---------|--------|---------|---------|---------|---------|
| barcode02 | 0 | 37 | 2 | 6 | 3 |
| barcode03 | 0 | 145 | 9 | 15 | 19 |
| barcode04 | 0 | 558 | 9 | 27 | 239 |
| barcode05 | 0 | 23 | 1 | 1 | 3 |
| barcode06 | 0 | 834 | 19 | 50 | 577 |
| barcode07 | 0 | 484 | 2 | 66 | 3 |
| barcode09 | 0 | 430 | 9 | 43 | 23 |
| barcode29 | 1 | 139 | 61 | 118 | 170 |
| barcode30 | 0 | 4 | 1 | 2 | 7 |
| barcode31 | 0 | 500 | 11 | 21 | 319 |
| barcode33 | 0 | 645 | 13 | 97 | 43 |
| barcode35 | 0 | 674 | 6 | 96 | 16 |
| barcode37 | 0 | 8 | 0 | 0 | 0 |
| barcode39 | 0 | 5 | 0 | 1 | 2 |
| barcode57 | 0 | 2 | 0 | 2 | 0 |
| barcode68 | 0 | 0 | 0 | 0 | 2 |
| barcode69 | 0 | 1743 | 41 | 105 | 619 |
| barcode70 | 0 | 750 | 4 | 129 | 18 |
| barcode71 | 0 | 956 | 15 | 153 | 24 |
| barcode72 | 0 | 974 | 124 | 131 | 26 |
| barcode73 | 0 | 431 | 2 | 51 | 19 |
| barcode74 | 0 | 32 | 34 | 105 | 89 |
| barcode76 | 0 | 1427 | 85 | 91 | 47 |
| barcode77 | 213 | 160 | 693 | 1609 | 2714 |
| barcode78 | 0 | 90 | 7 | 85 | 45 |
| barcode79 | 0 | 0 | 0 | 0 | 0 |
| barcode80 | 0 | 904 | 51 | 74 | 40 |
| barcode81 | 0 | 1786 | 146 | 206 | 44 |
| barcode82 | 0 | 78 | 1 | 6 | 15 |
| barcode83 | 0 | 755 | 105 | 101 | 20 |
| barcode84 | 1 | 408 | 73 | 59 | 21 |
| barcode85 | 0 | 180 | 1 | 23 | 5 |
| barcode86 | 0 | 0 | 0 | 0 | 0 |
| barcode87 | 0 | 94 | 15 | 14 | 3 |
| barcode88 | 1 | 470 | 96 | 67 | 24 |
| barcode90 | 0 | 59 | 40 | 160 | 119 |
| barcode91 | 0 | 696 | 16 | 42 | 59 |
| barcode92 | 1 | 2245 | 135 | 352 | 71 |
| barcode94 | 0 | 584 | 9 | 29 | 212 |

- **Viralrecon parameter update**: Ensured duplicates not filtered out, used options `-ff UNMAP,SECONDARY,QCFAIL`

---

### Wednesday, February 25, 2026
- Reanalyzed Illumina original sequence data with modified parameters `-ff UNMAP,SECONDARY,QCFAIL`
- **Note**: Viralrecon only accepts 6-column BED files

---

### Thursday, February 26, 2026

#### RSV-Analyzer Summary
- Merged summary tables from:
  - Illumina original
  - Nanopore ligation
  - Nanopore rapid

**Key Findings:**
- Illumina and Nanopore ligation: Much deeper sequencing depth compared to Nanopore rapid
- Nanopore rapid completeness: Slightly lower but acceptable
  - Lower completeness mainly caused by dropout of specific amplicons (1, 18, and 26)
- **Amplicon 1 observations**: 
  - All samples show zero depth
  - Exception: Sample 77 has good depth

---
