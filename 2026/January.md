
# Day-to-Day Lab Log

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
 
