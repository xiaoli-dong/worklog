# Work Log - March 2026

## Week 1

###  March 2-4, 2026
- vacation

---

### Thursday, March 5, 2026


[A guide to sequencing for genomic epidemiology](https://artic.network/viruses/mpxv/artic-mpxv-guide.html) 


![41392_2023_1675_Fig3_HTML](https://github.com/user-attachments/assets/2ded63c6-a0d9-4388-9228-6cf0b0536eb8)

The genome structure and potential antiviral targets of Mpox virus. The Mpox virus genome consists of a double-stranded linear DNA comprising approximately 196,858 base pairs. It consists of a central recognition region, two variable region, and two terminal inverted terminal repeats (ITRs) (Monkeypox virus strain Zaire, GenBank accession number: AF380138.1, web link: https://www.ncbi.nlm.nih.gov/nuccore/17529780). In the genome map, target genes implicated in the interaction between Mpox virus and antiviral drugs are listed. Most essential genes are located in the central region of the genome

- mpox NGS data anaysis background reading
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

- Our mpox data has been analyzed using pipelines and procedures originally put together by Petya. At the moment, two workflows are available: the viral pipeline developed by Vince and the viralrecon pipeline.
After reviewing current recommendations and community practices, the ARTIC pipeline appears to be the preferred approach for mpox analysis. It follows a workflow similar to the COVID analysis pipelines, which would help maintain consistency and make it easier to compare and share results across groups. However, the original ARTIC pipelines from both BCCDC and the ARTIC repository were not running successfully due to issues related to Hostile. I made a small modification to the illumina.nf script, and after adjusting a few lines the pipeline was able to run successfully.
---

### Friday, March 6, 2026

- worked on testing artic pipeline for nanopore data
- met Tarah to discuss about the generic pipelne:
  - if a well developed pipeline exists for an organism, we can use it for easy data sharing, standadize the procedure. if not , we will use our generic pipeline
  - for the mpox, we will use artic pipeline

## Week 2

### Monday, March 9, 2026

- **Group meeting**
  - Cyber security
  - Housekeeping:
    - eMag training; some STAR issues
    - Flu Nanopore vs Illumina: when using Nanopore, PCR is not as robust as with Illumina, resulting in lower genome completeness
    - `fluab` is now integrated into the pipeline launcher; need to learn how to run it
  - NGS updates:
    - Measles pipeline is using the BCCDC workflow without modification
    - Flu will use a newer primer scheme, which needs to be added to the pipeline

- **ARTIC mpox Nanopore pipeline testing (continued)**
  - Testing `align_trim` parameters in the ARTIC pipeline for mpox data

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

### Tuesday, March 10, 2026 (Sunny)

- **Continued testing of the ARTIC Nanopore mpox pipeline**

  - The original pipeline did not work under the `singularity` profile. Docker cannot be tested in our current environment.
  - The orginal configuration for artic pipeline is not working because it did not contain run_clair3.sh program and I changed it to artic pipeline container instead of filedbioinformatics
    ```
    # original one
    withLabel: artic {
        container = "quay.io/artic/fieldbioinformatics:1.7.3"
        memory    = { 4.GB * task.attempt }
        conda     = "bioconda::artic=1.7.3"
    }

    # new one
    withLabel: artic {
        container = "https://depot.galaxyproject.org/singularity/artic%3A1.8.5--pyhdfd78af_0"
        memory    = { 4.GB * task.attempt }
        conda     = "bioconda::artic=1.8.5"
    }
    ```
    I 
  - Added a `singularity` profile to `nextflow.config`.
  - Added an `override_model_dir` parameter because ARTIC MinION could not access the model files under the Singularity profile.
    - The default `--model-dir` is located at:
      ```
      $CONDA_PREFIX/bin/models/
      ```
    - However, the pipeline could not access this directory when running inside the container.
    - Downloaded the models manually using artic_get_models and used `--override_model_dir` to provide the directory path to the pipeline.
  - The `squirrel` version in the pipeline was **1.0.12**. Some parameters described in the paper were missing, and the tree-building process crashed.
    - Updated `squirrel` to **1.3.2**, which resolved the issue.
    - **Example command**
    
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

- **Example command**

```bash
nextflow run ../main.nf \
  --fastq ./fastq \
  --scheme_version bccdc-mpox/2500/v2.3.0 \
  --clade split \
  -profile singularity \
  --override_model r941_prom_sup_g5014 \
  --override_model_dir /nfs/Genomics_DEV/projects/xdong/deve/examples_pipelines/artic-mpxv-nf-2.1.0/test/model
-resume 
```
- Develope a working version of the /artic-network/artic-mpxv-nf pipeline
- download pipeline: https://github.com/artic-network/artic-mpxv-nf/archive/refs/tags/2.1.0.tar.gz
- tested the nanopore pipeine with new updated artic and squirrel software and it works successfuly
