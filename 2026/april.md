# Work Log - April 2026


## April 2, 2026

- mpox-analyzer developing
- meeting tarah to discuss mpox analysis for jamil's paper
- proble capture meeting with Ashlyn kirk and andrew cameon
  - expired reagents can be used
  - kits used in plsa: nextera, Illumina DNA Prep
  - regina using 300x sequencing , reagents: hybridization, universal blocking
  - what type of the samples: 8 samples in one pool capture, trying 12x
  - vsp panel from illumina , not working well for complex samples
  - sensentivity is viral dependent,
  - illumina has a nice ribosome kitto depelete robsom rRNA, upstream to get rid of is not working well, universal also binding to human ribosome and bind to adapter
- 
## Monday, April 6, 2026
- Group meeting: cap, quality 
- work on mpox

### Tuesday, April 7, 2026, Sunny
- mpxv-analyzer testing
- mpxv-analyzer SOP draft

### Wednesday, April 8, 2026
- sick day
### Thursday, April 9, 2027
- clean up home directories across all the nodes on the server and redirected some of the dot files to path/project/xdong
- WGU readship
  - tuition discount: full time employee, membership
  - apply for scholarships
  - accept enrollment by April 15 and finiance deadline April 22
- rsv pipeline failed because the pipeline failed to get includeConfig 'https://raw.githubusercontent.com/nf-core/configs/master/nfcore_custom.config', the ahs is blocking it. I added the following optons to the sbatch script to run the nextflow pipeline offline:
  ```
    # ======================================
    # Environment setup
    # ======================================
    # ahs is blocking download nfcore_custom.config at the run time
    # https://raw.githubusercontent.com/nf-core/configs/master/nfcore_custom.config
    #so run the pipeline in offline mode
    export NXF_OFFLINE='true'
  ```
- rsv pipeline bug fixing:
  - when making the summary report, the nextclade path for rsvB was wrong, put in "nextclade/nextclade/nextcalde.tsv" instead of "nextclade/nextclade.tsv".
  - updating the results for testing dataset under "testdata" dir
### Friday, April 10, 2026, Sunny day
- Dental
- commit the rsv bug fixes to github and updated the production directory
- meet jo to help her to clean her home dir
- todo: add both nextclade and squirrel clade and lineage assignment to the mpxv_master.tsv. may also add score
### Monday, April 13, 2026, Sunny
- dr appt
- pipeline launcher of nf-fluab problems:
  - output dir structures are not the same af fluab originally one
  - snp calling is using bcftools instead of freebayer
  - do not have fastq file renaming
  - missing nanopore procedure
- book a meeting with andrew to get the problem solved
- mpxv pipeline
- added both nextclade and squirrel clade, lineage, and score to the summary report
### Tuesday, April 14, 2026
- mainly working on refine mpxv-analyzer pipeline, merged illumina and nanopore data anlayis into one scripts
- meet Andrew to discuss about the nf-fluab intergraton into pipeline launcher. There aare some parameter issues and also missing nanopore procedure

### Wednesday, April 15 -16
- Added all the pipeline I implemented into the "Development_pipeline_update_tracker.xlsx
  - influenza
  - pcrvalidator
  - rsv-analyzer
  - mpxv-analyzer
  - artic-mpxv-illumina-nf
  - artic-mpxv-nf
  - nf-qcflow
  - nf-covflow
  - nf-viroflow
  - nf-taxflow
  - nf-assembflow
  - mev-analyzer
  - ospc-amplicon
  - nanopore-16srrna
  - human parechviruses
### Friday, April 17, 2026
- pcrValidator errors fro Jo when she was downloading covid and influenza data
  - IncompleteRead errors, NCBI connection drop
  - fix: catch exception + reduce batch size from 500 each request to 100
- Get update from Andrew about the progress of the influenza pipeline into pipeline launcher, it is done but needs more test, he is off and will work on it next Tuesday

------------
### Monday, April 20, 2026
- Jo is still having problems with covid and influenza downloading using pcrValidator
- working on testing mpxv-analyzer with following two runs, they have the same samples run on Nanopore Ligattion and Illumina Original protocol
  - 240822_S_N_132 Nanopore Ligation, barcodes 73-80
  - 240822_S_I_133 Illumina Original, barcodes 1-8
- when running artic pipeine with nanopore data, pointed the moddel directory to local directory, /nfs/APL_Genomics/db/prod/clair3/artic_models instead of "/user/local/bin/models" because it only included limited number of models with the artic singulairty image

