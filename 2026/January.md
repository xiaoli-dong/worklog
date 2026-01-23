
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
- rsv-analyzer meeting:
  - nanopore:
    - qc petya's pipeline used chopper cutoff: 300-1200, rsv-analyzer nf-qcflow used: 200-1500
    - viralassembly pipeline
      - rsv-analyzer: min_len 500, max_len 1500, variant_caler=clair3, model: clair3_models/r1041_e82_400bps_hac_v430
      - viralassembly default parameters: medaka_model = 'r941_min_hac_g507', clair3_model = 'r941_prom_sup_g5014'
  - illumina:
    - rsv-analyzer: fastp cutoff: len 50, avg_qual 20, nbase=0
    - I did not find  how she did qc
  - following up:
    - do more comparison between nanopore rapid vs ligation: 231219_S_N_391_Ligation vs 231221_S_N_392_Rapid; 240124_S_N_020_Rapid vs 240124_S_N_021_Ligation; 241230_S_N_187_Rapid vs 012 samples
    - for nanopore rapid, the data contains a lot short fragmnet, in order to keep more reliabe data, we increase quality cutoff with those short sequences
  - <img height="250" alt="image" src="https://github.com/user-attachments/assets/d43ae36b-d873-4777-b106-18a750619c2d" />
- rsv-analyzer:
  - create rsv_nanopore_pipeline_for_rapid.sh: run nf-qcflow twice with different quality cutoffs
    - q15 for lengths are between 200-499
    - q10 for lengths are greater or equal to 500bp
- continue working on pathogenseq1.2: hostile process is having bugs, after generating hostile output, the rename is not working well. 

## January 16, Friday
- checked the analysis results from yestday's jobs to see whether length cutoff of 200-1500bp makes a big differences? it generated the exact same rsv_master.tsv file. I looked into the viralassembly pipeline and min_len, and max_len parameter were designed for arctic pipeline. it is not applied for our data. It is using all the data we supplied, which is the data passed qc (200-1500bp)
- setup comparison pairs kara sent to me on Thursday
## January 19, Monday
- Group meeting to do:
  - nf-fluab: need to exclude the empty consensus files from the final report
  - Linux tutorial access
  - rsv-analyzer comparision:
    - nanopore rapid protocol consistently produce short fragments, will do a paired run comparison with petya's analysis in terms of the how much good data retained in each pipeline for the rapid protcol
    - deliver the comparison matrix for them to make decisions.
- Working on finish the linux tutorial
- 
## January 20, Tuesday
- Working on linux tutorial, delivered to the group
- bioinformatics meeting:
  - stefan is working on CPO
  - andrew: improvide pathogenseq integration into pipeline launcher, nf-fluab tutorial is ready
  - stefan, silas, petya will help stefan to working on data cleaning
- continues to working on rsv data comparison:
  - compared viraassembly out vs rsv-analyzer depth and completeness profile using _250121_S_N_012 (nanopore rapid). While completeness is very close, the depth profile is quite different. the viralassembly reported much high value for a lot samples.
  - read into petya's pipeline and viralassembly pipeline. In the viralassembly pipeline, the depth profile were generated with "samtool depth" command and it is generated with "mosdepth" command in rsv-analyzer:
    ```
    mosdepth \
    --threads 6 \
    --fasta reference.fasta \
    -n --fast-mode --by 200 --use-median --thresholds 0,10,30,100,500,1000 \
    250121_S_N_012-barcode75 \
    250121_S_N_012-barcode75.bam

    ```
  - Conducted some online studies, it seems the results differences are expected especially when the data is fragmented. When sequences are short (200–500 bp), samtools depth reports higher depth than mosdepth
     
  ## January 21, Wednesday
  - why rsv-analyzer produced lower number of the mapped reads with more input for mapping when compared with petya's pipeline (viralassembly)
    - viralassembly uses "samtools view -c -F 0x904 sample.bam" command but rsv-analyzer uses "samtools stats" command. I copied one bam file over to generate the mapped reads number with both tool, they give the same number. It means the discripency was not caused by tool difference
    - read over viralassembly pipelie again, the min_length and max_length was used by "artic guppyplex" to do the length fitlering before chopper. 
    - read the rsv-analyzer code, in the pipeline, the custom config files were not correctly passed to the pipeline and the custom configurations were ignored and the defauts were used, fixed the bug
    - setup job with length cutoff of 200-1500 bp with newly updated pipeline.
  -- working on pathogenseq pipeline to replace hostile with deacon,
