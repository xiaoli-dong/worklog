
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
    - the original rapid data were analyzed by vince's pipeline "APL_Genomics/VL/PRL_virus_assembly_v4.sh", it gives 99% completeness but rsv-analyzer gives max 80% completeness. 
    - things to do:
      - look into vince's pipeline, figure out why?
      - look into the bam files before viralassembly variant calling to see whether there are any reads mapped to those zero depth region. if there are, why they were filtered out?
      - petya is going to process 012 samples rapid run with vince's pipeline
  
  ### virusassembly pipeline:
    - three bam files were generated for each sample step by step:
    ```
      250116_S_N_012-barcode02.sorted.bam
      250116_S_N_012-barcode02.trimmed.rg.sorted.bam (supplied to variant calling
      250116_S_N_012-barcode02.primertrimmed.rg.sorted.bam
    ```
   
    - The commands to generate bam files
    
      ```
      minimap2 -a -x map-ont -t 6 reference.fasta 250116_S_N_012-barcode02.processed.fastq.gz \
      | samtools view -bS -F 4 \
      | samtools sort -o 250116_S_N_012-barcode02.sorted.bam
      
      samtools index 250116_S_N_012-barcode02.sorted.bam
      
      # *trimmed.rg.sorted.bam generated , align version 1.2.4
      align_trim --normalise 1000 --start --remove-incorrect-pairs --report 250116_S_N_012-barcode02.alignreport-start.txt scheme.bed \
        < 250116_S_N_012-barcode02.sorted.bam 2> 250116_S_N_012-barcode02.alignreport-start.er | samtools sort -T 250116_S_N_012-barcode02 - -o 250116_S_N_012-barcode02.trimmed.rg.sorted.bam
      samtools index 250116_S_N_012-barcode02.trimmed.rg.sorted.bam
    
      # *primmertrimmed.rg.sorted.bam generated
      align_trim \
          --normalise 1000 \
          --remove-incorrect-pairs \
          --report 250116_S_N_012-barcode02.alignreport-primers.txt \
          scheme.bed \
          < 250116_S_N_012-barcode02.sorted.bam 2> 250116_S_N_012-barcode02.alignreport-primers.er | samtools sort -T 250116_S_N_012-barcode02 - -o 250116_S_N_012-barcode02.primertrimmed.rg.sorted.bam
      
      samtools index 250116_S_N_012-barcode02.primertrimmed.rg.sorted.bam
      ```
    - variant calling
      ```
      run_clair3.sh --bam_fn=250116_S_N_012-barcode02.trimmed.rg.sorted.bam \
          --bed_fn=tiling_region.bed \
          --ref_fn=reference.fasta \
          --threads=8 \
          --platform='ont' \
          --model_path="$MODEL_PATH" \
          --output="250116_S_N_012-barcode61-out" \
          --min_coverage=10 \
          --haploid_precise \
          --enable_long_indel \
          --fast_mode \
          --include_all_ctgs \
          --no_phasing_for_fa
      ```
    - visualized bam files to see whether there are any reads mapped to the references originally?, if it is yes, check which step were elimilated?
      - there are reads mapped to several zero depth regions but there were filtered out when using align_trim tool to trim off the primers
        The follwoing is one of the example output and the sample was sequenced by nanopore ligation:
      
        ```
        # depth profiles using raw mapping files without primmer region and primer region trimmed bam in viralassembly. align_trim version 1.2.4
        #amplicon_mosdepth.coverage for 250116_S_N_012-barcode02 
        chrom   start   end     region  coverage_original_bam        coverage_primertrimmed_bam        coverage_trimmed_bam_v1.8.5 sample
        PP109421.1      0       768     RSVA-750_1      0        0      0  250116_S_N_012-barcode02
        PP109421.1      622     1360    RSVA-750_2      557     531     532  250116_S_N_012-barcode02
        PP109421.1      1182    1965    RSVA-750_3      1183    1168    1001  250116_S_N_012-barcode02
        PP109421.1      1849    2590    RSVA-750_4      2329    1398    1005  250116_S_N_012-barcode02
        PP109421.1      2299    3041    RSVA-750_5      1324    1271    1002  250116_S_N_012-barcode02
        PP109421.1      2930    3669    RSVA-750_6      368     315     351  250116_S_N_012-barcode02
        PP109421.1      3258    3992    RSVA-750_7      369     318     351  250116_S_N_012-barcode02
        PP109421.1      3880    4660    RSVA-750_8      822     0       806  250116_S_N_012-barcode02
        PP109421.1      4432    5193    RSVA-750_9      1477    1274    1175  250116_S_N_012-barcode02
        PP109421.1      4927    5675    RSVA-750_10     1241    171     1171  250116_S_N_012-barcode02
        PP109421.1      5234    6034    RSVA-750_11     1237    171     1171  250116_S_N_012-barcode02
        PP109421.1      5794    6565    RSVA-750_12     1262    619     1237  250116_S_N_012-barcode02
        PP109421.1      6212    6985    RSVA-750_13     1253    622     623  250116_S_N_012-barcode02
        PP109421.1      6901    7651    RSVA-750_14     3114    2000    1001  250116_S_N_012-barcode02
        PP109421.1      7350    8106    RSVA-750_15     1698    1501    1003  250116_S_N_012-barcode02
        PP109421.1      8053    8928    RSVA-750_16     495     323     319  250116_S_N_012-barcode02
        PP109421.1      8624    9367    RSVA-750_17     38      37      37  250116_S_N_012-barcode02
        PP109421.1      9015    9902    RSVA-750_18     1       0       0  250116_S_N_012-barcode02
        PP109421.1      9846    10637   RSVA-750_19     399     0       392  250116_S_N_012-barcode02
        PP109421.1      10480   11269   RSVA-750_20     409     0       4  250116_S_N_012-barcode02 
        PP109421.1      10987   11773   RSVA-750_21     1802    1168    1004  250116_S_N_012-barcode02
        PP109421.1      11542   12314   RSVA-750_22     2977    2000    1996  250116_S_N_012-barcode02
        PP109421.1      12029   12807   RSVA-750_23     2863    1501    1001  250116_S_N_012-barcode02
        PP109421.1      12614   13386   RSVA-750_24     703     700     700  250116_S_N_012-barcode02
        PP109421.1      13243   13995   RSVA-750_25     858     846     847  250116_S_N_012-barcode02
        PP109421.1      13883   15158   RSVA-750_26     4       1       0  250116_S_N_012-barcode02
        PP109421.1      14082   14816   RSVA-750_27     4       1       0  250116_S_N_012-barcode02
    
        ```
       
     
        - modify the local viralassembly pipeline and disabled "--remove-incorrect-pairs" from align_trim commnad becasue there is no configuration option avaiable for that tool in viralassembly pipeline but the viralassembly pipeline crashed downstream
  
      ### vince's nanopore pipeline:
      the pipeline is versy simeple, there were not any filtering step after mapping or post process after variant calling and before consensus calling. it did not filter out primers
      
        ```
        mpileupDepth=100000
        ivarMinDepth=10
        ivarQual=10
        ivarFreqThreshold=0.75
        ONTKeepLen=20
        ONTQualThreshold=20
        
        minimap2 -ax map-ont -t "${cpus}" "${ref}" "$prefix"_rk.reads.fastq.gz samtools view -F 4 -Sb samtools sort -o aligned2ref/"$prefix".sorted.bam
        samtools index aligned2ref/"$prefix".sorted.bam
        keepLen="${ONTKeepLen}"
        qualThreshold="${ONTQualThreshold}"
  
        samtools flagstat -O tsv aligned2ref/"$prefix".sorted.bam > "$prefix"_bamstats.tsv
        samtools mpileup -A -d "${mpileupDepth}" -Q 0 aligned2ref/"$prefix".sorted.bam | ivar consensus -p consensus/"$prefix".consensus -t "${ivarFreqThreshold}" -m "${ivarMinDepth}" -q "${ivarQual}" -n N
        samtools coverage ./aligned2ref/"$prefix".sorted.bam -o "$prefix"_coverage.tsv
  
         ```
  
  ## January 26, 2026
  
    - Group meeting: update rsv-analyzer
    - Bioinformatics meeting:
    - help anita on how to get unmapped reads from bam file out, suggest her to use samtool to do it
    - work on debuging rsv-analyzer to see what is the reasons of the amplicon drop out
      
      ```
      # new version of the align_trim
      align_trim --samfile ../250116_S_N_012-barcode02.sorted.bam \
        --output 250116_S_N_012-barcode02.trimmed.rg.sorted.bam \
        --report align_trim_report.tsv \
        --amp-depth-report  amp_depth_report.tsv \
        --normalise 1000 scheme_7col.bed
  
      ```
  
  ## January 27, Tuesday:
  - continue working on rsv-analyzer to see why some of the regions get very shallow depth:
    - Visualize the depth profiles generated at the different stage, with different tools for nanpore ligation and one from illumina original protocol
  
      <img width="600" alt="image" src="https://github.com/user-attachments/assets/dbfe2a6f-1a0b-4b07-acb9-f47d7fc9649f" />
      
      **Figure 1.** depth profile of illumina original protocol
      
       <img width="600" alt="image" src="https://github.com/user-attachments/assets/4bc0f140-cc0a-4c60-85c0-6f77ad567cb6" />
            
      **Figure 2.** depth profile with primer untrimmed bam: *.sorted.bam (before using align_trim)
      
      <img width="600"  alt="image" src="https://github.com/user-attachments/assets/87100333-19df-4a06-b372-c8d2c22a22d4" />
      
      **Figure 3.** depth profile with primer trimmed bam: *.primertrimmed.rg.sorted.bam
          
      
      <img width="600" alt="image" src="https://github.com/user-attachments/assets/8ab1502c-513c-4f50-ac7b-cf6cc20e275d" />
       
       **Figure 4.** depth profile *.trimmed.rg.sorted.bam using align_trim 1.8.5
  
      By looking into the visualization, amplicon 1, 18, 26, 27 are almost missing even from the original mapping. 20 is also not good. I feel those amplicons was not in the original product
  rsvA scheme
  
  - Are those missing regions have bigger amplicon size?
    | AmpliconID | RSV_A_Amplicon_Size (bp) | RSV_B_Amplicon_Size (bp) |
    |------------|--------------------------|--------------------------|
    | **1**          | 769                      | 754                      |
    | 2          | 739                      | 736                      |
    | 3          | 784                      | 773                      |
    | 4          | 742                      | 751                      |
    | 5          | 743                      | 742                      |
    | 6          | 740                      | 753                      |
    | 7          | 735                      | 764                      |
    | 8          | 781                      | 757                      |
    | 9          | 762                      | 776                      |
    | 10         | 749                      | 729                      |
    | 11         | 801                      | 781                      |
    | 12         | 772                      | 781                      |
    | 13         | 774                      | 763                      |
    | 14         | 751                      | 747                      |
    | 15         | 757                      | 766                      |
    | 16         | 876                      | 873                      |
    | 17         | 744                      | 764                      |
    | **18**         | 888                      | 783                      |
    | 19         | 792                      | 755                      |
    | 20         | 790                      | 754                      |
    | 21         | 787                      | 731                      |
    | 22         | 773                      | 745                      |
    | 23         | 779                      | 753                      |
    | 24         | 773                      | 844                      |
    | 25         | 753                      | 836                      |
    | **26**         | 1276                     | 854                      |
    | **27**         | 735                      | NA                       |
  
  
  - Are the primers in those missing regions not matching well with reference?
    - the bed files used in the rsv-analyzer
  
    ```
    PP109421.1      0       8       RSVA-750_1_LEFT_1       1       +       CGTACAACAAACTTGCGTAAACCA
    PP109421.1      742     768     RSVA-750_1_RIGHT_3      1       -       GCCTGTCTTTCATCWAGYTTTCTCAC
    PP109421.1      622     646     RSVA-750_2_LEFT_1       2       +       ACACCACAAAGACTGATGATCACA
    PP109421.1      1331    1360    RSVA-750_2_RIGHT_1      2       -       TGAGTATTTTTATGGTGTCTTCTCTTCCT
    PP109421.1      1182    1202    RSVA-750_3_LEFT_1       1       +       ATCCAACGGAGCACAGGAGA
    PP109421.1      1941    1965    RSVA-750_3_RIGHT_1      1       -       CACAACTTGTTCCATTTCTGCTTG
    PP109421.1      1849    1872    RSVA-750_4_LEFT_1       2       +       TTATGAATGCCTATGGTGCAGGG
    PP109421.1      2563    2590    RSVA-750_4_RIGHT_1      2       -       AGGGTCTTCTTTGAAACTTACTAGAGG
    PP109421.1      2299    2326    RSVA-750_5_LEFT_1       1       +       TGGGGCAAATAAATCATCATGGAAAAG
    PP109421.1      3011    3041    RSVA-750_5_RIGHT_1      1       -       AGAAATCTTCAAGTGATAGATCATTGTCAC
    PP109421.1      2930    2955    RSVA-750_6_LEFT_1       2       +       AGATGGCAAAAGACACATCAGATGA
    PP109421.1      3642    3669    RSVA-750_6_RIGHT_2      2       -       TTCACATAAAGCAATrATrTCATGTGT
    PP109421.1      3258    3280    RSVA-750_7_LEFT_1       1       +       TCCACATACACAGCTGCTGTTC
    PP109421.1      3967    3992    RSVA-750_7_RIGHT_1      1       -       TCTTCCATGGGTTTGATTGCAAATC
    PP109421.1      3880    3908    RSVA-750_8_LEFT_1       2       +       AAAGTCAATTCATAGTAGATCTTGGAGC
    PP109421.1      4635    4660    RSVA-750_8_RIGHT_2      2       -       TTTGGACATKYTTGCATTTGCCCCA
    PP109421.1      4432    4453    RSVA-750_9_LEFT_1       1       +       GCTACCAAGAGCTCGAGTCAA
    PP109421.1      5170    5193    RSVA-750_9_RIGHT_1      1       -       GTTGGATTGTTGCTGCATATGCT
    PP109421.1      4927    4949    RSVA-750_10_LEFT_1      2       +       CAGAATCCCCAGCTTGGAATCA
    PP109421.1      5648    5675    RSVA-750_10_RIGHT_1     2       -       TTTTGATTCTGTTTGATTTGGTCATGG
    PP109421.1      5234    5256    RSVA-750_11_LEFT_1      1       +       GAAAGAAAACCACCACCAAGCC
    PP109421.1      6011    6034    RSVA-750_11_RIGHT_2     1       -       CTTGGTAGTTCTCTTCTGGCTCG
    PP109421.1      5794    5818    RSVA-750_12_LEFT_1      2       +       TCAATCAACATGCAGTGCAGTTAG
    PP109421.1      6536    6565    RSVA-750_12_RIGHT_1     2       -       GACATGATAGAGTAACTTTGCTGTCTAAC
    PP109421.1      6212    6235    RSVA-750_13_LEFT_1      1       +       TCCACAAACAAGGCTGTAGTCAG
    PP109421.1      6959    6985    RSVA-750_13_RIGHT_1     1       -       CCACGATTTTTATTGGATGCTGTACA
    PP109421.1      6901    6924    RSVA-750_14_LEFT_1      2       +       AAGCAGCTCCGTTATCACATCTC
    PP109421.1      7629    7651    RSVA-750_14_RIGHT_1     2       -       CCTTCGTGACATATTTGCCCCA
    PP109421.1      7350    7370    RSVA-750_15_LEFT_1      1       +       CCAGAAGCACACCAGTCACA
    PP109421.1      8082    8106    RSVA-750_15_RIGHT_1     1       -       ACGTCTGCTGGCAATCTTTTTAAC
    PP109421.1      8053    8083    RSVA-750_16_LEFT_1      2       +       AGGAAGAACAATAAACAAACTATCCATCTG
    PP109421.1      8900    8928    RSVA-750_16_RIGHT_1     2       -       GCCCCAGTTTATTCAATATAGCATAGAC
    PP109421.1      8624    8654    RSVA-750_17_LEFT_1      1       +       GGAAGTTACATATTCAATGGTCCTTATCTC
    PP109421.1      9337    9367    RSVA-750_17_RIGHT_1     1       -       CTACTAAGGCTAATATCTTTCCATGTCAAG
    PP109421.1      9015    9042    RSVA-750_18_LEFT_2      2       +       CAGCTGTTAAGGATAATCAATCTCATC
    PP109421.1      9875    9902    RSVA-750_18_RIGHT_2     2       -       ACTACTTAACAAGTAAAATTTGGTCTC
    PP109421.1      9846    9874    RSVA-750_19_LEFT_2      1       +       CCATGGATGCTGTTAAAGTTAATTGCAA
    PP109421.1      10612   10637   RSVA-750_19_RIGHT_1     1       -       GCTGAGATCTGTGATGATAGAGCAC
    PP109421.1      10480   10510   RSVA-750_20_LEFT_2      2       +       CATTTTACAATTYTTyCCTGAAAGTCTTAC
    PP109421.1      11239   11269   RSVA-750_20_RIGHT_1     2       -       TGTGTCAAACTACCTATAGATTCTAGACTC
    PP109421.1      10987   11009   RSVA-750_21_LEFT_1      1       +       ACCAGTCAGACTCATGGAAGGT
    PP109421.1      11750   11773   RSVA-750_21_RIGHT_2     1       -       ACTGCCAGTCTATTGATTTCRCT
    PP109421.1      11542   11566   RSVA-750_22_LEFT_1      2       +       CCTCACAGAGGCTATAGTTCACTC
    PP109421.1      12292   12314   RSVA-750_22_RIGHT_1     2       -       TGGTTTAGTGGGTCCTCTCTCA
    PP109421.1      12029   12057   RSVA-750_23_LEFT_1      1       +       GAGATGATGAGGAAAAACATAACTTTGC
    PP109421.1      12781   12807   RSVA-750_23_RIGHT_1     1       -       TGGGAGGTTTCATCAAATGTATCTCA
    PP109421.1      12614   12641   RSVA-750_24_LEFT_1      2       +       CACTTTGATACTAGCCCTATTAATCGC
    PP109421.1      13358   13386   RSVA-750_24_RIGHT_1     2       -       CTGCTACATTAAGACGTTTAAGAAACCA
    PP109421.1      13243   13273   RSVA-750_25_LEFT_1      1       +       CAGTAGTTATTGGAAGTCTATGTCTAAGGT
    PP109421.1      13966   13995   RSVA-750_25_RIGHT_1     1       -       TGAATCTATTAATATGATGCCAAGGAAGC
    PP109421.1      13883   13911   RSVA-750_26_LEFT_2      2       +       AATACAGCCAAATCCAACCAACTTTACA
    PP109421.1      15126   15158   RSVA-750_26_RIGHT_2     2       -       AGATCAAAATGATAAYTTTAGGATTAGTTCAC
    PP109421.1      14082   14104   RSVA-750_27_LEFT_1      1       +       TCATAGGTGAAGGAGCAGGGAA
    PP109421.1      14792   14816   RSVA-750_27_RIGHT_1     1       -       GCTGAAAACTTCATTACGTCCAGC
    ```
    - aligned the primers from missing regions to references to see whether the alignments are good?
      - Amplicon 1: very bad alignment between primer and referecne 
      - Ampicon 18:
  
        ```
        1          cagctgttaaggataatcaatctcatc left primer
        2          tagctgtcaaggataatcaatctcatc  ref
                   .******.*******************
        ```
  
    - amplicon 26
      ```
      1          aatacagccaaatccaaccaactttaca forward primer
      2          aatacagccaaatctaaccaactttaca  ref
                 **************.*************
  
      1          agatcaaaatgataaytttaggattagttcac reverse primer
      2          agatcaaaatgataattttaggattagttcac
                 *************** **************** ref
      ```
    - amplicon 27: we have exact matched primer
    - matthew suggest to try the follwing pipelines: [artic-rsv](https://github.com/artic-network/artic-rsv) and [amplicon-nf](https://github.com/artic-network/amplicon-nf)
