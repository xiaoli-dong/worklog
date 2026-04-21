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