### Tuesday, April 21, 2026
- consolidating all the mpxv FASTQ files from multiple runs into a central directory using the Excel sheet anita shared
  - For run 137 (2024), there are multiple associated directories, unsure which of these should be used for downstream processing
    - 240829_S_N_137 (~5 GB; samples 58, 59)
    - 240830_S_N_137 (~8 GB; appears to be merged data for samples 58, 59)
    - 240829_S_N_137_Repeat (samples 58, 59)
  - cannot locate  240924_S_N_148 in the 2024_RUNS directory 
  - run 240910_S_N_139, the tracker lists samples 58 and 59, but the directory contains samples 85 and 86
  - process 260420_N_I_038 with nf-taxflow
    - fixed a few bugs in nf-taxflow: when update singlem singularity img, need to regenrate database
    - updated slurm.conf file, the original one included was not working 
### Wednesday, April 22, 2026
- consolidate nf-taxflow based on matthew's test data
  - udated software version: barrnap, singlem, sylph, slyph-tax, gtdbtk, kraken2
  - update database: gtdb r226 update to r232, also assoicated ssu_rrna database, kraken2 db updated to 2026 release, sylph, sylph-tax, .
  - testing
### Thursday, April 23, 2026

#### ospc-amplicon analysis review:
The ospC (Outer Surface Protein C) gene in Borrelia burgdorferi is located on a 26-kb circular plasmid (cp26). It encodes a surface-exposed lipoprotein crucial for the transmission of Lyme disease from ticks to mammals. a semi-nested PCR approach was used to detect and genotype the ospC gene

| Primer Name               | Direction | PCR Round | Sequence (5'→3')            | Length (bp) |
|--------------------------|----------|-----------|-----------------------------|-------------|
| Bburg_ospC_For1 (OC6+)   | Forward  | Outer     | AAAGAATACATTAAGTGCGATATT    | 24          |
| Bburg_ospC_Rev1 (OC623-) | Reverse  | Outer     | TTAAGGTTTTTTTTGGACTTTCTGC   | 25          |
| Bburg_ospC_Rev2 (OC602-) | Reverse  | Nested    | GGGCTTGTAAGCTCTTTAACTG      | 22          |

The amplicon data were done qc by usign nf-qcflow and quality controlled reads (with primer at the both ends retained) then be  analyzed by [CONCOMPRA](https://academic.oup.com/bib/article/26/1/bbae642/7924278). CONCOMPRA is a tool that generates a de novo, consensus-based sequence database from Oxford Nanopore Technology (ONT) amplicon sequencing data, which is then used for abundance profiling of microbial communities. The key steps in the CONCOMPRA workflow are:

##CONCOMPRA Workflow##

- Reads outside the expected amplicon length range are discarded (500-700 bp)
- Forward reads are identified and primers are trimmed using primer-chop 
- Insertion, deletion, and substitution rates are estimated from the primer mapping 
- Top 80% of forward reads with fewest expected errors are retained using Filtlong 
- Reads are clustered based on 3mer composition using UMAP-OPTICS 
- Consensus sequences are generated from a subset of reads within each cluster using lamassemble
- Potentially chimeric consensus sequences are flagged using vsearch uchime_denovo 
- Consensus sequences are deduplicated and mapped back to the original reads to generate an abundance table (92% identity)
- Consensus sequences flagged as chimeric are removed to obtain the final, chimera-free consensus sequence table

##Why Merging consensus sequences across samples##
The CONCOMPRA workflow generates consensus sequences for each sample individually, and then merges these consensus sequences across all samples. This approach serves two key purposes:
- It allows CONCOMPRA to detect and remove "local chimeric sequences" - sequences that are chimeric within a single sample. Identifying and removing these local chimeras helps improve the accuracy of the final consensus sequence set.
- Merging the consensus sequences across samples creates a comprehensive database of unique sequences that can then be used to profile the abundance of each sequence in each sample. This allows CONCOMPRA to quantify the community composition without relying on a pre-existing reference database.

By generating consensus sequences de novo and merging them across samples, CONCOMPRA is able to characterize microbial communities without being limited by the availability or completeness of reference databases.

#### meet with kanti to discuss the ospc analysis results
  - keep current analysis strategy, kant to confirm whether those program identified consensus needs to be included to the furthur anlayis and she get back as "yes"
  - furthur analysis: split consensus to samples, phylogenetic analysis
---

### Friday, April 24, 2026 

#### reading


