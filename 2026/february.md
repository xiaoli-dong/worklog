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

## Friday, February 5, 2026

### RSV data analysis troubleshooting
> <sub>We were talking about where we left this off and want to get more clarity on how to follow up.We agree that it looks like some amplicons have lower amplification efficiency and thus end up with fewer reads, however, the Illumina data does show genome coverage in this region (albeit lower), so what are we removing in the nanopore data that we are retaining in the Illumina data? The main motivation behind this question is that it is not trivial to go back to the drawing board and optimize primers and PCR for full genome sequencing of RSV. If we are absolutely sure that we cannot retrieve reads for these amplicons using Viralassembly on the ONT data, we will have to go back to the drawing board.Another clarification is that when Petya mentioned that primer sequences seems “potentially modified” in a group meeting a few weeks ago, she only meant that the new primers were ordered by ProvLab, the sequences are identical to the ones given to us by Henry’s lab.Let us know your thoughts and we will decide how we will proceed on the wet lab side.</sub>