## January 22, Thursday 
- rsv data analysis comparison continue:
  - the analysis were hangling with all the data when setting up the minlen=200 for rsv-analyzer, it might be slrum configuration issues ( do not have enoght resouce for some of the samples), changed slurm.config to be the exact same as the one pathogenseq used.
  - use only 10 samples to do the test with rsv-analyzer (minlen=200 and maxlen=1500). the analysis finished fast and checked the results, rsv-analyzer mapped read counts are higher than petya's viralassembly  
  - so, the the mapped reads count differences were caused by the reads length cutoff (minlen=200, maxlen=1500, the defualt configuration is 200-3000, in petya's qc, she was using 300-1200
  - call a meeting to review the resulsts before sending over to the group
- nf-qcflow pipeline: tested, updated slurm.config and nextflow.config file and moved it to production directory (qcflow_pipeline)
- nf-covflow pipeline: tested, updated slurm.config and nextflow.config files. move it to production directory (covflow_pipeline)
- rsv-analyzer pipeline: move it to production directory (inside rsv directory)
## January 23, Friday
- RSV data analysis meeting (anita, kanti, jo, petya, kara)
  - the descripency between petya's pipeline and rsv-analyzer was caused by the length cutoff (500-1500 and 300-1200)
  - the original rapid data were analyzed by vince's pipeline "APL_Genomics/VL/PRL_virus_assembly_v4.sh", it gives 99% completeness but viralassembly and rsv-analyzer gives max 80% completeness. some of the amplicons get zero coverage (rsvA: 1, 8, 18, 19, 20, 21) in virassembly and rsv-analyzer
  - things to do:
    - look into vince's pipeline, figure out why?
    - look into the bam files before viralassembly variant calling
    - petya is going to process 012 samples rapid run with vince's pipeline
- bam files:
- in virusassembly pipeline, three bam files were generated for each sample step by step:
  - 250116_S_N_012-barcode02.sorted.bam
  - 250116_S_N_012-barcode02.trimmed.rg.sorted.bam
  - 250116_S_N_012-barcode02.primertrimmed.rg.sorted.bam (used to do the variant calling)

  Here is the commands for each step in viralassembly pipeline
  ```
  minimap2 \
    -a \
    -x map-ont \
    -t 6 \
    reference.fasta \
    250116_S_N_012-barcode02.processed.fastq.gz \
  | samtools view -bS -F 4 - \
  | samtools sort -o 250116_S_N_012-barcode02.sorted.bam

  samtools index 250116_S_N_012-barcode02.sorted.bam


  align_trim \
    --normalise 1000 --start \
    --remove-incorrect-pairs \
    --report 250116_S_N_012-barcode02.alignreport-start.txt \
    scheme.bed \
    < 250116_S_N_012-barcode02.sorted.bam 2> 250116_S_N_012-barcode02.alignreport-start.er | samtools sort -T 250116_S_N_012-barcode02 - -o 250116_S_N_012-barcode02.trimmed.rg.sorted.bam

  samtools index 250116_S_N_012-barcode02.trimmed.rg.sorted.bam

  align_trim \
      --normalise 1000 \
      --remove-incorrect-pairs \
      --report 250116_S_N_012-barcode02.alignreport-primers.txt \
      scheme.bed \
      < 250116_S_N_012-barcode02.sorted.bam 2> 250116_S_N_012-barcode02.alignreport-primers.er | samtools sort -T 250116_S_N_012-barcode02 - -o 250116_S_N_012-barcode02.primertrimmed.rg.sorted.bam
  
  samtools index 250116_S_N_012-barcode02.primertrimmed.rg.sorted.bam
  ```
  - visualized bam files to see whether there are any reads mapped to the references originally?, if it is yes, check which step were elimilated?
    -  it seems the "align_trim --remove-incorrect-pairs" are removing the reads in those droped regions
    -  dropped regions:
      -  amplicon_1: 1-768 (not good for all the platform)
      -  amplicon_8: 3880-4660 
      -  amplicon_18: 9015-9905 (mapping is very bad evern before align_trim for nanopore, even with "--remove-incorrect-pairs" removed, the depth is very bad, only a few depth, it is also not good in illumina run)
      -  amplicon_19: 9846-10637
      -   amplicon_20: 10480-11269 (nanopore mapping is very bad in the original mapping without any post process yet, the ilumina is better)
      -   amplicon_21: 10987-11773
      -   amplicon_26: 13883-15158 (original mapping is shallow, illumina is a little better)
    - modify the local viralassembly pipeline and disabled "--remove-incorrect-pairs" option. there is no configuration option avaiable for that tool in viralassembly pipeline
    - vince's nanopore pipeline is very simple:
      ```
      mpileupDepth=100000
      ivarMinDepth=10
      ivarQual=10
      ivarFreqThreshold=0.75
      ONTKeepLen=20
      ONTQualThreshold=20
      
      minimap2 -ax map-ont -t "${cpus}" "${ref}" "$prefix"_rk.reads.fastq.gz \
      | samtools view -F 4 -Sb \
      | samtools sort -o aligned2ref/"$prefix".sorted.bam

      samtools index aligned2ref/"$prefix".sorted.bam

      keepLen="${ONTKeepLen}"
      qualThreshold="${ONTQualThreshold}"

      samtools flagstat -O tsv aligned2ref/"$prefix".sorted.bam > "$prefix"_bamstats.tsv
      samtools mpileup -A -d "${mpileupDepth}" -Q 0 aligned2ref/"$prefix".sorted.bam \
        | ivar consensus -p consensus/"$prefix".consensus -t "${ivarFreqThreshold}" -m "${ivarMinDepth}" -q "${ivarQual}" -n N
      
      samtools coverage ./aligned2ref/"$prefix".sorted.bam -o "$prefix"_coverage.tsv

      
      
      ```
