# Work Log - March 2026

## Week 1

### March 2–4, 2026

- Vacation

---

### Thursday, March 5, 2026

[A Guide to Sequencing for Genomic Epidemiology](https://artic.network/viruses/mpxv/artic-mpxv-guide.html)

![41392_2023_1675_Fig3_HTML](https://github.com/user-attachments/assets/2ded63c6-a0d9-4388-9228-6cf0b0536eb8)

*The genome structure and potential antiviral targets of Mpox virus. The Mpox virus genome consists of a double-stranded linear DNA comprising approximately 196,858 base pairs. It includes a central recognition region, two variable regions, and two terminal inverted repeats (ITRs) (Monkeypox virus strain Zaire, GenBank accession number: AF380138.1, [NCBI link](https://www.ncbi.nlm.nih.gov/nuccore/17529780)). In the genome map, target genes implicated in the interaction between Mpox virus and antiviral drugs are listed. Most essential genes are located in the central region of the genome.*

- **Mpox NGS data analysis — background reading**
  - Mpox virus is a large double-stranded DNA virus (~200 kb). Whole-genome sequencing (WGS) is more challenging than for small RNA viruses due to genome size, repetitive elements, and terminal regions.
  - It has a low GC content of ~33%, which results in a higher frequency of A/T homopolymeric tract regions. Nanopore R9 chemistry performs poorly at resolving homopolymeric tracts above 6 bp; R10 is improved and can reliably handle up to ~9–10 bp.
  - The MPXV genome contains large (6.5–17.5 kb) inverted terminal repeats (ITRs) at each end.
  - Large deletions are commonly observed (e.g., a large deletion distinguishes Clade I and Clade Ib). Amplicon-based methods may not easily differentiate between biological deletions and amplicon drop-outs.
  - The MPXV genome contains short tandem repeats and undergoes genome rearrangements, which pose difficulties for amplicon sequencing approaches.
  - **Sequencing and analysis strategy:**
    - Amplicon sequencing introduces synthetic primer sequences into the sequencing data that do not necessarily reflect the true sequence of the target; these must be systematically identified and removed from analysis. It is therefore critical that the correct primer scheme and reference genome are specified, or results may be incorrect.
    - A common and pervasive problem with amplicon sequencing is laboratory contamination with PCR amplification products, which can render sequencing results unreliable as sequences may carry over between samples. This is most commonly observed in samples with low genome copy numbers (high Ct), or in regions with amplicon drop-out due to mutations in the primer-binding site. The most common way to detect contamination is to include positive and negative controls in each sequencing batch. Positive control sequences should be verified against the expected sequence on each run, and negative controls should be generally "clean" (generally <0.1% of the average read count in real samples).
  - **Bioinformatics recommendations:**
    - For Nanopore amplicon sequence data generated with the ARTIC protocol (standard or rapid kit), the recommended tool is `fieldbioinformatics 1.4.0`.
    - For Illumina amplicon sequencing, the ARTIC team recommends [artic-mpxv-illumina-nf](https://github.com/artic-network/artic-mpxv-illumina-nf), an EPI2ME wrapper for [BCCDC's Illumina amplicon bioinformatics pipeline](https://github.com/BCCDC-PHL/mpxv-artic-nf).
    - Additional pipeline options are summarized at the PHA4GE GitHub: [mpxv-bioinfo-solutions](https://github.com/pha4ge/pipeline-resources/blob/main/docs/mpxv-bioinfo-solutions.md).
    - For phylogenetic reconstruction, it is recommended to mask genome regions likely to contain errors, particularly low-complexity and repetitive areas. [Squirrel](https://artic.network/software/squirrel) provides best-practice MPXV phylogenetics and APOBEC3 reconstruction.

- Our mpox data has previously been analyzed using pipelines and procedures originally developed by Petya. Two workflows are currently available: the viral pipeline developed by Vince and the `viralrecon` pipeline. After reviewing current recommendations and community practices, the ARTIC pipeline appears to be the preferred approach for mpox analysis. It follows a workflow similar to the COVID analysis pipelines, which would help maintain consistency and facilitate data sharing across groups. However, the original ARTIC pipelines from both BCCDC and the ARTIC repository were not running successfully due to issues with `Hostile`. A small modification was made to the `illumina.nf` script, and after adjusting a few lines, the pipeline ran successfully.

---

### Friday, March 6, 2026

- Worked on testing the ARTIC pipeline for Nanopore data.
- Met with Tarah to discuss the generic pipeline approach:
  - If a well-developed pipeline exists for an organism, we should use it to facilitate data sharing and standardize procedures. Otherwise, we will fall back to our generic pipeline.
  - For mpox, we will use the ARTIC pipeline.

---

## Week 2

### Monday, March 9, 2026

- **Group meeting**
  - Cybersecurity update.
  - Housekeeping:
    - eMag training; some STAR issues outstanding.
    - Flu Nanopore vs. Illumina: when using Nanopore, PCR is not as robust as with Illumina, resulting in lower genome completeness.
    - `fluab` is now integrated into the pipeline launcher; need to learn how to run it.
  - NGS updates:
    - The Measles pipeline is using the BCCDC workflow without modification.
    - Flu will use a newer primer scheme, which needs to be added to the pipeline.

- **ARTIC mpox Nanopore pipeline testing (continued)**
  - Testing `align_trim` parameters in the ARTIC pipeline for mpox data.

```bash
# Nanopore
align_trim --normalise 200 ./store_dir/primer-schemes/bccdc-mpox/2500/v2.3.0/primer.bed \
  --primer-match-threshold 35 --min-mapq 20 --remove-incorrect-pairs --trim-primers \
  --report fastq.alignreport.csv \
  --amp-depth-report fastq.amplicon_depths.tsv \
  < fastq.sorted.bam > fastq.primertrimmed.rg.sam

# Illumina
align_trim.py --normalise 200 primer.bed --paired --no-read-groups \
  --primer-match-threshold 35 --min-mapq 20 --trim-primers \
  --report 115_S115_L001.alignreport.csv \
  --amp-depth-report 115_S115_L001.amplicon_depths.tsv \
  < 115_S115_L001.sorted.bam 2> 115_S115_L001.alignreport.err \
  | samtools sort -T 115_S115_L001 - -o 115_S115_L001.primertrimmed.rg.sorted.bam \
  && samtools index 115_S115_L001.primertrimmed.rg.sorted.bam
```

---

### Tuesday, March 10, 2026

- **Continued testing of the ARTIC Nanopore mpox pipeline**

  - The original pipeline did not work under the `singularity` profile. Docker cannot be tested in our current environment.
  - The original configuration for the ARTIC pipeline was not functional because it did not include the `run_clair3.sh` program. The container was changed from `fieldbioinformatics` to the ARTIC pipeline container:

    ```
    # Original
    withLabel: artic {
        container = "quay.io/artic/fieldbioinformatics:1.7.3"
        memory    = { 4.GB * task.attempt }
        conda     = "bioconda::artic=1.7.3"
    }

    # Updated
    withLabel: artic {
        container = "https://depot.galaxyproject.org/singularity/artic%3A1.8.5--pyhdfd78af_0"
        memory    = { 4.GB * task.attempt }
        conda     = "bioconda::artic=1.8.5"
    }
    ```

  - Added a `singularity` profile to `nextflow.config`.
  - Added an `override_model_dir` parameter because ARTIC MinION could not access model files under the Singularity profile.
    - The default `--model-dir` is located at `$CONDA_PREFIX/bin/models/`, but the pipeline could not access this directory when running inside the container.
    - Models were downloaded manually using `artic_get_models`, and `--override_model_dir` was used to provide the directory path to the pipeline.
  - The `squirrel` version bundled in the pipeline was **1.0.12**. Several parameters described in the paper were missing, and the tree-building process crashed.
    - Updated `squirrel` to **1.3.2**, which resolved the issue.
    - Example command:

    ```bash
    squirrel all_consensus.fasta \
      -o xiaoli \
      --clade split \
      --seq-qc \
      --run-apobec3-phylo \
      --tree-file xiaoli_tree \
      --include-background \
      --outfile xiaoli_all_consensus.aln.fasta \
      --tempdir xiaoli_squirrel_tmp \
      -t 4
    ```

  - When running the pipeline, `override_model_dir` must be provided as an **absolute path**.

- **Example pipeline command:**

```bash
nextflow run ../main.nf \
  --fastq ./fastq \
  --scheme_version bccdc-mpox/2500/v2.3.0 \
  --clade split \
  -profile singularity \
  --override_model r941_prom_sup_g5014 \
  --override_model_dir /nfs/Genomics_DEV/projects/xdong/deve/examples_pipelines/artic-mpxv-nf-2.1.0/test/model \
  -resume
```

- **Developed a working version of the `artic-network/artic-mpxv-nf` pipeline:**
  - Downloaded pipeline: <https://github.com/artic-network/artic-mpxv-nf/archive/refs/tags/2.1.0.tar.gz>
  - Tested the Nanopore pipeline with updated ARTIC 1.8.9 and Squirrel v1.3.2 — runs successfully.

---

### Wednesday, March 11, 2026

- **Reading the Squirrel paper:**
  - [Pathoplexus](https://pathoplexus.org/) is a new open-source database that makes it easier for researchers and public health workers to share, review, and act on viral pathogen genomic data.
  - By default, Squirrel masks regions of the MPXV genome that are known to be challenging to assemble or align. Additional community resources provide complementary masking guidance, such as the mask file maintained by the International Pathogen Surveillance Network, hosted at [collaboratory-mpox-genomics-phylomasking](https://github.com/WHO-Collaboratory/collaboratory-mpox-genomics-phylomasking). Importantly, it is recommended that users do not automatically exclude all sites flagged by Squirrel.
  - Clade assignment within Squirrel was introduced to allow users to input data from a sequencing run containing multiple clades, without requiring manual splitting of the sequence file by clade (which could introduce user error).
  - The classifier is robust to increasing diversity well beyond within-clade diversity; however, model accuracy declines with increasing ambiguity as informative sites become missing. A 20% ambiguity cutoff is therefore recommended for accurate clade assignment, consistent with the cutoff used in Nextclade.

- **Developed a working version of the ARTIC mpox pipeline for both Illumina and Nanopore data.**
  Since the Nanopore version of the pipeline lacks QC modules and the QC modules in the Illumina pipeline are buggy, reads QC will be performed using `nf-qcflow`, with dehosting and trimming functions skipped in the Illumina pipeline.
  - **Illumina version:**
    - Downloaded the newest version of the pipeline.
    - Added a local configuration to the `conf` directory: Singularity profile configuration.
    - Added `skip_read_trimming` parameter.
    - Upgraded Squirrel to v1.3.2.
    - Configured SLURM profile.
    - Example command:

    ```bash
    nextflow run ../main.nf \
      --directory fastq \
      --scheme_name bccdc-mpox/2500/v2.3.0 \
      --outdir results \
      -profile singularity \
      --clade cladeiib \
      --prefix artic_test \
      -resume
    ```

  - **Nanopore version:**
    - Developed a working version with SLURM profile.
  - Both pipelines are located under the `deve` directory.

---

### Thursday, March 12, 2026

- Double-checked Squirrel analysis results for the Illumina test run.
  - Clade assignment for sample 116 appears uncertain:

    ```
    taxon,prediction,score,imputation_score,non_zero_ids,non_zero_scores,designated
    115_S115_L001,IIb,1.0,0.9894765164877873,IIb,1.0,
    116_S116_L001,Ib,0.8333333333333334,0.9363228699551569,IIa;Ib,0.16666666666666666;0.8333333333333334,
    ```

  - **Concern:** When running the ARTIC pipeline, clade information must be specified. If the sample is Clade Ib but Clade IIb was specified (and the corresponding reference used), the resulting consensus quality may be poor.
  - To investigate, downloaded the [representative background genome set for each clade](https://github.com/aineniamh/squirrel/blob/main/squirrel/data/background.panel.fasta) as defined in the Squirrel paper, created a Mash sketch file, then used `mash screen` to determine which clade sample 116 belongs to. Result: sample 116 belongs to Clade Ib.

    ```bash
    mash sketch -i background.panel.fasta -o background.panel.fasta.msh
    mash screen -i 0.95 -w -v 0.1 -p 6 background.panel.fasta.msh \
      fastq/116_S116_L001_R1_001.fastq.gz \
      fastq/116_S116_L001_R2_001.fastq.gz > 116.screen

    # Result:
    0.999618        992/1000        2950    0       PP601212.1      clade=CladeIb
    ```

- **Started development of the `mpox-analyzer` pipeline.**
  - Illumina version developed and tested — working.

---

### Friday, March 13, 2026

- Timesheet PP07 is due.
- Submitted a Linux account creation request for Jenna, who needs access to run the pipeline launcher.
- **Continued work on `mpox-analyzer` — Nanopore portion:**

  ```bash
  sh ../../script/mpox_nanopore_pipeline.sh samplesheet.csv results
  ```

- **Mpox recombinant strain (Clade 1b + 2b):**
  - [WHO Disease Outbreak News: Mpox recombinant virus with genomic elements of Clades Ib and IIb](https://www.who.int/emergencies/disease-outbreak-news/item/2026-DON595)
  - [Virological: Inter-Clade Recombinant Mpox Virus Detected in England in a Traveller Returned from Asia](https://virological.org/t/inter-clade-recombinant-mpox-virus-detected-in-england-in-a-traveller-recently-returned-from-asia/1015/1)
  - Downloaded the recombinant consensus [GCA_977880215.1](https://www.ebi.ac.uk/ena/browser/view/GCA_977880215.1) and tested whether Squirrel with `--clade split` can correctly classify it (lab result: Clade I).
    - Squirrel does not accept gzipped input files.
    - Squirrel crashed when the FASTA header was: `>ENA|OZ375330|OZ375330.1 Monkeypox virus isolate MPXV_UK_2025_GD25-156 genome assembly, chromosome: 1`
    - After renaming the header to `>ENA|OZ375330|OZ375330.1`, Squirrel completed successfully.
    - Result: Squirrel assigned it to Clade IIb with a low confidence score (0.5):

    ```bash
    # Command
    squirrel test.fasta \
      -o xiaoli \
      --clade split \
      --seq-qc \
      --run-apobec3-phylo \
      --tree-file xiaoli_tree \
      --include-background \
      --outfile xiaoli_test.aln.fasta \
      --tempdir xiaoli_squirrel_tmp \
      -t 4

    # Result
    taxon,prediction,score,imputation_score,non_zero_ids,non_zero_scores,designated
    ENA|OZ375330|OZ375330.1,IIb,0.5,0.9579813798631235,IIb,0.5,
    ```

    - Tested Nextclade for classification — it correctly identified the sequence as a recombinant:

    ```bash
    nextclade run test.fasta \
      --input-dataset ../illumina/nextstrain/mpox/all-clades \
      --output-all nextclade

    # Result:
    0       ENA|OZ375330|OZ375330.1 Ib/IIb          recombinant
    ```

  - **Conclusion:** Nextclade should be included in the pipeline, as Squirrel alone is insufficient for reliable clade assignment in recombinant cases.

---

### Monday, March 16, 2026

- Group meeting.
- Bioinformatics meeting.
- **`mpox-analyzer` testing:**
  - Added `--completeness` parameter to the command line. This sets the genome completeness cutoff for phylogenetic analysis and clade assignment using Squirrel and Nextclade. The recommended default cutoff is 0.8 (80%).
- Validated the new mpox pipeline.

---

### Tuesday, March 17, 2026

- **RSV-analyzer meeting:**
  - Anita will review primers to identify ways to improve drop-out amplicon efficiency.
  - Some of the original runs were amplified using Kingston's primers and may include primer 27. Anita will send the run names for further analysis.
  - Task: Process Illumina rapid protocol data and add it to the comparison Excel sheet to share with the group.

---

### Wednesday, March 18, 2026

- **Follow-up from the RSV-analyzer meeting:**
  - Delivered Illumina (rapid and original) and Nanopore (ligation and rapid) analysis comparison tables.
  - Received email: three primers were missing in the wet lab — they have been ordered. Primer 27 was already included in the bed file. Added `RSVB_750_9_RIGHT2` to the bed file:

    ```
    RSVA_750_27_LEFT,TCATAGGTGAAGGAGCAGGGAA,1
    RSVA_750_27_RIGHT,GCTGAAAACTTCATTACGTCCAGC,1
    RSVB_750_9_RIGHT2,GAGTCTCTTTTGTTTGTTTTGGT,1
    ```

  - The following runs were processed with primer pools from Kingston. Analysis requested to evaluate whether amplicon 27 for RSV-A performs well and whether amplicon 9 for RSV-B is improved:

    ```
    231219_S_N_391  ONT Ligation Kit
    231221_S_N_392  ONT Rapid Barcoding Kit
    240124_S_N_020  ONT Rapid Barcoding Kit
    240124_S_N_021  ONT Ligation Kit
    ```

---

### Thursday, March 19, 2026

- Processed RSV sequence data from runs using Kingston's primers; cleaned up bed files.
- **mpox pipeline validation:**
  - Working on summary report modules.
  - Added `covflow` to the pipeline.
- Submitted transcript to IERF for evaluation.

---

### Monday–Friday, March 23–27, 2026

- Consolidated `rsv-analyzer` and `nf-viroflow`; deployed v0.1.2 to the production directory; updated SOP.
- Continued work on the mpox pipeline.

---

### Monday, March 30, 2026

- **Group meeting:**
  - Pipeline launcher for `fluAB` is still not working.
  - `rsv-analyzer` and the partially developed `viroflow` were uploaded to the production directory; SOP was updated. Needs to be tested by another team member before switching over.
- **CAP meeting:**
  - CAP internal audit scheduled for Wednesday, May 27, 2026.
  - Bioinformatics competency procedures to be implemented:
    - Linux basics
    - SLURM basics
    - Pipeline launcher
    - Software tracker (development and production)
- **Borrelia ospC (Kevin):** Kevin stopped by to discuss his interest in three aspects of the data: strains unique to Alberta; geographic origin (not all tick species affect humans); and location data, which they have available.
- Continued consolidation of `mpox-analyzer`.

---

### Tuesday, March 31, 2026

- Continued work on the mpox pipeline.
- Helped Jenna with server access and provided an introduction to Linux basics.
