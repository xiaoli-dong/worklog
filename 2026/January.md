
# Day-to-Day Work Log

## January 5
- Lab meeting: continued testing and validation of the **rsv-analyzer** pipeline.
- Met with **Kanti** to discuss and align on the work plan for the coming period.
- Re-familiarized myself with the RSV pipeline after the holiday.
- Set up **four RSV analysis jobs**.

## January 6
- Reviewed **rsv-analyzer** analysis outputs.
  - Identified missing fields in the summary report.
  - Debugged the pipeline and fixed the issue causing incomplete summaries.
- Walked **Anita** through visualization of **influenza segment BAM files** in **IGV**.
- Noticed an issue in **influenza segment 4** for some positive control samples:
  - High number of ambiguous bases observed in the first ~60 bp.
  - Affected samples include:
    - `251211_S_I_084-S85`
    - `251204_S_I_083-S76`
    - `251204_S_I_083-S79`

## Issues & To-Do
- Investigate the cause of ambiguous bases at the 5′ end of influenza segment 4 in positive controls.
  - Check alignment quality, primer/amplicon design, and reference sequence.
- Follow up with Anita and the team after further investigation.

## January 7

- SNAPPI meeting:
  - Vermygora Oksana presented adaptive sampling work;
  - CFIA/ACIA Theileriosis project (~90% host reads) using host + bacterial genomes for depletion;
  - assembly tools used: Flye, Canu, metaMDBG;
  - adaptive sampling webinar: https://www.youtube.com/watch?v=Rz-cg0ptuks;
  - half-flow-cell adaptive sampling configurable in MinKNOW (512 channels);
  - new Dorado release may integrate into MinKNOW with more FASTQ header info.

- Genomics technical huddle:
  - Updates on 16S NGS, RSV-analyzer testing, COVID TAT updated to 14 days in PLNA; Paul working on wastewater metagenomics; Xiaoli is about to work on 16s reporting tool, mpox pipelines, and generic virus pipelines.

- Influenza genomics meeting (BCCDC):
  - Participants included Shannon Russell, John Palmer, Jessica Caleta, John Tyson, Tarah, Matthew Croxen, Andrew Lindsay, and Kanti;
  - bccdc switched flu pipeline to nf-flu
  - no dashboard for flu data: stage-based panels and ad-hoc reports;
  - BCCDC to share report and they are building a tool to scan antiviral resistance mutations;
  - AB sequencing strategy: surveillance list, normalized by age and gender, 10 samples/week and 40 samples/month.

- nf-fluab pipeline validation: for sample 251211_S_I_084-S85 segment 4, a lot ambigious bases at the first 70bp from 5' end. IGV review confirmed consensus is right; use 251211_S_I_084-S85-bwa.rmdup.bam to do the visualization in IGV; update sent to Anita and Kanti.
- continue working on rsv-analyzer validation
## January 8
- Genomics working group meeting
- meet Anita to discuss what has happened to some of the influenza positive controls:
  - mapping visualization of some of the segment 4 is very coloful, it means a lot mismatches.
  - for me, it looks like positive control is contaminated
- work on rsv consensus comparion between different methods
- Get email from Vince that he has test pathogenseq2 and there are some errors and needs to follow up
## January 9
- working on rsv-analyzer validation

## January 12 Monday
- one day off as vacation
## January 13, Tuesday
- working on consensus comparison between different platforms
  - mafft aligned sequences from rsv-analyzer, viralrecon, viralassembly to the rsvA references:
  ```
  mafft rsvA_reference.fasta > rsvA_reference.mafft_aln.fasta
  mafft --add all_rsvA.consensus.fasta --keeplength rsvA_reference.mafft_aln.fasta > all_rsvA.consensus.mafft_aln.fasta
  snp-dists all_rsvA.consensus.mafft_aln.fasta > all_rsvA.snp-dists.tsv
  python filter_match.py  all_rsvA.snp-dists.tsv all_rsvA.snp-dists.filtered.tsv
  #split aligned sequences into files, one file per analysis
  python split_aligned.py all_rsvA.consensus.mafft_aln.fasta
  #output differences between paired samples in the format of:
  #sample  reference       250121_S_N_012_len      250121_S_N_012_len      250121_S_N_012_Ns%      250121_S_N_012_Ns% status  diff_count      percent_identity        base_differences
    s2      PP109421.1      15225   15225   42.62   39.88   IDENTICAL       0       100.00  []
    s3      PP109421.1      15225   15225   41.94   40.61   IDENTICAL       0       100.00  []
    s4      PP109421.1      15225   15225   34.66   31.47   IDENTICAL       0       100.00  []
    s5      PP109421.1      15225   15225   56.38   51.34   DIFFERENT       1       99.98   [6971:C>A]
  python compare_pairs_pisition_by_position.py 250121_S_N_012.mafft_align.fasta 250121_S_N_012_viralassembly.mafft_align.fasta 250121_S_N_012.rsv-analyzer_vs_viralassembly.diffs.tsv

  ```
  By look into the results, the consensus are similar between paired samples by skiping Ns
- metagenome meeting
  - goal to enrich dna and rna virus from waste water:
  - treatment dna, 
## January 14, Wednesday
- nanopore webinar: global lessons from metagenomics for future critical care and pathogen surveillance
- pathogenseq1.2 testing based on vince's testing results:
  - PneumoCaT failed for all the samples of the illumina testing data: this is caused by the fastq file produced by hostile, in the new version of the hostile, the output file was *_fastp_1.hostile_clean_1* and *_fastp_2.hostile_clean_2*, the sample name would be *_fastp_1 and *_fastp_2, paired reads were treated as two single reads.
## January 15, Thursday
- prepare for the rsv-analyzer meeting:
- nanopore:
  - qc petya's pipeline used chopper cutoff: 300-1200, rsv-analyzer nf-qcflow used: 200-1500
  - viralassembly pipeline
    - rsv-analyzer: min_liength 500, max_len 1500, variant_caler=clair3, model: clair3_models/r1041_e82_400bps_hac_v430
    - viralassembly default parameters: 
    '''
    // Medaka if using
    medaka_model = 'r941_min_hac_g507'
    // Clair3 if using
    clair3_model = 'r941_prom_sup_g5014'
    clair3_user_variant_model = ''
    clair3_no_pool_split = false
   '''
- illumina:
  - rsv-analyzer: fastp cutoff: len 50, avg_qual 20, nbase=0
  - I did not find  how she did qc
  
